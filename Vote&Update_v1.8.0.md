Lumera oylama ve Guncelleme
```
lumerad tx gov vote 4 yes --from wallet --chain-id lumera-testnet-2 --gas-prices 0.1ulume --gas auto --gas-adjustment 1.6 -y
```
Update:
```
cd $HOME
```


# 1️⃣ Lumera v1.8.0 binary dosyasını indir
```
wget -O lumera_v1.8.0_linux_amd64.tar.gz https://github.com/LumeraProtocol/lumera/releases/download/v1.8.0/lumera_v1.8.0_linux_amd64.tar.gz
```

# 2️⃣ Arşivi çıkart
```
tar -xvzf lumera_v1.8.0_linux_amd64.tar.gz
```
# 3️⃣ Çalıştırma izni ver
```
chmod +x lumerad
```
# 4️⃣ Cosmovisor upgrade dizinini oluştur
```
mkdir -p $HOME/.lumera/cosmovisor/upgrades/v1.8.0/bin
```
# 5️⃣ Binary dosyasını upgrade klasörüne taşı
```
mv lumerad $HOME/.lumera/cosmovisor/upgrades/v1.8.0/bin/lumerad
```
🧩 WasmVM v3.0.0-ibc2.0 Güncellemesi
```
cd $HOME
```
# 1️⃣ Dosyayı indir
```
wget https://github.com/CosmWasm/wasmvm/releases/download/v3.0.0-ibc2.0/libwasmvm.x86_64.so
```
# 2️⃣ Checksum doğrulaması için kontrol dosyasını indir
```
wget https://github.com/CosmWasm/wasmvm/releases/download/v3.0.0-ibc2.0/checksums.txt
```
# 3️⃣ Dosya bütünlüğünü doğrula
```
sha256sum -c checksums.txt | grep libwasmvm.x86_64.so
```
✅ Eğer sonuç “OK” çıkarsa, devam et:

# 4️⃣ Dosyayı sisteme yerleştir
```
sudo mv libwasmvm.x86_64.so /usr/lib
```
