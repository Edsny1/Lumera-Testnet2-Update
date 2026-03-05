# Lumera v1.11.0 Güncelleme Talimatları

## 📋 Güncelleme Adımları

### Adım 1: Governance Oylaması 🗳️

```bash
# Proposal #10 için OY VER
lumerad tx gov vote 10 yes --from wallet --chain-id lumera-testnet-2 --gas-prices 0.1ulume --gas auto --gas-adjustment 1.6 -y
```

### Adım 2: Çalışma Dizinine Geç 📁

```bash
cd $HOME
```

### Adım 3: v1.11.0 Binary'sini İndir 📥

```bash
wget -O lumera_v1.11.0_linux_amd64.tar.gz https://github.com/LumeraProtocol/lumera/releases/download/v1.11.0/lumera_v1.10.1_linux_amd64.tar.gz
```

### Adım 4: Arşivi Çıkar 📦

```bash
tar -xvzf lumera_v1.11.0_linux_amd64.tar.gz
```

### Adım 5: Binary'ye Çalıştırma İzni Ver ✅

```bash
chmod +x lumerad
```

### Adım 6: Cosmovisor Upgrade Dizini Oluştur 📂

```bash
mkdir -p $HOME/.lumera/cosmovisor/upgrades/v1.11.0/bin
```

### Adım 7: Binary'yi Upgrade Dizinine Taşı 🚀

```bash
mv $HOME/lumerad $HOME/.lumera/cosmovisor/upgrades/v1.11.0/bin/lumerad
```

### Adım 8: Kurulumu Kontrol Et ✔️

```bash
ls -l $HOME/.lumera/cosmovisor/upgrades/v1.11.0/bin/
```

**Beklenen Çıktı:**

```
-rwxr-xr-x 1 root root ... lumerad
```

### Adım 9: Güncelleme Öncesi Kontroller 🔍

```bash
# Mevcut versiyonu kontrol et
lumerad version

# Node sync durumunu kontrol et
curl -s localhost:${LUMERA_PORT}657/status | jq .result.sync_info

# Proposal durumunu kontrol et
lumerad q gov proposal 9
```

## ⚠️ Önemli Notlar

1. **Otomatik Güncelleme**: Cosmovisor, proposal onaylandıktan sonra belirlenen block height'ta otomatik güncellemeyi yapacaktır
2. **Manuel Müdahale Gereksiz**: Binary'yi upgrade klasörüne koyduğunuzda, Cosmovisor gerekli zamanda otomatik geçiş yapacaktır
3. **Node Durumu**: Güncelleme sırasında node'unuzun çalışır durumda ve sync olması gerekir
4. **Yedekleme**: Güncelleme öncesi önemli verilerinizi yedeklemeniz önerilir

## 🔄 Güncelleme Sonrası Kontroller

```bash
# Node loglarını izle
sudo journalctl -fu lumerad -o cat

# Yeni versiyonu kontrol et (v1.11.0 olmalı)
lumerad version

# Node durumunu kontrol et
curl -s localhost:${LUMERA_PORT}657/status | jq

# Block height'ı kontrol et
lumerad status 2>&1 | jq .SyncInfo.latest_block_height
```

## 🧹 Temizlik (Opsiyonel)

```bash
# İndirilen tar.gz dosyasını sil
rm -f $HOME/lumera_v1.11.0_linux_amd64.tar.gz
```
