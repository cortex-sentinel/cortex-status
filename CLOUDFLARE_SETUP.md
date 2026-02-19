# Cloudflare Pages Setup Guide

This guide walks through connecting the `cortex-status` GitHub repository to Cloudflare Pages for automatic deployment.

## Prerequisites

- ✅ GitHub repo: `cortex-sentinel/cortex-status` (already exists)
- ✅ Automated publishing: Every 15 minutes via cron (already running)
- ⏳ Cloudflare account (free tier)
- ⏳ Cloudflare Pages project

## Step 1: Create Cloudflare Account

1. **Sign up:** https://dash.cloudflare.com/sign-up
2. **Choose free plan** (unlimited requests, perfect for status dashboard)
3. **Verify email**

## Step 2: Connect GitHub

1. **Go to Pages:** https://dash.cloudflare.com → "Pages"
2. **Click "Create a project"**
3. **Connect GitHub:**
   - Click "Connect to Git"
   - Authorize Cloudflare Pages app
   - Select `cortex-sentinel/cortex-status` repository

## Step 3: Configure Build Settings

**Important:** We push pre-built files, so no build process is needed!

```
Framework preset:        None
Branch:                  main
Build command:           (leave empty)
Build output directory:  /  (root directory)
Root directory:          /  (default)
```

**Why no build command?**
- Files are generated locally by `generate-public-dashboard.js`
- Cron pushes pre-built HTML/JSON to GitHub
- Cloudflare just serves the static files

## Step 4: Deploy

1. **Click "Save and Deploy"**
2. **Wait ~30 seconds** for initial deployment
3. **Note your URL:** `cortex-status.pages.dev` (or similar)

## Step 5: Configure Custom Domain (Optional)

If you want a custom domain like `status.cortex.dev`:

1. **In Cloudflare Pages:**
   - Go to your project → "Custom domains"
   - Click "Set up a custom domain"
   - Enter your subdomain: `status.cortex.dev`

2. **Add DNS Record:**
   - Go to Cloudflare DNS settings
   - Type: CNAME
   - Name: `status.cortex` (or just `status`)
   - Target: `cortex-status.pages.dev`
   - Proxy: ✅ Enabled (orange cloud)
   - TTL: Auto

3. **Wait for SSL:**
   - Cloudflare auto-provisions SSL certificate
   - Usually takes 5-10 minutes
   - Check status in Pages → Custom domains

## Step 6: Update README

Once deployed, update the README.md with your public URL:

```bash
cd ~/.openclaw/workspace-improver/cortex-status-deploy
```

Edit `README.md`:
```markdown
**Platform:** Cloudflare Pages  
**URL:** https://cortex-status.pages.dev  (or your custom domain)
```

Commit:
```bash
git add README.md
git commit -m "Add Cloudflare Pages URL"
git push origin main
```

## Verification

1. **Check deployment:**
   - Visit your Pages URL
   - Should see Cortex status dashboard
   - Check "Last Updated" timestamp

2. **Test auto-updates:**
   - Wait 15 minutes (next cron run)
   - Refresh dashboard
   - Should see updated timestamp

3. **Check GitHub:**
   - Go to https://github.com/cortex-sentinel/cortex-status/commits/main
   - Should see "Update dashboard YYYY-MM-DD-HHMM" commits every 15 min

## Troubleshooting

### Dashboard shows old data
- Check: Is cron running? `openclaw cron list`
- Check: GitHub commits happening? (look for recent pushes)
- Check: Cloudflare build succeeded? (Pages → Deployments)

### Cloudflare build fails
- **Solution:** Set build command to empty (we push pre-built files)
- Set output directory to `/` (root)
- No framework preset needed

### Custom domain not working
- DNS propagation takes time (up to 24h, usually 5-10 min)
- Check CNAME record is correct
- Check Cloudflare proxy is enabled (orange cloud)

### 404 errors
- Check build output directory is `/` (root), not `/public` or similar
- Check files exist in repo: index.html, data.json, etc.

## Security & Privacy

**What's public:**
- System health status (healthy/degraded/down)
- Queue statistics (counts only, no task details)
- Cost metrics (aggregate daily/weekly/monthly)
- Recent cron execution summary (job names, durations, status)

**What's NOT public:**
- Memory file contents
- Specific task details or descriptions
- Private logs, environment variables
- Credentials or secrets

Dashboard is designed for **public consumption** — safe to share with anyone.

## Cost

**Cloudflare Pages Free Tier:**
- ✅ Unlimited requests
- ✅ Unlimited bandwidth
- ✅ 500 builds/month (we use ~2,880 pushes/month at 15-min intervals — but no builds!)
- ✅ Custom domain + SSL included
- ✅ **Total cost: $0/month**

Since we push pre-built files, we don't consume build minutes. Perfect for this use case.

## Alternative: GitHub Pages

If you prefer GitHub Pages (simpler, no Cloudflare account needed):

1. **Go to repo settings:** https://github.com/cortex-sentinel/cortex-status/settings/pages
2. **Source:** Deploy from branch `main`, root directory `/`
3. **Wait ~1 minute** for deployment
4. **URL:** https://cortex-sentinel.github.io/cortex-status

**Pros:** Zero setup, already works  
**Cons:** Slower CDN than Cloudflare, no custom domain (unless you own cortex-sentinel.github.io)

## Next Steps

After setup:

1. ✅ Test the dashboard URL
2. ✅ Verify auto-updates (wait 15 min, check timestamp)
3. ⏳ Share URL with team/stakeholders
4. ⏳ Add to documentation, README files
5. ⏳ Consider adding monitoring alerts (e.g., Pingdom, UptimeRobot)

## See Also

- [DEPLOY.md](../../workspace/cortex-dashboard/DEPLOY.md) — Full deployment guide with all options
- [README.md](./README.md) — Dashboard overview
- Dashboard generator: `~/.openclaw/workspace/cortex-dashboard/scripts/generate-public-dashboard.js`
- Cron task: `~/.openclaw/cron/ops/dashboard-publish.md`
