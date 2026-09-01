# autoengineer — idea archive

A static site on GitHub Pages. It records how the idea developed and holds the current
single source of truth we build the startup from.

## Layout

```
index.html          the archive home and the list of pages — edit this when you add one
assets/site.css     the only stylesheet, shared by every page
pages/              one HTML file per page
pages/_template.html  starting point, with every available component shown
```

No build step, no dependencies. Commit HTML to `main`, Pages publishes it.

## Testing before you push

Serve the folder locally and click through it:

```
python3 -m http.server 8000
# open http://localhost:8000, check the pages you touched, then Ctrl+C
```

Check for broken relative links (missing pages, wrong `../` depth):

```
python3 - <<'PY'
import re, os
for root, _, fs in os.walk('.'):
    if '.git' in root: continue
    for f in fs:
        if not f.endswith('.html'): continue
        p = os.path.join(root, f)
        for href in re.findall(r'href="([^"]+)"', open(p).read()):
            if href.startswith(('http', '#', 'mailto')): continue
            t = href.split('#')[0]
            if not t or t.endswith('/'): continue
            if not os.path.exists(os.path.normpath(os.path.join(root, t))):
                print('MISSING', p, '->', href)
PY
```

Both should be run any time you add or move a page.

## Adding a page

1. `cp pages/_template.html pages/your-topic.html` — lowercase, hyphens, no date in the filename.
2. Fill in the masthead and write the body. Reuse the classes in the template so pages look alike.
3. Link the pages you build on with a plain relative link, e.g. `<a href="other-page.html">`.
   Pages sit in the same folder, so no `../pages/` needed between them.
4. List those same pages in the `Related pages` block at the bottom of your page.
5. Add one entry at the top of the list in `index.html`, with date, author, status and a
   `Builds on` link to whatever it follows from.
6. Commit.

## Conventions

- One page, one question.
- Date it and sign it. Dates in the page never change.
- Do not rewrite an older page you disagree with. Write a new one, link back to it, and mark
  the old entry `Superseded` in `index.html`.
- `Current` means we act on it. Keep the number of `Current` pages small.
- Say what would prove the page wrong.
