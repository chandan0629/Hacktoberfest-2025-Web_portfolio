<script setup>
import { ref, onMounted, watch } from 'vue'

// Identity and links
const NAME = 'Chandan Kumar Raj'
const TITLE = 'Full‑Stack Software Engineer'
const TAGLINE = 'I build resilient, performant web apps and data‑driven systems with clean architecture, scalable APIs, and delightful UX.'
const LOCATION = 'India'
const EMAIL = 'mailto:chandantoaws@gmail.com'
const EMAIL_TEXT = 'chandantoaws@gmail.com'
const GITHUB_USER = 'chandan0629'
const GITHUB_URL = `https://github.com/${GITHUB_USER}`
const LINKEDIN_URL = 'https://www.linkedin.com/in/chandan-kumar-raj-210839210/'

// Theme
const THEME_KEY = 'portfolio_theme_v1'
const theme = ref('dark')
const prefersDark = window.matchMedia('(prefers-color-scheme: dark)')
const mqHoverFine = window.matchMedia('(hover: hover) and (pointer: fine)')
const mqReduceMotion = window.matchMedia('(prefers-reduced-motion: reduce)')
const BASE = import.meta.env.BASE_URL
function applyTheme(t) { document.documentElement.setAttribute('data-theme', t) }
function loadTheme() {
  const saved = localStorage.getItem(THEME_KEY)
  if (saved === 'light' || saved === 'dark') theme.value = saved
  else theme.value = prefersDark.matches ? 'dark' : 'light'
}
function toggleTheme() { theme.value = theme.value === 'dark' ? 'light' : 'dark' }
watch(theme, (t) => { localStorage.setItem(THEME_KEY, t); applyTheme(t) })
const navOpen = ref(false)
function toggleNav() { navOpen.value = !navOpen.value }
function closeNav() { navOpen.value = false }

// Data: GitHub user + repos
const user = ref(null)
const repos = ref([])

