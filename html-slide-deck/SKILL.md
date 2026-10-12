---
name: "html-slide-deck"
description: "Build a polished, animated, single-file HTML slide deck (keyboard/click navigation, fragment reveals, inline SVG diagrams with curved connectors, fullscreen, jump menu). Use when the user wants a presentation/deck/slides as an interactive HTML file rather than PPTX — pair with a brand skill (e.g. mulesoft-brand) for colors and typography."
---

# HTML Slide Deck — build guide

Produce a **single self-contained `.html` file**: a fullscreen, keyboard- and click-navigable slide deck with step-by-step fragment reveals, inline-SVG diagrams with curved connectors, a jump menu, and a fullscreen toggle. Pair this skill with a brand skill (e.g. `mulesoft-brand`) for the color palette and typography — this skill owns *structure, motion, diagrams, and the lessons that make it look professional*.

Everything is inline (CSS, JS, base64 images). No external scripts, fonts, or CSS — they are blocked in sandboxed previews and break offline. The file must work by double-clicking it in a browser.

## Workflow (follow in order)

1. **Load the brand skill first** (colors, fonts, logo rules). Define its palette as CSS variables in `:root`.
2. **Agree the plan before building.** Decks go through many revisions — settle the narrative arc, slide count, and tone up front with `AskUserQuestion`. For a sales/technical audience, a strong opening hook beats a feature list.
3. **Build the shell once** (CSS + nav engine), then add slides one at a time with `Edit`. Large base64 images bloat the file and make full-file reads fail — insert images via a short placeholder token (e.g. `LOGO_B64`) and substitute the real data-URI with a tiny Python script in bash. For repeated structural edits on a big file, prefer a Python regex script over many `Edit` calls.
4. **Verify structurally after every batch** (see Verification). You usually cannot screenshot inside the sandbox, so lean on these checks.
5. **Deliver** with `present_files` and a `computer://` link.

## Core principles (hard-won)

- **One idea per slide; the slide anchors, the presenter carries.** Resist density. If a slide reads like a document, split it or cut text. ~20 words of visible text is a good ceiling for content slides.
- **Narrative hook over catalog.** Open with something concrete the audience recognizes ("behind every screen you use, data that doesn't come from magic"), *then* reveal the product. Carry one worked example (one product, one order) across multiple slides instead of abstract diagrams.
- **Insufflate key messages throughout, don't dump them at the end.** Put a strong takeaway line on the slide where it's earned (banner or headline), and keep a final recap slide that restates them.
- **Reveal progressively.** Build diagrams/lists step by step on click so the presenter controls tempo. Spaghetti-then-hub, layer-by-layer stacking, "add a channel without touching systems" — these land because they animate.
- **Diagrams: inline SVG, ≤6 elements, curved connectors.** No library. Connect boxes with smooth cubic Bézier paths (not arrows-everywhere) for a clean, pro look. Keep a legend only if truly needed — otherwise label inline.
- **Verify facts before asserting them in content.** When a claim about the product/tech goes on a slide, be sure it's correct (e.g. a pattern the product actually supports). Reword niche jargon for the audience.
- **Match requests precisely, but flag genuinely bad ideas.** If a user instruction will hurt the result (e.g. a double reading-level that serves no one), say so and propose better — don't silently execute.

## File skeleton

