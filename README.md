# kite-mcp-templates

[![Go Reference](https://pkg.go.dev/badge/github.com/algo2go/kite-mcp-templates.svg)](https://pkg.go.dev/github.com/algo2go/kite-mcp-templates)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Embedded HTML templates + static assets for the algo2go ecosystem.
Surfaces 60+ HTML templates (dashboard, landing, admin pages, MCP App
widgets, OAuth callback) plus CSS + appbridge.js + static/* via Go's
`embed` directive.

Used by [`Sundeepg98/kite-mcp-server`](https://github.com/Sundeepg98/kite-mcp-server)
for serving the dashboard, OAuth flow, and MCP App widget shells.

## Why a separate module?

HTML/CSS/JS templates are orthogonal UI primitives — usable by any
algo2go project (broker dashboards, monitoring, future broker
adapters) independent of `kite-mcp-server`. Hosting as a module:

- Centralizes template source-of-truth across consumers
- Lets template content version independently of server logic
- Reduces dep-graph weight for users who only need template
  rendering bytes

## Stability promise

**v0.x — unstable.** Embedded files may be added/removed/renamed
between minor versions; `var FS embed.FS` signature is stable but
file paths inside the FS aren't. Pin `v0.1.0` deliberately.
v1.0 ships only after the embedded-file inventory is reviewed for
stability.

## Install

```bash
go get github.com/algo2go/kite-mcp-templates@v0.1.0
```

## Public API (templates.go, 11 LOC)

```go
package templates

import "embed"

//go:embed [60+ HTML files] static/*
var FS embed.FS
```

That is the entire API. Use `templates.FS.ReadFile("dashboard.html")`,
`fs.Sub(templates.FS, "static")`, or pass the FS to
`html/template.ParseFS`.

## Embedded inventory

- **Dashboard pages**: dashboard.html, activity.html, orders.html,
  alerts.html, paper.html, scanner.html, payoff.html, safety.html, ...
- **Admin pages**: admin_login.html, admin_users.html,
  admin_metrics.html, admin_sessions.html, admin_alerts.html, ...
- **MCP App widgets**: portfolio_app.html, activity_app.html,
  orders_app.html, alerts_app.html, options_chain_app.html,
  chart_app.html, hub_app.html, paper_app.html, ...
- **OAuth flow**: login_choice.html, login_success.html,
  browser_login.html, email_prompt.html, credentials_app.html
- **MFA flow**: admin_mfa_enroll.html, admin_mfa_verify.html
- **Static assets**: appbridge.js, dashboard-base.css, static/*

## Reference consumer

[`Sundeepg98/kite-mcp-server`](https://github.com/Sundeepg98/kite-mcp-server)
— consumed by:
- `app/http.go` — landing + dashboard rendering
- `oauth/handlers.go` — OAuth callback + login success pages
- `kc/ops/handler.go` — admin/user dashboard rendering
- `mcp/ext_apps.go` — MCP App widget shells (with CSS injection)

## License

MIT — see [LICENSE](LICENSE).

## Authors

Original design: [Sundeepg98](https://github.com/Sundeepg98) (Zerodha
Tech). Multi-module promotion (2026-05-10): algo2go contributors.
