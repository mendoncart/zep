# ZEP Community Edition (Containerized)

This repository provides a ready-to-use Docker setup for running the **ZEP Community Edition**, along with its supporting services (database, graph engine, and dashboard).

## 🧱 Folder Structure

You should have the following directory layout on your host machine:

```
└── 📁opt
    └── 📁zep
        └── 📁zep
        │   └── zep.yaml              # your configuration file (contains secrets)
        ├── 📁zep-db                    # PostgreSQL data
        ├── 📁neo4j_data                # Neo4j data
        └── 📁dashboard                 # Dashboard files
            ├── index.html            # your dashboard web UI
            └── default.conf          # nginx configuration

````

> **Do not store `zep.yaml` inside the repository.**  
> This file often contains API keys or sensitive data. Mount it as a volume instead.

---

## 🐳 Docker Compose Setup

1. Copy `legacy/docker-compose.ce.yaml` to your deployment directory.
2. Adjust paths to match your host environment if needed.
3. Make sure the following files exist:
   - [`/opt/zep/zep/zep.yaml`](https://github.com/mendoncart/zep/blob/main/legacy/zep.yaml)
   - [`/opt/zep/dashboard/index.html`]()
   - [`/opt/zep/dashboard/default.conf`]()

Then run:

```bash
docker compose -f docker-compose.ce.yaml up -d
````

This will start:

* `zep` (main service)
* `graphiti` (graph engine)
* `neo4j` (graph database)
* `db` (PostgreSQL with pgvector)
* `zep-dashboard` (nginx serving your UI)
* `zep-graph` (graph visualization)

---

## 🔧 Configuration Notes

* Edit `/opt/zep/zep/zep.yaml` to configure your instance.
* Environment variables (such as API keys) can override YAML settings if supported by ZEP.
* You can safely update the dashboard or NGINX configuration by modifying the files in `/opt/zep/dashboard/`.

---

## 🚀 Building and Publishing the Docker Image

To build and publish the image manually from GitHub Actions:

1. Go to the **Actions** tab.
2. Select **Manual Release and Docker Publish (ZEP CE)**.
3. Click **Run workflow**.

This will:

* Automatically bump the version number (`vX.Y.Z`),
* Create a GitHub Release with changelog,
* Build the image from `legacy/Dockerfile.ce`,
* Push it to GitHub Container Registry:

```
ghcr.io/your-username/your-repository:latest
```

and

```
ghcr.io/your-username/your-repository:vX.Y.Z
```

---

## 🧩 Notes

* You can pull the latest version of ZEP CE with:

```bash
docker pull ghcr.io/your-username/your-repository:latest
```

* Update your compose file to use that image tag to stay current.
* Make sure `/opt/zep/` directories exist and have the right permissions before running the stack.

---

## 🛠 Example Commands

Check logs:

```bash
docker compose -f docker-compose.ce.yaml logs -f zep
```

Stop all services:

```bash
docker compose -f docker-compose.ce.yaml down
```
