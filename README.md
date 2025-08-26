# 🚀 OPC UA Client Text IO

**Yapay Zeka destekli Node.js tabanlı OPC UA Client uygulaması**  
OPC UA sunucularından veri okuyarak metin dosyasına kaydeder.  

---

## 📦 Docker ile Kullanım

### 1. Docker Image Oluşturma
```bash
# Docker image'ı build et
docker build -t opcua-client-text-io .

# Veya docker-compose ile
docker-compose build
```

### 2. Farklı Makine Tipleri ile Çalıştırma  

#### 🏭 Machine 1 (Ramp1-Ramp4)
```bash
# docker-compose ile
docker-compose -f docker-compose.machine1.yml up -d

# veya Docker ile
docker run -d   --name opcua-client-machine1   -v $(pwd)/config:/app/config   -e MACHINE_TYPE=machine1   opcua-client-text-io
```

#### 🏭 Machine 2 (Ramp5-Ramp8)
```bash
# docker-compose ile
docker-compose -f docker-compose.machine2.yml up -d

# veya Docker ile
docker run -d   --name opcua-client-machine2   -v $(pwd)/config:/app/config   -e MACHINE_TYPE=machine2   opcua-client-text-io
```

#### ⚙️ Default (Tüm Ramp'ler)
```bash
# docker-compose ile
docker-compose up -d

# veya Docker ile
docker run -d   --name opcua-client   -v $(pwd)/config:/app/config   opcua-client-text-io
```

### 3. Logları İzleme
```bash
# docker-compose ile
docker-compose -f docker-compose.machine1.yml logs -f
docker-compose -f docker-compose.machine2.yml logs -f
docker-compose logs -f

# veya Docker ile
docker logs -f opcua-client-machine1
docker logs -f opcua-client-machine2
docker logs -f opcua-client
```

### 4. Uygulamayı Durdurma
```bash
# docker-compose ile
docker-compose -f docker-compose.machine1.yml down
docker-compose -f docker-compose.machine2.yml down
docker-compose down

# veya Docker ile
docker stop opcua-client-machine1 opcua-client-machine2 opcua-client
docker rm opcua-client-machine1 opcua-client-machine2 opcua-client
```

---

## ⚙️ Konfigürasyon

Config dosyası `config/config.txt` içerisinde bulunur:  

```ini
# OPC UA server endpoint
endpointUrl=opc.tcp://your-server:4840

# Security settings
securityMode=None
securityPolicy=None
userName=
password=

# Node IDs to monitor
nodes=ns=2;s=Tag1,ns=2;s=Tag2

# Operation mode
mode=monitor  # monitor veya read
intervalMs=1000

# Output file
output=output.txt
```

---

## 📝 Çıktı Formatı

Veriler `output/output.txt` dosyasına şu formatta kaydedilir:  

```
2024-01-15T10:30:45.123Z | ns=2;s=Tag1 | 123.45 | Good | src=2024-01-15T10:30:45.000Z | srv=2024-01-15T10:30:45.000Z
```

**Format:**  
`Zaman | NodeID | Değer | Durum | Kaynak Zamanı | Sunucu Zamanı`

---

## 📂 Volume Mounts

- `./config` → `/app/config`: Konfigürasyon dosyaları

---

## 📜 Loglar

Uygulama logları Docker logs üzerinden izlenebilir:  

```bash
docker-compose logs -f
# veya
docker logs -f opcua-client
```

---

## 🌍 Environment Variables

- `NODE_ENV` → `production` / `development`  
- `MACHINE_TYPE` → `machine1`, `machine2`, `default`  

---

## 🏭 Makine Tipleri

- **machine1** → Ramp1, Ramp2, Ramp3, Ramp4  
- **machine2** → Ramp5, Ramp6, Ramp7, Ramp8  
- **default** → Tüm Ramp tag’leri (`config.txt` üzerinden)  
