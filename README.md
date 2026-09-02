# COL Strategy — Website

A static 4-page site (Home, Services, About, Contact) for **colstrategy.com**, built as plain HTML/CSS/JS so it can be hosted directly on GitHub Pages — no build step required.

## What's here

```
index.html          Home
services.html        Services
about.html            About
contact.html          Contact
css/styles.css        All styling (colors, type, layout)
js/main.js             Mobile nav toggle
assets/
  cairn-mark.png        Your logo mark, background removed (from Confluence_Logo_Cairn.ai)
  favicon.ico            Browser tab icon
  apple-touch-icon.png   iOS home-screen icon
CNAME                   Tells GitHub Pages this site serves colstrategy.com
```

## Before you publish — three things to fill in

1. **Schedule button** (`contact.html`) — currently a placeholder. Search for `schedule-link` in `contact.html` and replace `href="#"` with your real scheduling link (Calendly, etc.), then delete the placeholder JS block right below it (the `schedule-link` click handler) since it's only there to explain the placeholder.
2. **Contact form** (`contact.html`) — right now "Send" opens the visitor's email client with a pre-filled message to `ryan@colstrategy.com` (works with no backend, since GitHub Pages can't run server code). If you'd rather have submissions land silently in your inbox or a spreadsheet, sign up free at [formspree.io](https://formspree.io), and replace the `<form id="contact-form">` tag with:
   ```html
   <form id="contact-form" action="https://formspree.io/f/YOUR_FORM_ID" method="POST">
   ```
   then delete the JS `submit` handler in the `<script>` at the bottom of the page.
3. **LinkedIn / email** — currently pulled from your old site (`ryan@colstrategy.com`, `linkedin.com/in/wryanberger`). Update anywhere in the HTML if either has changed.

## Publish to GitHub Pages

1. Create a new **public** repo on GitHub (e.g. `col-strategy-site`).
2. Push this folder to it:
   ```bash
   cd col-strategy-site
   git init
   git add .
   git commit -m "Initial COL Strategy site"
   git branch -M main
   git remote add origin https://github.com/YOUR_USERNAME/col-strategy-site.git
   git push -u origin main
   ```
3. On GitHub: **Settings → Pages** → under "Build and deployment," set **Source** to `Deploy from a branch`, branch `main`, folder `/ (root)`. Save.
4. Still on that page, under **Custom domain**, enter `colstrategy.com` and save (GitHub will re-detect the `CNAME` file already in the repo). Wait for DNS to verify, then check **Enforce HTTPS** once it's available.

## Point your domain at it (keeping Google Workspace email)

Your site's DNS and your email's DNS are separate records — pointing the site to GitHub Pages will **not** break Workspace email, as long as you leave the `MX` records alone. Whoever manages your domain's DNS (Google Domains/Squarespace, or wherever `colstrategy.com` is registered) needs these records:

| Type | Host | Value | Notes |
|---|---|---|---|
| A | @ | 185.199.108.153 | GitHub Pages |
| A | @ | 185.199.109.153 | GitHub Pages |
| A | @ | 185.199.110.153 | GitHub Pages |
| A | @ | 185.199.111.153 | GitHub Pages |
| CNAME | www | YOUR_USERNAME.github.io | so www.colstrategy.com works too |
| MX / TXT (existing) | — | — | **leave your current Google Workspace mail records exactly as they are** |

DNS changes can take anywhere from a few minutes to ~24 hours to propagate. Until "Enforce HTTPS" is available in GitHub's Pages settings, that's GitHub still issuing the certificate — normal, just wait.

## Previewing locally

Any static server works, e.g. from the project folder:
```bash
python3 -m http.server 8000
```
then open `http://localhost:8000`.

## Editing content

Everything is plain HTML — open any `.html` file and edit the text directly. Shared design lives in `css/styles.css` (colors are CSS variables at the top of the file under `:root`, so a palette tweak is a one-line change). The header/nav and footer are repeated at the top/bottom of each page since there's no templating layer — if you rename a page or add a new one, update the `<nav class="main-nav">` block and footer links on **all four** pages to match.
