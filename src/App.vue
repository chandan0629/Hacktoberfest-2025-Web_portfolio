<script setup>
import { ref, onMounted, watch } from "vue";

// Identity and links
const NAME = "Chandan Kumar Raj";
const TITLE = "Full‑Stack Software Engineer";
const TAGLINE =
  "I build resilient, performant web apps and data‑driven systems with clean architecture, scalable APIs, and delightful UX.";
const LOCATION = "India";
const EMAIL = "mailto:chandantoaws@gmail.com";
const EMAIL_TEXT = "chandantoaws@gmail.com";
const GITHUB_USER = "chandan0629";
const GITHUB_URL = `https://github.com/${GITHUB_USER}`;
const LINKEDIN_URL = "https://www.linkedin.com/in/chandan-kumar-raj-210839210/";
// Please update with ur profile links
const TWITTER_URL = "https://x.com/chandankum13363?s=21";
const INSTAGRAM_URL = "https://instagram.com/chandan_629";

// Theme
const THEME_KEY = "portfolio_theme_v1";
const theme = ref("dark");
const prefersDark = window.matchMedia("(prefers-color-scheme: dark)");
const mqHoverFine = window.matchMedia("(hover: hover) and (pointer: fine)");
const mqReduceMotion = window.matchMedia("(prefers-reduced-motion: reduce)");
const BASE = import.meta.env.BASE_URL;
function applyTheme(t) {
  document.documentElement.setAttribute("data-theme", t);
}
function loadTheme() {
  const saved = localStorage.getItem(THEME_KEY);
  if (saved === "light" || saved === "dark") theme.value = saved;
  else theme.value = prefersDark.matches ? "dark" : "light";
}
function toggleTheme() {
  theme.value = theme.value === "dark" ? "light" : "dark";
}
watch(theme, (t) => {
  localStorage.setItem(THEME_KEY, t);
  applyTheme(t);
});
const navOpen = ref(false);
function toggleNav() {
  navOpen.value = !navOpen.value;
}
function closeNav() {
  navOpen.value = false;
}

// Data: GitHub user + repos
const user = ref(null);
const repos = ref([]);