```html
<!DOCTYPE html><html lang="fr"><head><meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<style>
  :root{
    color-scheme: light;
    /* brand palette vars from the brand skill, e.g.: */
    --e15:#0C1A3F; --e30:#0B2D9B; --e50:#1B5CE6; --c60:#0D9DDA; --c80:#90D0FE; --c95:#EAF5FE; --c40:#05628A;
    --yellow:#F5C518; --green:#2E9E5B; --pink:#D43FAA; --violet:#6B3FA0; --red:#E0453E;
    /* fonts: name the official brand font first, then safe fallbacks */
    --head:'Avant Garde for Salesforce','Century Gothic',Futura,sans-serif;
    --body:'Salesforce Sans','Helvetica Neue',Arial,sans-serif;
    --mule-logo:url('LOGO_B64'); /* substitute real data-URI later */
  }
  *{margin:0;padding:0;box-sizing:border-box;}
  html,body{height:100%;}
  body{font-family:var(--body);background:var(--e15);color:var(--e15);overflow:hidden;}
  .deck{width:100vw;height:100vh;position:relative;}
  .slide{position:absolute;inset:0;display:none;flex-direction:column;padding:4.6vh 5.2vw;overflow:hidden;animation:fade .45s ease;}
  .slide.active{display:flex;}
  .slide.center{justify-content:center;align-items:center;text-align:center;}
  @keyframes fade{from{opacity:0;transform:translateY(12px);}to{opacity:1;transform:none;}}
  h1,h2,h3,h4{font-family:var(--head);font-weight:700;letter-spacing:-.01em;line-height:1.08;}
  .em{color:var(--e50);} .dark .em,.dark2 .em{color:var(--c80);}
  .light{background:var(--c95);} .white{background:#fff;}
  .dark{background:linear-gradient(135deg,#1B5CE6 0%,#0B2D9B 55%,#0C1A3F 100%);color:#fff;}
  .dark2{background:radial-gradient(circle at 80% 10%,#13337a 0%,#0B2D9B 42%,#0C1A3F 100%);color:#fff;}
  .kicker{font-family:var(--head);font-weight:700;font-size:1.05vw;letter-spacing:.15em;text-transform:uppercase;color:var(--e50);margin-bottom:1.1vh;}
  .dark .kicker,.dark2 .kicker{color:var(--c80);}
  .hb{margin-bottom:2.2vh;} .hb h2{font-size:2.6vw;color:var(--e15);} .dark .hb h2,.dark2 .hb h2{color:#fff;}
  .hb .sub{font-size:1.15vw;color:var(--c40);margin-top:.7vh;font-weight:400;font-family:var(--head);} .dark .hb .sub,.dark2 .hb .sub{color:var(--c80);}
  /* fragments (progressive reveal) */
  .frag{opacity:0;transform:translateY(14px);transition:opacity .5s ease,transform .5s ease;}
  .frag.in{opacity:1;transform:none;}
  .frag.pop{transform:scale(.9);} .frag.pop.in{transform:scale(1);}
  /* takeaway banner */
  .takeaway{margin-top:1.6vh;background:linear-gradient(135deg,#0C1A3F,#13337a);border-left:5px solid var(--yellow);border-radius:10px;padding:.9vw 1.4vw;display:flex;align-items:center;gap:.9vw;}
  .takeaway .tx{color:#fff;font-size:1.02vw;font-family:var(--head);font-weight:700;} .takeaway .tx b{color:var(--yellow);}
  /* nav + chrome */
  .progress{position:fixed;top:0;left:0;height:4px;background:var(--e50);z-index:60;transition:width .35s ease;}
  .nav{position:fixed;bottom:2.1vh;left:50%;transform:translateX(-50%);display:flex;align-items:center;gap:14px;z-index:50;background:rgba(12,26,63,.72);backdrop-filter:blur(8px);padding:8px 16px;border-radius:30px;}
  .nav button{background:none;border:none;color:#fff;font-size:19px;cursor:pointer;width:30px;height:30px;border-radius:50%;display:flex;align-items:center;justify-content:center;}
  .nav button:hover{background:rgba(255,255,255,.18);}
  .nav .count{color:var(--c80);font-family:var(--head);font-size:13px;min-width:52px;text-align:center;}
  .menubtn{position:fixed;bottom:2.1vh;left:2vw;z-index:55;width:38px;height:38px;border-radius:50%;background:rgba(12,26,63,.72);backdrop-filter:blur(8px);color:#fff;border:none;cursor:pointer;font-size:16px;}
  .logo{position:absolute;top:3vh;right:3.8vw;display:flex;align-items:center;gap:9px;z-index:10;}
  .logo .mark{width:30px;height:30px;background-image:var(--mule-logo);background-size:contain;background-repeat:no-repeat;background-position:center;}
  .logo .txt{font-family:var(--head);font-weight:700;font-size:14px;color:var(--e30);} .logo.inv .txt{color:#fff;}
  .hint{position:absolute;bottom:2.3vh;right:3.8vw;font-size:.75vw;color:var(--c40);font-family:var(--head);opacity:.7;z-index:10;}
  .dark .hint,.dark2 .hint{color:var(--c80);}
  footer.f{position:absolute;bottom:1.5vh;left:5.2vw;font-size:.78vw;color:var(--c40);font-family:var(--head);}
  .dark footer.f,.dark2 footer.f{color:var(--c80);} .center footer.f{left:50%;transform:translateX(-50%);}
  /* jump menu */
  .menu-ov{position:fixed;inset:0;z-index:70;background:rgba(12,26,63,.55);backdrop-filter:blur(5px);display:none;align-items:center;justify-content:center;}
  .menu-ov.open{display:flex;}
  .menu-card{background:#fff;border-radius:16px;box-shadow:0 30px 70px rgba(11,45,155,.3);padding:1.4vw 1.2vw;width:min(42vw,560px);max-height:82vh;overflow:auto;}
  .menu-item{display:flex;align-items:center;gap:.9vw;padding:.7vw .9vw;border-radius:10px;cursor:pointer;}
  .menu-item:hover{background:var(--c95);} .menu-item .mnum{width:28px;height:28px;border-radius:8px;background:var(--c95);color:var(--e50);font-family:var(--head);font-weight:700;display:flex;align-items:center;justify-content:center;}
  .menu-item.cur .mnum{background:var(--e50);color:#fff;} .menu-item .mttl{font-family:var(--head);font-weight:700;color:var(--e15);} .menu-item .mttl small{display:block;font-weight:400;font-size:.78vw;color:var(--c40);}
  @media(max-width:900px){ .hb h2{font-size:5vw;} }
</style></head><body>
<div class="progress" id="progress"></div>
<div class="deck" id="deck">
  <section class="slide active dark center" data-frags="0"> ... </section>
  <section class="slide light" data-frags="3" data-anim="myhook"> ... fragments with data-f="1..3" ... </section>
  <!-- more slides -->
</div>
<button class="menubtn" id="menuBtn" aria-label="Sommaire">☰</button>
<button class="menubtn" id="fsBtn" aria-label="Plein écran" style="left:calc(2vw + 46px);">⛶</button>
<div class="menu-ov" id="menuOv"><div class="menu-card" id="menuCard"><h3>Aller à…</h3></div></div>
<div class="nav"><button id="prev">‹</button><span class="count" id="count">1 / N</span><button id="next">›</button></div>
<script>/* engine — see below */</script>
</body></html>
```

