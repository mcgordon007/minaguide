# Deployment Guide

This guide covers how to deploy the Mina the Hollower Guide to Cloudflare Pages.

## Prerequisites

- A GitHub account
- A Cloudflare account (free tier works fine)
- This repository pushed to your GitHub account

## Step 1: Push to GitHub

1. Create a new repository on GitHub named `minaguide` (or your preferred name)
2. Push this code to your repository:

```bash
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/minaguide.git
git push -u origin main
```

## Step 2: Connect to Cloudflare Pages

1. Log in to your [Cloudflare Dashboard](https://dash.cloudflare.com)
2. Navigate to **Pages** in the left sidebar
3. Click **Create a project**
4. Select **Connect to Git**
5. Authorize Cloudflare to access your GitHub account
6. Select the `minaguide` repository
7. Click **Begin setup**

## Step 3: Configure Build Settings

Since this is a static HTML site, no build step is required:

| Setting | Value |
|---------|-------|
| Project name | `minaguide` (or your choice) |
| Production branch | `main` |
| Framework preset | `None` |
| Build command | *(leave empty)* |
| Build output directory | `/` (root) |

Click **Save and Deploy**

## Step 4: Wait for Deployment

Cloudflare Pages will:
1. Clone your repository
2. Deploy the static files
3. Provide you with a URL like `https://minaguide.pages.dev`

## Step 5: Update URLs (Important!)

After your first deployment, you need to update the URLs in your files:

### Option A: Use Relative URLs (Recommended)

The files are already configured with relative URLs (`/`) which work automatically on any domain. No changes needed.

### Option B: Use Absolute URLs (For Better SEO)

If you want absolute URLs for better SEO and social sharing:

1. **Update all HTML files** - Replace relative URLs in meta tags:
   ```html
   <!-- Change from: -->
   <meta property="og:url" content="/index.html">
   <!-- To: -->
   <meta property="og:url" content="https://minaguide.pages.dev/index.html">
   ```

2. **Update sitemap.xml** - Replace relative paths with absolute URLs:
   ```xml
   <!-- Change from: -->
   <loc>/index.html</loc>
   <!-- To: -->
   <loc>https://minaguide.pages.dev/index.html</loc>
   ```

3. **Update robots.txt** - Update the sitemap reference:
   ```
   Sitemap: https://minaguide.pages.dev/sitemap.xml
   ```

4. Commit and push changes - Cloudflare will automatically redeploy

## Step 6: Custom Domain (Optional)

To use your own domain:

1. In Cloudflare Pages, go to your project
2. Click **Custom domains**
3. Click **Set up a custom domain**
4. Enter your domain (e.g., `minaguide.com`)
5. Follow the DNS instructions provided

### DNS Configuration

If using Cloudflare as your DNS provider:
1. Add a CNAME record pointing to your Pages domain
2. Enable the orange cloud (proxied) for CDN benefits

Example DNS record:
```
Type: CNAME
Name: @ (or www)
Target: minaguide.pages.dev
Proxy status: Proxied
```

## Step 7: Verify Deployment

Check the following after deployment:

- [ ] All pages load correctly
- [ ] Navigation links work
- [ ] Images display properly
- [ ] CSS styles apply
- [ ] Mobile responsiveness works
- [ ] Favicon appears
- [ ] Social media previews work (use [Facebook Debugger](https://developers.facebook.com/tools/debug/) and [Twitter Card Validator](https://cards-dev.twitter.com/validator))

## Environment Variables

This static site does not require any environment variables.

## Preview Deployments

Cloudflare Pages automatically creates preview deployments for:
- Pull requests
- Non-production branches

Access preview URLs in your Cloudflare Pages dashboard.

## Rollbacks

To rollback to a previous version:

1. Go to your Cloudflare Pages project
2. Click **Deployments**
3. Find the version you want to restore
4. Click the three dots menu
5. Select **Rollback to this deployment**

## Performance Optimization

The site is already optimized with:
- Cloudflare CDN (automatic)
- Caching headers (configured in `_headers`)
- Minified assets (if applicable)
- Image optimization (use WebP format where possible)

## Security Headers

Security headers are configured in `_headers`:
- X-Frame-Options: DENY
- X-Content-Type-Options: nosniff
- Referrer-Policy: strict-origin-when-cross-origin
- Content-Security-Policy: Restrictive default policy
- Permissions-Policy: Restricted feature access

## Troubleshooting

### 404 Errors on Page Refresh

If you get 404 errors when refreshing pages:
- The `_redirects` file should handle this automatically
- Ensure the file is in the root directory
- Check Cloudflare Pages logs for redirect errors

### Images Not Loading

- Verify image paths are relative (`images/filename.jpg`)
- Check that images are committed to the repository
- Ensure file extensions match exactly (case-sensitive)

### CSS/JS Not Loading

- Check browser console for errors
- Verify file paths in HTML
- Ensure files are in the repository

### SEO Issues

- Test with [Google Rich Results Test](https://search.google.com/test/rich-results)
- Verify sitemap.xml is accessible at `/sitemap.xml`
- Submit sitemap to Google Search Console

## Monitoring

Cloudflare Pages provides:
- Deployment logs
- Analytics (requests, bandwidth, visits)
- Real-time logs (on paid plans)

Access these in your Cloudflare Dashboard under your Pages project.

## Additional Resources

- [Cloudflare Pages Documentation](https://developers.cloudflare.com/pages/)
- [Cloudflare Pages Headers](https://developers.cloudflare.com/pages/configuration/headers/)
- [Cloudflare Pages Redirects](https://developers.cloudflare.com/pages/configuration/redirects/)

## Support

For deployment issues:
1. Check [Cloudflare Status](https://www.cloudflarestatus.com/)
2. Review [Cloudflare Community](https://community.cloudflare.com/)
3. Open an issue on this GitHub repository
