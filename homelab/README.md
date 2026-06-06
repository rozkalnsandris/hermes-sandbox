# Homelab Docker Configuration

Pilna homelab setup ar monitoringu, UI, un smart home integrācijām.

## 📋 Komponenti

| Serviss | Ports | Apraksts |
|---------|-------|---------|
| **Prometheus** | 9090 | Metrikas savākšana |
| **Node Exporter** | 9100 | Sistēmas metriki |
| **Grafana** | 3030 | Datu vizualizācija |
| **Grafana Renderer** | 8081 | Attēlu renderēšana |
| **AdGuard Home** | 53 (DNS) | DNS filtrs, ad-blocker |
| **Uptime Kuma** | (host) | Uptime monitoring |
| **Nginx Proxy Manager** | 80, 443, 81 | Reverse proxy, SSL |
| **Portainer** | 9000, 9443 | Docker UI |
| **Home Assistant** | (host) | Smart home automation |
| **Matter Server** | (host) | Matter protokols |

## 🔧 Iestatīšana

### 1. Kopē .env failu

```bash
cp .env.example .env
```

### 2. Pārrediģē .env ar saviem dati

```bash
nano .env
```

**Obligātie mainīgie:**
- `AUTH_TOKEN` - Grafana Renderer token (random string)
- `GF_SECURITY_ADMIN_PASSWORD` - Grafana admin parole
- `GF_RENDERING_RENDERER_TOKEN` - Jāspēlē ar AUTH_TOKEN
- `GF_SERVER_ROOT_URL` - Tavs domēns vai localhost
- `GF_SERVER_DOMAIN` - Tavs domēns vai localhost
- `MATTER_SERVER_DATA_PATH` - Matter servera datu ceļš

### 3. Palaist Docker Compose

```bash
docker-compose up -d
```

### 4. Pārbaudīt statusu

```bash
docker-compose ps
docker logs portainer
```

## 🔒 Drošības piezīmes

⚠️ **DROŠĪBAS BRĪDINĀJUMS:**
- **NEKAD** necommit'o `.env` failus uz Git!
- `.env` ir iekļauts `.gitignore`
- Vienmēr izmanto stipras paroles (16+ simboli)
- Regulāri atjaunina konteineru attēlus: `docker-compose pull && docker-compose up -d`
- Paši izmanto TLS/SSL (Nginx Proxy Manager var automātiski iegūt Let's Encrypt sertifikātus)

## 📝 Mainīgo maiņa

Ja rediģē `.env`:

```bash
# Restartē konteinerus, kas izmanto mainīgos
docker-compose down
docker-compose up -d
```

## 🗂️ Failu struktūra

```
homelab/
├── docker-compose.yml      # Docker Compose konfigurācija
├── .env.example            # Paraugvērtības (neizmanto!)
├── .gitignore              # Git ignorētie faili
├── README.md               # Šis fails
└── prometheus.yml          # (nav iekļauts, papildus setup)
```

## 📚 Resursi

- [Docker Compose Docs](https://docs.docker.com/compose/)
- [Grafana Docs](https://grafana.com/docs/)
- [Home Assistant Docs](https://www.home-assistant.io/docs/)
- [Portainer Docs](https://docs.portainer.io/)

## 🔗 Adreses (no RPi5: 192.168.0.180)

- Prometheus: http://192.168.0.180:9090
- Grafana: http://192.168.0.180:3030
- Portainer: https://192.168.0.180:9443
- Nginx Proxy Manager: http://192.168.0.180:81
- Home Assistant: http://192.168.0.180:8123
- Uptime Kuma: (integrēts)

---

**Izveidots:** 2026-06-06  
**Repu:** https://github.com/rozkalnsandris/hermes-sandbox
