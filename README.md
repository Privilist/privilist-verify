# Privilist Auth Verify Page

A lightweight verification page for Supabase authentication sessions that intelligently routes users to either the web app or mobile app based on the `target` parameter.

## Features

- **Smart Routing**: Routes to web or app based on `?target=app` or `?target=web` parameter
- **Universal Links**: Uses universal links for seamless app opening (iOS AASA + Android assetlinks)
- **Fallback Handling**: Automatically falls back to web if app doesn't open within 1.2s
- **Hash Preservation**: Preserves Supabase auth hash (`#access_token=...`) during redirect
- **Interactive Detection**: Shows manual button if page isn't visibly interactive

## Usage

### Web Redirect
```
https://verify.privilist.com/?target=web#access_token=...
→ Redirects to: https://booking.privilist.com/router#access_token=...
```

### App Redirect
```
https://verify.privilist.com/?target=app#access_token=...
→ Tries: https://app.privilist.com/router#access_token=...
→ Fallback: https://booking.privilist.com/router#access_token=...
```

## Deployment

Deployed on Vercel with automatic deployments from the main branch.

### Deploy Command
```bash
vercel --prod
```

## Configuration

### Supabase Email Templates
Update your Supabase email templates to use this verify page:

```html
<a href="https://verify.privilist.com/?target=web{{ .TokenHash }}">Confirm Email</a>
<a href="https://verify.privilist.com/?target=app{{ .TokenHash }}">Open in App</a>
```

### Universal Links Setup

#### iOS (Apple App Site Association)
Host at `https://app.privilist.com/.well-known/apple-app-site-association`

#### Android (Digital Asset Links)
Host at `https://app.privilist.com/.well-known/assetlinks.json`

## Development

This is a static HTML page with no build process required.

### Local Testing
```bash
python3 -m http.server 8000
# Visit: http://localhost:8000/?target=web#access_token=test
```

## License

Proprietary - Privilist
