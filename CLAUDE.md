# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Javier Borquez's personal academic site, served by GitHub Pages from the `main` branch of
`JavierBorquez/JavierBorquez.github.io` at https://javierborquez.github.io/.

Static HTML/CSS/JS only — no build step, no package manager, no tests. To preview locally, open
[index.html](index.html) in a browser or run `python3 -m http.server` from the repo root (the
latter is needed for anything path- or hash-sensitive). Deployment is `git push` to `main`;
GitHub Pages publishes the repo root as-is.

## Structure and conventions

The site started from the BootstrapMade **iPortfolio** template (Bootstrap 5.3.3) but has since
been pared down to only what this site actually uses — the orphan template pages
(`portfolio-details.html`, `service-details.html`, `starter-page.html`), and every vendor library
and CSS/JS block the live page didn't reference (Swiper, Isotope, GLightbox, Typed.js, PureCounter,
Waypoints, imagesLoaded, the PHP contact form, the About/Stats/Skills/Services/Testimonials CSS
sections, redundant Bootstrap/AOS/bootstrap-icons build variants) are gone. Before adding a new
template feature back, check whether it's genuinely needed — this repo intentionally stays minimal.

- **[index.html](index.html) is the whole site.** It holds every section (`#hero`, `#resume`,
  `#portfolio` — relabeled "Research" — and `#contact`) as anchors within one page; the sidebar
  nav scroll-spies between them. Content edits are hand-edits to this file.
- **[assets/vendor/](assets/vendor/) is third-party and unmodified**: Bootstrap (CSS only — no JS
  components are used, so the JS bundle was removed), Bootstrap Icons, and AOS (scroll animations).
  Never edit vendor files directly; add a library by dropping a vendored copy in and wiring up a
  `<script>`/`<link>` in `index.html`. The one CDN dependency is academicons, used for the Google
  Scholar icon. When vendoring a library, keep only the build actually referenced (e.g. the single
  minified CSS/JS file) rather than the full multi-format download — that's the pattern already
  used for Bootstrap/AOS/bootstrap-icons here.
- **[assets/js/main.js](assets/js/main.js) started as the stock template bundle**, now trimmed to
  only what `index.html` uses: header toggle, mobile-nav-hide-on-click, preloader removal,
  scroll-top button, AOS init, hash-scroll correction, and navmenu scrollspy. It is not defensive
  — `headerToggleBtn.addEventListener` and `scrollTop.addEventListener` assume those elements
  exist — so if you remove `#header`/`.scroll-top`/etc. markup from `index.html`, update main.js
  in the same change or a later statement will throw and abort every initializer after it.
- **[assets/css/main.css](assets/css/main.css) is the compiled template CSS**, trimmed to the
  sections `index.html` actually uses (general/shared, header, nav, footer, preloader, scroll-top,
  sections/section-titles, hero, resume, portfolio). The Sass sources were pro-version-only and are
  gone, so edit the CSS directly. Theming runs through CSS custom properties declared at the top
  (`--accent-color`, `--background-color`, `--heading-color`, the `--nav-*` set); `.light-background`
  and `.dark-background` are preset classes that re-declare those vars for a section. Prefer
  changing a variable or applying a preset class over adding new rules.

## SEO / discoverability

- `index.html`'s `<head>` carries a filled-in `description`, Open Graph/Twitter card tags, a
  `canonical` link, and a `schema.org` `Person` JSON-LD block — keep these in sync whenever name,
  role, affiliation, or headshot changes (they currently say Postdoctoral Research Associate,
  Safe Robotics Lab, Princeton).
- [robots.txt](robots.txt) and [sitemap.xml](sitemap.xml) live at the repo root (required location
  for GitHub Pages). The sitemap lists just `index.html` since that's the entire site — add an
  entry only if a real additional page is ever added back.
- [llms.txt](llms.txt) is a plain-text summary (bio, publications, key links) aimed at LLM agents,
  following the emerging (non-standardized) `llms.txt` convention. Update it alongside the
  publications list and hero bio so it doesn't drift from the HTML.

## Things not to break

- Keep the "Designed by BootstrapMade" credit in the footer — the free template license requires it.
- [google1c04d7c38dc1047c.html](google1c04d7c38dc1047c.html) is a Google Search Console verification
  token. It must stay at the repo root with that exact filename and contents.
- The contact section deliberately just shows a plain email address — there is no working contact
  form (the template's version required the pro tier's mailer, so it was removed rather than kept
  as dead code).
- Publications in the `#portfolio` section are a hand-maintained `<ul>`; new entries follow the
  existing `Authors. <strong>Title.</strong><br><em>Venue, Year. [IEEE][arXiv][web]</em>` shape.
