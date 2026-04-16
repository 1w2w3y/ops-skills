# AMG skills

Claude Code plugin for [Azure Managed Grafana](https://learn.microsoft.com/en-us/azure/managed-grafana/) — health checks, cost analysis, and diagnostics for Azure resources via the built-in AMG-MCP server.

## Install

Add the marketplace:

```
/plugin marketplace add 1w2w3y/ops-skills
```

Install the plugin:

```
/plugin install amg-toolkit@ops-skills
```

Then set environment variables:

```bash
export AMG_MCP_URL="https://<your-grafana-endpoint>/api/azure-mcp"
```

```bash
export AMG_MCP_TOKEN="<your-service-account-token>"
```

## Update

When a new version of the plugin is released, refresh the marketplace metadata:

```
/plugin marketplace update ops-skills
```

Then re-run install to pull the new version:

```
/plugin install amg-toolkit@ops-skills
```

Updates are manual — Claude Code does not auto-poll the marketplace. Claude Code detects updates by comparing the `version` field in `marketplace.json`; if the plugin maintainer bumped the version, the install command pulls the new release. You can verify the currently installed version with:

```
/plugin list
```

## Uninstall

```
/plugin uninstall amg-toolkit@ops-skills
```

Add `--keep-data` to preserve saved configuration and reports.

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
