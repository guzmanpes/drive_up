# Car Line — Pickup Roster

A simple, phone-friendly tool for matching students to their car during dismissal pickup, using a printed QR tag and a short Family Code — no license plate needed. It's a single static page hosted free on GitHub Pages. All staff phones read and write the *same* roster because the app stores the data as a file inside your GitHub repo.

**This holds student names — keep the repo private, and check with your school's IT/admin before rolling it out.**

## 1. Create the repo

1. Go to github.com → **New repository**.
2. Name it something like `pickup-roster`.
3. Set visibility to **Private**.
4. Create it, then upload **all the files from this folder** to the root of the repo — `index.html`, `manifest.webmanifest`, `favicon.ico`, `favicon-32.png`, `favicon-16.png`, `apple-touch-icon.png`, `icon-192.png`, `icon-512.png` (drag-and-drop several at once works fine on GitHub's web upload page, or use `git push`). The icon files give the app a proper icon when someone bookmarks it or adds it to their home screen — `index.html` alone is enough for the app to *function*, but you'll get a generic icon instead.

## 2. Turn on GitHub Pages

1. In the repo, go to **Settings → Pages**.
2. Under "Build and deployment," set **Source** to "Deploy from a branch."
3. Pick your branch (usually `main`) and folder `/ (root)`. Save.
4. GitHub gives you a URL like `https://yourname.github.io/pickup-roster/`. That's the link staff will open on their phones. (Note: GitHub Pages sites are publicly reachable at that URL even if the repo is private — the *page* is public, but the roster *data* is only readable/writable through the API using your token, so don't share the token, and consider this when deciding what data you're comfortable with. If your school needs the page itself locked down too, ask IT about an organization plan with Pages access restrictions.)

## 3. Create an access token (do this once, share the process not the token)

Each staff member should ideally have their **own** token, so access can be revoked individually if a phone is lost.

1. On GitHub: **Settings (your profile) → Developer settings → Personal access tokens → Fine-grained tokens → Generate new token.**
2. Name it (e.g. "Pickup app — Ms. Ortiz's phone").
3. Set **Repository access** to "Only select repositories" → choose your `pickup-roster` repo.
4. Under **Permissions → Repository permissions**, set **Contents** to **Read and write**. Leave everything else as "No access."
5. Generate the token and copy it — GitHub only shows it once.

## 4. Connect the app on each phone

1. Open the GitHub Pages URL on the staff member's phone.
2. Go to the **Settings** tab in the app.
3. Enter the repo owner (your GitHub username or org), repo name, branch (`main`), and the token from step 3.
4. Tap **Save & connect**. The app will confirm it's synced.

The token is stored only in that phone's browser storage — it is never sent anywhere except directly to GitHub's API.

**Tip:** tap **Add to Home Screen** (Safari) or **Install app** (Chrome) to put a proper car icon on the home screen and have it open full-screen without the browser address bar, like a real app. One thing to know: once you do this, iOS can treat the home-screen icon and a regular Safari tab of the same page as having *separate* saved connections — if you switch between opening it via the icon and via a browser bookmark, it can look like the app randomly "logged you out." Stick to one consistent way of opening it (the home screen icon is recommended) to avoid this.

**If a device does lose its connection:** in Settings, tap **📋 Copy connection info** on a device that's still connected, save that somewhere safe (a password manager, not a plain note), and use **📥 Paste connection info** on the device that needs reconnecting — it fills in all four fields at once instead of retyping them.

## 5. Using the queue

A **Queue** tab now sits next to Look up. Workflow:

1. Scan the car's tag, or type a family code / student name in **Look up** as a car pulls up.
2. On a match, tap **+ Add to queue** on the result card.
3. The **Queue** tab shows everyone waiting, in the order they were added, with the next car up front and highlighted.
4. Whoever is calling names at the door taps **Picked up** to remove that car once it's loaded.
5. If a name was added by mistake — wrong car tapped, a typo, a duplicate scan — tap **🗑️ Remove** instead of Picked up. This is the important distinction: **Picked up** logs a confirmed pickup in the pickup log (Settings → Pickup log), while **Remove** deletes the entry outright without logging anything, since it was never a real pickup. Both ask for confirmation first.

**Quick-add (temporary — not on file)** — for a student who genuinely isn't on the roster at all, being picked up just for today (not a normal enrollment situation). There's an always-visible button right on the Look Up page — tap **➕ Quick-add (temporary — not on file)**, type their name, and they're in the queue immediately. No confirmation step, since this is built for the exact moment every second at the curb matters. Important distinction: **this never creates a roster record** — it only ever touches today's queue, tagged with a red **"📝 not on file"** label so it's clear at a glance. If this student needs to be a permanent part of your roster going forward, add them properly via the **Add** tab afterward — that's a deliberate, separate action, not something quick-add does silently.

**Add rider** — for when a parent is picking up a child who isn't normally in their car (a neighbor's kid, a friend riding home for a playdate, etc.). Tap **➕ Add rider** on the queue entry for the car that's actually here, type the other student's name, and confirm. A few things about how this works:
- It searches the *whole* roster, not just today's queue — the student can belong to any car.
- If the name matches more than one student, you'll get a numbered list to pick from (add a grade to your search, like "Maya 4", to narrow it down before that point).
- The confirmation shows exactly who you're about to add and which car they're normally on, so there's a check before it's final.
- This only ever changes *today's queue* — neither car's permanent roster record is touched. The added student shows up with a small **🚗 rider** tag so it's clear at a glance they're not normally in this car, and they get their own ✕ to remove if plans change.
6. **Clear all** wipes the queue — use this at the end of dismissal, not mid-line.
7. When a whole batch of cars just left at once (e.g. 10 cars pulled away together), tap **✅ Clear first N picked up** instead of tapping Picked up ten separate times — enter the count, review the preview of exactly who's about to be cleared, and confirm. It only touches the front of the *active* line (placeholders and parked cars are untouched either way), and each one gets logged in the pickup log exactly as if you'd tapped Picked up on it individually.

