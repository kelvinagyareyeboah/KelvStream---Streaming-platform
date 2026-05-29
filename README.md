<div align="center">

```
██╗  ██╗███████╗██╗     ██╗   ██╗███████╗████████╗██████╗ ███████╗ █████╗ ███╗   ███╗
██║ ██╔╝██╔════╝██║     ██║   ██║██╔════╝╚══██╔══╝██╔══██╗██╔════╝██╔══██╗████╗ ████║
█████╔╝ █████╗  ██║     ██║   ██║███████╗   ██║   ██████╔╝█████╗  ███████║██╔████╔██║
██╔═██╗ ██╔══╝  ██║     ╚██╗ ██╔╝╚════██║   ██║   ██╔══██╗██╔══╝  ██╔══██║██║╚██╔╝██║
██║  ██╗███████╗███████╗ ╚████╔╝ ███████║   ██║   ██║  ██║███████╗██║  ██║██║ ╚═╝ ██║
╚═╝  ╚═╝╚══════╝╚══════╝  ╚═══╝  ╚══════╝   ╚═╝   ╚═╝  ╚═╝╚══════╝╚═╝  ╚═╝╚═╝     ╚═╝
```

**FAANG-grade adaptive bitrate streaming · HLS transcoding pipeline · luxury dark UI**

