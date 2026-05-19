# Backend Learning Notes — FnB Dashboard

## 1. What localStorage Was

Before Supabase, the app saved data in the browser's built-in memory called localStorage.
It worked like a sticky note on your specific computer, in your specific browser.

Problems:
- Close the tab → data was gone (for some things)
- Open on your phone → nothing there
- Your worker opens the link → empty screen
- Clear your browser → everything deleted

localStorage still exists in this app for the Notes section only.

---

## 2. What Supabase Changed

Supabase is a real database living on a server in the cloud.
When you add an event, it doesn't save to your browser — it saves to Supabase.

What that means:
- Close the tab → data is still there when you come back
- Open on your phone → same data
- Your brother opens the link → same data
- You lose your laptop → data is safe

Think of localStorage as a whiteboard in your office.
Think of Supabase as a filing cabinet in a secure building that anyone you authorize can access.

---

## 3. What CRUD Means in This App

CRUD is the 4 things any app does with data.

| Letter | Word   | In This App                              | Tested |
|--------|--------|------------------------------------------|--------|
| C      | Create | Adding a new catering event or expense   | Yes    |
| R      | Read   | Loading your events when the page opens  | Yes    |
| U      | Update | Changing an event's status dropdown      | Yes    |
| D      | Delete | Hitting the delete button on a row       | Yes    |

All 4 were tested manually on the live site and confirmed in Supabase.

---

## 4. What Auth Means in This App

Auth = authentication = the login system.

Before auth, anyone with the link could open the dashboard and see or add your business data.

What auth added:
- A login screen that appears before the dashboard
- Only users with a valid email + password can get in
- The app remembers you're logged in (session) so you don't re-login on every refresh
- A Sign Out button that ends your session

Users added in Supabase:
- You (owner) — full access
- Your manager — full access for now (role restriction comes later)

---

## 5. What RLS Means and Why It Matters

RLS = Row Level Security.

The login screen protects the front-end (what you see in the browser).
RLS protects the actual database behind the scenes.

Why both are needed:
Your Supabase URL and API key are visible in the HTML file, which is public on GitHub.
Someone technically skilled could use those to query your database directly — completely bypassing the login screen.

RLS stops that at the database level. Supabase now checks every query:
- Not logged in → blocked, returns nothing
- Logged in → allowed

Policy set on both tables (events and food_truck):
- Command: ALL (covers read, write, update, delete)
- Applied to: authenticated users only

Tested: confirmed data loads when logged in, login screen appears in incognito, session survives page refresh.

---

## 6. What Is Still Unsafe or Rough

**API key exposed in HTML file**
The Supabase URL and key are hardcoded in index.html, which is public on GitHub.
The publishable key is designed to be client-facing, but it's still visible.
Long-term fix: use environment variables or a backend proxy (more advanced).

**No role-based access**
Both you and your brother have identical access.
Eventually your brother should only be able to add food truck expenses — not see or edit catering event financials.
Fix: add user roles in Supabase and update RLS policies to check which user is logged in.

**No password reset**
If you or your brother forget the password, there's no "forgot password" flow yet.
Fix: add Supabase Auth email recovery (short task when needed).

**Notes still use localStorage**
The Notes section saves to the browser only, not Supabase.
Not a problem right now, but worth migrating later.

---

## 7. What to Learn Next

In order of priority:

1. **Role-based access** — restrict your brother to food truck section only
2. **Migrate notes to Supabase** — so notes sync across devices
3. **Environment variables** — hide the API key properly
4. **Password reset flow** — so users aren't locked out
5. **Mobile input page** — simple page your brother can use from his iPhone at the truck
