# atlas-starter-docker

A text-to-SQL data analyst agent powered by [Atlas](https://useatlas.dev).

This project is configured for **PostgreSQL**. Ask natural-language questions, and the agent explores a semantic layer, writes validated SQL, and returns interpreted results.

## Quick Start

1. **Install dependencies:**
   ```bash
   bun install
   ```

2. **Configure environment:** Edit `.env` with your API key and database URL.

3. **Generate semantic layer:**
   ```bash
   bun run atlas -- init          # From your database
   bun run atlas -- init --demo   # Or load demo data
   ```

4. **Run locally:**
   ```bash
   bun run dev
   ```
   API at [http://localhost:3000](http://localhost:3000).

## Deploy with Docker

1. Build the image (includes nsjail for explore isolation):
   ```bash
   docker build -t atlas-starter-docker .
   ```

2. Run:
   ```bash
   docker run -p 3000:3000 \
     -e ATLAS_PROVIDER=anthropic \
     -e ANTHROPIC_API_KEY=sk-ant-... \
     -e ATLAS_DATASOURCE_URL=postgresql://... \
     atlas-starter-docker
   ```

3. To build without nsjail (smaller image, dev only):
   ```bash
   docker build --build-arg INSTALL_NSJAIL=false -t atlas-starter-docker .
   ```

## Project Structure

```
atlas-starter-docker/
├── src/                # Application source (API + UI)
├── bin/                # CLI tools (atlas init, enrich, eval)
├── data/               # Demo datasets (SQL seed files)
├── semantic/           # Semantic layer (YAML — entities, metrics, glossary)
├── .env                # Environment configuration
└── docs/deploy.md      # Full deployment guide
```

## Commands

| Command | Description |
|---------|-------------|
| `bun run dev` | Start dev server |
| `bun run build` | Production build |
| `bun run start` | Start production server |
| `bun run atlas -- init` | Generate semantic layer from database |
| `bun run atlas -- init --demo` | Load simple demo dataset |
| `bun run atlas -- init --demo cybersec` | Load cybersec demo (62 tables) |
| `bun run atlas -- diff` | Compare DB schema vs semantic layer |
| `bun run atlas -- query "question"` | Headless query (table output) |
| `bun run test` | Run tests |

## Environment Variables

| Variable | Required | Description |
|----------|----------|-------------|
| `ATLAS_PROVIDER` | Yes | LLM provider (`anthropic`, `openai`, `bedrock`, `ollama`, `gateway`) |
| Provider API key | Yes | e.g. `ANTHROPIC_API_KEY=sk-ant-...` |
| `ATLAS_DATASOURCE_URL` | Yes | Analytics database connection string |
| `DATABASE_URL` | No | Atlas internal Postgres (auth, audit). Auto-set on most platforms |
| `ATLAS_MODEL` | No | Override the default LLM model |
| `ATLAS_ROW_LIMIT` | No | Max rows per query (default: 1000) |

See `docs/deploy.md` for the full variable reference.

## Learn More

- [Atlas Documentation](https://useatlas.dev)
- [GitHub](https://github.com/AtlasDevHQ/atlas)
