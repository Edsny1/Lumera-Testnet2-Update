```
lumerad tx gov vote 6 yes --from wallet --chain-id lumera-testnet-2 --gas-prices 0.1ulume --gas auto --gas-adjustment 1.6 -y
```

```
cd $HOME
```
```
wget -O lumera_v1.8.5_linux_amd64.tar.gz https://github.com/LumeraProtocol/lumera/releases/download/v1.8.5/lumera_v1.8.5_linux_amd64.tar.gz
```
```
tar -xvzf lumera_v1.8.5_linux_amd64.tar.gz
```
```
chmod +x lumerad
```
```
mkdir -p $HOME/.lumera/cosmovisor/upgrades/v1.8.5/bin
```
```
mv $HOME/lumerad $HOME/.lumera/cosmovisor/upgrades/v1.8.5/bin/lumerad
```
```
ls -l $HOME/.lumera/cosmovisor/upgrades/v1.8.5/bin/
```
Expected Output:
```
-rwxr-xr-x 1 root root ... lumerad
```
