# osiris-intel — Entity Resolution Server

Separate Express microservice (not part of the Next.js app). The "brain" for
person/entity lookups: ingests sanctions data, resolves entities via Wikidata.

## What it does

- Ingests the OpenSanctions OFAC SDN bulk CSV (refreshed every 24h)
- Resolves entities on demand via Wikidata SPARQL (aggressive LRU cache, 24h TTL)
- Exposes `GET /resolve` — the single endpoint other services query

## Run it

```bash
cd intel
npm install        # dependencies are NOT committed to git (see .gitignore)
npm start          # serves on port 4000 (override with INTEL_PORT)
```

Or via Docker: `docker build -f intel/Dockerfile .` (see repo `docker-compose.yml`).

## Security notes

- Outbound requests only to allowlisted domains
- SPARQL inputs sanitized against injection
- Rate-limited per client IP

## Deploy

This is a **separate deployment** from the main Next.js app — it needs its own
host/process on port 4000. The main app calls it at the configured intel URL.
