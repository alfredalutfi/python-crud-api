# python-crud-api

Python REST API sederhana (tanpa framework) dengan pipeline CI/CD Jenkins → Docker.

## Endpoint

| Method | Path | Keterangan |
|---|---|---|
| GET | `/health` | Status layanan |
| GET | `/messages` | Daftar pesan + total |
| POST | `/messages` | Body `{"message": "..."}` (1–500 karakter) |
| DELETE | `/messages?index=N` | Hapus pesan ke-N |

Data disimpan di `/var/lib/backend_data/messages.json` (named volume `production-api-data`).

## Menjalankan manual

```bash
docker build -t python-crud-api:manual .
docker volume create production-api-data
docker run -d --name production-api-container -p 5001:5000 \
  -v production-api-data:/var/lib/backend_data python-crud-api:manual
curl -i http://localhost:5001/health
```

> Port host 5001 dipakai karena port 5000 di macOS sudah digunakan AirPlay Receiver.

## Test

```bash
python3 -m unittest -v test_app.py
```

## Pipeline (Jenkinsfile)

Checkout → Syntax check → Unit test → Build image (tag `BUILD_NUMBER`, `sha-<commit>`, `latest`)
→ Deploy (named volume tetap) → Health check (curl + Docker HEALTHCHECK).

Image Jenkins lab ada di `jenkins/Dockerfile` (Jenkins LTS + python3 + Docker CLI).

Webhook test Mon Nov  2 18:59:07 WIB 2026
