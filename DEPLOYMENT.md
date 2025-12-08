# Deployment Guide for Vercel

## Quick Deploy

1. **Push to GitHub** ✅ (Already done!)
   - Repository: https://github.com/Shaurya-Sinha3301/Taxaformer-Final

2. **Deploy to Vercel**
   
   ### Option A: One-Click Deploy
   Click this button to deploy:
   
   [![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https://github.com/Shaurya-Sinha3301/Taxaformer-Final)

   ### Option B: Manual Deploy
   
   1. Go to [vercel.com](https://vercel.com)
   2. Sign in with your GitHub account
   3. Click "Add New Project"
   4. Import your repository: `Shaurya-Sinha3301/Taxaformer-Final`
   5. Configure project:
      - **Framework Preset:** Next.js
      - **Root Directory:** `taxaformer` (if needed)
      - **Build Command:** `npm run build`
      - **Install Command:** `npm install --legacy-peer-deps`
   6. Click "Deploy"

## Important Configuration

### Build Settings

The project includes a `vercel.json` file with optimal settings:

```json
{
  "buildCommand": "npm run build",
  "devCommand": "npm run dev",
  "installCommand": "npm install --legacy-peer-deps",
  "framework": "nextjs",
  "regions": ["iad1"]
}
```

### Why `--legacy-peer-deps`?

The project uses React 19, but some dependencies (like `react-day-picker`) haven't updated their peer dependencies yet. The `--legacy-peer-deps` flag allows installation to proceed.

## Post-Deployment

After deployment, Vercel will provide you with:
- **Production URL:** `https://your-project.vercel.app`
- **Preview URLs:** For each branch/PR
- **Analytics:** Built-in performance monitoring

## Custom Domain (Optional)

1. Go to your project settings in Vercel
2. Navigate to "Domains"
3. Add your custom domain
4. Follow DNS configuration instructions

## Environment Variables

Currently, no environment variables are required. If you add backend services later:

1. Go to Project Settings → Environment Variables
2. Add your variables
3. Redeploy for changes to take effect

## Troubleshooting

### Build Fails

If the build fails, check:
1. The install command includes `--legacy-peer-deps`
2. Node.js version is 18.x or higher (set in Project Settings)
3. All dependencies are in `package.json`

### Runtime Errors

- Check browser console for client-side errors
- Check Vercel logs for server-side errors
- Ensure dynamic imports are used for browser-only libraries

## Performance Optimization

The app is already optimized with:
- ✅ Dynamic imports for heavy libraries (Three.js, GSAP, Leaflet)
- ✅ Next.js automatic code splitting
- ✅ Tailwind CSS purging
- ✅ Image optimization via Next.js Image component

## Monitoring

Vercel provides:
- Real-time logs
- Performance analytics
- Error tracking
- Deployment history

Access these in your Vercel dashboard.

## Support

For issues:
- Check [Vercel Documentation](https://vercel.com/docs)
- Review [Next.js Deployment Guide](https://nextjs.org/docs/deployment)
- Open an issue on GitHub

---

**Your app is now ready for production! 🚀**
