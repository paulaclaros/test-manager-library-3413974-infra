# Manager Library — Infraestructura (Docker Compose)

> **Evaluación Técnica · Ficha ADSO 3413974 · SENA**  
> Definición de la infraestructura contenerizada de *Manager Library* mediante Docker Compose.

---

## 🏗️ 1. Servicios Orquestados

| Servicio | Imagen / Contexto | Puerto Interno | Puerto Anfitrión | Descripción |
| :--- | :--- | :--- | :--- | :--- |
| **`db`** | `postgres:16-alpine` | `5432` | — | Motor relacional PostgreSQL 16. Arranca con esquema vacío (Artículo V). |
| **`service`** | `../test-manager-library-3413974-api` | `8080` | `8000` / `5000` | Backend API en Laravel 10 (PHP 8.2). Aplica migraciones e inicializa admin. |
| **`portal`** | `../test-manager-library-3413974-app` | `80` | `8080` | Frontend SPA en React 18 servido mediante Nginx alpine. |

---

## 🚀 2. Despliegue con un solo comando

Cumpliendo el requisito **RNF-5** (*El sistema levanta con un solo comando y la base arranca vacía*):

```bash
# 1. Copiar variables de entorno
cp .env.example .env

# 2. Levantar los contenedores en segundo plano
docker compose up -d --build
```

---

## 🔍 3. Comprobación de Salud (Healthchecks)

* **Base de datos:** `pg_isready -U manager_library -d manager_library`
* **Backend API:** `GET /health` responde `{"status":"ok"}`
* **Portal Frontend:** `wget -q http://127.0.0.1/`

---

## 🛑 4. Detener el Entorno

```bash
docker compose down
```
