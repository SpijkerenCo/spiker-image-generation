# Simple Deployment Steps

## One-Time Setup

### 1️⃣ Deploy Convex Backend
```bash
convex login
convex deploy --prod
```
**Output:** `https://happy-animal-123.convex.cloud`
**Copy this URL!**

### 2️⃣ Configure Railway
Go to Railway dashboard → Variables → Add:
```
VITE_CONVEX_URL=https://happy-animal-123.convex.cloud
VITE_SUPABASE_URL=your-supabase-url
VITE_SUPABASE_ANON_KEY=your-supabase-key
VITE_GOOGLE_DRIVE_API_KEY=your-drive-key
NODE_ENV=production
```

### 3️⃣ Deploy to Railway
```bash
git add .
git commit -m "Deploy to production"
git push origin main
```

**Done!** Your app is live.

---

## Regular Updates

### Frontend Changes (UI, components, pages):
```bash
git push
```
Railway auto-deploys. That's it!

### Backend Changes (src/convex/ files):
```bash
convex deploy --prod
```
No need to touch Railway.

### Both Frontend + Backend:
```bash
# 1. Deploy backend first
convex deploy --prod

# 2. Then deploy frontend
git push
```

---

## How They Connect

```
User visits:
https://your-app.railway.app
         ↓
Railway serves React app
         ↓
React app reads: VITE_CONVEX_URL
         ↓
Makes API calls to:
https://happy-animal-123.convex.cloud
         ↓
Convex handles auth/database
         ↓
Returns data to React app
```

---

## Quick Commands

```bash
# Check Convex deployment
convex env

# View Convex logs
convex logs

# Open Convex dashboard
convex dashboard

# Redeploy Convex
convex deploy --prod

# Redeploy Railway
git push
```

---

## Important Notes

✅ **Railway does NOT deploy Convex functions**
- You must run `convex deploy --prod` separately

✅ **Convex URL must be in Railway variables**
- Variable name: `VITE_CONVEX_URL`
- Must start with `VITE_` for Vite to expose it

✅ **Two separate services, one app**
- Railway = Frontend hosting
- Convex = Backend hosting
- Connected via environment variable

✅ **Different URLs for dev and prod**
- Dev: `https://dev-deployment-123.convex.cloud`
- Prod: `https://prod-deployment-456.convex.cloud`
- Use prod URL in Railway!

---

## Troubleshooting

**App can't connect to Convex?**
1. Check Railway variables
2. Verify `VITE_CONVEX_URL` is correct
3. Make sure it's the PROD URL, not dev

**Functions not working?**
1. Deploy Convex: `convex deploy --prod`
2. Check Convex logs: `convex logs`

**Changes not showing?**
- Frontend: `git push`
- Backend: `convex deploy --prod`
