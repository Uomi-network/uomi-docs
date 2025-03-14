# Spin up a validator

{% hint style="warning" %}
**CAUTION**

Validators are responsible for the network stability, it is very important to be able to react at any time of the day or night in case of trouble. We strongly encourage validators to set up a monitoring and alerting system, learn more about this from our secure setup guide.
{% endhint %}

### Start the AI Service

#### Prerequisite

Must have conda installed, follow the instruction [here](https://docs.conda.io/projects/conda/en/latest/user-guide/install/index.html).

Clone the [uomi-node-ai](https://github.com/Uomi-network/uomi-node-ai/) repo

### Installation Steps

#### 1. Set up Conda Environment

First, open a terminal and create a new conda environment:

```bash
conda create -n uomi-ai python=3.10 -y
```

If you've just installed Miniconda/Anaconda, initialize conda in your shell:

```bash
# For bash users
source ~/miniconda3/etc/profile.d/conda.sh
# For zsh users
source ~/miniconda3/etc/profile.d/conda.sh

# Activate the environment
conda activate uomi-ai
```

#### 2. Install PyTorch with CUDA Support

Install PyTorch and related packages:

```bash
conda install pytorch==2.2.0 torchvision==0.17.0 torchaudio==2.2.0 pytorch-cuda=12.1 -c pytorch -c nvidia
```

Verify the installation:

```bash
python -c "import torch; print(torch.cuda.is_available())"
```

This should print `True` if CUDA is properly installed.

#### 3. Install CUDA Development Tools

Install CUDA toolkit and NVCC compiler:

```bash
conda install -c nvidia/label/cuda-12.1.0 cuda-nvcc=12.1 cuda-toolkit=12.1
```

Verify the installation:

```bash
nvcc --version
```

#### 4. Install Additional Dependencies

Install the required Python packages:

```bash
# Install Optimum
pip install -U "optimum>=1.20.0"

# Install AutoGPTQ
pip install auto-gptq --no-build-isolation

# Install Transformers
pip install transformers

# Install specific NumPy version
conda install numpy=1.24

# Install Flash Attention
pip install flash-attn --no-build-isolation

# Install Flask
pip install flask
```

### Troubleshooting

#### Common Issues

1. **CUDA Not Found**
   * Verify NVIDIA drivers are installed: `nvidia-smi`
   * Check CUDA installation: `nvcc --version`
   * Ensure PyTorch CUDA is properly installed: `python -c "import torch; print(torch.version.cuda)"`
2. **Build Failures**
   *   Ensure you have build tools installed:

       ```bash
       sudo apt-get update
       sudo apt-get install build-essential
       ```
   *   For Auto-GPTQ issues, try installing from source:

       ```bash
       git clone https://github.com/AutoGPTQ/AutoGPTQ.git
       cd AutoGPTQ
       rm -rf build/*
       python setup.py install
       ```
3. **Version Conflicts**
   * If you encounter package conflicts, try creating a fresh environment
   * Consider using `pip install --no-deps` for problematic packages

#### Verification

To verify the complete setup, run this test script:

```python
import torch
import transformers
import auto_gptq
import flash_attn
import flask

print(f"PyTorch version: {torch.__version__}")
print(f"CUDA available: {torch.cuda.is_available()}")
print(f"CUDA version: {torch.version.cuda}")
print(f"Transformers version: {transformers.__version__}")
```

### Create the service

1. Create a new systemd service file for the AI component:

```bash
sudo nano /etc/systemd/system/uomi-ai.service
```

2. Add the following content to the service file:

```ini
[Unit]
Description=UOMI AI API Server
After=network.target

[Service]
User=uomi
WorkingDirectory=/home/uomi/uomi-node-ai
ExecStart=/bin/bash -c "source /home/uomi/miniconda3/etc/profile.d/conda.sh && conda activate uomi-ai && python3 uomi-ai.py"
Restart=always
RestartSec=10
TimeoutSec=30
StartLimitIntervalSec=500
StartLimitBurst=5

[Install]
WantedBy=multi-user.target
```

**Important Notes:**

* The `WorkingDirectory` path (`/home/uomi/uomi-node-ai`) should be adjusted to match your actual installation directory
* The `ExecStart` command assumes:
  * Miniconda is installed in `/home/uomi/miniconda3`
  * A conda environment named `uomi-ai` exists
  * The main Python script is named `uomi-ai.py`
* Modify these paths and names according to your specific setup

3. Enable and start the AI service:

```bash
sudo systemctl daemon-reload
sudo systemctl enable uomi-ai.service
sudo systemctl start uomi-ai.service
```

4. Verify the service is running:

```bash
sudo systemctl status uomi-ai.service
```

5. Monitor the AI service logs:

```bash
sudo journalctl -u uomi-ai.service -f
```

## Set-up the validator Node

### Download the binary

Make sure to download the uomi bin and genesis file on your machine from: [https://github.com/Uomi-network/uomi-node/releases/latest](https://github.com/Uomi-network/uomi-node/releases/latest)

### Directory Structure Setup

Create the necessary directories and set appropriate permissions:

```bash
# Create directories
sudo mkdir -p /var/lib/uomi
sudo mkdir -p /usr/local/bin

# Create user for the service
sudo useradd --no-create-home --shell /usr/sbin/nologin uomi

# Set permissions
sudo chown -R uomi:uomi /var/lib/uomi
```

### Binary and Genesis Setup

Get available peers at [https://app.uomi.ai/peers](https://app.uomi.ai/peers)

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

1. Copy the Uomi binary to the correct location:

```bash
sudo cp ./uomi /usr/local/bin/uomi
sudo chmod +x /usr/local/bin/uomi
```

2. Copy the genesis file:

```bash
sudo cp ./genesis.json /usr/local/bin/genesis.json
```

#### Create validator account <a href="#service-parameters" id="service-parameters"></a>

This account is the one that will validate blocks and will save output from Agents

```
/usr/local/bin/uomi key generate --scheme Sr25519
```

You will get something like:

```
Secret phrase:     tackle rebuild neither turn degree real capital armed giraffe novel resist human
Network ID:        substrate
Secret seed:       0x5c9499827e606bcf284e8a650a96ce13ebf0484bd64a280507ff96d99da6e174
Public key (hex):  0x14ceec080a6aa464b4235dd951a85669d25e4fa659578f01a7a7dc5c95454931
Account ID:        0x14ceec080a6aa464b4235dd951a85669d25e4fa659578f01a7a7dc5c95454931
Public key (SS58): 5CXzHQ3DmYKemRAyY6NPyKsDWiChbeeKtxW3q5RWpmdUQvdR
SS58 Address:      5CXzHQ3DmYKemRAyY6NPyKsDWiChbeeKtxW3q5RWpmdUQvdR
```

Generate Ed25519 key for GRANDPA using the same seed phrase:

```bash
/usr/local/bin/uomi key inspect --scheme Ed25519 "YOUR_SEED_PHRASE"
```

you'll need the secret phrase and the secret seed later.

{% hint style="warning" %}
**CAUTION**

Save your secret phrase and secret seed in a safe place (preferably offline), it's the only way to recover your account if you lose it
{% endhint %}

### Systemd Service Configuration

Create a systemd service file at `/etc/systemd/system/uomi.service`:

```ini
iniCopy[Unit]
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
    --validator \
    --name "YOUR_NODE_NAME" \
    --chain "/usr/local/bin/genesis.json" \
    --base-path "/var/lib/uomi" \
    --state-pruning 1000 \
    --blocks-pruning 1000 \
    --enable-evm-rpc \
    --rpc-cors all \
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

### Enable and start the service:

```bash
sudo systemctl daemon-reload
sudo systemctl enable uomi.service
sudo systemctl start uomi.service
```

### Add your key to your node

Once your node is running, insert your keys:

{% hint style="warning" %}
**ATTENTION**\
Replace _**SECRET\_PHRASE**_ and _**PUBLIC\_KEY**_ with your data received with the first command "key generate **--scheme Sr25519**"
{% endhint %}

```bash
for key_type in babe imon uomi ipfs; do
    curl -H "Content-Type: application/json" \
        -d '{"id":1, "jsonrpc":"2.0", "method":"author_insertKey", "params":["'$key_type'", "SECRET_PHRASE", "PUBLIC_KEY"]}' \
        http://localhost:9944
done
```

{% hint style="warning" %}
**ATTENTION**\
Replace _**SECRET\_PHRASE**_ and _**PUBLIC\_KEY**_ with your data received with the first command "key inspect **--scheme Ed25519**"
{% endhint %}

```bash
curl -H "Content-Type: application/json" \
    -d '{"id":1, "jsonrpc":"2.0", "method":"author_insertKey", "params":["gran", "SECRET_PHRASE", "PUBLIC_KEY"]}' \
    http://localhost:9944
```

This data will be stored on your local node, no one can have access to them.

### Add the account  <a href="#verify-synchronization" id="verify-synchronization"></a>

Now you can import the account you created previously with SECRET\_PHRASE to your wallet extension and fund it with the tokens you want to bond

#### Verify synchronization[​](https://docs.astar.network/docs/build/nodes/collator/spinup_collator#verify-synchronization) <a href="#verify-synchronization" id="verify-synchronization"></a>

Before jumping to the next steps, you have to wait until your node is **fully synchronized**. This can take a long time depending on the chain height.

Check the current synchronization:

```
journalctl -f -u uomi -n100
```

### Session Keys[​](https://docs.astar.network/docs/build/nodes/collator/spinup_collator#session-keys) <a href="#session-keys" id="session-keys"></a>

**Author session keys**[**​**](https://docs.astar.network/docs/build/nodes/collator/spinup_collator#author-session-keys)

Run the following command to author session keys:

```bash
curl -H "Content-Type: application/json" -d '{"id":1, "jsonrpc":"2.0", "method": "author_rotateKeys", "params":[]}' http://localhost:9944
```

The result will look like this (you just need to copy the result):

```
{"jsonrpc":"2.0","result":"0x600e6cea49bdeaab301e9e03215c0bcebab3cafa608fe3b8fb6b07a820386048","id":1}
```

**Register as validator**

Go to the [Polkadot.js portal](https://subexp.uomi.ai) and connect to the network.

Go to _**Network > Staking > Accounts**_ and then select **Validator** on the right&#x20;

<figure><img src="../../../.gitbook/assets/Screenshot 2024-08-29 alle 09.46.45.png" alt=""><figcaption></figcaption></figure>

Select the account you funded previously, the amount you want to bond and the payment method and click **next.**

Now insert the key you copied before and follow the instructions, click "bond & validate" and then submit the transaction.

{% hint style="info" %}
**INFO**\
Onboarding takes place at `n+1` session.
{% endhint %}

You can now [set your identity](set-your-identity.md).&#x20;