## Navigation + fragment engine (drop-in)

Each `<section class="slide" data-frags="K">` declares how many click-steps it has. Elements with `class="frag" data-f="n"` appear once `step>=n`. `data-hideafter="m"` hides an element once `step>=m` (e.g. an attack arrow that passes before a shield blocks it). `data-anim="name"` triggers a per-slide JS hook. **Always keep `data-frags` equal to the max `data-f` on that slide** (or 0), unless the slide is fully JS-driven.

```js
const slides=[...document.querySelectorAll('.slide')];
let i=0, step=0;
const count=document.getElementById('count'), progress=document.getElementById('progress');
const fragCount=s=>parseInt(s.dataset.frags||'0',10);

// jump menu titles — one [title, subtitle] per slide, in order
const slideTitles=[ ['Intro','hook'], /* ... */ ];
const menuOv=document.getElementById('menuOv'), menuCard=document.getElementById('menuCard');
function buildMenu(){ menuCard.querySelectorAll('.menu-item').forEach(e=>e.remove());
  slides.forEach((s,k)=>{ const t=slideTitles[k]||['Slide '+(k+1),'']; const el=document.createElement('div');
    el.className='menu-item'+(k===i?' cur':''); el.innerHTML='<div class="mnum">'+(k+1)+'</div><div class="mttl">'+t[0]+'<small>'+(t[1]||'')+'</small></div>';
    el.addEventListener('click',()=>{closeMenu();enter(k,false);}); menuCard.appendChild(el); }); }
function openMenu(){buildMenu();menuOv.classList.add('open');}
function closeMenu(){menuOv.classList.remove('open');}
document.getElementById('menuBtn').addEventListener('click',openMenu);
menuOv.addEventListener('click',e=>{if(e.target===menuOv)closeMenu();});

function toggleFs(){ const el=document.documentElement;
  if(!document.fullscreenElement){(el.requestFullscreen||el.webkitRequestFullscreen||el.msRequestFullscreen).call(el);}
  else{(document.exitFullscreen||document.webkitExitFullscreen||document.msExitFullscreen).call(document);} }
document.getElementById('fsBtn').addEventListener('click',toggleFs);

function applyFrags(){ const s=slides[i];
  s.querySelectorAll('.frag').forEach(f=>{ const fn=parseInt(f.dataset.f||'0',10); let on=fn<=step;
    const h=f.dataset.hideafter?parseInt(f.dataset.hideafter,10):null; if(h!==null&&step>=h)on=false; f.classList.toggle('in',on); });
  const a=s.dataset.anim; if(a&&hooks[a])hooks[a](s,step); }        // per-slide hooks
const hooks={ /* myhook:(s,step)=>{...}  — custom animated slides register here */ };

function render(){ slides.forEach((s,k)=>s.classList.toggle('active',k===i));
  if(slides[i].dataset.anim&&binds[slides[i].dataset.anim])binds[slides[i].dataset.anim]();  // optional one-time bind (e.g. clickable zones)
  applyFrags(); count.textContent=(i+1)+' / '+slides.length; progress.style.width=((i+1)/slides.length*100)+'%'; }
const binds={};
function enter(idx,atEnd){ i=Math.max(0,Math.min(slides.length-1,idx)); step=atEnd?fragCount(slides[i]):0; render(); }
function next(){ if(step<fragCount(slides[i])){step++;applyFrags();return;} if(i<slides.length-1)enter(i+1,false); }
function prev(){ if(step>0){step--;applyFrags();return;} if(i>0)enter(i-1,true); }
document.getElementById('next').onclick=next; document.getElementById('prev').onclick=prev;
document.addEventListener('keydown',e=>{
  if(e.key==='Escape'){closeMenu();return;}
  if(e.key==='m'||e.key==='M'){menuOv.classList.contains('open')?closeMenu():openMenu();return;}
  if(e.key==='f'||e.key==='F'){toggleFs();return;}
  if(menuOv.classList.contains('open'))return;
  if(e.key==='ArrowRight'||e.key==='PageDown'||e.key===' '){e.preventDefault();next();}
  if(e.key==='ArrowLeft'||e.key==='PageUp'){e.preventDefault();prev();}
  if(e.key==='Home')enter(0,false); if(e.key==='End')enter(slides.length-1,true); });
let sx=0; document.addEventListener('touchstart',e=>sx=e.touches[0].clientX);
document.addEventListener('touchend',e=>{const d=e.changedTouches[0].clientX-sx;if(Math.abs(d)>50){d<0?next():prev();}});
render();
```

