# GitHub Pages Deployment Instructions

## Step 1: Create GitHub Repository

1. Go to https://github.com/new
2. Repository name: `conform-studio-auth` (or any name you prefer)
3. Description: "OAuth callback handler for Conform Studio"
4. **Public** repository (required for free GitHub Pages)
5. **Don't** initialize with README (we already have files)
6. Click "Create repository"

## Step 2: Push Code to GitHub

Run these commands in terminal:

```bash
cd /Users/haste/Desktop/haste-conform-auth-bug/auth-callback-site

# Initialize git
git init

# Add all files
git add .

# Commit
git commit -m "Initial OAuth callback site"

# Add your GitHub repository as origin
# Replace YOUR_USERNAME with your GitHub username
git remote add origin https://github.com/YOUR_USERNAME/conform-studio-auth.git

# Push to GitHub
git branch -M main
git push -u origin main
```

## Step 3: Enable GitHub Pages

1. Go to your repository on GitHub
2. Click **Settings** (in the repository, not your profile)
3. Scroll down to **Pages** in the left sidebar
4. Under "Source", select:
   - Source: **Deploy from a branch**
   - Branch: **main**
   - Folder: **/ (root)**
5. Click **Save**

## Step 4: Configure Custom Domain (for auth.conform.studio)

### In GitHub:
1. Still in Settings → Pages
2. Under "Custom domain", enter: `auth.conform.studio`
3. Click **Save**
4. Check "Enforce HTTPS" (may take a few minutes to be available)

### In GoDaddy:
1. Log in to GoDaddy
2. Go to **DNS Management** for conform.studio
3. Add a new record:
   - Type: **CNAME**
   - Name: **auth**
   - Value: **YOUR_USERNAME.github.io** (replace with your GitHub username)
   - TTL: **1 hour**
4. Click **Save**

## Step 5: Wait for DNS Propagation

- GitHub Pages deployment: ~5-10 minutes
- DNS propagation: ~5 minutes to 1 hour
- HTTPS certificate: ~15 minutes after DNS works

## Step 6: Test Your Setup

Once deployed, test these URLs:
1. https://auth.conform.studio (should show "Invalid Request")
2. https://auth.conform.studio?code=test123 (should show success)
3. https://auth.conform.studio?error=test (should show error)

## Step 7: Update OAuth Providers

Update all your OAuth providers with the new callback URL:

### Supabase:
- URL Configuration → Redirect URLs
- Add: `https://auth.conform.studio`

### Google Cloud Console:
- APIs & Services → Credentials → OAuth 2.0 Client ID
- Add: `https://auth.conform.studio`

### Apple Developer:
- Services → Sign In with Apple → Configure
- Add: `https://auth.conform.studio`

## Step 8: Update Your App

In your Conform Studio app, update the Supabase manager:

```python
# Set use_web_callback=True for production
result = supabase_manager.sign_in_with_oauth(
    provider="google",
    use_web_callback=True
)
```

Or update the redirect URL in `supabase_manager.py`:
```python
if use_web_callback:
    redirect_url = "https://auth.conform.studio"
```

## Troubleshooting

### "404 Not Found" on GitHub Pages
- Wait 5-10 minutes for deployment
- Check if repository is public
- Verify Pages is enabled in Settings

### Custom domain not working
- Check CNAME record in GoDaddy
- Verify CNAME file exists in repository
- Wait for DNS propagation (use `nslookup auth.conform.studio`)

### HTTPS not working
- Wait for GitHub to provision certificate
- Make sure "Enforce HTTPS" is checked
- Clear browser cache

### OAuth provider errors
- Ensure exact URL match (with or without trailing slash)
- Check if HTTPS is specified in OAuth settings
- Verify no typos in the URL

## Alternative: Quick Test Without Custom Domain

If you want to test immediately without setting up the custom domain:

1. Your site will be available at: `https://YOUR_USERNAME.github.io/conform-studio-auth/`
2. Update OAuth providers with this URL temporarily
3. Later switch to custom domain