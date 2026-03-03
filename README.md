Installing OpenVAS (which is now officially known as **Greenbone Vulnerability Management** or **GVM**) on Debian 13 (Trixie) can be done in a couple of ways.

Because GVM is a complex framework with many interconnected components (PostgreSQL, Redis, web interfaces, scanners), **Greenbone officially recommends using Docker**. It isolates the environment and saves you from a lot of dependency and update headaches. However, if you prefer a bare-metal installation, Debian 13 does include the GVM packages directly in its repositories.

Here is how to do both.

### Method 1: The Docker Route (Highly Recommended)

This is the cleanest and most stable way to run GVM, as Greenbone maintains the containers directly.

**1. Install Docker and Docker Compose**
Make sure your system is up to date and has Docker installed:

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install docker.io docker-compose-v2 curl -y
sudo systemctl enable --now docker
sudo usermod -aG docker $USER

```

*(Note: You will need to log out and log back in for the `usermod` group change to take effect).*

**2. Download the Greenbone Community Containers**
Create a directory for your GVM deployment and download the official compose file:

```bash
mkdir -p ~/greenbone-community-container && cd ~/greenbone-community-container
curl -f -L https://greenbone.github.io/docs/latest/_static/docker-compose-22.4.yml -o docker-compose.yml

```

**3. Pull and Start the Containers**
Pull the latest container images and spin up the environment in the background:

```bash
docker compose -f docker-compose.yml -p greenbone-community-edition pull
docker compose -f docker-compose.yml -p greenbone-community-edition up -d

```

The initial sync of vulnerability data (NVTs, CERT, SCAP data) will happen automatically but takes a while. You can monitor the sync progress by checking the logs:

```bash
docker compose -f docker-compose.yml -p greenbone-community-edition logs -f

```

---

### Accessing the Scanner

You can access the Greenbone Security Assistant (the web interface) by navigating to:
**`https://127.0.0.1:9392`** (or your server's IP address) in your web browser.
