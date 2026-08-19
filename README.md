# Piedmont Showroom — Prototype

A self-service showroom page for internal buy-in. Plain static site, no build
step. The page reads its data **directly from the committed spreadsheet**, so
updating the site means editing Excel and pushing — no agency, no code changes.

## How it works
- `index.html` — the page. On load it fetches `PNC_Showroom_Inventory_Tracker.xlsx`,
  reads the **Showroom Inventory** tab, and builds the cards.
- `PNC_Showroom_Inventory_Tracker.xlsx` — the source of truth. Edit in Excel.
- `photos/` — unit images, named `<Unit ID>.jpg` (e.g. `sharp-sx.jpg`).

Behavior pulled from the sheet: Available/Pending units show, Rented/Sold hide;
Qty 1 gets the red "Only One Available" badge; blank prices show "Request quote";
Rent-to-Own months drives the credit line; the "(EXAMPLE)" row is ignored.

## Your update workflow (self-service)
1. Edit `PNC_Showroom_Inventory_Tracker.xlsx` in Excel — change a price, flip a
   Status to `Sold`, add a row for a new unit.
2. Add any new photo to `photos/`, named to match the Unit ID.
3. `git add . && git commit -m "update inventory" && git push`
4. Vercel redeploys automatically (~30 s). Done.

## First-time setup (once)
    git init
    git add .
    git commit -m "showroom prototype"
    git branch -M main
    git remote add origin <your-repo-url>
    git push -u origin main
Then at **vercel.com/new**, import the repo and Deploy. No framework; if asked
for an Output Directory, enter `.` (a single dot). You get a
`…vercel.app` URL for the demo.

## Preview locally before pushing
Browsers block reading the spreadsheet over `file://`, so don't just double-click
`index.html`. Serve the folder over HTTP:
    npx serve
    # or
    python -m http.server
Then open the printed `http://localhost:...` address.

## Going public later (one-time agency/IT task)
Keep managing everything here. When ready for a branded URL, ask whoever controls
Piedmont's DNS to add a CNAME for **showroom.piedmontnational.com** pointing at
this Vercel project. That's their only involvement — you own content and deploys.

## Data notes to clean up in the sheet
- Sharp SX **Photo Filename** is a full Windows path — set it to just
  `sharp-sx.jpg` (the code strips the path anyway, but keep the sheet clean).
- Garrido rent is `379.1666…` — the page rounds to `$379`; consider setting a
  clean number.
- Delete the `(EXAMPLE)` row when convenient (already ignored by the page).
- Don't rename the column headers — the page keys on them.
