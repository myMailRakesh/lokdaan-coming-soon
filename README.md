# Web Password Reset Setup

This folder contains the web-based password reset functionality for the Lokdaan app. This serves as a fallback when users can't open the mobile app directly from the reset email.

## Files

- `reset-password.html` - The main password reset web page
- `config.js` - Configuration file for Supabase and app settings

## Setup Instructions

### 1. Configure Supabase Settings

Edit `config.js` and replace the placeholder values:

```javascript
window.LOKDAAN_CONFIG = {
    SUPABASE_URL: 'https://your-project.supabase.co',
    SUPABASE_ANON_KEY: 'your-anon-key-here',
    APP_SCHEME: 'lokdaan://',
    // ... other settings
};
```

### 2. Update App Configuration

In your React Native app, update `config/webConfig.js`:

```javascript
export const WEB_CONFIG = {
    resetPasswordUrl: 'https://your-domain.com/reset-password.html',
    // ... other settings
};
```

### 3. Host the Files

Upload both `reset-password.html` and `config.js` to your web hosting service:

- **GitHub Pages**: Place files in a `gh-pages` branch
- **Netlify**: Drag and drop the files or connect to your repo
- **Vercel**: Deploy directly from your repository
- **Firebase Hosting**: Use `firebase deploy` with these files in your public folder
- **Any static hosting**: Upload both files to your web server

### 4. Configure Supabase Auth Settings

In your Supabase dashboard:

1. Go to **Authentication** → **URL Configuration**
2. Add your web domain to **Site URL**
3. Add your web domain to **Redirect URLs**

Example redirect URLs:
- `https://your-domain.com/reset-password.html`
- `lokdaan://reset-password-confirm` (for app deep linking)

### 5. Test the Flow

1. Send a password reset email from your app
2. Click the link in the email
3. The page should either:
   - Open your mobile app (if installed)
   - Show the web password reset form (if app not available)

## How It Works

### Email Link Flow

1. User requests password reset in the mobile app
2. Supabase sends email with link to your web page
3. Web page detects if it's a password reset callback
4. If tokens are present in URL, shows "Set New Password" form
5. If no tokens, shows "Request Reset Email" form

### App Integration

The web page tries to open the mobile app using deep links:
- `lokdaan://auth` - Opens to login screen
- The page includes a fallback for when the app isn't installed

### Smart Fallback

If the app doesn't open within 3 seconds, users can:
- Complete the password reset process directly on the web
- Download the app from app stores (if you configure the store URLs)

## Customization

### Styling

The page uses a modern, responsive design. You can customize:
- Colors in the CSS (look for `#667eea` and `#764ba2`)
- Logo (currently shows "L" - replace with your actual logo)
- App name and messaging

### Functionality

- Add analytics tracking
- Customize error messages
- Add additional validation
- Integrate with your own backend if needed

## Security Notes

- The page uses HTTPS (required for Supabase auth)
- Tokens are processed client-side and sent directly to Supabase
- No sensitive data is stored on your web server
- The page works entirely with Supabase's auth system

## Troubleshooting

### Common Issues

1. **"Configuration not found"**
   - Make sure `config.js` is in the same directory as `reset-password.html`
   - Check that your Supabase URL and key are correct

2. **"Invalid redirect URL"**
   - Add your web domain to Supabase redirect URLs
   - Make sure the URL exactly matches your hosting domain

3. **App doesn't open**
   - Verify deep link configuration in iOS/Android
   - Test deep links work: `lokdaan://auth`

4. **Password reset fails**
   - Check browser console for errors
   - Verify Supabase configuration
   - Check that tokens in URL are valid (not expired)

### Testing Locally

You can test the page locally:

1. Start a local server: `python -m http.server 8000`
2. Visit: `http://localhost:8000/reset-password.html`
3. Configure Supabase to allow `http://localhost:8000` as a redirect URL

## Production Checklist

- [ ] Updated `config.js` with production Supabase credentials
- [ ] Updated `webConfig.js` in React Native app with production web URL
- [ ] Added web domain to Supabase redirect URLs
- [ ] Uploaded both HTML and config files to hosting
- [ ] Tested password reset flow end-to-end
- [ ] Tested app deep linking works
- [ ] Verified HTTPS is working on your domain