**Parked** — for a car that needs to step out of the active line without being marked as picked up (waiting on an extra sibling, a family running late to the curb, etc.):
- Tap **🅿️ Parked** on a waiting car to move it into a separate **Parked** section below the main list — it's no longer holding up the line, but hasn't left campus either.
- Tap **↩️ Back in line** to return it to the active queue (it rejoins at the back, with a fresh arrival time).
- Tap **Picked up** from either section once the car actually leaves — parked cars don't need to go back in line first.

**Reserve spots ahead** — for when a second person gets ahead of the main scanner (say, 20 cars down the line) and wants to start adding names without their car jumping ahead of ones that are physically in front of it. Tap **🎫 Reserve spots ahead**, enter how many unscanned cars are ahead of them, and that many dashed "— car not yet scanned —" placeholder rows appear in the line.

There are two distinct add buttons, and which one to use depends on who you are in this moment:
- **"+ Add to queue"** — for the front-of-line scanner, working through cars in the order they physically arrive. No different behavior needed here at all; keep scanning normally, and each one automatically fills the oldest still-open placeholder, closing the gaps in from the front.
- **"➕ Add after queue"** — for the second person, further down the line. This one always lands at the *true end* of the whole queue, skipping every placeholder gap entirely, and the entry gets a visible "2nd adder" tag so it's clear it wasn't added in scan order.

So the flow for getting ahead: tap **Reserve spots ahead**, enter your count, then find your own car in Look Up and tap **➕ Add after queue** specifically — not the regular button. Any placeholder nobody gets to (overestimated the count) has its own **✕ Remove** button on that row.

The queue is shared the same way the roster is (stored as `queue.json` in the same repo), so every phone sees the same live order. Because several phones may add/remove entries within seconds of each other, the app automatically retries a save a few times if it detects someone else wrote first — you shouldn't notice this happening, but if you ever see "please try that action again," just tap it once more.

## 6. Printing and using QR car tags

