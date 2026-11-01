# @gicm/dashboard

Formatters and view-model builders for AI agent dashboards: metric cards, an event feed, a cost chart and a service health grid, as plain data your UI can draw.

The package turns raw numbers and events into display-ready structures (formatted values, trend colours, summaries) so a dashboard stays thin. It is for developers building an agent monitoring page who want the formatting and aggregation logic tested and separate from the view. Despite the "React component library" wording in `package.json`, the source contains no JSX and does not import React: each `render*` function takes props and returns an object, and React is declared only as a peer dependency.

Part of [gICM](https://github.com/0motionguy/gICM), a family of OpenClaw agent packages.

## What it does

- **Formatters:** `formatPrice` (micro-prices up to billions), `formatLargeNumber` (K, M, B), `formatDuration`, `formatPercent`, `formatTokens`, `formatBytes` and `getRelativeTime`. NaN, Infinity, zero and negatives are handled.
- **Colour helpers:** `getStatusColor`, `getTrendColor` and `getActivityColor`.
- **Status cards:** `renderStatusCard` formats one metric with its prefix, suffix and trend; `renderMetricsPanel` builds a grid of them with up and down counts.
- **Event feed:** `renderEventFeed` sorts activity events newest first, caps them at `maxItems` (50 by default), adds relative times and colours per type (info, success, warning, error, agent, cost, security, deploy) and reports `hasMore`.
- **Cost chart:** `renderCostChart` returns per-point cost, budget and savings with an over-budget flag, plus total cost, total budget, total savings and average daily cost.
- **Health grid:** `renderHealthGrid` formats service status, latency, uptime and last check, with counts per status.
- **Config and types:** a zod schema for theme, poll interval, activity limit, locale and currency, plus the data types above.

## Example

```ts
import { renderMetricsPanel } from "@gicm/dashboard";

const panel = renderMetricsPanel({
  metrics: [{ id: "spend", label: "Spend today", value: 12.4, prefix: "$", trend: "up", trendValue: 8.2 }],
});
// panel.cards[0].formattedValue, panel.cards[0].trend, panel.summary
```

## How it's built

TypeScript (ESM) bundled with tsup and tested with vitest; zod defines the config schema. Everything is a pure function over plain objects, which is why it runs under any UI framework or on the server.

## Repository layout

- `src/formatters.ts`: number, price, duration, byte and colour helpers
- `src/components/`: `StatusCard`, `MetricsPanel`, `EventFeed`, `CostChart`, `HealthGrid` (data builders)
- `src/types.ts`: data types and the config schema
- `src/__tests__/`: vitest suite (30 test cases)
- `skill/SKILL.md`: the OpenClaw skill definition

## Run it locally

```bash
pnpm install
pnpm build   # tsup -> dist/
pnpm test    # vitest (add --run for a single pass)
```

## Status

Beta. Version 1.0.0, a single extracted release (February 2026) with a test suite. It provides data builders only; there are no rendered components or styles yet.
