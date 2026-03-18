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

## 8. Debug Mode

1. Add `?debug=1` to the URL: `https://replymateai.app/upgrade/?debug=1`
2. Open DevTools → Console (F12)
3. Look for `[ReplyMate Debug]` messages – they show:
   - Whether any language section has the `active` class
   - Whether `#en` exists
   - The `lang` URL param (if any)
   - After 500ms: computed `display` and `visibility` of the content
4. If `en display` shows `none`, something is overriding our CSS

## 9. Test: Skip Subscription UI (isolate the cause)

Add `?debug=2` to the URL and sign in: `https://replymateai.app/upgrade/?debug=2`

This disables the subscription UI (Cancel button, plan markers, etc.). If the page **stays visible** after sign-in with `?debug=2`, the subscription UI code is likely causing the blank page. If it still goes blank, the cause is elsewhere (e.g. extension, Supabase, OAuth redirect).

## 10. Inspect the DOM

1. Right-click the page → **Inspect**
2. In the **Elements** tab, find `<div id="en" class="language-content active">`
3. Check if it has `active` in the class list
4. In the **Styles** panel, see if `display: none` is applied (and from which rule)

---

**403 on `/auth/v1/user`:** This can appear before sign-in. Ensure `replymateai.app` is in Supabase Redirect URLs and that your Supabase project allows the origin. It may not block sign-in if the session comes from the OAuth hash.

**Multiple GoTrueClient:** If you have the ReplyMate Chrome extension installed, it may create a second Supabase client on the page. Test with the extension **disabled** (or in incognito without extensions) to see if the blank page goes away.

**Most likely cause:** Supabase redirect URLs not including `https://replymateai.app/**`. Add it and try again.
