<div align="center">

# ˗ˏˋ traefik ˎˊ˗

**Reverse proxy Docker -- un `compose up`, tout est routé.**
Dashboard, discovery automatique, configuration dynamique.

<br>

[![Traefik](https://img.shields.io/badge/Traefik-3.6-7aa2f7?style=for-the-badge&logo=traefikproxy&logoColor=white)](https://traefik.io)
[![Docker](https://img.shields.io/badge/Docker-Compose-7dcfff?style=for-the-badge&logo=docker&logoColor=white)](https://docs.docker.com/compose/)

</div>

<br>

## Features

**Discovery automatique** -- Traefik détecte les containers Docker et crée les routes à la volée via les labels. Zéro config manuelle par service.

**Configuration dynamique** -- Le dossier `config/dynamic/` est watché en continu. Ajouter un fichier YAML suffit pour déclarer middlewares, TLS ou routes custom.

**Dashboard** -- Interface d'administration intégrée, accessible sur `traefik.localhost`.

<br>

## Structure

```
traefik/
├── compose.yaml                # Stack Traefik
└── config/
    ├── traefik.yaml            # Configuration statique
    └── dynamic/                # Configurations dynamiques (watchées)
        └── .gitkeep
```

<br>

## Stack

| Composant | Technologie |
|-----------|-------------|
| **Reverse proxy** | Traefik 3.6 |
| **Entrypoints** | HTTP (:80) |
| **Providers** | Docker, File |
| **Infra** | Docker Compose |

<br>

## Installation

```bash
git clone git@github.com:keyxmare/traefik.git
cd traefik
docker compose up -d
```

Le dashboard est disponible sur [`traefik.localhost`](http://traefik.localhost).

<br>

## Utilisation

### Exposer un service

Ajouter ces labels dans le `compose.yaml` du service à router :

```yaml
services:
  app:
    image: mon-app
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.app.rule=Host(`app.localhost`)"
    networks:
      - traefik

networks:
  traefik:
    external: true
```

### Ajouter une configuration dynamique

Déposer un fichier YAML dans `config/dynamic/` -- Traefik le prend en compte automatiquement.

```yaml
# config/dynamic/middlewares.yaml
http:
  middlewares:
    redirect-https:
      redirectScheme:
        scheme: https
        permanent: true
```

<br>

---

<div align="center">

[MIT](LICENSE)

</div>
