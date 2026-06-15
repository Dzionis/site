# Writing posts

How blog posts work on this site. There is **no build step** — posts are plain
HTML files you copy from a template, and you deploy by copying the folder.

## Layout

```
site/
├─ index.html                      # home page; the Writing section lists posts
├─ assets/
│  └─ blog.css                     # shared styles for every post (the only CSS)
├─ writing/
│  ├─ _TEMPLATE.html               # copy this to start a new post
│  └─ smoothing-noisy-gps.html     # sample post
└─ WRITING.md                      # this file
```

## Add a post (3 steps)

**1. Create the page**

```sh
cp writing/_TEMPLATE.html writing/your-post-slug.html
```

The slug is the URL, so keep it lowercase-with-dashes and no spaces, e.g.
`watch-first-intervals.html`.

**2. Write it**

Open the new file and edit everything marked `EDIT:` — the `<title>` and meta
tags (used for search results and link previews), the `<h1>`, the date and
read-time, the tags, and the body inside `<div class="prose">`.

The body is just HTML. You have these building blocks (all styled already):

| You want…        | Write…                                                        |
|------------------|---------------------------------------------------------------|
| paragraph        | `<p>…</p>`                                                     |
| big opening line | `<p class="lede-text">…</p>`                                   |
| section / sub    | `<h2>…</h2>` / `<h3>…</h3>`                                    |
| bold / italic    | `<strong>…</strong>` / `<em>…</em>`                            |
| link             | `<a href="https://…">…</a>`                                   |
| inline code      | `<code>…</code>`                                              |
| code block       | `<pre><code>…</code></pre>` (escape `<`, `&` as `&lt;` `&amp;`)|
| list             | `<ul><li>…</li></ul>` or `<ol>…</ol>`                          |
| quote            | `<blockquote><p>…</p></blockquote>`                            |
| callout / aside  | `<div class="callout"><span class="clabel">Note</span><p>…</p></div>` |
| image            | `<figure><img src="../assets/img/x.png" alt="…"><figcaption>…</figcaption></figure>` |

Put any images under `assets/img/` and reference them as `../assets/img/…`.

**3. List it on the home page**

In `index.html`, find the `<!-- POST-LIST:START -->` … `<!-- POST-LIST:END -->`
block in the Writing section. Copy one `<a class="post">` entry to the **top**
(newest first), point `href` at your new file, and set the title, date, and dek:

```html
<a class="post" href="writing/your-post-slug.html">
  <span class="ptitle">Your post title</span>
  <span class="pmeta">Mon YYYY · N min</span>
  <span class="dek">One-line teaser shown under the title.</span>
</a>
```

That's it. Repeat for each post.

## Preview locally

Because posts link a shared stylesheet, open them through a local server (not
`file://`, which can block the CSS):

```sh
cd site
python3 -m http.server 8000
# then visit http://localhost:8000/writing/your-post-slug.html
```

## Deploy

No build. Copy the whole `site/` folder (`index.html`, `assets/`, `writing/`)
to GitHub Pages / Vercel / Netlify, or push it to your Pages repo.

## Notes & conventions

- **Drafts:** to tease a post before it has a page, add a `<a class="post">`
  entry with `href="#writing"` and `<span class="pmeta">Draft · YYYY</span>` —
  that's what the remaining items in the list are. Give it a real `href` when
  the page exists.
- **Styling:** all post styling lives in `assets/blog.css`. Change it once and
  every post updates. Its `:root` color/font tokens mirror `index.html` — if you
  ever retheme the site, update both (it's the only duplicated block).
- **Why a shared stylesheet** instead of inlining CSS in each post? So a new
  post is a small file and there's one place to evolve the design. If you ever
  want a single fully self-contained file (e.g. to hand someone one `.html`),
  paste the contents of `blog.css` into a `<style>` tag in that post.
- **Why no syntax highlighting?** It would need a JS library and a build/CDN
  dependency; the minimal look is deliberate. Add Prism/Shiki later if you want.
