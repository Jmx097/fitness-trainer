# My Gym Workout

A dead-simple workout tracker built for one person to use reliably on an iPhone at the gym. No accounts, no internet needed once installed, no clutter. She opens it, sees her exercises, taps each set as she finishes, adjusts the weight with big +/− buttons, and taps **Finish & Save**. Her past workouts are kept on the phone so she can see what weight she used last time.

It's a single web page (a PWA) — nothing to install from the App Store, nothing to log into.

## What it does

- Shows today's workout as big, readable cards with a plain-English description of each move.
- Tap a **Set** button to mark it done; the card turns green when all sets are complete.
- Set the **weight** with large +/− buttons (no tiny keyboard typing).
- **Finish & Save** stores the session on the phone. Next time, each exercise starts at the weight she used last time.
- **📖 button** shows past workouts.
- Works **fully offline** after the first open, and **installs to the home screen** like a real app.

## Put it on her iPhone (the easy way)

1. **Publish it** with GitHub Pages — see below. You'll get a link like `https://yourname.github.io/mom-gym/`.
2. Text her that link.
3. On her iPhone, she opens it in **Safari** (must be Safari, not Chrome).
4. She taps the **Share** button (the square with an arrow at the bottom).
5. She scrolls down and taps **Add to Home Screen**, then **Add**.
6. A "Gym" icon appears on her home screen. From now on she just taps that — it opens full-screen and works even with no signal.

## Publish to GitHub Pages

1. Create a new repository on GitHub (e.g. `mom-gym`). Make it **Public**.
2. Upload all the files in this folder, keeping the structure:
   ```
   index.html
   manifest.webmanifest
   sw.js
   icons/icon-192.png
   icons/icon-512.png
   icons/apple-touch-icon.png
   ```
   (Drag the whole `mom-gym` folder's contents into GitHub's "upload files" page, or use git.)
3. In the repo, go to **Settings → Pages**.
4. Under **Build and deployment → Source**, choose **Deploy from a branch**, pick branch **main** and folder **/ (root)**, then **Save**.
5. Wait a minute, refresh, and GitHub shows your live link at the top of the Pages settings. That's the link to send her.

### Using git instead
```bash
cd mom-gym
git init
git add .
git commit -m "My gym workout tracker"
git branch -M main
git remote add origin https://github.com/YOURNAME/mom-gym.git
git push -u origin main
```
Then enable Pages as in step 3–5 above.

## Changing the workout

The routine is knee-friendly and uses only machines, cables, and bodyweight — **no dumbbells, no leg press, no deep-knee or high-impact moves.** It also **rotates**: each slot (Warm Up, Lower Body, Chest & Arms, etc.) has a *pool* of exercises, and every time she taps Finish the app advances to a different one from each pool, so the workout is fresh each session. When an exercise comes back around, it remembers the weight she last used for it.

Open `index.html` and edit the `ROUTINE` block near the top of the `<script>`. Each slot looks like this:

```js
{ slot: "Chest & Arms", pool: [
  { id: "ch_press", name: "Seated chest press",
    desc: "Push the handles forward, then return slowly.",
    icon: "chest", start: 20 },
  { id: "ch_pecdeck", name: "Chest fly machine", desc: "...", icon: "chest", start: 20 }
]},
```

- Add or remove items in any `pool` to change what can show up. More items = more variety before anything repeats.
- `id` must be unique for each exercise (it's how weights are remembered).
- `name` / `desc` — what she reads. Keep descriptions plain and short.
- `start` — the starting weight the first time (afterward it remembers her last weight).
- `icon` — one of: `bike, walk, legs, chest, back, shoulder, core`.
- For Warm Up / Cool Down slots, the slot has `time: true` and items just show a single "Done" button (no weight).

You can also change these settings right above the workout list:

```js
const SETS_PER_EXERCISE = 3;   // sets of each strength move
const REPS_TARGET = 10;        // reps to aim for
const UNIT = "lb";             // "lb" or "kg"
const WEIGHT_STEP = 5;         // how much each +/- press changes the weight
```

After editing, also bump the version in `sw.js` (change `mom-gym-v1` to `mom-gym-v2`) so the new version downloads to her phone.

## Want real exercise photos instead of the simple icons?

The icons are plain line drawings on purpose — they always load and never break offline. If you'd rather show photos: add your image files to an `images/` folder, then in `index.html` swap the `<div class="thumb">...</div>` for an `<img>`. Keep the images small and bundled in the repo (don't link to other websites) so they still work without internet. Ask me and I'll wire it up for you.

## Notes

- Data lives only on her phone (in the browser's local s