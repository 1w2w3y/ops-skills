# AMG Toolkit

Claude Code plugin for [Azure Managed Grafana](https://learn.microsoft.com/en-us/azure/managed-grafana/) — health checks, cost analysis, and diagnostics for Azure resources via the built-in AMG-MCP server.

## Install

```
/plugin marketplace add 1w2w3y/ops-skills
/plugin install amg-toolkit@ops-skills
```

Then set environment variables:

```bash
export AMG_MCP_URL="https://<your-grafana-endpoint>/api/azure-mcp"
export AMG_MCP_TOKEN="<your-service-account-token>"
```

## Prerequisites

- An **Azure Managed Grafana** instance with the [MCP server enabled](https://learn.microsoft.com/en-us/azure/managed-grafana/grafana-mcp-server)
- A Grafana service account token or Entra ID token
- [Claude Code](https://claude.ai/code)

## Available Skills

| Skill | Command | Description |
|---|---|---|
| PostgreSQL Flex | `/amg-check-pg-flex 7d` | Fleet-wide health check for Azure PostgreSQL Flexible Servers |
| Cosmos DB MongoDB | `/amg-check-cosmosdb-mongo-ru 7d` | Health check for Cosmos DB for MongoDB (RU) accounts |
| Storage Account | `/amg-check-storage-account 7d` | Health check for Azure Storage Accounts |
| Key Vault | `/amg-check-key-vault 7d` | Health check for Azure Key Vaults |
| Azure Spend | `/amg-check-azure-spend` | Monthly cost analysis for Azure subscriptions |

## How It Works

Each skill queries Azure resources through the AMG-MCP (Model Context Protocol) server built into Azure Managed Grafana. Skills follow a phased workflow:

1. **Validate datasource** — confirm Azure Monitor datasource
2. **Discover resources** — inventory via Azure Resource Graph
3. **Tier 1 scan** — key metrics across the entire fleet
4. **Tier 2 deep dive** — detailed investigation of abnormal resources
5. **Report** — findings with known issue cross-reference

On first run, each skill auto-discovers the Azure Monitor datasource UID and prompts for the subscription ID(s) to scan. Configuration is saved to `memory/<skill-name>/config.md`.

## Environment Variables

| Variable | Description |
|---|---|
| `AMG_MCP_URL` | Your Grafana MCP endpoint (e.g., `https://my-grafana.wcus.grafana.azure.com/api/azure-mcp`) |
| `AMG_MCP_TOKEN` | Bearer token for authentication |

## License

See [LICENSE](LICENSE).
