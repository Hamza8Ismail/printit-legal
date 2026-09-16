# printit-legal

The three public pages Google Play requires before it will accept the PrintIt submission,
plus an index page that links them.

| File | What it is |
|---|---|
| `index.html` | Landing page linking the other three |
| `privacy.html` | Privacy policy — **the URL Play asks for** |
| `terms.html` | Terms of service |
| `delete-account.html` | Account deletion — **the other URL Play asks for** |
| `style.css` | Shared styling. No fonts, scripts, trackers or cookies |

These pages load nothing from anywhere except `style.css`, which sits next to them. No
analytics, no cookie banner, no CDN. A page whose job is "here is what we do with your
data" should not itself be collecting any.

---

## Status: two placeholders left

Five of the seven placeholders were filled on 15 September 2026:

| Placeholder | Filled with |
|---|---|
| `[LEGAL ENTITY NAME]` | **PrintIt** — the product name, deliberately, until the سجل تجاري comes through |
| `[PUBLICATION DATE]` | **15 September 2026** |
| `[RESPONSE DAYS]` | **30** (calendar days) |
| `[MINIMUM AGE]` | **13** |
| `[GOVERNING LAW AND COURTS]` | **Egyptian law, courts of Cairo** (`terms.html`, section 14) |

**Two are still open, and both are waiting on the same thing — the domain.**

### `[SUPPORT EMAIL]` — 14 lines, 24 occurrences

| File | Lines |
|---|---|
| `index.html` | 9 *(comment)*, 56 |
| `privacy.html` | 10 *(comment)*, 266, 282, 291, 302 |
| `terms.html` | 6 *(comment)*, 147, 201 |
| `delete-account.html` | 22 *(comment)*, 63, 174, 181 |

### `[REGISTERED ADDRESS]` — 6 lines, 6 occurrences

| File | Lines |
|---|---|
| `privacy.html` | 10 *(comment)*, 43, 304 |
| `terms.html` | 6 *(comment)*, 43, 203 |

Lines marked *(comment)* are inside the HTML comment at the top of each file. They are
notes to you and nobody sees them, but replace them too so the checklist stays honest.

Each `[SUPPORT EMAIL]` in the body appears **twice on the same line** — once inside
`href="mailto:..."` and once as the visible link text. Replace both, or the link will point
somewhere different from what it reads.

### The one command that finds what is left

Run this from anywhere, including `C:\Windows\System32`:

```powershell
Select-String -Path 'E:\Hamza\printit-legal\*.html' -Pattern '\[[A-Z][A-Z /]*\]' -CaseSensitive
```

Right now it prints 18 lines. When it prints nothing, the pages are ready to publish.

> **Do not drop `-CaseSensitive`.** Without it PowerShell matches lowercase too, and the
> command starts reporting `[erased]` in `delete-account.html` as if it were a placeholder.
> It is not — see below.

> **`[erased]` in `delete-account.html` is not a placeholder.** It is the literal text the
> database writes over a deleted customer's address. Leave it exactly as it is.

---

## Changing the company name later

The legal entity is named **once per page**, and nowhere else. Every other mention says
"PrintIt" as the *product*, which is not affected by the commercial registration.

That matters more now than it did before, because the entity name is currently also the
word "PrintIt". Searching for `PrintIt` will match hundreds of harmless product mentions.
Do not do that. Find exactly the four lines that name the *entity* with:

```powershell
Select-String -Path 'E:\Hamza\printit-legal\*.html' -Pattern 'is operated by'
```

That returns exactly four lines — one per HTML file — and they are the whole edit:

| File | Line |
|---|---|
| `index.html` | 62 |
| `privacy.html` | 43 |
| `terms.html` | 43 |
| `delete-account.html` | 180 |

When the سجل تجاري comes through, change the text inside `<strong>...</strong>` on those
four lines and nothing else.

---

## Publishing to GitHub Pages

### 1. Git repository — already done

This folder is already a git repository with one commit on `main`. Nothing to do.

To see it:

```powershell
git -C E:\Hamza\printit-legal log --oneline
```

There is **no remote**. Nothing has been pushed anywhere. Everything below is yours to run
when you decide to.

### 2. Decide whether this goes public

