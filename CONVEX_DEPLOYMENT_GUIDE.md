# Convex Backend Deployment Guide

## What is Convex?

Convex is your **backend-as-a-service** that handles:
- User authentication
- Real-time database
- Server-side functions
- WebSocket connections

Your React app (on Railway) will connect to Convex via API calls.

## Architecture Overview

```
┌─────────────────────────────────────────────────────────┐
│  RAILWAY (Frontend)                                     │
│  https://your-app.railway.app                           │
│                                                         │
│  React App → Makes API calls to Convex                 │
└─────────────────────────────────────────────────────────┘
                        │
                        │ HTTPS/WebSocket
                        ▼
┌─────────────────────────────────────────────────────────┐
│  CONVEX (Backend)                                       │
│  https://your-deployment.convex.cloud                   │
│                                                         │
│  - Authentication (email OTP)                           │
│  - User management                                      │
│  - Real-time database                                   │
│  - Server functions from src/convex/                    │
└─────────────────────────────────────────────────────────┘
```

## Prerequisites

✅ Convex CLI installed (already done)
✅ Convex account (you'll create this)
✅ Your code in `src/convex/` folder

## Step-by-Step Deployment

### Step 1: Login to Convex

```bash
# This will open a browser for authentication
convex login
```

**What happens:**
- Opens browser to https://dashboard.convex.dev
- You'll create/login to your Convex account
- CLI gets authenticated

### Step 2: Initialize Convex Project

```bash
# Run from your project root
convex dev
```

**What this does:**
- Creates a new Convex project (or links to existing)
- Generates `.env.local` with `CONVEX_DEPLOYMENT` and `VITE_CONVEX_URL`
- Starts watching your `src/convex/` folder
- Deploys functions to development environment

**You'll see:**
```
? What would you like to configure? create a new project
? Project name: spijker-image
✔ Created project spijker-image-1234

Convex functions ready! (1.2s)
└─ http (http.ts)
└─ users (users.ts)

Dashboard: https://dashboard.convex.dev/t/your-team/your-project
```

### Step 3: Check Generated .env.local

After `convex dev`, your `.env.local` should have:

```env
# Convex (auto-generated)
CONVEX_DEPLOYMENT=dev:your-deployment-name-1234
VITE_CONVEX_URL=https://your-deployment-name-1234.convex.cloud

# Add your other variables
VITE_SUPABASE_URL=https://your-project.supabase.co
VITE_SUPABASE_ANON_KEY=your-supabase-anon-key
VITE_GOOGLE_DRIVE_API_KEY=your-google-drive-api-key
```

### Step 4: Test Locally

```bash
# Terminal 1: Run Convex dev (watches for changes)
convex dev

# Terminal 2: Run your React app
npm run dev
```

**Test:**
- Open http://localhost:5174
- Try authentication
- Check Convex dashboard for logs

### Step 5: Deploy to Production

```bash
# Deploy Convex functions to production
convex deploy --prod
```

**What happens:**
- Uploads all functions from `src/convex/`
- Creates production deployment
- Gives you production URL

**Output:**
```
✔ Deployed functions to production
Production URL: https://your-prod-deployment.convex.cloud

Add this to your Railway environment variables:
VITE_CONVEX_URL=https://your-prod-deployment.convex.cloud
```

### Step 6: Update Railway Environment Variables

In Railway dashboard, add:

```env
VITE_CONVEX_URL=https://your-prod-deployment.convex.cloud
```

Then redeploy Railway app.

## Convex Dashboard

Access at: https://dashboard.convex.dev

**Features:**
- View all functions
- Monitor logs in real-time
- See database tables
- Check authentication status
- View API usage

## Your Convex Functions

Located in `src/convex/`:

```
src/convex/
├── auth.config.ts       # Auth configuration
├── auth.ts              # Auth handlers
├── http.ts              # HTTP endpoints
├── schema.ts            # Database schema
├── users.ts             # User management
└── auth/
    └── emailOtp.ts      # Email OTP provider
```

## Common Commands

```bash
# Start development (watches for changes)
convex dev

# Deploy to production
convex deploy --prod

# View logs
convex logs

# Open dashboard
convex dashboard

# Check deployment status
convex env

# Run a function manually
convex run users:list

# Clear all data (dev only)
convex data clear
```

## Environment Variables Explained

### Development (.env.local)
```env
CONVEX_DEPLOYMENT=dev:your-deployment-name-1234
VITE_CONVEX_URL=https://your-deployment-name-1234.convex.cloud
```

### Production (Railway)
```env
VITE_CONVEX_URL=https://your-prod-deployment.convex.cloud
```

**Note:** `VITE_` prefix is required for Vite to expose variables to browser.

## Troubleshooting

### Issue: "No Convex deployment found"
```bash
# Solution: Initialize Convex
convex dev
```

### Issue: "Functions not updating"
```bash
# Solution: Redeploy
convex deploy --prod
```

### Issue: "Authentication not working"
- Check `VITE_CONVEX_URL` is set correctly
- Verify email OTP configuration in `auth.config.ts`
- Check Convex dashboard logs

### Issue: "CORS errors"
- Convex automatically handles CORS
- Check if URL is correct in `.env.local`

## Deployment Workflow

### For Development:
```bash
# 1. Start Convex dev server
convex dev

# 2. Start React dev server
npm run dev

# 3. Make changes to src/convex/
# Convex auto-deploys changes
```

### For Production:
```bash
# 1. Deploy Convex backend
convex deploy --prod

# 2. Copy production URL
# Output: https://your-prod-deployment.convex.cloud

# 3. Add to Railway environment variables
# VITE_CONVEX_URL=https://your-prod-deployment.convex.cloud

# 4. Deploy frontend to Railway
git push
```

## Cost

Convex has a **generous free tier**:
- 1 million function calls/month
- 1 GB storage
- 1 GB bandwidth
- Perfect for development and small apps

## Next Steps

1. **Run `convex dev`** to initialize
2. **Test locally** with your React app
3. **Deploy to production** with `convex deploy --prod`
4. **Add URL to Railway** environment variables
5. **Redeploy Railway** app

## Quick Start Commands

```bash
# Complete deployment in 3 commands:

# 1. Login (one-time)
convex login

# 2. Initialize and test locally
convex dev

# 3. Deploy to production
convex deploy --prod
```

That's it! Your backend is deployed and ready to use.
