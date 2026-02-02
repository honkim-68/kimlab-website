# Kim Lab Website

This is the website for the Kim Lab at Rosalind Franklin University, built with [Astro](https://astro.build/).

## Prerequisites

Before you begin, you need to install:

1. **Node.js** (version 18 or higher)
   - Download from: https://nodejs.org/
   - Choose the "LTS" (Long Term Support) version
   - Run the installer and follow the prompts

2. **Git** (if not already installed)
   - Download from: https://git-scm.com/downloads
   - Run the installer and follow the prompts

To verify installation, open Terminal and run:
```bash
node --version   # Should show v18.x.x or higher
npm --version    # Should show 9.x.x or higher
git --version    # Should show git version x.x.x
```

## Getting Started

### 1. Install Dependencies

Open Terminal, navigate to the project folder, and run:

```bash
cd kimlab-website
npm install
```

This downloads all the necessary packages (may take a minute).

### 2. Run the Development Server

```bash
npm run dev
```

This starts a local server. Open your browser and go to:
```
http://localhost:4321
```

You'll see your website! Any changes you make to the files will automatically refresh in the browser.

### 3. Stop the Server

Press `Ctrl + C` in the Terminal to stop the server.

## Editing Your Website

### File Structure

```
kimlab-website/
├── src/
│   ├── pages/           # Website pages
│   │   ├── index.astro      # Home page
│   │   ├── research.astro   # Research page
│   │   ├── people.astro     # People page
│   │   ├── publications.astro # Publications page
│   │   └── contact.astro    # Contact page
│   ├── components/      # Reusable components
│   │   ├── Header.astro     # Navigation header
│   │   └── Footer.astro     # Page footer
│   ├── layouts/         # Page layouts
│   │   └── Layout.astro     # Main layout template
│   └── styles/          # CSS styles
│       └── global.css       # Global styles
├── public/              # Static files (images, etc.)
├── astro.config.mjs     # Astro configuration
└── package.json         # Project dependencies
```

### Adding Images

1. Put your images in the `public/` folder
2. Reference them in your pages like: `/kimlab-website/your-image.jpg`

Example:
```html
<img src="/kimlab-website/lab-photo.jpg" alt="Lab photo">
```

### Editing Content

Open any `.astro` file in a text editor (like VS Code). The content is mostly HTML with some special Astro syntax at the top between `---` marks.

---

## Deploying to GitHub Pages

Follow these steps to publish your website online for free using GitHub Pages.

### Step 1: Create a GitHub Account

If you don't have one already:
1. Go to https://github.com/
2. Click "Sign up"
3. Follow the registration process

### Step 2: Create a New Repository

1. Log in to GitHub
2. Click the **+** icon in the top right corner
3. Select **"New repository"**
4. Fill in the details:
   - **Repository name**: `kimlab-website`
   - **Description**: "Kim Lab website" (optional)
   - **Visibility**: Select **Public**
   - Do NOT check "Add a README file" (we already have one)
5. Click **"Create repository"**

You'll see a page with setup instructions. Keep this page open.

### Step 3: Update Configuration for Your Username

1. Open `astro.config.mjs` in a text editor
2. Replace `YOUR_GITHUB_USERNAME` with your actual GitHub username:

```javascript
export default defineConfig({
  site: 'https://YOUR_GITHUB_USERNAME.github.io',
  base: '/kimlab-website',
});
```

For example, if your username is "hongkyunkim":
```javascript
export default defineConfig({
  site: 'https://hongkyunkim.github.io',
  base: '/kimlab-website',
});
```

3. Save the file

### Step 4: Create GitHub Actions Workflow

Create the deployment configuration file:

1. Create a new folder: `.github/workflows/` in your project
2. Create a file named `deploy.yml` inside it with this content:

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [main]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: "20"
          cache: npm

      - name: Install dependencies
        run: npm ci

      - name: Build Astro
        run: npm run build

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./dist

  deploy:
    needs: build
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

### Step 5: Push Your Code to GitHub

Open Terminal in your project folder and run these commands one by one:

```bash
# Initialize git repository
git init

# Add all files
git add .

# Create first commit
git commit -m "Initial commit: Kim Lab website"

# Rename branch to main (if needed)
git branch -M main

# Connect to GitHub (replace YOUR_USERNAME with your GitHub username)
git remote add origin https://github.com/YOUR_USERNAME/kimlab-website.git

# Push your code to GitHub
git push -u origin main
```

**Note**: If prompted, enter your GitHub username and password. For the password, you may need to use a Personal Access Token instead:
1. Go to GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic)
2. Click "Generate new token (classic)"
3. Give it a name, select "repo" scope
4. Click "Generate token"
5. Copy the token and use it as your password

