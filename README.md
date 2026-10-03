# Castle Hills Business Alliance Directory

Static website (plain HTML/CSS/JS, no build step), deployed to HostGator shared hosting straight from
GitHub using cPanel's built-in **Git Version Control** feature — no SSH, no FTP credentials to manage.

## How deployment works

1. This repo is cloned once inside your HostGator account (outside `public_html`, so the `.git` folder
   is never publicly reachable).
2. Every time you click **Deploy HEAD Commit** in cPanel, it runs the `.cpanel.yml` file in this repo,
   which copies the live site files into `public_html`.
3. `.cpanel.yml` only copies the files the site actually needs (`index.html`, `privacy.html`, etc. and
   `.htaccess`) — it never copies `.git`, `.github`, `.cpanel.yml` itself, or `README.md` into the live site.

## One-time setup

### 1. Push this repo to GitHub

Easiest way, no terminal needed:
1. Go to github.com -> **New repository** -> name it (e.g. `castle-hills-directory`) -> **Public** -> Create.
   (Public is fine here — there's nothing secret in these files. If you'd rather keep it private, that's
   fine too, see the note at the bottom.)
2. On the new repo's page, click **uploading an existing file**, then drag in every file and folder from
   this package (including the dot-files: `.htaccess`, `.cpanel.yml`, `.gitignore`, and the `.github` folder).
   GitHub's uploader does show dot-files once you drag a whole unzipped folder in.
3. Commit the upload.

### 2. Clone it into cPanel

1. In cPanel, open **Git Version Control** -> **Create**.
2. Leave **Clone a Repository** turned on.
3. **Clone URL**: `https://github.com/<your-username>/castle-hills-directory.git`
4. **Repository Path**: change it to `repositories/castle-hills-directory` (so the full path becomes
   something like `/home1/escuela/repositories/castle-hills-directory` — keeps it separate from `public_html`).
5. **Repository Name**: anything you like, e.g. `Castle Hills Directory`.
6. Click **Create**.

### 3. Check `.cpanel.yml`'s deploy path matches your account

Open `.cpanel.yml` in this repo and confirm the `DEPLOYPATH` line matches your actual `public_html` path —
cPanel showed it to you as `/home1/escuela/public_html/` in the Repository Path field, so that's already
set correctly for your account. (If you ever move to a different HostGator account, update this line.)

### 4. First deploy

1. Back in **Git Version Control**, click **Manage** next to your new repo.
2. Open the **Pull or Deploy** tab.
3. Click **Deploy HEAD Commit**.

That copies the files into `public_html` for the first time. Your site is now live from the repo.

## Day-to-day usage (updating the site)

1. Edit a file on GitHub (click the pencil icon on any file to edit right in the browser, or upload a
   replacement file) and commit the change.
2. In cPanel -> **Git Version Control** -> **Manage** -> **Pull or Deploy** tab:
   - Click **Update from Remote** (pulls the new commit from GitHub).
   - Click **Deploy HEAD Commit** (copies the updated files into `public_html`).

Two clicks, no zip files, no File Manager uploads, full history on GitHub so you can always see what
changed and when.

## Fully automatic option (optional)

If you'd rather not click those two buttons after every change, there's also a GitHub Actions + FTP
workflow included (`.github/workflows/deploy.yml`) that pushes changes to `public_html` automatically
on every commit — no clicking required, but it does need an FTP username/password stored as GitHub
secrets. See the comments in that file, or ask me and I'll walk you through wiring it up instead of
(or alongside) the cPanel method above. You only need one of the two methods, not both.

## Private repo note

If you make the GitHub repo private instead of public, cPanel's clone step needs a way to authenticate.
The simplest way is a GitHub **personal access token**: use a Clone URL of the form
`https://<token>@github.com/<your-username>/castle-hills-directory.git` instead of the plain URL. Ask me
if you'd like help generating that token.
