# Google OAuth Troubleshooting Guide

## Current Issue: OAuth Loading Loop

The Google OAuth is getting stuck in a "loading" state. Here are the most likely causes and solutions:

## 1. Google Cloud Console Configuration

**CRITICAL**: Check your Google Cloud Console OAuth 2.0 Client configuration:

### Required Settings:
- **Authorized JavaScript origins**: 
  - `http://localhost:3000` (for development)
  - Your production frontend URL
  
- **Authorized redirect URIs**: 
  - `http://localhost:5000/auth/google/callback` (for development)
  - Your production backend callback URL

### Steps to Fix:
1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Navigate to APIs & Services > Credentials
3. Find your OAuth 2.0 Client ID
4. Click Edit
5. Add the URLs above to the appropriate sections
6. Save changes

## 2. Environment Variables

Verify these are set correctly in your `.env` file:

```bash
GOOGLE_CLIENT_ID=your_google_client_id_here
GOOGLE_CLIENT_SECRET=your_google_client_secret_here
BACKEND_URL=http://localhost:5000
FRONTEND_URL=http://localhost:3000
SESSION_SECRET=your_secure_session_secret
JWT_SECRET=your_secure_jwt_secret
```

## 3. Common Issues and Fixes

### Issue: OAuth Consent Screen
- Make sure your OAuth consent screen is configured
- Add test users if your app is in testing mode
- Verify scopes include `profile` and `email`

### Issue: Client ID Mismatch
- Ensure the Client ID in your `.env` matches the one in Google Cloud Console
- Check for extra spaces or characters

### Issue: Redirect URI Mismatch
- The callback URL must EXACTLY match what's in Google Cloud Console
- Check for trailing slashes, http vs https, port numbers

## 4. Testing Steps

1. **Test OAuth Configuration**:
   Visit: `http://localhost:3000/oauth-debug`
   
2. **Test Backend Connectivity**:
   Visit: `http://localhost:5000/debug/oauth-config`
   
3. **Test OAuth Flow**:
   Visit: `http://localhost:5000/auth/google`

## 5. Debug Information

Check browser console and backend logs for:
- CORS errors
- Redirect URI mismatch errors
- Authentication failures
- Token processing errors

## 6. Recent Fixes Applied

1. **Session Configuration**: Fixed session cookies for OAuth compatibility
2. **Error Handling**: Added comprehensive error handling and logging
3. **Token Processing**: Improved frontend token handling with better error recovery
4. **Debug Tools**: Added OAuth debug page and enhanced logging

## 7. Next Steps

If the issue persists:
1. Check the browser network tab during OAuth flow
2. Verify Google Cloud Console settings match exactly
3. Test with a fresh incognito/private browser window
4. Check if your Google account has the necessary permissions

## 8. Production Considerations

When deploying to production:
- Update `BACKEND_URL` and `FRONTEND_URL` to production URLs
- Add production URLs to Google Cloud Console
- Use HTTPS for production OAuth
- Set `NODE_ENV=production`