Controls to tell the user: **← →** or Space navigate (through fragments then slides), **F** fullscreen, **M** jump menu, Esc closes menu/exits fullscreen.

## Diagram patterns (inline SVG)

- **Layered architecture (bottom→top or left→right):** colored bands per layer; boxes = small apps with a logo chip + 2-line label (name + sublabel) left-aligned after the chip so text never overflows. Give boxes generous width and **≥40px gaps** so they never touch. Horizontal layer labels colored per layer (avoid rotated vertical text — hard to read).
- **Curved connectors:** `<path d="M x1,y1 C cx,y1 cx,y2 x2,y2" fill="none" stroke="#90D0FE" stroke-width="2.2"/>`. Compute endpoints from the real box centers; when you move a box, recompute every path that touches it.
- **"Spaghetti vs hub":** reveal point-to-point links on click (N×(N-1)/2 grows), then collapse to a single hub. Great for "integration is hard" → "there's a better way".
- **Flowing data line (CSS):** `.wire{stroke-dasharray:3 5;}` + `@keyframes wireflow{to{stroke-dashoffset:-16;}}` with `animation:wireflow 2.4s linear infinite;` — thin (`stroke-width:~1.1`), low opacity, elegant.
- **"Add X without touching Y":** show the established architecture, then reveal the new piece highlighted (dashed accent border) connecting to the existing one, with a green "unchanged" badge on the untouched part.
- **Tool-count comparison:** reveal 3-4 third-party tools per lifecycle stage, a running total, then collapse to "1 platform". Chiffré and punchy.
- **Security scenario:** legit request passes; attacker requests pass while unprotected (`data-hideafter`), then a policy shield blocks them (✕), then a monitoring panel + an audit log that cascades line by line (`.logline` reveal via a hook).