function openNew(url) {
  window.open(url, "_blank", "noopener");
}
function scrollTo(sel) {
  const el = document.querySelector(sel);
  if (el) el.scrollIntoView({ behavior: "smooth", block: "start" });
}
function getLiveUrl(r) {
  try {
    if (r.homepage && /^https?:\/\//i.test(r.homepage)) return r.homepage;
    if (r.has_pages) return `https://${GITHUB_USER}.github.io/${r.name}/`;
    if (r.name === "my-portfolio")
      return `https://${GITHUB_USER}.github.io/${r.name}/`;
  } catch (e) {}
  return null;
}

// Skills with icons
const skills = ref([
  // Languages
  { label: "Java", icon: "☕", logo: "/logos/java.svg" },
  { label: "Python", icon: "🐍", logo: "/logos/python.svg" },

  // Core Web & Backend
  { label: "TypeScript", icon: "🟦", logo: "/logos/typescript.svg" },
  { label: "JavaScript", icon: "🟨", logo: "/logos/javascript.svg" },
  { label: "Vue 3", icon: "🟩", logo: "/logos/vue.svg" },
  { label: "Vite", icon: "⚡", logo: "/logos/vite.svg" },
  { label: "Node.js", icon: "🟢", logo: "/logos/nodejs.svg" },
  { label: "Express", icon: "🚇", logo: "/logos/express.svg" },
  { label: "GraphQL", icon: "🔺", logo: "/logos/graphql.svg" },
  { label: "REST", icon: "🔗" },
  { label: "PostgreSQL", icon: "🐘", logo: "/logos/postgresql.svg" },
  { label: "MongoDB", icon: "🍃", logo: "/logos/mongodb.svg" },
  { label: "Prisma", icon: "🔷", logo: "/logos/prisma.svg" },
  { label: "TailwindCSS", icon: "🌬️", logo: "/logos/tailwindcss.svg" },
  { label: "Design Systems", icon: "🎨" },

  // Cloud & DevOps
  { label: "AWS", icon: "☁️", logo: "/logos/aws.svg" },
  { label: "Azure", icon: "🔷", logo: "/logos/azure.svg" },
  { label: "GCP", icon: "🌐", logo: "/logos/gcp.svg" },
  { label: "Docker", icon: "🐳", logo: "/logos/docker.svg" },
  { label: "Kubernetes", icon: "☸️", logo: "/logos/kubernetes.svg" },
  { label: "Terraform", icon: "🧩", logo: "/logos/terraform.svg" },
  { label: "GitHub Actions", icon: "⚙️", logo: "/logos/githubactions.svg" },
  { label: "CI/CD", icon: "🔁" },
  { label: "Linux", icon: "🐧", logo: "/logos/linux.svg" },
  { label: "NGINX", icon: "🚦", logo: "/logos/nginx.svg" },

  // Data Science & ML
  { label: "Pandas", icon: "🐼", logo: "/logos/pandas.svg" },
  { label: "NumPy", icon: "🔢", logo: "/logos/numpy.svg" },
  { label: "scikit-learn", icon: "🤖", logo: "/logos/scikitlearn.svg" },
  { label: "TensorFlow", icon: "🧠", logo: "/logos/tensorflow.svg" },
  { label: "PyTorch", icon: "🔥", logo: "/logos/pytorch.svg" },
  { label: "Jupyter", icon: "📓", logo: "/logos/jupyter.svg" },
  { label: "Matplotlib", icon: "📈", logo: "/logos/matplotlib.svg" },
  { label: "Data Visualization", icon: "📊" },
]);

// Prefix public asset logos with BASE for GitHub Pages compatibility
skills.value = skills.value.map((s) =>
  s.logo ? { ...s, logo: BASE + s.logo.replace(/^\//, "") } : s
);

// Click handler for skills
function onSkillClick(skill) {
  alert(`You clicked: ${skill.label}`);
}

// Lightweight 3D tilt effect
function attachTilt(selector, maxTilt = 8) {
  if (!mqHoverFine.matches || mqReduceMotion.matches) return;
  const els = document.querySelectorAll(selector);
  els.forEach((el) => {
    if (el.dataset.tiltInit) return;
    el.dataset.tiltInit = "1";
    el.style.transformStyle = "preserve-3d";
    const onMove = (e) => {
      const r = el.getBoundingClientRect();
      const cx = r.left + r.width / 2;
      const cy = r.top + r.height / 2;
      const dx = (e.clientX - cx) / (r.width / 2);
      const dy = (e.clientY - cy) / (r.height / 2);
      const rx = (-dy * maxTilt).toFixed(2);
      const ry = (dx * maxTilt).toFixed(2);
      el.style.transform = `perspective(900px) rotateX(${rx}deg) rotateY(${ry}deg)`;
      el.classList.add("elevate");
    };
    const onLeave = () => {
      el.style.transform = "perspective(900px) rotateX(0deg) rotateY(0deg)";
      el.classList.remove("elevate");
    };
    el.addEventListener("mousemove", onMove);
    el.addEventListener("mouseleave", onLeave);
  });
}

onMounted(async () => {
  loadTheme();
  applyTheme(theme.value);
  try {
    if (window.twemoji)
      window.twemoji.parse(document.body, { folder: "svg", ext: ".svg" });
  } catch (e) {}
  try {
    const userRes = await fetch(`https://api.github.com/users/${GITHUB_USER}`);
    user.value = await userRes.json();
  } catch {}
  try {
    const reposRes = await fetch(
      `https://api.github.com/users/${GITHUB_USER}/repos?per_page=100&sort=updated`
    );
    const all = await reposRes.json();
    if (Array.isArray(all)) {
      repos.value = all
        .filter((r) => !r.fork)
        .sort((a, b) => (b.stargazers_count || 0) - (a.stargazers_count || 0))
        .slice(0, 8);
      // Attach tilt for projects after DOM updates
      setTimeout(() => attachTilt(".project"), 0);
    }
  } catch {}
  // Attach tilt to chips after mount
  setTimeout(() => attachTilt(".chip", 5), 0);

  // Add fade-in animations for sections
  const observer = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          entry.target.classList.add("visible");
          observer.unobserve(entry.target);
        }
      });
    },
    { threshold: 0.1 }
  );
  document.querySelectorAll(".section, .hero").forEach((el) => {
    el.classList.add("fade-in");
    if (el.classList.contains("hero"))
      el.classList.add("visible"); // Hero visible by default
    else observer.observe(el);
  });
});

