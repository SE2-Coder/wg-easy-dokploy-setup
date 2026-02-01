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

### Troubleshooting & Critical Notes

> [!IMPORTANT]
> **Cloud Provider Firewall (Lightsail/Oracle/AWS)**:
> You **MUST** open port **51820 UDP** directly in your cloud provider's firewall panel. Opening it in Dokploy is not enough for the VPN tunnel.

1. **Web UI Access**: Port `51821 TCP`.
2. **Handshake but no data**: This is fixed in the current `docker-compose.yml` with `MTU=1200` and `privileged: true`.
3. **Password Issues**: Remember to use a **Bcrypt hash** for `PASSWORD_HASH` and escape `$` characters with `$$` in the Dokploy panel if necessary.
4. **IP Forwarding**: Ensure your host has forwarding enabled:
   ```bash
   echo "net.ipv4.ip_forward = 1" | sudo tee -a /etc/sysctl.conf
   sudo sysctl -p
   ```

## Built by SE2-Code

Maintainer: [SE2-Coder](https://github.com/SE2-Coder)
Website: [https://www.se2code.com](https://www.se2code.com)

### Links of Interest
- [WG-Easy GitHub](https://github.com/wg-easy/wg-easy)
- [Dokploy Documentation](https://dokploy.com/docs)
- [Bcrypt Generator](https://bcrypt-generator.com/)
