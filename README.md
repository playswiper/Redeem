# Swiper Redeem Form

GitHub Pages site for the Swiper order redemption form.

## One-time CAPTCHA setup

This form uses Cloudflare Turnstile with server-side verification. The browser site key is public; the secret key must never be placed in this repository.

1. In Cloudflare, create a Turnstile widget and allow the hostname `playswiper.github.io` (also add `redeem.playswiper.com` if you later use that domain).
2. Replace `YOUR_CLOUDFLARE_TURNSTILE_SITE_KEY` in `index.html` in both places with the widget's **site key**.
3. In Supabase Dashboard → Project Settings → Edge Functions → Secrets, add `TURNSTILE_SECRET_KEY` with the widget's **secret key**.
4. GitHub Pages: enable Pages for this repository from the `main` branch, `/(root)`.

Never put the Turnstile secret or Supabase service-role key in this repository.

## Backend

Supabase project: `upghyzhawctughyojwpo`

Edge Function: `redeem-submit`

Database table: `public.redeem_submissions`

The Edge Function verifies Turnstile server-side and inserts through a service-role-only database function. A database-enforced 30-second cooldown is keyed by a keyed hash of the request IP. Raw IP addresses are not stored.

## View submissions

Open Supabase Dashboard → Table Editor → `public.redeem_submissions`. Only project administrators/service-role access can read the submissions. Public and authenticated client roles have no table privileges and no SELECT policy.
