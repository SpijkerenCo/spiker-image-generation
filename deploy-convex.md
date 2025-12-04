# Quick Convex Deployment

## Run These Commands in Order:

### 1. Login to Convex (One-time)
```bash
convex login
```
- Opens browser
- Create/login to Convex account
- CLI gets authenticated

### 2. Initialize Convex (Development)
```bash
convex dev
```
- Creates new project
- Generates `.env.local` with Convex URL
- Deploys functions to dev environment
- **Keep this running** while developing

### 3. Test Locally
Open new terminal:
```bash
npm run dev
```
- Visit http://localhost:5174
- Test authentication
- Check if Convex connection works

### 4. Deploy to Production
```bash
convex deploy --prod
```
- Deploys to production environment
- Gives you production URL
- Copy this URL for Railway

### 5. Update Railway
In Railway dashboard → Variables:
```
VITE_CONVEX_URL=https://your-prod-deployment.convex.cloud
```

### 6. Redeploy Railway
```bash
git push
```

## That's It! 🎉

Your app now has:
- ✅ Frontend on Railway
- ✅ Backend on Convex
- ✅ Connected via environment variables

## Check Status

```bash
# View Convex dashboard
convex dashboard

# View logs
convex logs

# Check deployment
convex env
```