### Step 6: Enable GitHub Pages

1. Go to your repository on GitHub
2. Click **Settings** (tab at the top)
3. Click **Pages** (in the left sidebar)
4. Under "Build and deployment":
   - **Source**: Select **"GitHub Actions"**
5. Wait for the deployment (you can watch progress in the "Actions" tab)

### Step 7: View Your Website

After deployment completes (usually 1-2 minutes), your website will be live at:

```
https://YOUR_USERNAME.github.io/kimlab-website/
```

---

## Making Updates

After your site is deployed, making updates is simple:

### 1. Edit Files Locally

Make changes to your `.astro` files using any text editor.

### 2. Test Changes

```bash
npm run dev
```

Check your changes at http://localhost:4321

### 3. Push Updates

```bash
git add .
git commit -m "Description of your changes"
git push
```

GitHub will automatically rebuild and deploy your site within 1-2 minutes.

---

## Customization Tips

### Changing Colors

Edit `src/styles/global.css` and modify the CSS variables at the top:

```css
:root {
  --primary-color: #1a365d;    /* Dark blue - main color */
  --secondary-color: #2c5282;  /* Medium blue */
  --accent-color: #3182ce;     /* Light blue - links, buttons */
}
```

### Adding Lab Members

Edit `src/pages/people.astro` and add entries to the `labMembers` array.

### Adding Publications

Edit `src/pages/publications.astro` and add entries to the `publications` array.

### Adding a Custom Domain (Optional)

If you want to use a custom domain (like `kimlab.org`):

1. Purchase a domain from a registrar (Namecheap, Google Domains, etc.)
2. Create a file called `CNAME` in the `public/` folder with just your domain:
   ```
   kimlab.org
   ```
3. Update `astro.config.mjs`:
   ```javascript
   export default defineConfig({
     site: 'https://kimlab.org',
     base: '/',  // Change to '/' for custom domain
   });
   ```
4. Update DNS records at your domain registrar to point to GitHub Pages
5. In GitHub repo Settings → Pages, enter your custom domain

---

## Troubleshooting

### "npm: command not found"
Node.js is not installed or not in your PATH. Reinstall Node.js from https://nodejs.org/

### Build fails on GitHub
- Check the "Actions" tab for error messages
- Ensure all files are committed and pushed
- Verify `astro.config.mjs` has correct settings

### Images not showing
- Make sure images are in the `public/` folder
- Use the correct path: `/kimlab-website/image-name.jpg`

### Changes not appearing on live site
- Wait 1-2 minutes for deployment
- Clear your browser cache (Cmd+Shift+R on Mac, Ctrl+Shift+R on Windows)
- Check the "Actions" tab on GitHub for deployment status

---

## Need Help?

- Astro Documentation: https://docs.astro.build/
- GitHub Pages Documentation: https://docs.github.com/pages
- GitHub Actions Documentation: https://docs.github.com/actions

---

## Commands Reference

| Command | Description |
|---------|-------------|
| `npm install` | Install dependencies |
| `npm run dev` | Start development server |
| `npm run build` | Build for production |
| `npm run preview` | Preview production build locally |
