# Cortex Public Status Dashboard

Real-time status dashboard for the Cortex AI operational system.

## What's Here

This repository contains a **static dashboard** that displays:

- ✅ **System Health** — OpenClaw status, agent count, cron job count
- 📊 **Work Queue** — Pending, in-progress, completed, and failed tasks
- 💰 **Cost Metrics** — Daily, weekly, and monthly AI model usage costs
- 📈 **Recent Activity** — Last 24 hours of cron executions

## Architecture

```
Cortex (Local)
    ↓
cortex-dashboard (SQLite)
    ↓
generate-public-dashboard.js
    ↓
Static Files (this repo)
    ↓
Cloudflare Pages
    ↓
Public URL
```

## Deployment

This dashboard is automatically updated every 15 minutes via cron job `ops:dashboard-publish`.

**Platform:** Cloudflare Pages  
**URL:** [To be configured]

## Files

- `index.html` — Single-page dashboard with inline CSS/JS
- `data.json` — Status data (JSON API)
- `_headers` — Security headers (CSP, X-Frame-Options, etc.)
- `robots.txt` — SEO configuration

## Privacy

This dashboard displays **aggregate statistics only**. No sensitive data is exposed:

- ❌ Memory file contents
- ❌ Specific task details
- ❌ Private logs
- ❌ Credentials or secrets

## Source

Generated from: `~/.openclaw/workspace/cortex-dashboard/scripts/generate-public-dashboard.js`

## Last Updated

Auto-generated: See git commit history