[![React](https://img.shields.io/badge/React-18.x-61dafb?style=flat-square&logo=react&logoColor=white&labelColor=161b22)](https://react.dev)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.x-3178c6?style=flat-square&logo=typescript&logoColor=white&labelColor=161b22)](https://www.typescriptlang.org)
[![Vite](https://img.shields.io/badge/Vite-4.x-646cff?style=flat-square&logo=vite&logoColor=white&labelColor=161b22)](https://vitejs.dev)
[![Node.js](https://img.shields.io/badge/Node.js-16+-3fb950?style=flat-square&logo=node.js&logoColor=white&labelColor=161b22)](https://nodejs.org)
[![FFmpeg](https://img.shields.io/badge/FFmpeg-HLS%20pipeline-007808?style=flat-square&logo=ffmpeg&logoColor=white&labelColor=161b22)](https://ffmpeg.org)
[![License](https://img.shields.io/badge/License-MIT-bc8cff?style=flat-square&labelColor=161b22)](./LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-39d353?style=flat-square&labelColor=161b22)](./CONTRIBUTING.md)

</div>

![KelvStream Banner](banner.png)

---

## `$ whoami`

**KelvStream** is a premium, high-performance streaming platform that demonstrates the full client-to-server lifecycle of modern web video delivery. It features a live **HLS transcoding pipeline** powered by FFmpeg, adaptive bitrate switching, YouTube v3 API integration, and a bespoke luxury violet/obsidian UI — built entirely with React, TypeScript, and a custom CSS design system.

> *"Not a tutorial clone. A production-grade streaming architecture."*

---

## `$ ls features/`

### 🎬 Adaptive Bitrate Streaming (HLS)
The platform implements a real, local HLS transcoding pipeline — the same architecture used by Netflix, YouTube, and Twitch:

- **FFmpeg Transcoding** — uploaded `.mp4` files are processed by a custom Express backend that spawns an async FFmpeg child process
- **Dual-Profile Segmenting** — raw media is split into 6-second `.ts` chunks across two quality profiles:
  - `720p HD` — high quality, high bitrate
  - `360p SD` — standard quality, low bandwidth
- **Master Manifest** — a `master.m3u8` acts as the entry point for the ReactPlayer component to perform client-side **ABR shifts** in real time
- **Local HLS Badge** — uploaded videos surface at the top of the feed tagged `⚡ Local HLS`

### 🎨 Premium Design System
- Deep obsidian body (`#0a0a0f`) with ultraviolet (`#7c3aed`) and pink neon (`#c084fc`) gradients
- Glassmorphism navbars and sidebars with `backdrop-filter: blur(20px)`
- Micro-animations on hover — play button overlays, card translations, pulsating pipeline loaders
- Fully responsive — flex + grid layouts for mobile and desktop

### 📡 YouTube v3 API Integration
Browse, search, and play remote YouTube content alongside local HLS streams — all in one unified feed.

---

## `$ cat ARCHITECTURE.md`

```
[ UPLOAD ]  →  [ EXPRESS SERVER ]  →  [ FFMPEG CHILD PROCESS ]
                                              │
                          ┌───────────────────┴──────────────────┐
                          │                                       │
                    [ 720p HD ]                            [ 360p SD ]
                    .ts chunks                             .ts chunks
                          │                                       │
                          └───────────────┐───────────────────────┘
                                          │
                                  [ master.m3u8 ]
                                          │
                                  [ React Client ]
                                  ReactPlayer (HLS.js)
                                          │
                              [ ABR — auto quality switch ]
```

---

## `$ ls project/`

```
youtube-clone-main/
├── client/                         # React + TypeScript frontend (Vite)
│   ├── src/
│   │   ├── components/             # Navbar, Feed, VideoDetail, Player, etc.
│   │   └── utils/                  # API services (YouTube v3 + local HLS)
│   └── public/                     # Icons and static assets
└── server/                         # Node.js + Express backend
    ├── uploads/                    # Temporary raw video storage
    └── transcoded/                 # HLS manifests (.m3u8) + segments (.ts)
```

---

## `$ cat INSTALL.md`

### Prerequisites

| Requirement | Version | Install |
|---|---|---|
| Node.js | v16+ | [nodejs.org](https://nodejs.org) |
| Yarn | latest | `npm install -g yarn` |
| FFmpeg | any | see below |

#### Install FFmpeg

```bash
# macOS
brew install ffmpeg

# Windows (winget)
winget install Gyan.FFmpeg
# then add FFmpeg /bin to your System PATH

# Ubuntu / Debian
sudo apt install ffmpeg
```

---

## `$ cat START.md`

### Step 1 — Backend (Transcoding Server)

```bash
cd server
npm install
npm start
```

> Server runs at `http://localhost:5000`
> Hosts the **Transcoding Control Panel** for raw video submission and pipeline monitoring.

---

### Step 2 — Frontend Client

```bash
cd client
yarn install
```

Create a `.env` file inside `client/`:

```env
VITE_REACT_APP_RAPID_API_KEY=your_rapidapi_key_here
```

> Get your free key at [rapidapi.com](https://rapidapi.com) → search **YouTube v3**

```bash
yarn dev
```

> Client runs at `http://localhost:5173`

---

## `$ cat VERIFY.md`

```
[1]  Open  →  http://localhost:5000
[2]  Upload any .mp4 file via the Transcoding Control Panel
[3]  Wait for FFmpeg to finish segmenting
[4]  Open  →  http://localhost:5173
[5]  Your video appears at the top of the feed  ⚡ Local HLS
[6]  Click to play — ABR kicks in automatically
```

---

## `$ cat package.json | jq '.dependencies'`

### Client

| Package | Purpose |
|---|---|
| `react` + `react-dom` | UI framework |
| `typescript` | Type safety |
| `vite` | Dev server + bundler |
| `react-player` | HLS.js-powered video player |
| `axios` | YouTube v3 API requests |

### Server

| Package | Purpose |
|---|---|
| `express` | HTTP server + upload routes |
| `multer` | Raw video file handling |
| `fluent-ffmpeg` | FFmpeg child process wrapper |
| `cors` | Cross-origin for local dev |

---

## `$ cat ROADMAP.md`

```
[✓] HLS transcoding pipeline (720p + 360p)
[✓] Adaptive bitrate switching (ABR)
[✓] YouTube v3 API integration
[✓] Master manifest (master.m3u8)
[✓] Local HLS feed badge + priority surfacing
[✓] Luxury glassmorphism design system
[ ] DASH streaming support
[ ] Live stream ingestion (RTMP)
[ ] User authentication + upload history
[ ] Cloud transcoding (AWS MediaConvert)
[ ] CDN delivery via CloudFront / Cloudflare
[ ] Multi-resolution profiles (1080p, 4K)
[ ] Subtitle / caption track support
```

---

## `$ cat CONTRIBUTING.md`

Pull requests, bug reports, and feature suggestions are welcome.

```bash
# Fork → clone → branch
git checkout -b feat/your-feature

# Install all deps
cd client && yarn install
cd ../server && npm install

# Run both in parallel (two terminals)
npm start          # from /server
yarn dev           # from /client

# Before submitting
yarn lint
yarn build         # ensure no type errors
```

---

## `$ cat LICENSE`

MIT License — © 2025 [Agyare Kelvin Yeboah](https://kelvinagyareyeboah.me)

Free to use, modify, and distribute with attribution.

---

## `$ whoami --links`

<div align="center">

[![GitHub](https://img.shields.io/badge/GitHub-kelvinagyareyeboah-161b22?style=flat-square&logo=github&logoColor=white)](https://github.com/kelvinagyareyeboah)
[![Twitter](https://img.shields.io/badge/Twitter-@_yo_kelvin-161b22?style=flat-square&logo=x&logoColor=white)](https://x.com/_yo_kelvin)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-agyarekelvinyeboah-0a66c2?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/agyarekelvinyeboah)
[![Website](https://img.shields.io/badge/Website-kelvinagyareyeboah.me-3fb950?style=flat-square&logo=safari&logoColor=white)](https://kelvinagyareyeboah.me)
[![Zoharix](https://img.shields.io/badge/Company-zoharix.tech-bc8cff?style=flat-square&logo=vercel&logoColor=white)](https://zoharix.tech)

---

*built with intention by [@kelvinagyareyeboah](https://github.com/kelvinagyareyeboah) · co-founder @ [Zoharix](https://zoharix.tech)*

</div>