// Re-attach tilt when theme changes (to ensure visual reset)
watch(theme, () => {
  setTimeout(() => {
    attachTilt(".project");
    attachTilt(".chip", 5);
  }, 50);
});
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

        <div
          :class="['nav-links', { open: navOpen }]"
          id="primary-navigation"
          style="
            width: fit-content;
            display: grid;
            padding-left: 22px;
            grid-template-columns: repeat(5, 1fr);
            gap: 22px;
          "
        >
          <a href="#about" @click="closeNav">About</a>
          <a href="#skills" @click="closeNav">Skills</a>
          <a href="#projects" @click="closeNav">Projects</a>
          <a href="#contact" @click="closeNav">Contact</a>
          <a :href="EMAIL" @click="closeNav">Email</a>
        </div>
        <div
          style="
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            padding-left: 22px;
            gap: 22px;
          "
        >
          <a
            :href="GITHUB_URL"
            target="_blank"
            rel="noopener"
            class="icon"
            aria-label="GitHub"
            @click="closeNav"
          >
            <svg viewBox="0 0 16 16" aria-hidden="true">
              <path
                fill="currentColor"
                fill-rule="evenodd"
                d="M8 0C3.58 0 0 3.58 0 8c0 3.54 2.29 6.53 5.47 7.59.4.07.55-.17.55-.38 0-.19-.01-.82-.01-1.49-2.01.37-2.53-.49-2.69-.94-.09-.23-.48-.94-.82-1.13-.28-.15-.68-.52-.01-.53.63-.01 1.08.58 1.23.82.72 1.21 1.87.87 2.33.66.07-.52.28-.87.51-1.07-1.78-.2-3.64-.89-3.64-3.95 0-.87.31-1.59.82-2.15-.08-.2-.36-1.02.08-2.12 0 0 .67-.21 2.2.82.64-.18 1.32-.27 2-.27.68 0 1.36.09 2 .27 1.53-1.04 2.2-.82 2.2-.82.44 1.1.16 1.92.08 2.12.51.56.82 1.27.82 2.15 0 3.07-1.87 3.75-3.65 3.95.29.25.54.73.54 1.48 0 1.07-.01 1.93-.01 2.2 0 .21.15.46.55.38A8.013 8.013 0 0016 8c0-4.42-3.58-8-8-8z"
              />
            </svg>
          </a>
          <a
            :href="LINKEDIN_URL"
            target="_blank"
            rel="noopener"
            class="icon"
            aria-label="LinkedIn"
            @click="closeNav"
          >
            <svg viewBox="0 0 24 24" aria-hidden="true">
              <path
                fill="currentColor"
                d="M20.447 20.452h-3.554v-5.569c0-1.328-.025-3.037-1.852-3.037-1.853 0-2.136 1.447-2.136 2.942v5.664H9.352V9h3.414v1.561h.049c.476-.9 1.637-1.85 3.37-1.85 3.605 0 4.27 2.371 4.27 5.455v6.286zM5.337 7.433a2.062 2.062 0 11.002-4.124 2.062 2.062 0 01-.002 4.124zM6.99 20.452H3.684V9h3.307v11.452z"
              />
            </svg>
          </a>
          <a
            :href="TWITTER_URL"
            target="_blank"
            rel="noopener"
            class="icon flex items-center justify-center w-8 h-8"
            aria-label="Twitter"
            @click="closeNav"
          >
            <svg
              viewBox="0 0 24 24"
              aria-hidden="true"
              :class="{ 'fill-black': !isDarkMode, 'fill-white': isDarkMode }"
              class="w-6 h-6 transition-colors duration-300"
            >
              <path
                fill="currentColor"
                d="M18.244 2H21l-6.43 7.36L22 22h-4.766l-5.01-6.41L6.57 22H3l6.86-7.86L2 2h4.766l4.774 6.186L18.244 2zm-2.27 17h1.22L8.16 5h-1.3l9.115 14z"
              />
            </svg>
          </a>

          <aside
            class="nav-actions icon"
            style="height: 46px; width: 46px; padding: 0px"
          >
            <button
              class="theme-toggle"
              style="
                height: 46px;
                width: 46px;
                padding: 0px;
                aspect-ratio: 1/1;
                display: flex;
                justify-content: center;
                align-items: center;
              "
              @click="toggleTheme"
              :aria-label="
                'Switch to ' + (theme === 'dark' ? 'light' : 'dark') + ' theme'
              "
            >
              <svg
                v-if="theme !== 'dark'"
                width="24"
                height="24"
                viewBox="0 0 24 24"
                fill="currentColor"
              >
                <path d="M21.64 13a9 9 0 01-10.63-10.63A9 9 0 1021.64 13z" />
              </svg>
              <svg v-else width="24" height="24" viewBox="0 0 24 24">
                <svg
                  viewBox="0 0 24 24"
                  fill="none"
                  xmlns="http://www.w3.org/2000/svg"
                >
                  <rect width="24" height="24" />
                  <path
                    fill-rule="evenodd"
                    clip-rule="evenodd"
                    d="M8 12C8 9.79086 9.79086 8 12 8C14.2091 8 16 9.79086 16 12C16 14.2091 14.2091 16 12 16C9.79086 16 8 14.2091 8 12Z"
                    fill="currentColor"
                  />
                  <path
                    fill-rule="evenodd"
                    clip-rule="evenodd"
                    d="M12 2C12.5523 2 13 2.44772 13 3V5C13 5.55228 12.5523 6 12 6C11.4477 6 11 5.55228 11 5V3C11 2.44772 11.4477 2 12 2Z"
                    fill="currentColor"
                  />
                  <path
                    fill-rule="evenodd"
                    clip-rule="evenodd"
                    d="M19.7071 4.29289C20.0976 4.68342 20.0976 5.31658 19.7071 5.70711L17.7071 7.70711C17.3166 8.09763 16.6834 8.09763 16.2929 7.70711C15.9024 7.31658 15.9024 6.68342 16.2929 6.29289L18.2929 4.29289C18.6834 3.90237 19.3166 3.90237 19.7071 4.29289Z"
                    fill="currentColor"
                  />
                  <path
                    fill-rule="evenodd"
                    clip-rule="evenodd"
                    d="M18 12C18 11.4477 18.4477 11 19 11H21C21.5523 11 22 11.4477 22 12C22 12.5523 21.5523 13 21 13H19C18.4477 13 18 12.5523 18 12Z"
                    fill="currentColor"
                  />
                  <path
                    fill-rule="evenodd"
                    clip-rule="evenodd"
                    d="M16.2929 16.2929C16.6834 15.9024 17.3166 15.9024 17.7071 16.2929L19.7071 18.2929C20.0976 18.6834 20.0976 19.3166 19.7071 19.7071C19.3166 20.0976 18.6834 20.0976 18.2929 19.7071L16.2929 17.7071C15.9024 17.3166 15.9024 16.6834 16.2929 16.2929Z"
                    fill="currentColor"
                  />
                  <path
                    fill-rule="evenodd"
                    clip-rule="evenodd"
                    d="M12 18C12.5523 18 13 18.4477 13 19V21C13 21.5523 12.5523 22 12 22C11.4477 22 11 21.5523 11 21V19C11 18.4477 11.4477 18 12 18Z"
                    fill="currentColor"
                  />
                  <path
                    fill-rule="evenodd"
                    clip-rule="evenodd"
                    d="M7.70711 16.2929C8.09763 16.6834 8.09763 17.3166 7.70711 17.7071L5.70711 19.7071C5.31658 20.0976 4.68342 20.0976 4.29289 19.7071C3.90237 19.3166 3.90237 18.6834 4.29289 18.2929L6.29289 16.2929C6.68342 15.9024 7.31658 15.9024 7.70711 16.2929Z"
                    fill="currentColor"
                  />
                  <path
                    fill-rule="evenodd"
                    clip-rule="evenodd"
                    d="M2 12C2 11.4477 2.44772 11 3 11H5C5.55228 11 6 11.4477 6 12C6 12.5523 5.55228 13 5 13H3C2.44772 13 2 12.5523 2 12Z"
                    fill="currentColor"
                  />
                  <path
                    fill-rule="evenodd"
                    clip-rule="evenodd"
                    d="M4.29289 4.29289C4.68342 3.90237 5.31658 3.90237 5.70711 4.29289L7.70711 6.29289C8.09763 6.68342 8.09763 7.31658 7.70711 7.70711C7.31658 8.09763 6.68342 8.09763 6.29289 7.70711L4.29289 5.70711C3.90237 5.31658 3.90237 4.68342 4.29289 4.29289Z"
                    fill="currentColor"
                  />
                </svg>
              </svg>
            </button>
            <button
              class="menu-toggle"
              @click="toggleNav"
              :aria-expanded="navOpen.toString()"
              aria-controls="primary-navigation"
              aria-label="Toggle menu"
              style="height: 46px; width: 46px"
            >
              <svg
                width="24"
                height="24"
                viewBox="0 0 24 24"
                fill="currentColor"
                aria-hidden="true"
              >
                <path d="M3 6h18v2H3V6zm0 5h18v2H3v-2zm0 5h18v2H3v-2z" />
              </svg>
            </button>
          </aside>
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
            <button class="btn neutral" @click="openNew(GITHUB_URL)">
              View GitHub
            </button>
            <button class="btn secondary" @click="openNew(LINKEDIN_URL)">
              Connect on LinkedIn
            </button>
            <button class="btn secondary" @click="openNew(TWITTER_URL)">
              Connect on Twitter
            </button>
            <button class="btn neutral" @click="openNew(EMAIL)">
              Email Me
            </button>
          </div>
        </div>
        <div class="hero-avatar">
          <img
            :src="user?.avatar_url || BASE + 'vite.svg'"
            :alt="user?.name || 'Avatar'"
          />
        </div>
      </div>
    </header>

    <main>
      <!-- ABOUT -->
      <section id="about" class="section">
        <div class="container">
          <h3 class="section-title">About</h3>
          <p>
            Full‑stack engineer specializing in Vue, Node, and TypeScript, with
            a focus on performance, reliability, and clean architecture.
          </p>
          <p>
            Comfortable across Cloud & DevOps (AWS, Docker, Kubernetes,
            Terraform) and Data Science tooling (Python, Pandas, scikit‑learn)
            to deliver end‑to‑end solutions.
          </p>
          <p>
            I enjoy designing DX‑friendly APIs, building accessible, elegant
            UIs, and instrumenting apps with metrics and observability for
            continuous improvement.
          </p>
          <p>
            Curious, pragmatic, and collaborative—I fit well across teams,
            communicate clearly, and enjoy mentoring and open source.
          </p>
        </div>
      </section>

      <!-- SKILLS -->
      <section id="skills" class="section">
        <div class="container">
          <h3 class="section-title">Skills</h3>
          <div class="chips">
            <span
              v-for="s in skills"
              :key="s.label"
              class="chip"
              @click="onSkillClick(s)"
              style="cursor: pointer"
            >
              <span v-if="s.logo && !s.failed" class="chip-logo"
                ><img
                  :src="s.logo"
                  :alt="s.label + ' logo'"
                  @error="s.failed = true"
              /></span>
              <span v-else class="chip-icon" aria-hidden="true">{{
                s.icon
              }}</span>
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
              <p class="project-desc">{{ r.description || "—" }}</p>
              <div class="project-actions">
                <button class="btn" @click="openNew(r.html_url)">
                  Open Repo
                </button>
                <button
                  v-if="getLiveUrl(r)"
                  class="btn secondary"
                  @click="openNew(getLiveUrl(r))"
                >
                  Live
                </button>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- CONTACT -->
      <section id="contact" class="section">
        <div class="container contact">
          <h3 class="section-title">Contact</h3>
          <p>
            Interested in collaborating or hiring? Reach out via email or
            connect on LinkedIn.
          </p>

          <div class="contact-actions">
            <button class="btn neutral" @click="openNew(EMAIL)">
              <svg
                width="20"
                height="20"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="2"
                stroke-linecap="round"
                stroke-linejoin="round"
                aria-hidden="true"
              >
                <path
                  d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"
                ></path>
                <polyline points="22,6 12,13 2,6"></polyline>
              </svg>
              Email
            </button>

            <button class="btn secondary" @click="openNew(LINKEDIN_URL)">
              <svg
                width="20"
                height="20"
                viewBox="0 0 24 24"
                fill="currentColor"
                aria-hidden="true"
              >
                <path
                  d="M20.447 20.452h-3.554v-5.569c0-1.328-.025-3.037-1.852-3.037-1.853 0-2.136 1.447-2.136 2.942v5.664H9.352V9h3.414v1.561h.049c.476-.9 1.637-1.85 3.37-1.85 3.605 0 4.27 2.371 4.27 5.455v6.286zM5.337 7.433a2.062 2.062 0 11.002-4.124 2.062 2.062 0 01-.002 4.124zM6.99 20.452H3.684V9h3.307v11.452z"
                ></path>
              </svg>
              LinkedIn
            </button>

            <button class="btn neutral" @click="openNew(GITHUB_URL)">
              <svg
                width="20"
                height="20"
                viewBox="0 0 16 16"
                fill="currentColor"
                aria-hidden="true"
              >
                <path
                  fill-rule="evenodd"
                  d="M8 0C3.58 0 0 3.58 0 8c0 3.54 2.29 6.53 5.47 7.59.4.07.55-.17.55-.38 0-.19-.01-.82-.01-1.49-2.01.37-2.53-.49-2.69-.94-.09-.23-.48-.94-.82-1.13-.28-.15-.68-.52-.01-.53.63-.01 1.08.58 1.23.82.72 1.21 1.87.87 2.33.66.07-.52.28-.87.51-1.07-1.78-.2-3.64-.89-3.64-3.95 0-.87.31-1.59.82-2.15-.08-.2-.36-1.02.08-2.12 0 0 .67-.21 2.2.82.64-.18 1.32-.27 2-.27.68 0 1.36.09 2 .27 1.53-1.04 2.2-.82 2.2-.82.44 1.1.16 1.92.08 2.12.51.56.82 1.27.82 2.15 0 3.07-1.87 3.75-3.65 3.95.29.25.54.73.54 1.48 0 1.07-.01 1.93-.01 2.2 0 .21.15.46.55.38A8.013 8.013 0 0016 8c0-4.42-3.58-8-8-8z"
                ></path>
              </svg>
              GitHub
            </button>

            <button class="btn neutral" @click="openNew(TWITTER_URL)">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                width="20"
                height="20"
                viewBox="0 0 24 24"
                fill="currentColor"
                aria-hidden="true"
              >
                <path
                  d="M23 3a10.9 10.9 0 0 1-3.14 1.53A4.48 4.48 0 0 0 22.4.36a9.1 9.1 0 0 1-2.88 1.1A4.52 4.52 0 0 0 16.11 0c-2.5 0-4.52 2.03-4.52 4.52 0 .35.04.7.11 1.03A12.84 12.84 0 0 1 3.15 1.67a4.52 4.52 0 0 0 1.4 6.03A4.48 4.48 0 0 1 2.8 7v.06c0 2.18 1.55 4 3.6 4.42a4.52 4.52 0 0 1-2.05.08 4.52 4.52 0 0 0 4.22 3.14A9.05 9.05 0 0 1 1 19.54a12.8 12.8 0 0 0 6.94 2.03c8.33 0 12.89-6.9 12.89-12.89 0-.2 0-.39-.01-.58A9.22 9.22 0 0 0 23 3z"
                />
              </svg>
              Twitter
            </button>

            <button class="btn neutral" @click="openNew(INSTAGRAM_URL)">
              <svg
                width="20"
                height="20"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="2"
                stroke-linecap="round"
                stroke-linejoin="round"
                aria-hidden="true"
              >
                <rect x="2" y="2" width="20" height="20" rx="5" ry="5"></rect>
                <path
                  d="M16 11.37A4 4 0 1 1 12.63 8 4 4 0 0 1 16 11.37z"
                ></path>
                <line x1="17.5" y1="6.5" x2="17.51" y2="6.5"></line>
              </svg>
              Instagram
            </button>
          </div>
        </div>
      </section>
    </main>

    <footer class="footer">
      <div class="container">
        © {{ new Date().getFullYear() }} {{ NAME }} • Built with Vue 3 + Vite
      </div>
    </footer>
  </div>
