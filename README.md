# Weather InTouch — MCP Server

Give any AI agent live weather **and official government severe-weather warnings** — the one thing
generic weather integrations don't have.

**Weather InTouch** exposes a public [Model Context Protocol](https://modelcontextprotocol.io)
server so assistants and automation tools (Claude, ChatGPT, Cursor, n8n, Home Assistant, …) can
pull forecasts and **official CAP alerts** aggregated from national authorities in 7 countries —
each with mandatory source attribution.

> 🌐 **Endpoint:** `https://api.weatherintouch.com/api/mcp` (Streamable HTTP)
> 🔑 **No API key required** — read-only, anonymous.
> 🏠 **More:** [weatherintouch.com](https://weatherintouch.com)

## Why this one

Most weather MCP servers are thin wrappers over a free forecast API. This one adds the piece nobody
else ships: **official government warnings (CAP)** from **NWS (US), ČHMÚ (CZ), DWD (DE), MET Norway
(NO), MSC (Canada), IMD (India), Met Éireann (IE)** — point-in-polygon matched, normalised, and
attributed. Ask _"are there any weather warnings for Prague this weekend?"_ and your agent answers
with the authoritative ČHMÚ warning, cited.

## Tools

| Tool                    | Input                  | Returns                                                        |
| ----------------------- | ---------------------- | -------------------------------------------------------------- |
| `resolve_location`      | `query`                | Candidate coordinates (name, lat, lon, country)                |
| `get_forecast`          | `place` or `lat`+`lon` | Full forecast: current + daily + hourly + air quality + alerts |
| `get_current`           | `place` or `lat`+`lon` | Instant conditions only                                        |
| **`get_severe_alerts`** | `place` or `lat`+`lon` | **Official government warnings (CAP) with attribution**        |

Every tool accepts a plain place name (geocoded for you) or explicit `lat`/`lon`.

## Connect

**Claude Code**

```bash
claude mcp add --transport http weather-intouch https://api.weatherintouch.com/api/mcp
```

**Claude Desktop / Cursor / Windsurf** — add a remote MCP server by URL:

```
https://api.weatherintouch.com/api/mcp
```

**n8n / Home Assistant / any MCP client** — point the MCP (Streamable HTTP) node/integration at the
same URL. No credentials.

**Try it in the Inspector**

```bash
npx @modelcontextprotocol/inspector
# Transport: Streamable HTTP · URL: https://api.weatherintouch.com/api/mcp · Connect → List Tools
```

## Example

> **You:** Should I expect any weather warnings driving from Brno to Prague tomorrow?
>
> **Assistant** _(calls `get_severe_alerts` for both cities)_ **:** ČHMÚ has a **High Winds**
> warning (severity: severe) active for the Praha region tomorrow afternoon… _(source: ČHMÚ)_

## Data sources & attribution

Forecast: [Open-Meteo](https://open-meteo.com). Warnings: the respective national meteorological
authorities (CC BY 4.0 where applicable), surfaced with attribution on every alert. Geocoding:
[OpenStreetMap Nominatim](https://nominatim.org).

## About

Built by [Weather InTouch](https://weatherintouch.com) — active weather monitoring, alerts, and
forecast-driven automation. Questions or a higher-volume / commercial integration?
See [weatherintouch.com](https://weatherintouch.com).
