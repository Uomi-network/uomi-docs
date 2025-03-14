# Binary

In this guide, we will use the binary provided in Uomi release.

If you have experience with Rust compilation, you can also build the binary from the repo.

## Installing an Archive Node

### System Requirements

> ⚠️ **Minimum Hardware Requirements**
>
> * RAM: 16GB&#x20;
> * Storage: 500GB
> * CPU: 8 cores
> * Good network connectivity

### Prerequisites

Before starting the installation, ensure you have:

* Ubuntu 20.04 LTS or higher
* Downloaded the uomi bin and genesis file from: [https://github.com/Uomi-network/uomi-node/releases/latest](https://github.com/Uomi-network/uomi-node/releases/latest)
* Get available peers at [https://app.uomi.ai/peers](https://app.uomi.ai/peers)
* Root or sudo privileges
*   The following packages installed:

    ```bash
    sudo apt-get update
    sudo apt-get install -y \
        curl \
        jq \
        build-essential \
        libssl-dev \
        pkg-config \
        cmake \
        git \
        libclang-dev
    ```

### Installation Steps

#### 1. Prepare the Environment

Create necessary directories and user:

```bash
# Create service user
sudo useradd --no-create-home --shell /usr/sbin/nologin uomi

# Create node directory
sudo mkdir -p /var/lib/uomi
sudo chown -R uomi:uomi /var/lib/uomi
```

#### 2. Install Binary Files

Add copied peers inside the genesis.json file:

```json
{
  "name": "Uomi",
  "id": "uomi",
  "chainType": "Live",
  "bootNodes": [PASTE_PEERS_ARRAY_HERE],
 "telemetryEndpoints": null,
  "protocolId": null,
  "properties": {
    "tokenDecimals": 18,
    "tokenSymbol": "UOMI"
  },
...
```

Then install binary and genesis files:

```bash
# Copy binary to system path
sudo cp ./uomi /usr/local/bin/
sudo chmod +x /usr/local/bin/uomi

# Copy genesis file
sudo cp ./genesis.json /usr/local/bin/
```

#### 3. Create Service File

Create a systemd service file at `/etc/systemd/system/uomi.service`:

```ini
[Unit]
Description=Uomi Node
After=network.target
StartLimitIntervalSec=0

[Service]
Type=simple
User=uomi
Group=uomi
Restart=always
RestartSec=10
LimitNOFILE=65535
ExecStart=/usr/local/bin/uomi \
    --name "your-archive-node-name" \
    --chain "/usr/local/bin/genesis.json" \
    --base-path "/var/lib/uomi" \
    --pruning archive \
    --rpc-cors all \
    --rpc-external \
    --rpc-methods Safe \
    --enable-evm-rpc \
    --prometheus-external \
    --telemetry-url "wss://telemetry.polkadot.io/submit/ 0"

# Hardening
ProtectSystem=strict
PrivateTmp=true
PrivateDevices=true
NoNewPrivileges=true
ReadWritePaths=/var/lib/uomi

[Install]
WantedBy=multi-user.target
```

#### 4. Start the Node

```bash
# Reload systemd
sudo systemctl daemon-reload

# Enable service
sudo systemctl enable uomi.service

# Start service
sudo systemctl start uomi.service
```

#### 5. Monitor the Node

Check node status:

```bash
sudo systemctl status uomi.service
```

View logs:

```bash
tail -f /var/log/uomi.log
```

### Verifying Installation

You can verify your node is running correctly by:

1. Checking the service status:

```bash
sudo systemctl status uomi.service
```

2. Verifying RPC endpoint:

```bash
curl -H "Content-Type: application/json" \
    -d '{"id":1, "jsonrpc":"2.0", "method": "system_health", "params":[]}' \
    http://localhost:9944
```

### Common Issues

> 🔧 **Troubleshooting**
>
> 1. **Service Won't Start**
>    * Check logs: `journalctl -u uomi.service -f`
>    * Verify file permissions
>    * Ensure ports are not in use
> 2. **Sync Issues**
>    * Verify network connectivity
>    * Check disk space
>    * Ensure sufficient RAM

### Maintenance

#### Updating the Node

1. Stop the service:

```bash
sudo systemctl stop uomi.service
```

2. Replace the binary:

```bash
sudo cp ./new-uomi /usr/local/bin/uomi
sudo chmod +x /usr/local/bin/uomi
```

3. Restart the service:

```bash
sudo systemctl start uomi.service
```

#### Backup

Regularly backup your node data:

```bash
sudo tar -czf uomi-backup-$(date +%Y%m%d).tar.gz /var/lib/uomi
```

### Security Recommendations

1. **Firewall Configuration**

```bash
sudo ufw allow 9944/tcp  # RPC
sudo ufw allow 30333/tcp # P2P
```

2. **Regular Updates**

* Keep the system updated
* Monitor security announcements
* Update the node software when new versions are released

### Stay Connected

Join our Discord community to:

* Get the latest updates about node operations
* Connect with other node operators
* Receive technical support
* Participate in community discussions

> 💬 **Join Our Community**\
> Join the [UOMI Discord server](https://discord.com/invite/KXh72E2gPe)
