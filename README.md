# Lumera Testnet-2 Güncelleme Talimatları

## ⚠️ ÖNEMLİ UYARI
**Bu adımları sadece 2025-07-02T16:00:00Z saatinden SONRA uygulayın!**
Genesis validator değilseniz, bu zamandan önce güncelleme yapmayın.

## 📋 Güncelleme Adımları

### Adım 1: Gerekli Dosyaları İndirin (Zaman sınırı YOK - önceden indirebilirsiniz)

```
# Çalışma dizinine geçin
cd $HOME

# v1.6.0 binary dosyasını indirin
wget https://github.com/LumeraProtocol/lumera/releases/download/v1.6.0/lumera_v1.6.0_linux_amd64.tar.gz

# Dosyayı çıkarın
tar -xvf lumera_v1.6.0_linux_amd64.tar.gz

# Yeni genesis.json dosyasını indirin
wget https://raw.githubusercontent.com/LumeraProtocol/lumera-networks/refs/heads/master/testnet-2/genesis.json -O genesis_testnet2.json

# Yeni claims.csv dosyasını indirin
wget https://raw.githubusercontent.com/LumeraProtocol/lumera-networks/refs/heads/master/testnet-2/claims.csv -O claims_testnet2.csv
```

### Adım 2: Node'u Durdur ⏹️

```
sudo systemctl stop lumerad
```

### Adım 3: Chain Verilerini Temizle 🗑️
**DİKKAT: Bu işlem testnet-1 verilerinizi silecektir!**

```
lumerad tendermint unsafe-reset-all
rm -rf ~/.lumera/wasm
```

### Adım 4: Cosmovisor ile Binary ve Ağ Dosyalarını Güncelle 🔄

```
# Yeni binary için dizin oluştur
mkdir -p ~/.lumera/cosmovisor/genesis/bin

# Yeni binary'yi kopyala
cp $HOME/lumerad ~/.lumera/cosmovisor/genesis/bin/lumerad
chmod +x ~/.lumera/cosmovisor/genesis/bin/lumerad

# Mevcut link'i kaldır ve yeni link oluştur
unlink ~/.lumera/cosmovisor/current
ln -s ~/.lumera/cosmovisor/genesis ~/.lumera/cosmovisor/current

# Genesis dosyasını değiştir
cp $HOME/genesis_testnet2.json ~/.lumera/config/genesis.json

# Claims dosyasını kopyala
cp $HOME/claims_testnet2.csv ~/.lumera/config/claims.csv
```

### Adım 5: Chain ID ve Peers'i Güncelle 🆔

```
# Yeni chain ID'yi ayarla
lumerad config set client chain-id lumera-testnet-2
```

# Seeds ve Persistent Peers'i güncelle
```
URL="https://lumera-rpc.coinsspor.com/net_info"
response=$(curl -s $URL)
PEERS=$(echo $response | jq -r '.result.peers[] | select(.remote_ip | test("^[0-9]{1,3}(\\.[0-9]{1,3}){3}$")) | "\(.node_info.id)@\(.remote_ip):" + (.node_info.listen_addr | capture(":(?<port>[0-9]+)$").port)' | paste -sd "," -)

echo "PEERS=\"$PEERS\""

sed -i 's|^persistent_peers *=.*|persistent_peers = "'$PEERS'"|' $HOME/.lumera/config/config.toml
```
Thanks Coinsspor

### Adım 6: Node'u Başlat 🚀

```
sudo systemctl start lumerad
```

### Adım 7: Logları Kontrol Et 📊

```
# Node durumunu kontrol et
sudo journalctl -fu lumerad -o cat

# Sync durumunu kontrol et
curl -s localhost:${LUMERA_PORT}657/status | jq .result.sync_info
```

## 🔍 Kontrol Komutları

```
# Node versiyonunu kontrol et
lumerad version

# Sync durumunu kontrol et
local_height=$(curl -s localhost:${LUMERA_PORT}657/status | jq -r .result.sync_info.latest_block_height)
echo "Node height: $local_height"

# Chain ID'yi kontrol et
lumerad status | jq -r .NodeInfo.network
```

## 💡 Sorun Giderme

Eğer node başlatmada sorun yaşarsanız:

```
# Service durumunu kontrol et
sudo systemctl status lumerad

# Detaylı log için
sudo journalctl -u lumerad --since "1 hour ago" -f

# Config dosyalarını kontrol et
lumerad config show-node-id
lumerad config show-validator
```

## ⚠️ Önemli Hatırlatmalar

1. **Zamanlama**: Bu adımları sadece 2025-07-02T16:00:00Z'den sonra uygulayın
2. **Veri Kaybı**: Bu işlem testnet-1 verilerinizi tamamen silecektir
3. **Cüzdan**: Cüzdan kelimelerinizi yedeklediğinizden emin olun
4. **Validator**: Validator iseniz, sync tamamlandıktan sonra validator durumunuzu kontrol edin

## 🎯 Güncelleme Sonrası

Güncelleme tamamlandıktan sonra:
- Node'unuz testnet-2'ye bağlanacak
- Yeni genesis blok'tan sync olmaya başlayacak
- Eski validator anahtarlarınız geçerli olacak (cüzdan kelimeleri aynı)
