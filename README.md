# CHOP NATION Dashboard

Internal business dashboard for CHOP NATION: Halalicious Hibachi — a food truck and catering operation.

**Live app:** https://food-beverage-dashboard.vercel.app

---

## What It Does

- Tracks catering events: revenue, food cost, deposit, profit, guest count, status
- Tracks food truck daily expenses by category (Groceries, Supplies, Fuel, Other)
- Auto-calculates profit and monthly expense totals
- Role-based access: owner sees everything, staff sees food truck section only
- Shared notes synced across devices

## Stack

- **Frontend:** Single HTML file (HTML + CSS + vanilla JavaScript)
- **Backend:** Supabase (PostgreSQL)
- **Auth:** Supabase Auth (email/password, password reset)
- **Deployment:** Vercel (auto-deploys on push to main)
- **Repo:** GitHub

## Supabase Tables

| Table | Purpose |
|-------|---------|
| `events` | Catering event records |
| `food_truck` | Daily truck expense logs |
| `notes` | Single shared notes entry |
| `user_roles` | Maps user IDs to roles (owner / staff) |

RLS is enabled on all tables. Authenticated users have full access. Role-based UI restrictions are enforced on the frontend.

## Local Setup

No build tools or dependencies. Open `index.html` directly in a browser or use VS Code Live Server.

To deploy changes:

```
git add .
git commit -m "your message"
git push
```

Vercel auto-deploys on push to `main`.

## Security Notes

- Uses Supabase publishable (anon) key — safe for frontend use
- RLS policies block unauthenticated database access
- Service role key is never used in frontend code
- Role-based access restricts staff from viewing catering financials

## Rough Areas / Known Limitations

- Supabase URL and anon key are hardcoded in `index.html` (environment variables not yet configured)
- Role restrictions are frontend-only — RLS does not yet enforce per-role table access at the database level
- No audit log or change history
- Notes are shared across all users (not per-user)