GitHub Pages does not serve a private repository on a free account. To get a working
`github.io` URL the repo has to be **public**, which means the files *and the entire commit
history* become readable by anyone.

Google's reviewer also opens these URLs while signed out, so a private repo would fail
review regardless.

**This is not easily undone.** You can flip a repo back to private later, but anything that
was public may already have been cloned, cached or indexed. Decide before you push, not
after.

### 3. Create the repository and push

Your `gh` CLI is authenticated as **Hamza8Ismail** with `repo` scope. One command:

```powershell
gh repo create printit-legal --public --source E:\Hamza\printit-legal --remote origin --push
```

Or, if you would rather create it by hand at <https://github.com/new> (empty, public, named
`printit-legal`, no README and no `.gitignore`), then run these two, in order:

```powershell
git -C E:\Hamza\printit-legal remote add origin https://github.com/Hamza8Ismail/printit-legal.git
```

```powershell
git -C E:\Hamza\printit-legal push -u origin main
```

### 4. Switch Pages on

1. Go to `https://github.com/Hamza8Ismail/printit-legal/settings/pages`
2. Under **Build and deployment**, set **Source** to **Deploy from a branch**
3. Set the branch to **`main`** and the folder to **`/ (root)`**
4. Click **Save**

The first build takes a minute or two. The Pages settings page shows a green banner with
the live URL when it is done.

### 5. Your URLs

GitHub Pages lowercases the account name, so the URLs are:

```
https://hamza8ismail.github.io/printit-legal/
https://hamza8ismail.github.io/printit-legal/privacy.html
https://hamza8ismail.github.io/printit-legal/terms.html
https://hamza8ismail.github.io/printit-legal/delete-account.html
```

Open each one in a private browsing window before you submit anything. If a page loads but
is unstyled, `style.css` did not get committed.

---

## Where these URLs go in Play Console

| Play Console field | URL |
|---|---|
| **App content → Privacy policy** | `.../privacy.html` |
| **App content → Data safety → Data deletion** → *"provide a URL"* | `.../delete-account.html` |
| **Store listing → Terms of service** (optional) | `.../terms.html` |

Play requires **two** deletion routes: this public page **and** the in-app one
(*You → Delete account*). Both already exist — do not remove the in-app route.

---

## Updating a page later

Edit the file, then run these three, in order. Each works from anywhere:

```powershell
git -C E:\Hamza\printit-legal add -A
```

```powershell
git -C E:\Hamza\printit-legal commit -m "Update privacy policy"
```

```powershell
git -C E:\Hamza\printit-legal push
```

Pages rebuilds within about a minute. (The third command only works once you have done
step 3 above and a remote exists.)

---

## Contact details on the pages

The support WhatsApp number **+20 103 119 2395** is published on all four pages — under
*Questions* on the index, in the Contact section of the privacy policy and the terms, and
in the footer of the deletion page. It is the same number the app already ships in
`printit-app/lib/services/support.dart`, so every customer who has the APK already has it.

It is written as plain text, not as a `wa.me` link, so these pages still load nothing from
anywhere except `style.css`.

The WhatsApp number does **not** replace `[SUPPORT EMAIL]`. Both belong on the pages; one
is filled and one is still waiting on the domain.

---

## Keeping these pages true

They describe real behaviour in `printit-backend`, not a generic template. These claims
break if the backend changes:

| Claim on the pages | What it depends on |
|---|---|
| Files deleted 30 days after an order finishes | `retention_config.keep_files_days`, and the daily `purge-files` GitHub Action |
| Three hours to change your mind | `account_deletion_grace()` in migration `0062` |
| Signing in cancels a pending deletion | the `on_auth_user_signed_in` trigger |
| Four reasons deletion can be refused | `account_deletion_blockers()` |
| What survives an erasure | `erase_account()` |
| A shop sees no files or identity before accepting | `offered_job_summary` (`0022`) and the storage policies (`0018`) |
| We reply within 30 days | nothing in code — this is a promise you are making by hand |

If you change any of those, change the page in the same week.

---

## Rule 1

Nothing on these pages tells a customer which shop printed their order, or hints at a way
to find out. `terms.html` states the opposite outright, and the privacy policy describes
shop access only in the direction of *what the shop learns about the customer*. Keep it
that way when editing.
