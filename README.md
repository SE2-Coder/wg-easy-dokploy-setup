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
   - `WG_HOST`: Your domain (e.g., `vpn.se2code.com`). This ensures generated client configs use the domain.
   - `PASSWORD`: A secure password for the Web UI.

4. **Domain & SSL**:
   - In Dokploy, go to the **Domains** tab of your application.
   - Add your domain and enable **HTTPS** (Traefik will handle the SSL certificate).

4. **Firewall Setup**:
   Ensure the following ports are open on your VPS:
   - `51820/UDP`: WireGuard VPN port.
   - `51821/TCP`: Web UI port.

5. **Initial Access**:
   Once deployed, access the Web UI at `http://<your-ip>:51821`.

## Security Note

The `./etc_wireguard` directory and the `AccessAG.sql` file are automatically excluded via `.gitignore` and should **never** be committed to the repository. The latter contains sensitive access credentials.