</template>

<style>
/* THEME */
:root {
  --bg: #0a0f1a; /* dark background */
  --bg-3d-a: rgba(34, 211, 238, 0.12);
  --bg-3d-b: rgba(14, 165, 233, 0.18);
  --text: #e2e8f0;
  --muted: #94a3b8;
  --brand: #0ea5e9;
  --brand-2: #22d3ee;
  --panel: #0f172a;
  --border: rgba(148, 163, 184, 0.18);
  --chip: rgba(148, 163, 184, 0.12);
  --card: rgba(2, 6, 23, 0.5);
  --badge: rgba(148, 163, 184, 0.15);
  --shadow-sm: 0 6px 16px rgba(2, 8, 23, 0.35);
  --shadow: 0 10px 28px rgba(2, 8, 23, 0.5);
  --shadow-xl: 0 18px 50px rgba(2, 8, 23, 0.65);
}
:root[data-theme="light"] {
  --bg: #f7fafc; /* light background */
  --bg-3d-a: rgba(6, 182, 212, 0.08);
  --bg-3d-b: rgba(2, 132, 199, 0.1);
  --text: #0f172a;
  --muted: #475569;
  --brand: #0284c7;
  --brand-2: #06b6d4;
  --panel: #ffffff;
  --border: rgba(2, 6, 23, 0.08);
  --chip: rgba(2, 6, 23, 0.04);
  --card: #ffffff;
  --badge: rgba(2, 6, 23, 0.06);
  --shadow-sm: 0 6px 16px rgba(2, 8, 23, 0.08);
  --shadow: 0 10px 28px rgba(2, 8, 23, 0.12);
  --shadow-xl: 0 18px 50px rgba(2, 8, 23, 0.16);
}

