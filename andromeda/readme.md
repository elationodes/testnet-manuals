### About Andromeda Protocol Project

ANDROMEDA is an application platform layer that connects all public blockchains in the Cosmos ecosystem. Through our vast library of no-code smart contracts, users can harness the power of web3.

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

#### <strong>Hardware requirements</strong><br>
Memory: 8 GB RAM<br>
CPU: 4-Core<br>
Disk: 200 GB of storage<br>
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

