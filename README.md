# Deploying ConfHub Registration
## NAPRAS 2027 — step by step

**Time:** about 60 minutes
**You will not write any code.** You copy two files and fill in boxes.

> This uses **Engine v2**, which replaces the v1 engine from the earlier guide.
> v2 does registration, passes and check-in all in one file. If you never
> installed v1, ignore this note — you are starting fresh, which is simpler.

---

## What you'll have at the end

A working registration page at **napras2027.confhub.ng/register.html** where a delegate:

1. Picks their category, sees the price, adds a workshop or gala if they want
2. Registers and gets a unique payment reference
3. Pays **online by card** or **by bank transfer** — their choice
4. Gets their delegate pass and receipt once payment is confirmed

And a private sheet where you and the treasurer see exactly who has paid.

---

## Before you start

- [ ] Logged into Google as **confhub.ngr@gmail.com**
- [ ] The two files: `confhub-engine.gs` and `register.html`
- [ ] A laptop (not a phone)

**You do not yet need** NAPRAS's bank details or Paystack link. Deploy it with
placeholders, demo it to them in November, then paste their real details in.
Nothing needs rebuilding.

---

# PART A — Create the private sheet
*15 minutes*

> ⚠️ This sheet will hold delegate names, emails and phone numbers.
> It must stay **private**. Never set it to "anyone with the link".

### A1 — New spreadsheet

1. Go to **sheets.new**
2. Click **Untitled spreadsheet** top left
3. Name it: `NAPRAS 2027 — Registration (PRIVATE)`

### A2 — Paste the engine

1. Menu bar → **Extensions** → **Apps Script**
2. A code editor opens in a new tab
3. Click in the code area, press **Ctrl+A** (or **Cmd+A**), press **Delete**
4. Open `confhub-engine.gs` in Notepad or TextEdit, select all, copy
5. Paste into the empty editor
6. Click the **save icon** (or Ctrl+S)
7. Top left, rename the project to `ConfHub Engine`

### A3 — Wake up the menu

1. Go back to the spreadsheet tab
2. **Close that tab, then reopen the spreadsheet**
   *(the menu only appears on a fresh load — this trips everyone up)*
3. After **Help** in the menu bar you should now see **ConfHub**

### A4 — Create the tabs

Click **ConfHub → Set up sheets**.

Google will show warnings the first time. The exact path:

1. **Review permissions**
2. Choose **confhub.ngr@gmail.com**
3. *"Google hasn't verified this app"* → click **Advanced** (small link, bottom left)
4. Click **Go to ConfHub Engine (unsafe)**
5. Click **Allow**

> It says "unsafe" because Google doesn't know who wrote it. You did. It only
> touches this one spreadsheet and sends email from your own account. Once only.

✅ You now have five tabs: **ConfHub Settings**, **Fees**, **Extras**, **Delegates**, **Check-in Log**

---

# PART B — Set your prices
*10 minutes*

### B1 — The Fees tab

Seven categories are pre-filled with example prices. Change them to NAPRAS's real fees:

| category | fee | earlybird | active |
|---|---|---|---|
| Consultant | *your price* | *your price* | YES |
| Trainee / Resident | | | YES |
| Non-member | | | YES |
| Allied Health / Nurse | | | YES |
| Student | | | YES |
| Virtual / Remote delegate | | | YES |
| Exhibitor | | | YES |

- **Don't use commas or ₦** — type `150000`, not `₦150,000`
- To remove a category, set `active` to `NO` (keeps the history)
- Leave `earlybird` blank if a category has no discount

### B2 — The Extras tab

Pre-conference workshop and gala dinner are there as examples. Change, add or
switch off as needed. Set `active` to `NO` if NAPRAS isn't offering extras.

### B3 — Don't worry about getting these right yet

You can change every number later without touching code or redeploying.

---

# PART C — Fill in the settings
*10 minutes*

