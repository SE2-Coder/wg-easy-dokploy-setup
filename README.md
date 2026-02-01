# WG-Easy on Dokploy

Deploy your own VPN server with [WG-Easy](https://github.com/wg-easy/wg-easy) using Dokploy. This setup includes a beautiful Web UI for managing WireGuard clients.

## Features

- **Beautiful Web UI**: Easy to use interface.
- **QR Code Support**: Quickly connect devices by scanning a QR code.
- **Traffic Monitoring**: See real-time traffic statistics.
- **Persistent Storage**: All configurations are saved in `./etc_wireguard`.

## Deployment Steps

1. **Clone & Push**:
   - Push this repository to your GitHub account (linked to `git@github.com:SE2-Coder/wg-easy-dokploy-setup.git`).

2. **Dokploy Configuration**:
   - Go to your Dokploy Dashboard.
   - Create a new **Compose** deployment.
   - Point it to this repository.

3. **Environment Variables**:
   Add the following variables in Dokploy:
   - `WG_HOST`: Your domain (e.g., `vpn.se2code.com`).
   - `PASSWORD_HASH`: A **bcrypt** hash of your password.

#### How to generate the PASSWORD_HASH:
You can generate the hash running this command in your terminal (using Docker):
```bash
docker run --rm -it ghcr.io/wg-easy/wg-easy wghash 'YOUR_PASSWORD'
```
> [!IMPORTANT]
> If your password contains special characters (like `&`, `$`, `#`, `!`), you **MUST** wrap it in **single quotes** `'`.

Replace `YOUR_PASSWORD` with the password you want to use. Copy the resulting hash into Dokploy.

4. **Domain & SSL**:
   - In Dokploy, go to the **Domains** tab of your application.
   - Add your domain and enable **HTTPS**.
   - **Port**: Set the port to `51821` (this is the Web UI port).

4. **Firewall Setup**:
   Ensure the following ports are open on your VPS:
   - `51820/UDP`: WireGuard VPN port.
   - `51821/TCP`: Web UI port.

5. **Initial Access**:
   Once deployed, access the Web UI at `http://<your-ip>:51821`.

### Troubleshooting Authentication

If the Web UI loads but your password is rejected:

1. **Remove the old `PASSWORD` variable**: Ensure you have deleted the `PASSWORD` environment variable from Dokploy if it existed. Having both can cause conflicts.
2. **Verify the Hash**: The hash should start with `$2y$` or `$2b$`. Make sure you copied the **entire** string output by the `wghash` command without extra spaces or quotes.
3. **Escaping `$`**: In some cases, Dokploy might try to interpret the `$` in the hash. If you are still seeing errors, try entering the hash in Dokploy with single quotes around it (e.g., `'$2y$10$...'`) or check the application logs to see if the hash is being received correctly.
4. **Redeploy**: Always click **Redeploy** after changing environment variables.
