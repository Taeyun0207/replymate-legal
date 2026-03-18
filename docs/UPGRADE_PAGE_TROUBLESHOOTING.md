# Upgrade Page Blank After Sign-In – Troubleshooting

If you see a blank page with only "Cancel subscription" after signing in, try these steps:

## 1. Check Browser Console (F12)

1. Open **Developer Tools** (F12 or right-click → Inspect)
2. Go to the **Console** tab
3. Look for red error messages
4. Common issues:
   - `Failed to load resource` → script or asset 404
   - `CORS` errors → backend or Supabase blocking requests
   - `Uncaught` errors → JavaScript crash

## 2. Verify Supabase Redirect URLs

**Critical:** Supabase must allow `replymateai.app` for OAuth redirects.

1. Go to [Supabase Dashboard](https://supabase.com/dashboard) → your project → **Authentication** → **URL Configuration**
2. **Site URL:** `https://replymateai.app`
3. **Redirect URLs** – add all of these:
   - `https://replymateai.app/**`
   - `https://replymateai.app/upgrade/index.html`
   - `https://replymateai.app/`
4. Remove any old `taeyun0207.github.io` URLs if you’ve fully moved to the new domain
5. Click **Save**

## 3. Sign In on the Same Domain

- If you sign in on `taeyun0207.github.io` and then open `replymateai.app`, the session does **not** carry over (different origins).
- Always sign in directly on `https://replymateai.app/upgrade/`.

## 4. Check the URL After Sign-In

After Google sign-in, the URL might look like:

- `https://replymateai.app/upgrade/#access_token=...` (normal)
- `https://replymateai.app/upgrade/?error=...` (OAuth error)
- `http://localhost:...` (redirect URL misconfigured)

If you land on localhost, fix Supabase redirect URLs (step 2).

## 5. Hard Refresh & Cache

1. Press **Ctrl+Shift+R** (Windows) or **Cmd+Shift+R** (Mac)
2. Or clear site data: DevTools → Application → Storage → Clear site data

## 6. Test in Incognito

Open an incognito/private window and try the flow again. This rules out extensions or cached data.

## 7. Google OAuth Origins

1. Go to [Google Cloud Console](https://console.cloud.google.com) → **APIs & Services** → **Credentials**
2. Open your OAuth 2.0 Client ID (Web application)
3. Under **Authorized JavaScript origins**, add:
   - `https://replymateai.app`
4. Under **Authorized redirect URIs**, ensure:
   - `https://cmmoirdihefyswerkkay.supabase.co/auth/v1/callback`

## 8. Debug Mode (Optional)

Add `?debug=1` to the URL (e.g. `https://replymateai.app/upgrade/?debug=1`) and open the Console. Check for any `[ReplyMate]` logs or errors.

---

**Most likely cause:** Supabase redirect URLs not including `https://replymateai.app/**`. Add it and try again.
