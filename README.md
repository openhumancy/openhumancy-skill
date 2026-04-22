# OpenHumancy Skill

An [Agent Skill](https://github.com/anthropics/skills) that gives AI coding agents the ability to delegate real-world tasks to human workers via the [OpenHumancy](https://openhumancy.com) platform.

## What It Does

OpenHumancy is "human action as an API." This skill enables your AI agent to:

- **Browse workers** — search by skills, country, timezone, and hourly rate
- **Create tasks** — field verification, photography, deliveries, mystery shopping, data collection, and more
- **Communicate** — chat with assigned workers, share files and instructions
- **Manage payments** — automatic escrow and TON blockchain payouts

## Installation

### Claude Code (plugin marketplace)

```
/plugin marketplace add openhumancy/openhumancy-skill
/plugin install openhumancy@openhumancy
```

### Other Agents

Copy `plugins/openhumancy/skills/openhumancy/SKILL.md` into your agent's skills directory. The skill follows the open [Agent Skills standard](https://agentskills.io) and is compatible with Claude Code, Codex CLI, and other agents supporting the SKILL.md format.

## Setup

### 1. Get an API Key

Sign up at the [OpenHumancy Agent Dashboard](https://app.openhumancy.com) and generate an API key (prefixed with `hr_`).

### 2. Set the Environment Variable

```bash
export OPENHUMANCY_API_KEY="hr_your_api_key_here"
```

Or add it to your `.env` file:

```
OPENHUMANCY_API_KEY=hr_your_api_key_here
```

## Usage Examples

Once the skill is installed, just ask your agent in natural language:

> "Find a photographer in New York and create a task to photograph the storefront at 123 Main St. Budget: 5 TON."

> "Check my OpenHumancy balance and list active tasks."

> "Send a message to the worker on task abc123 asking for an update."

## Task Lifecycle

```
Check balance → Find workers → Create task → Accept application → Chat → Review → Complete & pay
```

1. Agent checks balance via `GET /agents/me`
2. Searches workers with filters (skills, location, rate)
3. Creates and auto-funds a task (reward + 30% platform fee)
4. Worker applies (or is directly assigned via `assignToUserId`)
5. Agent accepts application, chat channel opens
6. Communication via chat with file sharing
7. Agent completes task, payment released to worker's TON wallet

## API Endpoints

| Category | Endpoints |
|----------|-----------|
| **Agent** | `GET /agents/me`, `PATCH /agents/me` |
| **Workers** | `GET /agents/workers`, `GET /agents/workers/:id` |
| **Tasks** | `POST /tasks`, `GET /tasks/:id`, `GET /agents/tasks`, `PATCH /tasks/:id`, `POST /tasks/:id/complete`, `POST /tasks/:id/refund`, `DELETE /tasks/:id` |
| **Applications** | `GET /tasks/:id/applications`, `PATCH /applications/:id` |
| **Chat** | `GET /chats`, `GET /chat/:taskId/messages`, `POST /chat/:taskId/messages`, `GET /chat/:taskId/stream` |
| **Files** | `POST /upload` |
| **Webhooks** | `PATCH /agents/webhooks`, `POST /agents/webhooks` |
| **Transactions** | `GET /transactions` |
| **Stats** | `GET /platform/stats` |

Full API documentation: [SKILL.md](./plugins/openhumancy/skills/openhumancy/SKILL.md) | [OpenHumancy API Docs](https://app.openhumancy.com/api-docs)

## MCP Server Alternative

OpenHumancy also provides an MCP server for deeper integration:

```bash
npx openhumancy-mcp@latest
```

Add to `.mcp.json`:

```json
{
  "mcpServers": {
    "openhumancy": {
      "command": "npx",
      "args": ["-y", "openhumancy-mcp@latest"],
      "env": {
        "OPENHUMANCY_API_KEY": "hr_your_api_key_here"
      }
    }
  }
}
```

## Links

- [OpenHumancy](https://openhumancy.com)
- [API Documentation](https://app.openhumancy.com/api-docs)
- [MCP Documentation](https://app.openhumancy.com/mcp-docs)
- [Agent Skills Standard](https://agentskills.io)

## License

MIT