## Interactivity patterns

- **Real clickable mini-UI (e.g. a product page):** build a styled browser chrome + product card in HTML; make info rows `cursor:pointer`; clicking a row reveals its backing system and draws a connector. Compute connector paths in page pixels and recompute on `resize`. Wire via a `binds[name]` one-time listener attach guarded by a `_bound` flag.
- **Embedded video:** YouTube embeds often fail (owner disables embedding → "Error 153") and never render in sandbox previews. Default to a **styled clickable thumbnail** (dark gradient, red play button, title) linking to the video in a new tab. Only use a true inline `<video>` if you have a local file to base64 — you cannot fetch remote media from the sandbox.

## Images / logos

- Convert uploads to PNG and base64-embed them (`<img src="data:image/png;base64,...">` or SVG `<image href="data:...">`). Trim/resize first to keep weight down (a logo shown at ~30px doesn't need 2000px source).
- For a logo on a colored background, put it on a white circle, or make a **white silhouette** version (preserve alpha, paint pixels white) for dark fills.
- Reuse one data-URI via a CSS variable (`--mule-logo`) for CSS backgrounds; for SVG `<image>` you must repeat the href — keep the source small since it repeats.
- To replace a huge inline image you can't match with `Edit`, grab the data-URI from the CSS var with a regex and substitute via Python in bash.

## Verification (run after each batch via bash + python)

- `<section>`/`</section>`, `<svg>`/`</svg>`, and `<div>`/`</div>` counts balance **globally and per slide** (walk a depth counter; it must end at 0 and never go negative).
- No undefined CSS vars: every `var(--x)` has a matching `--x:` declaration.
- Per slide, `max(data-f) == data-frags` (or the slide is JS-driven — then confirm the JS step count matches `data-frags`).
- The nav `count` label and `slideTitles.length` match the number of slides.
- No leftover placeholder tokens (e.g. `LOGO_B64`), and all referenced element IDs exist.
- Strip base64 data-URIs before parsing so huge strings don't skew counts or exceed read limits.

## Tone & delivery

Keep responses concise: a `computer://` link and a short "what changed" summary — the user opens the deck themselves. When several independent edits are needed, batch them. After delivering, it's natural to offer a PPTX export as a follow-up, but the HTML deck is the primary artifact.