/* BASE */
* {
  box-sizing: border-box;
}
html,
body,
#app,
.app {
  min-height: 100%;
}
body {
  margin: 0;
  font-family: ui-sans-serif, system-ui, -apple-system, Segoe UI, Roboto,
    Helvetica, Arial, "Apple Color Emoji", "Segoe UI Emoji";
  color: var(--text);
  background: var(--bg);
}
a {
  color: var(--brand);
  text-decoration: none;
}
a:hover,
a:focus {
  text-decoration: underline;
}
.container {
  max-width: 1120px;
  margin: 0 auto;
  padding: 0 20px;
}

/* 3D BACKGROUND */
.bg3d {
  position: fixed;
  inset: 0;
  pointer-events: none;
  z-index: -1;
  overflow: hidden;
}
.bg3d::before {
  content: "";
  position: absolute;
  inset: -10%;
  background: radial-gradient(
      1200px 800px at 80% -10%,
      var(--bg-3d-a),
      transparent 60%
    ),
    radial-gradient(900px 700px at -10% 10%, var(--bg-3d-b), transparent 60%);
  filter: saturate(1.1);
}
.grid3d {
  position: absolute;
  left: -10%;
  right: -10%;
  bottom: -40%;
  height: 70%;
  background-image: linear-gradient(
      to right,
      rgba(148, 163, 184, 0.08) 1px,
      transparent 1px
    ),
    linear-gradient(to top, rgba(148, 163, 184, 0.08) 1px, transparent 1px);
  background-size: 40px 40px;
  transform: perspective(900px) rotateX(58deg);
  transform-origin: center top;
  mask-image: linear-gradient(
    to top,
    transparent 0%,
    black 40%,
    black 70%,
    transparent 100%
  );
}
.orb {
  position: absolute;
  width: 40vmin;
  height: 40vmin;
  border-radius: 50%;
  filter: blur(30px);
  opacity: 0.5;
  animation: float 12s ease-in-out infinite alternate;
}
.orb-a {
  top: 10%;
  left: -5%;
  background: radial-gradient(
    circle at 30% 30%,
    var(--brand-2),
    transparent 60%
  );
}
.orb-b {
  top: 20%;
  right: -10%;
  background: radial-gradient(circle at 70% 40%, var(--brand), transparent 60%);
  animation-duration: 14s;
}
.orb-c {
  bottom: -6%;
  left: 30%;
  background: radial-gradient(circle at 50% 50%, #f59e0b66, transparent 60%);
  animation-duration: 16s;
}
@keyframes float {
  to {
    transform: translate3d(0, -20px, 0) scale(1.05);
    opacity: 0.65;
  }
}

/* NAV */
.nav {
  position: sticky;
  top: 0;
  z-index: 50;
  background: rgba(2, 6, 23, 0.55);
  backdrop-filter: blur(10px) saturate(1.2);
  border-bottom: 1px solid var(--border);
  padding-top: env(safe-area-inset-top);
}
:root[data-theme="light"] .nav {
  background: rgba(255, 255, 255, 0.8);
}
.nav-row {
  display: flex;
  align-items: center;
  justify-content: space-between;
  height: 64px;
}
.brand {
  font-weight: 800;
  letter-spacing: 0.3px;
  cursor: pointer;
}
.nav-links {
  display: flex;
  align-items: center;
  gap: 12px;
  flex: 1;
  justify-content: space-evenly;
  flex-wrap: wrap;
}
.nav a {
  height: 100%;
  color: var(--text);
  opacity: 1;
  padding: 8px 10px;
  border-radius: 10px;
  background: var(--chip);
  border: 1px solid var(--border);
  text-decoration: none;
}
.nav a:hover {
  border-color: #646cff;
}
.icon {
  display: flex;
  justify-items: center;
  align-items: center;
  aspect-ratio: 1/1;
  width: 100%;
  height: 45px;
  padding: 8px;
}
.icon svg {
  width: 24px;
  height: 24px;
}
.theme-toggle {
  border: 1px solid var(--border);
  background: var(--panel);
  color: var(--text);
  border-radius: 8px;
  cursor: pointer;
  box-shadow: var(--shadow-sm);
}
.menu-toggle {
  display: none;
  border: 1px solid var(--border);
  background: var(--panel);
  color: var(--text);
  border-radius: 8px;
  padding: 6px 8px;
  cursor: pointer;
  box-shadow: var(--shadow-sm);
}
.nav-actions {
  display: flex;
  align-items: center;
  gap: 8px;
}

/* HERO */
.hero {
  padding: 70px 0;
}
.hero-inner {
  display: grid;
  grid-template-columns: 1.4fr 0.9fr;
  gap: 28px;
  align-items: center;
}
.hero-text h1 {
  margin: 0 0 6px;
  font-size: clamp(28px, 6vw, 44px);
  line-height: 1.1;
  text-shadow: 0 6px 20px rgba(2, 8, 23, 0.35);
}
.hero-text h2 {
  margin: 0 0 8px;
  font-weight: 700;
  font-size: clamp(16px, 3.2vw, 22px);
  color: var(--muted);
}
.tagline {
  color: var(--muted);
  max-width: 720px;
  font-size: 16px;
  line-height: 1.6;
}
.pill {
  display: inline-block;
  background: var(--chip);
  border: 1px solid var(--border);
  padding: 6px 10px;
  border-radius: 999px;
  margin-bottom: 10px;
  font-size: 12px;
  color: var(--muted);
  box-shadow: var(--shadow-sm);
}
.hero-cta {
  display: flex;
  gap: 12px;
  flex-wrap: wrap;
  margin: 16px 0 20px;
  justify-content: space-evenly;
}
.hero-cta .btn {
  min-width: 160px;
  padding: 8px 12px;
}
.hero-avatar img {
  width: 100%;
  max-width: 320px;
  border-radius: 18px;
  border: 1px solid var(--border);
  display: block;
  margin-left: auto;
  box-shadow: var(--shadow);
  transform: perspective(900px) rotateY(-8deg) rotateX(2deg);
}

/* BUTTONS */
.btn {
  appearance: none;
  border: none;
  outline: none;
  cursor: pointer;
  padding: 10px 14px;
  border-radius: 10px;
  color: #0b1220;
  font-weight: 700;
  letter-spacing: 0.2px;
  background: linear-gradient(180deg, var(--brand), #0284c7);
  box-shadow: 0 10px 20px rgba(14, 165, 233, 0.25),
    inset 0 0 0 1px rgba(148, 163, 184, 0.2);
  transition: transform 0.12s ease, filter 0.2s ease, box-shadow 0.2s ease;
}
.btn:hover {
  transform: translateY(-1px);
  filter: brightness(1.08);
  box-shadow: 0 14px 30px rgba(14, 165, 233, 0.3);
}
.btn.secondary {
  background: linear-gradient(180deg, #1f2937, #0b1220);
  color: var(--text);
}
:root[data-theme="light"] .btn.secondary {
  background: #f1f5f9;
  color: #0f172a;
  border: 1px solid var(--border);
  box-shadow: none;
}
.btn.tertiary {
  background: transparent;
  color: var(--text);
  border: 1px solid var(--border);
  box-shadow: none;
}
.btn.neutral {
  background: var(--chip);
  color: var(--text);
  border: 1px solid var(--border);
  box-shadow: none;
}

/* SECTION */
.section {
  padding: 56px 0;
}
.section-title {
  margin: 0 0 18px;
  font-size: 18px;
  letter-spacing: 0.3px;
  text-transform: uppercase;
  color: var(--muted);
}

/* SKILLS */
.chips {
  display: flex;
  gap: 12px;
  flex-wrap: wrap;
}
.chip {
  display: inline-flex;
  align-items: center;
  padding: 10px 12px;
  border-radius: 14px;
  border: 1px solid var(--border);
  background: linear-gradient(
    180deg,
    rgba(148, 163, 184, 0.15),
    rgba(148, 163, 184, 0.08)
  );
  box-shadow: var(--shadow-sm);
  transform: perspective(900px);
  transition: transform 0.2s ease, box-shadow 0.2s ease, filter 0.2s ease;
}
.chip-icon {
  margin-right: 8px;
  font-size: 1.2rem;
}
.chip .emoji {
  width: 1.2rem;
  height: 1.2rem;
  margin-right: 8px;
  vertical-align: -0.2em;
}
.chip-logo {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 22px;
  height: 22px;
  margin-right: 8px;
}
.chip-logo img {
  width: 100%;
  height: 100%;
  object-fit: contain;
  display: block;
  filter: drop-shadow(0 1px 1px rgba(0, 0, 0, 0.2));
}
.chip-label {
  font-size: 13px;
}
.chip.elevate {
  box-shadow: var(--shadow);
}

/* PROJECTS */
.projects {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: 18px;
}
.project {
  background: linear-gradient(
    180deg,
    rgba(148, 163, 184, 0.1),
    rgba(148, 163, 184, 0.06)
  );
  border: 1px solid var(--border);
  border-radius: 16px;
  padding: 16px;
  display: flex;
  flex-direction: column;
  box-shadow: var(--shadow-sm);
  transform: perspective(900px);
  transition: transform 0.2s ease, box-shadow 0.2s ease, filter 0.2s ease;
}
.project.elevate {
  box-shadow: var(--shadow-xl);
}
.project-head {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  gap: 10px;
}
.project-title {
  margin: 0;
  font-size: 16px;
}
.badges {
  display: flex;
  gap: 6px;
  flex-wrap: wrap;
}
.badge {
  background: var(--badge);
  border: 1px solid var(--border);
  border-radius: 999px;
  padding: 4px 8px;
  font-size: 12px;
  color: var(--muted);
}
.project-desc {
  color: var(--muted);
  min-height: 40px;
}
.project-actions {
  margin-top: auto;
  display: flex;
  gap: 8px;
}
.project-actions button {
  padding: 8px 12px;
}

/* CONTACT */
.contact {
  text-align: center;
}
.contact-actions {
  display: flex;
  gap: 12px;
  flex-wrap: wrap;
  justify-content: center;
}
.contact-actions .btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 8px 16px;
  gap: 4px;
}

.contact-actions .btn svg {
  margin-right: 6px;
}

/* FOOTER */
.footer {
  border-top: 1px solid var(--border);
  padding: 18px 0 24px;
  text-align: center;
  color: var(--muted);
}

/* ANIMATIONS */
.fade-in {
  opacity: 0;
  transform: translateY(20px);
  transition: opacity 0.6s ease, transform 0.6s ease;
}
.fade-in.visible {
  opacity: 1;
  transform: translateY(0);
}

/* Enhanced hover effects */
.brand:hover {
  text-shadow: 0 0 10px var(--brand);
}
.nav-links .icon:hover svg {
  animation: pulse 1s infinite;
}
@keyframes pulse {
  0%,
  100% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.1);
  }
}
.btn:active {
  transform: translateY(0) scale(0.98);
}

