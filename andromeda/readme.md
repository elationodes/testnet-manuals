### About Andromeda Protocol Project

ANDROMEDA is an application platform layer that connects all public blockchains in the Cosmos ecosystem. Through our vast library of no-code smart contracts, users can harness the power of web3.

### Services
<a href="https://api.andromeda-tn.elationodes.xyz"><strong>API</strong></a><br>
<a href="https://docs.andromedaprotocol.io/andromeda/"><strong>RPC</strong></a><br>
<a href="https://grpc.andromeda-tn.elationodes.xyz"><strong>gRPC</strong></a><br>
<strong>Peer</strong> - e1e281420ffeaae5e23d6ea08bad8787db71ebf9@65.109.88.178:11657

#### <strong>Useful Links</strong><br>
<a href="https://andromedaprotocol.io/"><strong>WebSite</strong></a><br>
<a href="https://docs.andromedaprotocol.io/andromeda/"><strong>Official Docs</strong></a><br>
<a href="https://github.com/andromedaprotocol"><strong>Github</strong></a>
<a href="https://testnet-ping.wildsage.io/andromeda"><strong>Explorer</strong></a>

#### <strong>Socials</strong><br>
<a href="https://twitter.com/andromedaprot"><strong>Twitter</strong></a><br>
<a href="https://t.me/andromedaprotocol"><strong>Telegram</strong></a><br>
<a href="https://discord.gg/hag5nGCW84"><strong>Discord</strong></a><br>
<a href="https://medium.com/andromeda-engineering"><strong>Medium</strong></a><br>

#### <strong>Min Hardware requirements</strong><br>
Memory: 8 GB RAM<br>
CPU: 4-Core<br>
Disk: 100 GB of storage<br>
OS: Ubuntu Linux 20.04 (LTS) x64<br>
Bandwidth: 1 Gbps for Download / 100 Mbps for Upload<br>

### <strong>Installation Guide</strong><br>

#### Update packages and Install dependencies
```
sudo apt update && sudo apt upgrade -y
sudo apt install curl iptables build-essential git wget jq make gcc nano tmux htop nvme-cli pkg-config libssl-dev libleveldb-dev tar net-tools clang git ncdu -y
```
#### Isntall Go
```
ver="1.19.4" && \
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz" && \
sudo rm -rf /usr/local/go && \
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz" && \
rm "go$ver.linux-amd64.tar.gz" && \
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile && \
source $HOME/.bash_profile && \
go version
```
#### Download & Compile Binaries
```
cd $HOME
rm -rf andromedad 
cd $HOME
git clone https://github.com/andromedaprotocol/andromedad.git
cd andromedad
git checkout galileo-3-v1.1.0-beta1 
make install
```
#### Init App (replace moniker with any name)
```
andromedad init <moniker> --chain-id galileo-3
andromedad config chain-id galileo-3
```
#### Downloading Genesis 
```
curl -s https://github.com/andromedaprotocol/testnets/blob/galileo-3/genesis.json
```
#### Set seeds and peers
```
SEEDS=""
PEERS="06d4ab2369406136c00a839efc30ea5df9acaf11@10.128.0.44:26656,43d667323445c8f4d450d5d5352f499fa04839a8@192.168.0.237:26656,29a9c5bfb54343d25c89d7119fade8b18201c503@192.168.101.79:26656,6006190d5a3a9686bbcce26abc79c7f3f868f43a@37.252.184.230:26656"
sed -i -e "s/^seeds *=.*/seeds = \"$SEEDS\"/; s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" $HOME/.andromedad/config/config.toml

```
#### Configure pruning
```
sed -i -e "s/^pruning *=.*/pruning = \"nothing\"/" $HOME/.andromedad/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"100\"/" $HOME/.andromedad/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"50\"/" $HOME/.andromedad/config/app.toml
```
#### Set minimum gas price
```
sed -i 's/minimum-gas-prices =.*/minimum-gas-prices = "0.025uandr"/g' $HOME/.andromedad/config/app.toml
```
#### Enable Prometheus and disable Indexing (optional)
```
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.andromedad/config/config.toml
sed -i -e "s/^indexer *=.*/indexer = \"null\"/" $HOME/.andromedad/config/config.toml
```
#### Clean old data
```
andromedad tendermint unsafe-reset-all --home $HOME/.andromedad --keep-addr-book
```
#### Create service file
```
sudo tee /etc/systemd/system/andromedad.service > /dev/null <<EOF
[Unit]
Description= Andromeda Protocol Node
After=network-online.target

[Service]
User=$USER
ExecStart=$(which andromedad) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF

sudo systemctl daemon-reload
sudo systemctl enable andromedad

```
#### Enable and start service
```
sudo systemctl daemon-reload
sudo systemctl enable andromedad
sudo systemctl restart andromedad
sudo journalctl -u andromedad -f
```
#### Creating Wallet and Validator (don't forget to save mnemonic for wallet)
```
andromedad keys add wallet
```
#### Join andromedad AI discord (link above) and request test tokens from the faucet
```
!request YOUR_WALLET_ADDRESS
```
#### Check balance of your wallet
```
andromedad q bank balances $(andromedad keys show wallet -a)
```
#### Check sync status of the node (do not create validator until node synchronized)
```
andromedad status 2>&1 | jq .SyncInfo.catching_up
```
#### Create Validator (change moniker to preferable. OPTIONAL - website, identity and details change to preferable or remove this data. )
```
andromedad tx staking create-validator \
--amount=100000uandr \
--pubkey=$(andromedad tendermint show-validator) \
--moniker="YOUR_MONIKER" \
--website="YOUR_WEBSITE" \
--identity="YOUR_KEYBASE_ID" \
--details="DETAILS_ABOUT_YOUR_SERVICES" \
--chain-id=galileo-3 \
--commission-rate=0.1 \
--commission-max-rate=0.2 \
--commission-max-change-rate=0.05 \
--min-self-delegation=1 \
--fees=10000uandr \
--from=wallet \
-y
```

#### Check your validator's data
```
andromedad q staking validator $(andromedad keys show wallet --bech val -a)
```

#### Stake to your validator
```
andromedad tx staking delegate $(andromedad keys show wallet --bech val -a) 1000000uandr --from wallet --chain-id galileo-3 --gas-prices 0.1uandr --gas-adjustment 1.5 --gas auto -y 
```