function openNew(url) { window.open(url, '_blank', 'noopener') }
function scrollTo(sel) { const el = document.querySelector(sel); if (el) el.scrollIntoView({ behavior: 'smooth', block: 'start' }) }
function getLiveUrl(r) {
  try {
    if (r.homepage && /^https?:\/\//i.test(r.homepage)) return r.homepage
    if (r.has_pages) return `https://${GITHUB_USER}.github.io/${r.name}/`
    if (r.name === 'my-portfolio') return `https://${GITHUB_USER}.github.io/${r.name}/`
  } catch (e) {}
  return null
}

// Skills with icons
const skills = ref([
  // Languages
  { label: 'Java', icon: '☕', logo: '/logos/java.svg' },
  { label: 'Python', icon: '🐍', logo: '/logos/python.svg' },

  // Core Web & Backend
  { label: 'TypeScript', icon: '🟦', logo: '/logos/typescript.svg' },
  { label: 'JavaScript', icon: '🟨', logo: '/logos/javascript.svg' },
  { label: 'Vue 3', icon: '🟩', logo: '/logos/vue.svg' },
  { label: 'Vite', icon: '⚡', logo: '/logos/vite.svg' },
  { label: 'Node.js', icon: '🟢', logo: '/logos/nodejs.svg' },
  { label: 'Express', icon: '🚇', logo: '/logos/express.svg' },
  { label: 'GraphQL', icon: '🔺', logo: '/logos/graphql.svg' },
  { label: 'REST', icon: '🔗' },
  { label: 'PostgreSQL', icon: '🐘', logo: '/logos/postgresql.svg' },
  { label: 'MongoDB', icon: '🍃', logo: '/logos/mongodb.svg' },
  { label: 'Prisma', icon: '🔷', logo: '/logos/prisma.svg' },
  { label: 'TailwindCSS', icon: '🌬️', logo: '/logos/tailwindcss.svg' },
  { label: 'Design Systems', icon: '🎨' },

  // Cloud & DevOps
  { label: 'AWS', icon: '☁️', logo: '/logos/aws.svg' },
  { label: 'Azure', icon: '🔷', logo: '/logos/azure.svg' },
  { label: 'GCP', icon: '🌐', logo: '/logos/gcp.svg' },
  { label: 'Docker', icon: '🐳', logo: '/logos/docker.svg' },
  { label: 'Kubernetes', icon: '☸️', logo: '/logos/kubernetes.svg' },
  { label: 'Terraform', icon: '🧩', logo: '/logos/terraform.svg' },
  { label: 'GitHub Actions', icon: '⚙️', logo: '/logos/githubactions.svg' },
  { label: 'CI/CD', icon: '🔁' },
  { label: 'Linux', icon: '🐧', logo: '/logos/linux.svg' },
  { label: 'NGINX', icon: '🚦', logo: '/logos/nginx.svg' },

  // Data Science & ML
  { label: 'Pandas', icon: '🐼', logo: '/logos/pandas.svg' },
  { label: 'NumPy', icon: '🔢', logo: '/logos/numpy.svg' },
  { label: 'scikit-learn', icon: '🤖', logo: '/logos/scikitlearn.svg' },
  { label: 'TensorFlow', icon: '🧠', logo: '/logos/tensorflow.svg' },
  { label: 'PyTorch', icon: '🔥', logo: '/logos/pytorch.svg' },
  { label: 'Jupyter', icon: '📓', logo: '/logos/jupyter.svg' },
  { label: 'Matplotlib', icon: '📈', logo: '/logos/matplotlib.svg' },
  { label: 'Data Visualization', icon: '📊' },
])

// Prefix public asset logos with BASE for GitHub Pages compatibility
skills.value = skills.value.map(s => s.logo ? ({ ...s, logo: (BASE + s.logo.replace(/^\//, '')) }) : s)

// Lightweight 3D tilt effect
function attachTilt(selector, maxTilt = 8) {
  if (!mqHoverFine.matches || mqReduceMotion.matches) return
  const els = document.querySelectorAll(selector)
  els.forEach((el) => {
    if (el.dataset.tiltInit) return
    el.dataset.tiltInit = '1'
    el.style.transformStyle = 'preserve-3d'
    const onMove = (e) => {
      const r = el.getBoundingClientRect()
      const cx = r.left + r.width / 2
      const cy = r.top + r.height / 2
      const dx = (e.clientX - cx) / (r.width / 2)
      const dy = (e.clientY - cy) / (r.height / 2)
      const rx = (-dy * maxTilt).toFixed(2)
      const ry = (dx * maxTilt).toFixed(2)
      el.style.transform = `perspective(900px) rotateX(${rx}deg) rotateY(${ry}deg)`
      el.classList.add('elevate')
    }
    const onLeave = () => {
      el.style.transform = 'perspective(900px) rotateX(0deg) rotateY(0deg)'
      el.classList.remove('elevate')
    }
    el.addEventListener('mousemove', onMove)
    el.addEventListener('mouseleave', onLeave)
  })
}

onMounted(async () => {
  loadTheme(); applyTheme(theme.value)
  try { if (window.twemoji) window.twemoji.parse(document.body, { folder: 'svg', ext: '.svg' }); } catch (e) {}
  try {
    const userRes = await fetch(`https://api.github.com/users/${GITHUB_USER}`)
    user.value = await userRes.json()
  } catch {}
  try {
    const reposRes = await fetch(`https://api.github.com/users/${GITHUB_USER}/repos?per_page=100&sort=updated`)
    const all = await reposRes.json()
    if (Array.isArray(all)) {
      repos.value = all
        .filter(r => !r.fork)
        .sort((a,b) => (b.stargazers_count||0) - (a.stargazers_count||0))
        .slice(0, 8)
      // Attach tilt for projects after DOM updates
      setTimeout(() => attachTilt('.project'), 0)
    }
  } catch {}
  // Attach tilt to chips after mount
  setTimeout(() => attachTilt('.chip', 5), 0)
})

// Re-attach tilt when theme changes (to ensure visual reset)
watch(theme, () => {
  setTimeout(() => {
    attachTilt('.project')
    attachTilt('.chip', 5)
  }, 50)
})
</script>

<template>
  <div class="app">
    <!-- Decorative 3D background -->
    <div class="bg3d" aria-hidden="true">
      <div class="orb orb-a"></div>
      <div class="orb orb-b"></div>
      <div class="orb orb-c"></div>
      <div class="grid3d"></div>
    </div>

    <!-- NAV -->
    <nav class="nav">
      <div class="container nav-row">
        <div class="brand" @click="scrollTo('#top')">{{ NAME }}</div>
        <div class="nav-actions">
          <button class="theme-toggle" @click="toggleTheme" :aria-label="'Switch to ' + (theme==='dark' ? 'light' : 'dark') + ' theme'">
            <svg v-if="theme==='dark'" width="20" height="20" viewBox="0 0 24 24" fill="currentColor"><path d="M21.64 13a9 9 0 01-10.63-10.63A9 9 0 1021.64 13z"/></svg>
            <svg v-else width="20" height="20" viewBox="0 0 24 24" fill="currentColor"><path d="M6.76 4.84l-1.8-1.79L3.17 4.84l1.79 1.8 1.8-1.8zm10.48 0l1.79-1.79 1.79 1.79-1.79 1.8-1.79-1.8zM12 2h0v3h0V2zm0 17h0v3h0v-3zM4.84 17.24l-1.67 1.67 1.79 1.79 1.67-1.67-1.79-1.79zM19.16 17.24l1.79 1.79-1.67 1.67-1.79-1.79 1.67-1.67zM2 12h3v0H2zm17 0h3v0h-3zM6.76 19.16l1.8 1.8-1.8-1.8z"/></svg>
          </button>
          <button class="menu-toggle" @click="toggleNav" :aria-expanded="navOpen.toString()" aria-controls="primary-navigation" aria-label="Toggle menu">
            <svg width="22" height="22" viewBox="0 0 24 24" fill="currentColor" aria-hidden="true"><path d="M3 6h18v2H3V6zm0 5h18v2H3v-2zm0 5h18v2H3v-2z"/></svg>
          </button>
        </div>
        <div :class="['nav-links', { open: navOpen }]" id="primary-navigation">
          <a href="#about" @click="closeNav">About</a>
          <a href="#skills" @click="closeNav">Skills</a>
          <a href="#projects" @click="closeNav">Projects</a>
          <a href="#contact" @click="closeNav">Contact</a>
          <a :href="EMAIL" @click="closeNav">Email</a>
          <a :href="GITHUB_URL" target="_blank" rel="noopener" class="icon" aria-label="GitHub" @click="closeNav">
            <svg viewBox="0 0 16 16" aria-hidden="true"><path fill="currentColor" fill-rule="evenodd" d="M8 0C3.58 0 0 3.58 0 8c0 3.54 2.29 6.53 5.47 7.59.4.07.55-.17.55-.38 0-.19-.01-.82-.01-1.49-2.01.37-2.53-.49-2.69-.94-.09-.23-.48-.94-.82-1.13-.28-.15-.68-.52-.01-.53.63-.01 1.08.58 1.23.82.72 1.21 1.87.87 2.33.66.07-.52.28-.87.51-1.07-1.78-.2-3.64-.89-3.64-3.95 0-.87.31-1.59.82-2.15-.08-.2-.36-1.02.08-2.12 0 0 .67-.21 2.2.82.64-.18 1.32-.27 2-.27.68 0 1.36.09 2 .27 1.53-1.04 2.2-.82 2.2-.82.44 1.1.16 1.92.08 2.12.51.56.82 1.27.82 2.15 0 3.07-1.87 3.75-3.65 3.95.29.25.54.73.54 1.48 0 1.07-.01 1.93-.01 2.2 0 .21.15.46.55.38A8.013 8.013 0 0016 8c0-4.42-3.58-8-8-8z"/></svg>
          </a>
          <a :href="LINKEDIN_URL" target="_blank" rel="noopener" class="icon" aria-label="LinkedIn" @click="closeNav">
            <svg viewBox="0 0 24 24" aria-hidden="true"><path fill="currentColor" d="M20.447 20.452h-3.554v-5.569c0-1.328-.025-3.037-1.852-3.037-1.853 0-2.136 1.447-2.136 2.942v5.664H9.352V9h3.414v1.561h.049c.476-.9 1.637-1.85 3.37-1.85 3.605 0 4.27 2.371 4.27 5.455v6.286zM5.337 7.433a2.062 2.062 0 11.002-4.124 2.062 2.062 0 01-.002 4.124zM6.99 20.452H3.684V9h3.307v11.452z"/></svg>
          </a>
        </div>
      </div>
    </nav>

    <!-- HERO -->
    <header id="top" class="hero">
      <div class="container hero-inner">
        <div class="hero-text">
          <div class="pill">Available for work • {{ LOCATION }}</div>
          <h1>{{ NAME }}</h1>
          <h2>{{ TITLE }}</h2>
          <p class="tagline">{{ TAGLINE }}</p>
          <div class="hero-cta">
            <button class="btn neutral" @click="openNew(GITHUB_URL)">View GitHub</button>
            <button class="btn secondary" @click="openNew(LINKEDIN_URL)">Connect on LinkedIn</button>
            <button class="btn neutral" @click="openNew(EMAIL)">Email Me</button>
          </div>
        </div>
        <div class="hero-avatar">
          <img :src="user?.avatar_url || (BASE + 'vite.svg')" :alt="user?.name || 'Avatar'" />
        </div>
      </div>
    </header>

    <main>
      <!-- ABOUT -->
      <section id="about" class="section">
        <div class="container">
          <h3 class="section-title">About</h3>
          <p>
            Full‑stack engineer specializing in Vue, Node, and TypeScript, with a focus on performance, reliability,
            and clean architecture.
          </p>
          <p>
            Comfortable across Cloud & DevOps (AWS, Docker, Kubernetes, Terraform) and Data Science tooling (Python,
            Pandas, scikit‑learn) to deliver end‑to‑end solutions.
          </p>
          <p>
            I enjoy designing DX‑friendly APIs, building accessible, elegant UIs, and instrumenting apps with metrics
            and observability for continuous improvement.
          </p>
          <p>
            Curious, pragmatic, and collaborative—I fit well across teams, communicate clearly, and enjoy mentoring and
            open source.
          </p>
        </div>
      </section>

      <!-- SKILLS -->
      <section id="skills" class="section">
        <div class="container">
          <h3 class="section-title">Skills</h3>
          <div class="chips">
            <span v-for="s in skills" :key="s.label" class="chip">
              <span v-if="s.logo && !s.failed" class="chip-logo"><img :src="s.logo" :alt="s.label + ' logo'" @error="s.failed = true" /></span>
              <span v-else class="chip-icon" aria-hidden="true">{{ s.icon }}</span>
              <span class="chip-label">{{ s.label }}</span>
            </span>
          </div>
        </div>
      </section>

      <!-- PROJECTS -->
      <section id="projects" class="section">
        <div class="container">
          <h3 class="section-title">Projects</h3>
          <div class="projects">
            <div v-for="r in repos" :key="r.id" class="project">
              <div class="project-head">
                <h4 class="project-title">{{ r.name }}</h4>
                <div class="badges">
                  <span v-if="r.language" class="badge">{{ r.language }}</span>
                  <span class="badge">★ {{ r.stargazers_count || 0 }}</span>
                  <span class="badge">⑂ {{ r.forks_count || 0 }}</span>
                </div>
              </div>
              <p class="project-desc">{{ r.description || '—' }}</p>
              <div class="project-actions">
                <button class="btn" @click="openNew(r.html_url)">Open Repo</button>
                <button v-if="getLiveUrl(r)" class="btn secondary" @click="openNew(getLiveUrl(r))">Live</button>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- CONTACT -->
      <section id="contact" class="section">
        <div class="container contact">
          <h3 class="section-title">Contact</h3>
          <p>Interested in collaborating or hiring? Reach out via email or connect on LinkedIn.</p>
          <div class="contact-actions">
            <button class="btn neutral" @click="openNew(EMAIL)">Email</button>
            <button class="btn secondary" @click="openNew(LINKEDIN_URL)">LinkedIn</button>
            <button class="btn neutral" @click="openNew(GITHUB_URL)">GitHub</button>
          </div>
                  </div>
      </section>
    </main>

    <footer class="footer">
      <div class="container">© {{ new Date().getFullYear() }} {{ NAME }} • Built with Vue 3 + Vite</div>
    </footer>
  </div>
</template>

<style>
/* THEME */
:root {
  --bg: #0a0f1a;            /* dark background */
  --bg-3d-a: rgba(34,211,238,0.12);
  --bg-3d-b: rgba(14,165,233,0.18);
  --text: #e2e8f0;
  --muted: #94a3b8;
  --brand: #0ea5e9;
  --brand-2: #22d3ee;
  --panel: #0f172a;
  --border: rgba(148,163,184,0.18);
  --chip: rgba(148,163,184,0.12);
  --card: rgba(2,6,23,0.5);
  --badge: rgba(148,163,184,0.15);
  --shadow-sm: 0 6px 16px rgba(2,8,23,0.35);
  --shadow: 0 10px 28px rgba(2,8,23,0.5);
  --shadow-xl: 0 18px 50px rgba(2,8,23,0.65);
}
:root[data-theme='light'] {
  --bg: #f7fafc;            /* light background */
  --bg-3d-a: rgba(6,182,212,0.08);
  --bg-3d-b: rgba(2,132,199,0.10);
  --text: #0f172a;
  --muted: #475569;
  --brand: #0284c7;
  --brand-2: #06b6d4;
  --panel: #ffffff;
  --border: rgba(2,6,23,0.08);
  --chip: rgba(2,6,23,0.04);
  --card: #ffffff;
  --badge: rgba(2,6,23,0.06);
  --shadow-sm: 0 6px 16px rgba(2,8,23,0.08);
  --shadow: 0 10px 28px rgba(2,8,23,0.12);
  --shadow-xl: 0 18px 50px rgba(2,8,23,0.16);
}

/* BASE */
* { box-sizing: border-box; }
html, body, #app, .app { min-height: 100%; }
body { margin: 0; font-family: ui-sans-serif, system-ui, -apple-system, Segoe UI, Roboto, Helvetica, Arial, "Apple Color Emoji", "Segoe UI Emoji"; color: var(--text); background: var(--bg); }
a { color: var(--brand); text-decoration: none; }
a:hover, a:focus { text-decoration: underline; }
.container { max-width: 1120px; margin: 0 auto; padding: 0 20px; }

/* 3D BACKGROUND */
.bg3d { position: fixed; inset: 0; pointer-events: none; z-index: -1; overflow: hidden; }
.bg3d::before { content: ""; position: absolute; inset: -10%; background:
  radial-gradient(1200px 800px at 80% -10%, var(--bg-3d-a), transparent 60%),
  radial-gradient(900px 700px at -10% 10%, var(--bg-3d-b), transparent 60%);
  filter: saturate(1.1);
}
.grid3d { position: absolute; left: -10%; right: -10%; bottom: -40%; height: 70%;
  background-image: 
    linear-gradient(to right, rgba(148,163,184,0.08) 1px, transparent 1px),
    linear-gradient(to top, rgba(148,163,184,0.08) 1px, transparent 1px);
  background-size: 40px 40px;
  transform: perspective(900px) rotateX(58deg);
  transform-origin: center top;
  mask-image: linear-gradient(to top, transparent 0%, black 40%, black 70%, transparent 100%);
}
.orb { position: absolute; width: 40vmin; height: 40vmin; border-radius: 50%; filter: blur(30px); opacity: .5; animation: float 12s ease-in-out infinite alternate; }
.orb-a { top: 10%; left: -5%; background: radial-gradient(circle at 30% 30%, var(--brand-2), transparent 60%); }
.orb-b { top: 20%; right: -10%; background: radial-gradient(circle at 70% 40%, var(--brand), transparent 60%); animation-duration: 14s; }
.orb-c { bottom: -6%; left: 30%; background: radial-gradient(circle at 50% 50%, #f59e0b66, transparent 60%); animation-duration: 16s; }
@keyframes float { to { transform: translate3d(0,-20px,0) scale(1.05); opacity: .65; } }

/* NAV */
.nav { position: sticky; top: 0; z-index: 50; background: rgba(2,6,23,0.55); backdrop-filter: blur(10px) saturate(1.2); border-bottom: 1px solid var(--border); padding-top: env(safe-area-inset-top); }
:root[data-theme='light'] .nav { background: rgba(255,255,255,0.8); }
.nav-row { display: flex; align-items: center; justify-content: space-between; height: 64px; }
.brand { font-weight: 800; letter-spacing: .3px; cursor: pointer; }
.nav-links { display: flex; align-items: center; gap: 12px; flex: 1; justify-content: space-evenly; flex-wrap: wrap; }
.nav a { color: var(--text); opacity: 1; padding: 8px 10px; border-radius: 10px; background: var(--chip); border: 1px solid var(--border); text-decoration: none; }
.icon svg { width: 20px; height: 20px; }
.theme-toggle { border: 1px solid var(--border); background: var(--panel); color: var(--text); border-radius: 8px; padding: 6px 8px; cursor: pointer; box-shadow: var(--shadow-sm); }
.menu-toggle { display: none; border: 1px solid var(--border); background: var(--panel); color: var(--text); border-radius: 8px; padding: 6px 8px; cursor: pointer; box-shadow: var(--shadow-sm); }
.nav-actions { display: flex; align-items: center; gap: 8px; }

/* HERO */
.hero { padding: 70px 0; }
.hero-inner { display: grid; grid-template-columns: 1.4fr .9fr; gap: 28px; align-items: center; }
.hero-text h1 { margin: 0 0 6px; font-size: clamp(28px, 6vw, 44px); line-height: 1.1; text-shadow: 0 6px 20px rgba(2,8,23,0.35); }
.hero-text h2 { margin: 0 0 8px; font-weight: 700; font-size: clamp(16px, 3.2vw, 22px); color: var(--muted); }
.tagline { color: var(--muted); max-width: 720px; font-size: 16px; line-height: 1.6; }
.pill { display: inline-block; background: var(--chip); border: 1px solid var(--border); padding: 6px 10px; border-radius: 999px; margin-bottom: 10px; font-size: 12px; color: var(--muted); box-shadow: var(--shadow-sm); }
.hero-cta { display: flex; gap: 12px; flex-wrap: wrap; margin: 16px 0 20px; justify-content: space-evenly; }
.hero-cta .btn { min-width: 160px; }
.hero-avatar img { width: 100%; max-width: 320px; border-radius: 18px; border: 1px solid var(--border); display: block; margin-left: auto; box-shadow: var(--shadow); transform: perspective(900px) rotateY(-8deg) rotateX(2deg); }

/* BUTTONS */
.btn { appearance: none; border: none; outline: none; cursor: pointer; padding: 10px 14px; border-radius: 10px; color: #0b1220; font-weight: 700; letter-spacing: .2px; background: linear-gradient(180deg, var(--brand), #0284c7); box-shadow: 0 10px 20px rgba(14,165,233,.25), inset 0 0 0 1px rgba(148,163,184,0.2); transition: transform .12s ease, filter .2s ease, box-shadow .2s ease; }
.btn:hover { transform: translateY(-1px); filter: brightness(1.08); box-shadow: 0 14px 30px rgba(14,165,233,.3); }
.btn.secondary { background: linear-gradient(180deg, #1f2937, #0b1220); color: var(--text); }
:root[data-theme='light'] .btn.secondary { background: #f1f5f9; color: #0f172a; border: 1px solid var(--border); box-shadow: none; }
.btn.tertiary { background: transparent; color: var(--text); border: 1px solid var(--border); box-shadow: none; }
.btn.neutral { background: var(--chip); color: var(--text); border: 1px solid var(--border); box-shadow: none; }

/* SECTION */
.section { padding: 56px 0; }
.section-title { margin: 0 0 18px; font-size: 18px; letter-spacing: .3px; text-transform: uppercase; color: var(--muted); }

/* SKILLS */
.chips { display: flex; gap: 12px; flex-wrap: wrap; }
.chip { display: inline-flex; align-items: center; padding: 10px 12px; border-radius: 14px; border: 1px solid var(--border); background: linear-gradient(180deg, rgba(148,163,184,0.15), rgba(148,163,184,0.08)); box-shadow: var(--shadow-sm); transform: perspective(900px); transition: transform .2s ease, box-shadow .2s ease, filter .2s ease; }
.chip-icon { margin-right: 8px; font-size: 1.2rem; }
.chip .emoji { width: 1.2rem; height: 1.2rem; margin-right: 8px; vertical-align: -0.2em; }
.chip-logo { display:inline-flex; align-items:center; justify-content:center; width: 22px; height: 22px; margin-right: 8px; }
.chip-logo img { width: 100%; height: 100%; object-fit: contain; display:block; filter: drop-shadow(0 1px 1px rgba(0,0,0,.2)); }
.chip-label { font-size: 13px; }
.chip.elevate { box-shadow: var(--shadow); }

/* PROJECTS */
.projects { display: grid; grid-template-columns: repeat(3, minmax(0, 1fr)); gap: 18px; }
.project { background: linear-gradient(180deg, rgba(148,163,184,0.10), rgba(148,163,184,0.06)); border: 1px solid var(--border); border-radius: 16px; padding: 16px; display: flex; flex-direction: column; box-shadow: var(--shadow-sm); transform: perspective(900px); transition: transform .2s ease, box-shadow .2s ease, filter .2s ease; }
.project.elevate { box-shadow: var(--shadow-xl); }
.project-head { display: flex; justify-content: space-between; align-items: flex-start; gap: 10px; }
.project-title { margin: 0; font-size: 16px; }
.badges { display: flex; gap: 6px; flex-wrap: wrap; }
.badge { background: var(--badge); border: 1px solid var(--border); border-radius: 999px; padding: 4px 8px; font-size: 12px; color: var(--muted); }
.project-desc { color: var(--muted); min-height: 40px; }
.project-actions { margin-top: auto; display: flex; gap: 8px; }

/* CONTACT */
.contact { text-align: center; }
.contact-actions { display: flex; gap: 12px; flex-wrap: wrap; justify-content: center; }
.contact-actions .btn { flex: 1 1 220px; min-width: 160px; }

/* FOOTER */
.footer { border-top: 1px solid var(--border); padding: 18px 0 24px; text-align: center; color: var(--muted); }

/* Accessibility and device optimizations */
.section { scroll-margin-top: 80px; }

/* Reduced motion support */
@media (prefers-reduced-motion: reduce) {
  .orb { animation: none !important; }
  .chip, .project, .btn { transition: none !important; }
  .grid3d { transform: none !important; }
}

/* Tablet refinements */
@media (max-width: 768px) {
  .hero { padding: 60px 0; }
  .section { padding: 44px 0; }
  .projects { gap: 14px; }
  .nav-row { height: 60px; position: relative; }
  .menu-toggle { display: inline-flex; align-items: center; justify-content: center; }
  .nav-links { position: absolute; left: 0; right: 0; top: 100%; background: var(--panel); border-bottom: 1px solid var(--border); box-shadow: var(--shadow); padding: 12px 16px; display: none; flex-direction: column; gap: 8px; }
  .nav-links.open { display: flex; }
  .nav-links a { text-align: left; }
  .nav-links .theme-toggle { display: none; }
}

/* RESPONSIVE */
@media (max-width: 1024px) {
  .hero-inner { grid-template-columns: 1fr; }
  .projects { grid-template-columns: repeat(2, minmax(0, 1fr)); }
}
@media (max-width: 640px) {
  .hero-cta .btn, .contact-actions .btn { flex: 1 1 100%; }
  .hero-text h1 { font-size: clamp(24px, 7.5vw, 32px); }
  .hero-avatar img { transform: none; margin-left: auto; margin-right: auto; max-width: 260px; }
  .chip { padding: 8px 10px; }
  .chip-logo { width: 18px; height: 18px; margin-right: 6px; }
  .chip-label { font-size: 12px; }
  .projects { grid-template-columns: 1fr; }
}
</style>