Camera-based license plate reading turned out to be unreliable (general OCR isn't built for the job — different fonts, angles, glare, decorative frames), so the app doesn't use plates at all anymore. Instead, each car gets a small printed QR tag to display in the windshield, which scans fast and near-perfectly, plus a short Family Code as a human-readable backup.

**Printing tags:**
1. Every car gets a tag code automatically the moment it's added to the roster (manually or via CSV) — nothing extra to do.
2. **Roster tab → 🖨️ Print all tags** prints one tag per car, or tap **🏷️ Tag** on a single roster row to print just that one (handy for adding a car mid-year).
3. **Printing just the newest additions:** if you've added a batch of new cars (say, 50 new students on top of your existing 700) and don't want to reprint everyone's tag again, use **"Print tags added since"** right below the toolbar — pick a date/time and tap **🖨️ Print those**. It defaults to 24 hours ago, but you can set it to any point in time. This goes by when each car was actually *added* to the roster, not when it was last edited — so a car that Match & Fill touched last week to fill in a Student ID won't get mixed in with today's truly new additions.
4. **Printing by grade** (handy for a bulk tag replacement affecting one grade level): use **"Print tags for a grade"** — the dropdown lists every grade currently in your roster, pick one and tap **🖨️ Print those** to print every car with at least one student in that grade.
5. **Printing a hand-picked set of families:** each row in the Roster tab has a checkbox. Check off however many families you need (e.g., specific families whose tags were lost or damaged), and a **"🖨️ Print selected"** bar appears at the top — tap it to print just that batch. **Clear selection** resets it. Selections stay in place while you search/filter the list, so you can check some names, search for more, and check those too before printing.
4. Each tag shows a QR code, the student name(s), grade, and a short backup code in case the QR ever gets damaged.
5. Print on cardstock if you can, cut along the dashed lines, and consider laminating — these get reused every day.
6. Hand tags out to families to keep displayed on the dashboard or clipped to a visor during pickup.

**Scanning tags at pickup:**
1. **Look up tab → 📷 Scan car tag.**
2. Hold the tag inside the box — no need to tap anything else. The moment it's recognized, that car is **added to the queue automatically** and you'll see a green confirmation.
3. Keep pointing the camera at each car's tag as it comes through — it scans continuously, so you can move straight from one car to the next.
4. Tap **Done scanning** to close the camera when there's a lull.

If a car doesn't have its tag (forgotten, lost, new student), manual lookup in **Look up** still works — type the Family Code if you know it from another list, or type the student's name (first name + last initial works fine), then tap **+ Add to queue**.

## 7. Load the roster

- **Manual add:** Add tab → each student gets their own row with First Name, Last Name, Student ID, and Grade — tap **+ Add another student** for siblings sharing the car. Only a first or last name is required per student. **Contact name(s)** apply to the whole car (since that's who you're reaching regardless of which kid is inside) — supports multiple entries separated by a semicolon (`;`), e.g. `Jane Chen; John Chen`. As you type a student's name, a live warning appears if that name is already on file under a *different* car, with a one-tap **Edit that car instead** shortcut — this is the main defense against accidental duplicates, since there's no plate to check against anymore.
- **Bulk import:** Add tab → CSV upload. Recognized columns (case/spacing don't matter): `Family Code`, `Student ID`, `First Name`, `Last Name`, `Grade`, `Teacher` (or `PES`), `Contact Name`, `Notes` — matching a typical school records export. Only First Name or Last Name is required. One row per student — siblings sharing a car just need the same `Family Code` value on multiple rows (or the same `Contact Name`, if there's no Family Code yet) and they'll be grouped together automatically, each keeping their own grade and Student ID. If neither is present, the row becomes its own separate car — merge it with siblings later via **Find duplicates** if needed. If a `Contact Name` cell needs more than one entry inside itself, separate them with a semicolon (not a comma) — a comma inside an unquoted cell would be read as a new column and break the row. A comma *inside a quoted cell* (e.g. an address like `"123 Main St, City, ST"`) is handled correctly and won't break parsing. Before the import runs, it checks whether any student in the file already appears elsewhere in the roster under a different Family Code — if so, you'll see a confirmation listing who, so you can catch an accidental re-upload before it creates a duplicate.

- **Match & fill (for a whole-school directory export with no Family Code column):** If your CSV is a school-wide directory rather than just your car-line families, importing it normally would create a brand-new duplicate entry for every student already in your roster (since there's no Family Code to merge on). Instead, after loading the CSV, tap **🔗 Match & fill into existing cars instead**. It matches directory rows to your existing cars by name, shows you each proposed match (high-confidence exact matches are pre-checked, everything else is left for you to review), and fills in Student ID / Grade / Contact Name into the *existing* car — no duplicates, and it never overwrites a Contact Name you already entered manually. Anything it can't confidently match is listed at the bottom so you can fill it in by hand later via Roster → Edit.
- **Importing everyone else:** once you've applied the matches you're confident in, the same screen shows an **"Import remaining N as new cars"** button — this adds every student who wasn't matched to an existing car as a brand-new entry (each automatically gets its own tag code the moment it's created). It re-groups siblings back into one car using the same Family Code/Contact Name grouping the file was parsed with. This checks for likely duplicates against your existing roster one more time before it runs, and only touches students who weren't already applied via a match, so it's safe to run after Match & Fill without creating duplicates of what you just merged.

## 7b. The "PES" field (teacher name, coded)

Each student can have a teacher name saved — but it's deliberately never labeled "Teacher Name" or shown as typed anywhere in the live app. It's labeled **PES** everywhere, and only a short auto-assigned code (like `-X1`) is ever displayed. This exists for one specific reason: parents very often identify their kid by first name + teacher's name rather than grade or last initial, and during live pickup there's no time to work out which grade a given teacher teaches.

- **Entering it:** Add/Edit → each student row has a **PES** field. Type the teacher's real name there, same as any other field.
- **Searching by it:** in Look Up, add any 2+ letter fragment of the teacher's name to your search — e.g. "Jose guz" finds a student named Jose whose PES field is "Guzman." Order doesn't matter, same as searching by grade.
- **Getting a code:** the first time a new teacher name is saved (via Add/Edit, or a CSV import), it's automatically assigned the next available code and that mapping is remembered forever after — the same teacher always gets the same code, everywhere, from then on. This mapping lives in its own small file (`teacher-codes.json`) in your repo, separate from the roster.
- **What actually shows on screen:** only ever the code (`PES -X1`) — in Roster, in Look Up, everywhere. The real teacher name is stored (so it stays searchable and the CSV export includes it for your own records), but the app itself never displays it as plain text.
- **This is a beta-period tool.** Once QR-tag scanning is the only way pickup happens, this whole field becomes unnecessary — it exists specifically to solve the "parent says a name and a teacher, staff has seconds to respond" problem during the current rollout.

## 8. Finding and merging duplicates later

Duplicates will happen over time — a student imported from a directory file gets a *second*, separate entry if a family is later added by hand without checking first. Three layers of defense:

- **As you type:** the Add form warns live if a name you're entering already exists elsewhere, before you ever save (see above) — this is the main line of defense.
- **On import:** both regular CSV import and "Import remaining as new cars" check for likely name matches against your existing roster and ask for confirmation before creating anything, rather than silently duplicating.
- **Cleanup for anything that still gets through:** **Roster tab → 🔗 Find duplicates** scans your whole roster for the same student name showing up on two different cars, and shows each pair side by side. Tap **"Keep [family code], merge other in"** to combine them — it keeps that car's tag, pulls in any missing Student ID/Grade, adds any siblings that were only on the other record, and merges Contact Name(s) without repeating duplicates. If it's just two different kids who happen to share a name, tap **Not a duplicate — ignore** to dismiss it.

**Finding students missing a Student ID:** the Roster tab shows a running count (e.g. "130 with a Student ID") right under the header. To actually see *who's* missing one, tap **⚠️ Missing Student ID** — it filters the list down to just those cars, with the specific student(s) missing an ID visibly flagged in red on each row (in case a car has some siblings with an ID and others without). Tap the button again to clear the filter.

**Tracking whether a tag has actually been printed:** every car gets a tag *code* the moment it's created, but that doesn't mean a physical tag has ever been printed and handed out. Each roster row now shows a small badge — **"🖨️ Never printed"** (red) or **"🖨️ Printed [date]"** (blue) — so you can tell the two apart at a glance. Tap **🖨️ Never Printed** in the toolbar to filter the list down to just the cars still waiting on their first tag. Printing is marked the moment you tap any print button (Print all tags, Print selected, the per-row 🏷️ Tag button, etc.) — it doesn't verify the physical printer actually produced paper, just that you told the app to print. If you manually change a car's Family Code in Edit to match an already-printed physical tag, its printed status resets, since that old badge was tracking the *previous* code.

## 9. Exporting and comparing against an outside list

- **Export your whole roster:** **Roster tab → ⬇️ Export CSV** downloads everything currently in the app — one row per student, same columns as import — so you can open it in Excel anytime for your own records or analysis. This is a snapshot; editing the downloaded file doesn't change anything in the app unless you re-import it.
- **Telling which rows belong to the same car:** the export includes a **`Family Code`** column — every sibling sharing a car shows the identical code. Sort or filter by this column in Excel to see the groupings clearly. This is the same short code used on that car's printed QR tag.
- **Safely re-importing an edited export:** if you edit the exported file and bring it back in via **Import into roster**, it recognizes the `Family Code` column and uses it to update the *exact same* car it came from. Don't delete or edit that column if you want your edits to land back on the right car instead of creating a new one; if you do remove it, importing will fall back to grouping by matching `Contact Name` instead.
- **Compare a new list against your roster (e.g. a fresh school records export):** in the Add tab, load the CSV like normal, then tap **🔍 Compare to roster (find who's missing)** instead of importing. It checks every student in that file against your roster — first by Student ID, then falling back to name matching (so someone already on your roster who just hasn't had their ID filled in yet won't show up as a false "missing") — and lists exactly who isn't there yet. Tap **⬇️ Download missing list** to get just those students as a CSV, ready to bring back in through the normal import or Match & Fill flow. This tool only reads and compares — it never changes your roster on its own.
- If you have an Excel file rather than CSV, save it as CSV first (**File → Save As → CSV**) before uploading — or drop the `.xlsx` in a chat with Claude to have it converted, the same way the original family roster file was handled.

One thing to know: if the record you *don't* keep already had its QR tag printed and handed to a family, that specific tag stops working after the merge (the surviving record keeps its own tag). Reprint a fresh tag for that car afterward if that's the case.

**Clearing old data you no longer collect:** if a field (like email, if you've decided you don't need it) has old data saved on existing cars, a regular CSV re-import with that column left blank **won't** erase it — the app deliberately never lets a blank field overwrite existing data, to protect against accidental data loss during partial re-imports. For a one-time, deliberate wipe instead, use **Settings → Roster maintenance → 🗑️ Clear all stored emails**, which removes it from every car in one confirmed action.

**Undoing a mistake without digging through GitHub by hand:** every save is automatically its own snapshot. **Settings → Recent changes → 🕐 View recent changes** shows your last 20 saves in plain language (what changed and when), each with a **↩️ Restore this version** button. It shows you how many cars that snapshot had versus your current roster before you confirm, and restoring is itself just another save — so if you restore the wrong one, you can restore forward again right after.

**A record of who actually got picked up:** the queue itself only shows who's waiting *right now* — once you tap "Picked up," that's gone. **Settings → Pickup log → 📋 View pickup log** keeps a separate running record instead: every confirmed pickup, with a timestamp, browsable by day and exportable as CSV. Tapping **Clear all** on the queue, or excluding an absent sibling via the ✕ on a queue entry, doesn't add anything here — only an actual "Picked up" tap counts as a confirmed pickup. The log grows by one entry per pickup indefinitely unless you clear it (Settings → Pickup log → 🗑️ Clear entire log) — export what you need periodically if you'd rather keep it trimmed.

## 10. At pickup

Two ways to get a car into the queue:
- **Scan its tag** (Look up → 📷 Scan car tag) — fastest, adds automatically.
- **Type its Family Code or the student's name** (Look up) if there's no tag on hand — tap **+ Add to queue** on the match.

Either way, the **Queue** tab shows who's up next for everyone, live.

## End of year

- Revoke each staff token from **Settings → Developer settings → Personal access tokens**.
- Optionally clear or archive the `data.json` file in the repo before next year's rollout.

## Limitations to know about

- This is a lightweight tool for a single school, not a hardened multi-tenant system. It assumes staff are trusted with read/write access to the roster.
- Every roster save fetches the latest version from GitHub, applies just your change to it, and retries automatically if another phone saved in the same instant — so two staff editing at once shouldn't clobber each other's work.
- There's no login screen beyond the GitHub token — anyone with a valid token can read and edit the roster, so treat tokens like passwords.
- Camera scanning requires HTTPS (GitHub Pages provides this automatically) and the browser's camera permission. It reads QR codes on-device only — no images are uploaded or stored anywhere. The QR scanning and printing libraries are bundled directly into `index.html` (not loaded from any external CDN), so they keep working even on networks that block third-party script domains.
- A tag only works if it's the one printed for that specific car. If a family loses theirs, reprint it from the Roster tab (🏷️ Tag) rather than making a new one from scratch — the QR is tied to the car's record, not to any personal info by itself.
