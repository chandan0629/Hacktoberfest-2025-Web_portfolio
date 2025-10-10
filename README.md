# Chandan – Developer Portfolio

Deploy:- (https://chandan629.netlify.app)

Responsive, modern developer portfolio built with Vue 3 + Vite. It features official skill logos with emoji fallbacks, a GitHub projects section, theme toggle (light/dark), subtle 3D visuals, and more.

## Features

- Modern stack: Vue 3 (Composition API) + Vite
- Responsive layout and mobile‑friendly nav with evenly spaced tabs
- Theme toggle (light/dark) persisted to localStorage
- Skills section
  - Official brand SVG logos for common tools (TypeScript, JavaScript, Vue, Vite, Node.js, Express, GraphQL, PostgreSQL, MongoDB, Prisma, TailwindCSS, AWS, Azure, GCP, Docker, Kubernetes, Terraform, etc.)
  - Emoji fallback + Twemoji for consistent cross‑platform rendering
  - Logo errors gracefully fall back to emoji
- Projects section
  - Loads your public GitHub repositories (non‑fork, sorted by stars)
  - Quick links to repo and optional live site
- Contact section
  - Responsive buttons (Email, LinkedIn, GitHub)
- Subtle 3D visuals (tilt effects on cards/chips, decorative background)

## Quick Start

```bash
# Install dependencies
npm install

# Start dev server
npm run dev
# Open the printed local URL (typically http://localhost:5173)

# Production build
npm run build

# Preview the production build
npm run preview
```

## Configuration

Edit the following constants in `src/App.vue` to customize the site:

- Identity & links
  - `NAME`, `TITLE`, `TAGLINE`, `LOCATION`
  - `EMAIL` (mailto address), `GITHUB_USER`, `GITHUB_URL`, `LINKEDIN_URL`
- Skills
  - Update the `skills` array: each item supports `label`, `icon` (emoji), and optional `logo` (path to SVG under `public/logos/...`)
  - If a logo fails to load, the emoji automatically displays
- Theme
  - Persisted with `localStorage` key `portfolio_theme_v1`

## Project Structure

```
/ (root)
├─ index.html               # App entry + Twemoji script
├─ public/
│  └─ logos/                # Official SVG logos used by Skills
├─ src/
│  ├─ App.vue               # Main UI + logic, sections, styles
│  ├─ main.js               # Vue bootstrap
│  └─ style.css             # Starter CSS (reset/utility)
└─ vite.config.js
```

## Notes

- GitHub API rate limits unauthenticated requests. If repositories fail to load temporarily, try again later or add a GitHub token and fetch with authentication if needed.
- The app includes Twemoji to make emojis look consistent across OS/browsers.
- Logos are served from `public/logos`. Missing or zero‑byte logo files are safely ignored in favor of emoji.

## License

MIT (feel free to adapt for your own portfolio)

## Author

Chandan Kumar Raj  
GitHub: https://github.com/chandan0629  
LinkedIn: https://www.linkedin.com/in/chandan-kumar-raj-210839210/

---

## Hacktoberfest 2025 & Contributions Welcome! 🎉

This project is open for Hacktoberfest 2025 and all contributors. Whether you’re fixing bugs, adding features, updating documentation, or suggesting ideas—everyone is welcome!

[![Hacktoberfest badge](https://img.shields.io/badge/Hacktoberfest-2025-blueviolet)](https://hacktoberfest.com/)

**How to participate:**
- Star this repo
- Follow me If u want
- Fork this repository
- Make your changes (code, docs, suggestions)
- Open a Pull Request (PR) with a clear description

**Tips for contributors:**
- Check for issues labeled `good first issue` or `hacktoberfest`
- Be respectful and follow our Code of Conduct
- Read our [CONTRIBUTING.md](CONTRIBUTING.md) for more details

Official Hacktoberfest site: [https://hacktoberfest.com/](https://hacktoberfest.com/)

Thank you for helping make open source better. Happy hacking!
