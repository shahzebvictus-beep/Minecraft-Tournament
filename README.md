# Minecraft Tournament Site (Next.js + Supabase)

## 1. Supabase setup
1. Create a free project at supabase.com.
2. SQL Editor → paste and run `supabase/schema.sql`.
3. Authentication → Providers → Email: enabled. (Optional: turn off "Confirm email" for faster sign-ups; if left on, players confirm by email, then log in and register.)
4. Authentication → URL Configuration → set Site URL to your deployed URL (and add `http://localhost:3000` as a redirect) so password-reset links work.
5. Project Settings → API → copy the Project URL and anon key.

## 2. Run locally
```
cp .env.example .env.local   # fill in the two values
npm install
npm run dev                  # http://localhost:3000
```

## 3. Deploy (Vercel)
Push to GitHub → import on vercel.com → add the two env vars → Deploy. Then put the Vercel URL into Supabase's Site URL.

## How the 40-player cap works
`register_player()` in the database takes an advisory lock, checks the count, and inserts in one transaction, so two people can't both take slot 40. Writes to `registrations` are blocked by RLS except through that function. The public list is the `participants` view (number, username, skin ID only).

## Known limitation
Minecraft usernames are validated against Mojang and skins use the real UUID, but this does not prove the player *owns* the account. Real ownership checks need Microsoft OAuth plus Xbox/Minecraft services (Azure app approval), which isn't practical for a few days' notice. To remove a fake or wrong entry, delete the row in Supabase → Table Editor → `registrations`.
