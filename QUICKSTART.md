# Quick Start

## 🚀 Deploy in 3 Steps

### 1. Push to GitHub
```bash
cd /Users/tomgoedhart/Elite/privilist-verify

# Create repo on GitHub first: https://github.com/new
# Name it: privilist-verify

# Then push:
git remote add origin git@github.com:YOUR_USERNAME/privilist-verify.git
git push -u origin main
```

### 2. Deploy to Vercel
Go to: https://vercel.com/new
- Import your `privilist-verify` repository
- Click "Deploy" (no configuration needed!)

### 3. Configure Domain
In Vercel dashboard:
- Add domain: `verify.privilist.com`
- Update DNS: `CNAME verify → cname.vercel-dns.com`

## ✅ What You Get

- **URL**: `https://verify.privilist.com`
- **Web redirect**: `?target=web` → `booking.privilist.com/router`
- **App redirect**: `?target=app` → `app.privilist.com/router` (with fallback)
- **Hash preserved**: Auth tokens automatically forwarded

## 📝 Update Supabase

In Supabase → Authentication → Email Templates:

```html
<!-- Replace your confirmation links with: -->
<a href="https://verify.privilist.com/?target=web{{ .TokenHash }}">
  Confirm Email
</a>
```

## 🧪 Test It

```bash
# Test web flow
open "https://verify.privilist.com/?target=web#access_token=test123"

# Test app flow
open "https://verify.privilist.com/?target=app#access_token=test123"
```

## 📚 Full Documentation

See `DEPLOYMENT.md` for complete setup instructions.