Open the **ConfHub Settings** tab and fill the yellow cells:

| Setting | What to put |
|---|---|
| Event name | `32nd NAPRAS Annual General Meeting` |
| Event dates | `August 2027` (refine when confirmed) |
| Event venue | `To be confirmed` |
| ID prefix | `NAP27` |
| Currency | `NGN` |
| **Registration open** | **`NO`** ← keep closed until NAPRAS agrees |
| Early bird ends | `2027-05-31` (or blank) |
| Pass page URL | `https://napras2027.confhub.ng/pass.html` |
| API URL | *leave blank — Part D fills this* |
| Desk passcode | make one up |
| Require payment before pass | `YES` |
| **Bank name** | *leave blank until NAPRAS gives you theirs* |
| **Account name** | *leave blank* |
| **Account number** | *leave blank* |
| **Paystack payment link** | *leave blank* |
| Sender name | `NAPRAS 2027 LOC` |
| Reply-to email | LOC secretary's email, or blank |
| Daily cap | `85` |
| **Test mode** | **`YES`** |
| Test address | your own email |

> ⚠️ **Registration open = NO** means the page shows "Registration is closed".
> That's correct for now. You're deploying the machinery, not opening the doors.

### About the two payment methods

**Bank transfer** — works as soon as you paste an account number. The delegate
sees the account details and their unique reference.

**Online card payment** — appears automatically once you paste a Paystack link.
Until then the button simply doesn't show.

The Paystack account must be **NAPRAS's own**, not yours. They're a registered
body so they can open one, money settles directly to them, and you never touch
it. Ask them for the link when you pitch in November.

---

# PART D — Publish the engine as a web address
*5 minutes*

This is how the registration page talks to your sheet.

1. Go to the **Apps Script** tab
2. Top right: blue **Deploy** → **New deployment**
3. Click the **gear icon** beside "Select type" → choose **Web app**
4. Fill in:
   - **Description:** `NAPRAS 2027`
   - **Execute as:** `Me (confhub.ngr@gmail.com)`
   - **Who has access:** `Anyone` ← **must say Anyone**
5. **Deploy**, authorise again if asked
6. Copy the **Web app URL** — it ends in `/exec`

### D1 — Save it in two places

1. Paste it into the **API URL** row of the Settings tab
2. Keep it on your clipboard — Part E needs it

> 📌 **"Anyone" isn't a security hole.** The passcode still guards the delegate
> list. "Anyone" just means delegates don't have to log into Google to register.

> ⚠️ **Any time you change the engine code afterwards:**
> Deploy → Manage deployments → pencil icon → Version: **New version** → Deploy.
> Saving alone does nothing. This is the single most common frustration.

---

# PART E — Publish the registration page
*15 minutes*

### E1 — Edit the one line

1. Open `register.html` in Notepad or TextEdit
2. Near the top find:
   ```
   const API_URL = "PASTE_YOUR_EXEC_URL_HERE";
   ```
3. Replace the placeholder with your `/exec` URL, keeping the quotes:
   ```
   const API_URL = "https://script.google.com/macros/s/AKfy.../exec";
   ```
4. Save

### E2 — New repository

1. **github.com** → **+** top right → **New repository**
2. Name: `napras2027`
3. **Public** → tick **Add a README file** → **Create repository**
4. **Add file → Upload files**
5. Drag in **`register.html`** and **`pass.html`**
6. **Commit changes**

> `pass.html` goes in now so it's ready later. It needs no editing at all.

### E3 — Cloudflare Pages

1. **dash.cloudflare.com** → **Workers & Pages** (left sidebar)
2. **Create** → **Pages** → **Connect to Git**
3. Choose the `napras2027` repository
4. Leave **every build setting empty** — there's nothing to build
5. **Save and Deploy**

### E4 — The subdomain

1. When the deploy finishes → **Custom domains** → **Set up a custom domain**
2. Enter: `napras2027.confhub.ng`
3. Confirm

