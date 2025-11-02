# Deployment Guide

## Step 1: Create GitHub Repository

1. Go to https://github.com/new
2. Repository name: `privilist-verify`
3. Description: "Supabase auth verification page for Privilist"
4. Visibility: Private
5. **Do NOT** initialize with README, .gitignore, or license (we already have these)
6. Click "Create repository"

## Step 2: Push to GitHub

```bash
cd /Users/tomgoedhart/Elite/privilist-verify
git remote add origin git@github.com:YOUR_USERNAME/privilist-verify.git
git push -u origin main
```

Replace `YOUR_USERNAME` with your GitHub username.

## Step 3: Deploy to Vercel

### Option A: Vercel Dashboard (Recommended)

1. Go to https://vercel.com/new
2. Click "Import Git Repository"
3. Select your `privilist-verify` repository
4. Configure project:
   - **Framework Preset**: Other
   - **Root Directory**: `./`
   - **Build Command**: (leave empty - static site)
   - **Output Directory**: `./`
5. Click "Deploy"

### Option B: Vercel CLI

```bash
# Install Vercel CLI if you haven't
npm i -g vercel

# Login to Vercel
vercel login

# Deploy
cd /Users/tomgoedhart/Elite/privilist-verify
vercel --prod
```

## Step 4: Configure Custom Domain

1. In Vercel dashboard, go to your project settings
2. Navigate to "Domains"
3. Add domain: `verify.privilist.com`
4. Follow Vercel's DNS configuration instructions
5. Add DNS records in your domain provider:
   - Type: `CNAME`
   - Name: `verify`
   - Value: `cname.vercel-dns.com`

## Step 5: Update Supabase Email Templates

Once deployed, update your Supabase email templates to use the verify page:

### Email Confirmation Template
```html
<a href="https://verify.privilist.com/?target=web{{ .TokenHash }}">
  Confirm your email
</a>
```

### Password Reset Template
```html
<a href="https://verify.privilist.com/?target=web{{ .TokenHash }}">
  Reset your password
</a>
```

### Magic Link Template
```html
<a href="https://verify.privilist.com/?target=web{{ .TokenHash }}">
  Sign in to Privilist
</a>
```

### For Mobile App
```html
<a href="https://verify.privilist.com/?target=app{{ .TokenHash }}">
  Open in app
</a>
```

## Step 6: Configure Supabase Redirect URLs

In your Supabase project settings:

1. Go to Authentication → URL Configuration
2. Add to "Redirect URLs":
   - `https://verify.privilist.com`
   - `https://booking.privilist.com/router`
   - `https://app.privilist.com/router`

## Step 7: Test the Flow

### Test Web Flow
```
https://verify.privilist.com/?target=web#access_token=test123
```
Should redirect to: `https://booking.privilist.com/router#access_token=test123`

### Test App Flow
```
https://verify.privilist.com/?target=app#access_token=test123
```
Should try: `https://app.privilist.com/router#access_token=test123`
Then fallback to web if app doesn't open.

## Troubleshooting

### Vercel Build Fails
- Ensure `vercel.json` is present
- Check that `index.html` exists in root directory

### Redirects Not Working
- Check browser console for errors
- Verify the hash is being preserved in the URL
- Test with browser dev tools open (Network tab)

### App Not Opening
- Verify universal links are configured (iOS AASA + Android assetlinks)
- Test on actual device (not simulator)
- Check that `app.privilist.com` is properly configured

## Monitoring

- Check Vercel Analytics for traffic and errors
- Monitor Supabase logs for authentication issues
- Set up Vercel deployment notifications in Slack/email
