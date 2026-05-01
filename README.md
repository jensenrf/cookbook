# The Kitchen — Personal Cookbook

A clean, fast, static cookbook built with Jekyll and hosted on GitHub Pages.

---

## Folder Structure

```
cookbook/
├── _config.yml          ← Site settings
├── _layouts/
│   ├── default.html     ← Base HTML shell
│   ├── recipe.html      ← Recipe page layout
│   └── page.html        ← Generic page layout
├── _recipes/            ← All your recipe files live here
│   ├── cacio-e-pepe.md
│   ├── brown-butter-cookies.md
│   └── roast-chicken.md
├── assets/
│   ├── css/
│   │   └── style.css    ← All styles
│   └── images/          ← Recipe photos go here
├── index.md             ← Home page (auto-generates recipe list)
└── about.md             ← About page
```

---

## Step 1 — Create the GitHub Repository

1. Go to [github.com/new](https://github.com/new)
2. Name it `cookbook` (or anything you like)
3. Set it to **Public** (required for free GitHub Pages)
4. **Do not** initialize with a README — you'll push your local files
5. Click **Create repository**

---

## Step 2 — Push Your Files

```bash
# In the cookbook/ folder:
git init
git add .
git commit -m "Initial cookbook"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/cookbook.git
git push -u origin main
```

---

## Step 3 — Enable GitHub Pages

1. In your repo, go to **Settings → Pages**
2. Under **Source**, select **Deploy from a branch**
3. Choose **main** branch and **/ (root)** folder
4. Click **Save**
5. Wait ~60 seconds, then visit: `https://YOUR_USERNAME.github.io/cookbook/`

> **Note:** GitHub Pages has Jekyll built-in. No Gemfile or local build step required — just push and it builds automatically.

---

## Step 4 — Add a New Recipe

1. Create a new file in `_recipes/` named with lowercase and hyphens:
   ```
   _recipes/lemon-tart.md
   ```

2. Copy this front matter template to the top of the file:
   ```yaml
   ---
   layout: recipe
   title: "Lemon Tart"
   description: "One-line description of the dish."
   category: Baking
   tags: [Dessert, French]
   prep_time: 30 min
   cook_time: 45 min
   servings: 8
   difficulty: Medium
   image: /assets/images/lemon-tart.jpg  ← optional

   ingredients:
     - 200g butter
     - 3 eggs
     - Add each ingredient as a list item
   ---
   ```

3. After the `---` closing line, write your method as a numbered Markdown list:
   ```markdown
   1. First step of the recipe.

   1. Second step. (Use `1.` for every step — Markdown auto-numbers them.)

   1. Third step.
   ```

4. (Optional) Add a photo:
   - Drop the image into `assets/images/`
   - Reference it as `/assets/images/your-photo.jpg` in the front matter

5. Commit and push:
   ```bash
   git add _recipes/lemon-tart.md assets/images/lemon-tart.jpg
   git commit -m "Add lemon tart recipe"
   git push
   ```

The recipe will appear on the home page automatically within ~60 seconds.

---

## Front Matter Reference

| Field | Required | Example |
|---|---|---|
| `layout` | Yes | `recipe` |
| `title` | Yes | `"Pasta Carbonara"` |
| `description` | No | One-sentence subtitle |
| `category` | No | `Pasta`, `Baking`, `Poultry` |
| `tags` | No | `[Italian, Quick, Vegetarian]` |
| `prep_time` | No | `15 min` |
| `cook_time` | No | `30 min` |
| `servings` | No | `4` |
| `difficulty` | No | `Easy`, `Medium`, `Hard` |
| `image` | No | `/assets/images/photo.jpg` |
| `ingredients` | Yes | YAML list of strings |

---

## Local Development (Optional)

To preview changes locally before pushing:

```bash
gem install bundler jekyll
jekyll serve
# → open http://localhost:4000
```