### E5 — Check it

Open **https://napras2027.confhub.ng/register.html**

You should see: **"Registration is closed"** with the NAPRAS event name.

✅ **That is success.** It proves the page reached your sheet and read the
settings. Registration is closed because you set it to NO.

❌ **"Could not load registration"** → the API URL is wrong, or the deployment
isn't set to "Anyone". Recheck Part D.

❌ **404 Not Found** → Cloudflare hasn't finished. Wait two minutes.

---

# PART F — Test it properly
*15 minutes. Don't skip.*

### F1 — Open the doors temporarily

Settings tab → **Registration open** → change to `YES`

### F2 — Add fake payment details

So you can see the full confirmation screen:

| Setting | Value |
|---|---|
| Bank name | `Test Bank` |
| Account name | `TEST — NAPRAS` |
| Account number | `0001234567` |

Leave the Paystack link blank for now.

### F3 — Register yourself

1. Reload the registration page
2. Fill it in with **your own name and email**
3. Pick a category, tick an extra
4. Watch the total update as you select
5. Click **Complete registration**

**You should see:** a green tick, a payment reference like `NAP27-4K7QX`, the
amount due, and the bank details with a warning to use the reference.

**Check your email** — the same details should arrive.

**Check the Delegates tab** — a new row with your details, `paid` = `NO`,
`amount_due` filled in.

### F4 — Test the guards

- Register again with the **same email** → refused, tells you your existing reference ✅
- Try to register with **no category chosen** → refused ✅

### F5 — Confirm the payment

1. In the **Delegates** tab, click anywhere on your row
2. **ConfHub → Confirm payment for selected rows**
3. `paid` becomes `YES`, `paid_at` is stamped

### F6 — Send the pass

1. **ConfHub → Send passes & receipts**
2. Check your email — pass link + receipt
3. Open the link on your phone → your delegate pass with a QR code

✅ **If that whole chain worked, the system is live and correct.**

### F7 — Reset before you leave it

1. Delete your test row from the Delegates tab
2. Settings → **Registration open** → back to `NO`
3. Clear the fake bank details
4. Leave **Test mode = YES**

---

# When NAPRAS says yes

Four things to change — no rebuilding:

1. **Fees tab** — their agreed prices
2. **Settings** — their real bank details
3. **Settings** — their Paystack link (online button appears automatically)
4. **Settings** — `Registration open` = `YES`, `Test mode` = `NO`

Then send them the link. That's the whole go-live.

---

# For the next association

Same two files, unchanged:

1. New private spreadsheet → paste `confhub-engine.gs` → Set up sheets
2. Fill their settings, fees and extras
3. Deploy → New deployment → copy the `/exec` URL
4. New repo with `register.html` (API URL edited) + `pass.html`
5. Cloudflare Pages → `theirevent.confhub.ng`

**About 30 minutes.** No code written, ever.

---

# If something goes wrong

| What you see | Fix |
|---|---|
| No **ConfHub** menu | Close the spreadsheet tab and reopen it |
| "Google hasn't verified this app" | Advanced → Go to ConfHub Engine → Allow |
| "Could not load registration" | API URL wrong, or access isn't "Anyone" |
| "Not configured yet" | You didn't replace `PASTE_YOUR_EXEC_URL_HERE` |
| Page shows "Registration is closed" | Correct if you set it to NO |
| Prices show as 0 | Fees tab has commas or ₦ — use plain numbers |
| Online payment button missing | No Paystack link in settings. Expected for now |
| Changed code, nothing happened | Deploy → Manage deployments → pencil → New version |
| No confirmation email | Test mode is YES — check the test address inbox |

---

# The three rules

1. **The delegate sheet stays private.**
2. **Test mode stays YES until you've registered yourself and seen it work.**
3. **Registration open stays NO until NAPRAS has agreed fees and given you their account.**

---

*ConfHub — confhub.ng*