/* Accessibility and device optimizations */
.section {
  scroll-margin-top: 80px;
}

/* Reduced motion support */
@media (prefers-reduced-motion: reduce) {
  .orb {
    animation: none !important;
  }
  .chip,
  .project,
  .btn,
  .fade-in {
    transition: none !important;
  }
  .grid3d {
    transform: none !important;
  }
  .nav a:hover,
  .brand:hover,
  .theme-toggle:hover {
    transform: none !important;
  }
  .nav-links .icon:hover svg {
    animation: none !important;
  }
}

/* Tablet refinements */
@media (max-width: 768px) {
  .hero {
    padding: 60px 0;
  }
  .section {
    padding: 44px 0;
  }
  .projects {
    gap: 14px;
  }
  .nav-row {
    height: 60px;
    position: relative;
  }
  .menu-toggle {
    display: inline-flex;
    align-items: center;
    justify-content: center;
  }
  .nav-links {
    position: absolute;
    left: 0;
    right: 0;
    top: 100%;
    background: var(--panel);
    border-bottom: 1px solid var(--border);
    box-shadow: var(--shadow);
    padding: 12px 16px;
    display: none;
    flex-direction: column;
    gap: 8px;
  }
  .nav-links.open {
    display: flex;
  }
  .nav-links a {
    text-align: left;
  }
  .nav-links .theme-toggle {
    display: none;
  }
}

/* RESPONSIVE */
@media (max-width: 1024px) {
  .hero-inner {
    grid-template-columns: 1fr;
  }
  .projects {
    grid-template-columns: repeat(2, minmax(0, 1fr));
  }
}
@media (max-width: 640px) {
  .hero-cta .btn,
  .contact-actions .btn {
    flex: 1 1 100%;
  }
  .hero-text h1 {
    font-size: clamp(24px, 7.5vw, 32px);
  }
  .hero-avatar img {
    transform: none;
    margin-left: auto;
    margin-right: auto;
    max-width: 260px;
  }
  .chip {
    padding: 8px 10px;
  }
  .chip-logo {
    width: 18px;
    height: 18px;
    margin-right: 6px;
  }
  .chip-label {
    font-size: 12px;
  }
  .projects {
    grid-template-columns: 1fr;
  }
}
</style>
