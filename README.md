# RESUMER — AI-Powered Resume Analyzer

> **Track Your Applications & Resume Ratings — Smarter.**  
> Upload your resume, get an instant ATS score, and receive AI-driven improvement tips tailored to your dream job.

# 🚀 Resumind AI

<p align="center">
  <a href="https://ai-resume-analyser-five-nu.vercel.app/">
    <img src="https://img.shields.io/badge/🔥%20Try%20Live%20Demo-Now-blue?style=for-the-badge&logo=vercel" />
  </a>
</p>

---

## 📸 Screenshots

| Home Page                            | Upload Page                            |
| ------------------------------------ | -------------------------------------- |
| ![Home](public/images/resume_01.png) | ![Upload](public/images/resume_02.png) |

> **Home** — View all tracked applications and resume ratings at a glance.  
> **Upload** — Submit your resume with job details and get instant AI feedback.

---

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Environment Variables](#environment-variables)
- [Core Workflows](#core-workflows)
- [Component Reference](#component-reference)
- [AI Feedback System](#ai-feedback-system)
- [PDF Processing Pipeline](#pdf-processing-pipeline)
- [Storage Architecture](#storage-architecture)
- [Routing](#routing)
- [Error Handling](#error-handling)
- [Docker Deployment](#docker-deployment)
- [Contributing](#contributing)
- [License](#license)

---

## 🧠 Overview

**Resumer** is a full-stack, AI-powered resume management and job application tracking web app. It allows users to:

- Upload resumes in PDF format
- Receive a real-time **ATS (Applicant Tracking System) score**
- Get **AI-generated improvement suggestions** matched against a specific job description
- **Track multiple applications** — each resume is stored with company name, job title, and feedback
- Preview resumes as images directly in the browser

The app is built on [Remix](https://remix.run/) (React-based SSR framework) with Tailwind CSS for styling, `pdfjs-dist` for client-side PDF rendering, and a custom service layer (`usePuterStore`) for authentication, file storage, key-value persistence, and AI inference.

---

## ✨ Features

| Feature                    | Description                                                               |
| -------------------------- | ------------------------------------------------------------------------- |
| 🔐 **Authentication**      | Secure user login/logout with redirect-based auth guard                   |
| 📄 **Resume Upload**       | Upload PDF resumes up to 20 MB via click or drag-and-drop                 |
| 🤖 **AI Feedback**         | ATS score + detailed improvement suggestions powered by AI                |
| 🖼️ **PDF Preview**         | First page of uploaded PDF auto-converted to image for card preview       |
| 📁 **Resume Gallery**      | Dashboard showing all uploaded resumes with scores and metadata           |
| 💾 **Persistent Storage**  | All resumes, images, and feedback stored via custom fs/kv abstraction     |
| 📊 **Score Visualization** | Circular score gauge and color-coded score badges                         |
| 🔍 **Detailed View**       | Full feedback breakdown per resume including ATS analysis and suggestions |
| 🗑️ **Data Management**     | Wipe route for clearing stored data during development/testing            |
| 📱 **Responsive UI**       | Mobile-friendly layout with Tailwind CSS utility classes                  |

---

## 🛠️ Tech Stack

| Layer                  | Technology                                      | Purpose                                       |
| ---------------------- | ----------------------------------------------- | --------------------------------------------- |
| **Frontend Framework** | [Remix](https://remix.run/) (v2)                | SSR routing, meta tags, data loading          |
| **UI Library**         | [React](https://react.dev/)                     | Component-based UI and state management       |
| **Styling**            | [Tailwind CSS](https://tailwindcss.com/)        | Utility-first CSS framework                   |
| **PDF Processing**     | [pdfjs-dist](https://mozilla.github.io/pdf.js/) | Client-side PDF parsing and canvas rendering  |
| **State & Services**   | Custom `usePuterStore` hook                     | Auth, file system, key-value store, AI access |
| **AI Service**         | Custom `ai.feedback` integration                | Resume analysis and scoring                   |
| **File Storage**       | Custom `fs` abstraction                         | File upload and retrieval                     |
| **Key-Value Store**    | Custom `kv` abstraction                         | Structured resume data persistence            |
| **Build Tool**         | [Vite](https://vitejs.dev/)                     | Fast HMR dev server and optimized builds      |
| **Type System**        | [TypeScript](https://www.typescriptlang.org/)   | Static typing across all source files         |
| **Containerization**   | [Docker](https://www.docker.com/)               | Consistent deployment environment             |

---

## 📁 Project Structure

```
resumer/
├── app/
│   ├── app.css                    # Global styles
│   ├── root.tsx                   # App root, global layout/providers
│   ├── routes.ts                  # Route definitions
│   │
│   ├── components/                # Reusable UI components
│   │   ├── Accordion.tsx          # Collapsible section component
│   │   ├── ATS.tsx                # ATS score display section
│   │   ├── Details.tsx            # Resume detail breakdown
│   │   ├── FileUploader.tsx       # Drag-and-drop PDF uploader
│   │   ├── Navbar.tsx             # Top navigation bar
│   │   ├── ResumeCard.tsx         # Card for gallery view
│   │   ├── ScoreBadge.tsx         # Color-coded score label
│   │   ├── ScoreCircle.tsx        # Circular score indicator
│   │   ├── ScoreGauge.tsx         # Gauge-style score visualization
│   │   └── Summary.tsx            # AI feedback summary display
│   │
│   ├── lib/                       # Utility and service libraries
│   │   ├── pdf2image.ts           # PDF → PNG conversion via pdfjs-dist
│   │   ├── puter.ts               # Puter service initializer/wrapper
│   │   └── utils.ts               # Shared utility functions
│   │
│   ├── routes/                    # Remix file-based routes
│   │   ├── auth.tsx               # Login/authentication page
│   │   ├── home.tsx               # Resume gallery dashboard
│   │   ├── resume.tsx             # Individual resume detail view
│   │   ├── upload.tsx             # Resume upload and analysis page
│   │   └── wipe.tsx               # Dev utility: clear all stored data
│   │
│   └── welcome/                   # Welcome/splash assets
│       ├── logo-dark.svg
│       ├── logo-light.svg
│       └── welcome.tsx
│
├── constants/
│   └── index.ts                   # App-wide constants (KV prefixes, etc.)
│
├── types/
│   ├── index.d.ts                 # Global type declarations
│   └── puter.d.ts                 # Type definitions for Puter services
│
├── public/
│   ├── favicon.ico
│   ├── pdf.worker.min.js          # PDF.js worker (CommonJS)
│   ├── pdf.worker.min.mjs         # PDF.js worker (ESM)
│   ├── icons/                     # SVG icon assets
│   │   ├── ats-bad.svg
│   │   ├── ats-good.svg
│   │   ├── ats-warning.svg
│   │   ├── back.svg
│   │   ├── check.svg
│   │   ├── cross.svg
│   │   ├── info.svg
│   │   ├── pin.svg
│   │   └── warning.svg
│   └── images/                    # Static image assets
│       ├── bg-auth.svg
│       ├── bg-main.svg
│       ├── bg-small.svg
│       ├── pdf.png
│       ├── resume_01.png
│       ├── resume_02.png
│       ├── resume_03.png
│       ├── resume-scan.gif
│       └── resume-scan-2.gif
│
├── Dockerfile                     # Docker container config
├── package.json
├── package-lock.json
├── react-router.config.ts         # React Router v7 config
├── tsconfig.json
└── vite.config.ts                 # Vite build config
```

---

## 🚀 Getting Started

### Prerequisites

- **Node.js** v18 or later
- **npm** v9 or later (or `pnpm` / `yarn`)
- A modern browser with JavaScript enabled

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/your-username/resumer.git
cd resumer

# 2. Install dependencies
npm install

# 3. Start the development server
npm run dev
```

The app will be available at `http://localhost:5173` by default.

### Production Build

```bash
# Build for production
npm run build

# Start the production server
npm start
```

---

## 🔑 Environment Variables

Create a `.env` file in the project root. The following variables may be required depending on your AI and storage backend configuration:

```env
# AI Service configuration (if using a remote API)
AI_API_KEY=your_ai_api_key_here
AI_API_URL=https://your-ai-endpoint.com

# Storage configuration (if using cloud storage)
STORAGE_BUCKET=your_bucket_name

# App config
NODE_ENV=development
```

> **Note:** The app uses a custom `usePuterStore` hook that abstracts these services. Check `app/lib/puter.ts` and `types/puter.d.ts` for the full service interface.

---

## 🔄 Core Workflows

### 1. Authentication Flow

```
User visits app
    ↓
usePuterStore().auth → check auth state
    ↓
Not authenticated → redirect to /auth?next=/
Authenticated     → render requested page
```

- Auth state is managed centrally via `usePuterStore`
- The `next` query param ensures the user is returned to their intended page after login

---

### 2. Resume Upload & Analysis Flow

```
User fills form (Company, Job Title, Job Description)
    ↓
Selects PDF file (max 20 MB)
    ↓
Form submitted
    ├── fs.upload(pdfFile)          → stores raw PDF
    ├── pdf2image(pdfFile)          → renders page 1 to canvas → PNG blob
    ├── fs.upload(imageBlob)        → stores preview image
    ├── Generate unique resume ID
    ├── ai.feedback(resumeText, jobDescription) → ATS score + suggestions
    └── kv.set("resume:<id>", resumeData)       → persist full record
    ↓
Navigate to /resume?id=<id>
```

---

### 3. Resume Gallery Flow

```
User visits / (home)
    ↓
kv.list("resume:*") → fetch all resume records
    ↓
Render <ResumeCard> for each:
    ├── Company name + Job title
    ├── Preview image (first page of PDF)
    └── AI score via <ScoreCircle>
    ↓
Click card → navigate to /resume?id=<id>
```

---

### 4. Resume Detail Flow

```
User visits /resume?id=<id>
    ↓
kv.get("resume:<id>") → load stored resume record
    ↓
Render full feedback:
    ├── <Summary>    — Overall AI summary
    ├── <ATS>        — ATS score breakdown
    ├── <Details>    — Improvement suggestions
    └── <ScoreGauge> — Visual score indicator
```

---

## 🧩 Component Reference

### `<Navbar />`

Top navigation bar displaying the **RESUMER** logo and the **Upload Resume** button. Present on all authenticated pages.

### `<ResumeCard />`

Displays a single resume entry in the gallery grid.

| Prop     | Type           | Description                           |
| -------- | -------------- | ------------------------------------- |
| `resume` | `ResumeRecord` | Full resume data object from kv store |

Renders: preview image, company name, job title, and `<ScoreCircle>`.

### `<ScoreCircle />`

Circular SVG-based score indicator showing the ATS score percentage visually.

| Prop    | Type     | Description       |
| ------- | -------- | ----------------- |
| `score` | `number` | ATS score (0–100) |

### `<ScoreGauge />`

Arc-shaped gauge with needle indicator for the resume detail view.

### `<ScoreBadge />`

Color-coded badge for score categories:

- 🟢 Green — High score (≥ 80)
- 🟡 Yellow — Medium score (50–79)
- 🔴 Red — Low score (< 50)

### `<FileUploader />`

Drag-and-drop and click-to-upload component for PDF files.

| Prop           | Type                   | Description                                |
| -------------- | ---------------------- | ------------------------------------------ |
| `onFileSelect` | `(file: File) => void` | Callback when a file is chosen             |
| `accept`       | `string`               | MIME types (defaults to `application/pdf`) |
| `maxSize`      | `number`               | Max file size in bytes (default 20 MB)     |

### `<ATS />`

Renders the ATS score section with category breakdown icons (`ats-good.svg`, `ats-warning.svg`, `ats-bad.svg`).

### `<Summary />`

Displays the top-level AI narrative feedback for the resume.

### `<Details />`

Accordion-based section showing granular improvement tips returned by the AI.

### `<Accordion />`

Generic collapsible section used by `<Details>` for expandable feedback categories.

---

## 🤖 AI Feedback System

The AI feedback pipeline is invoked via the `ai.feedback()` method from `usePuterStore`.

### Input

```typescript
ai.feedback({
  resumeText: string, // Extracted text content of the uploaded PDF
  jobTitle: string, // Job title entered by the user
  companyName: string, // Company name entered by the user
  jobDescription: string, // Job description entered by the user
});
```

### Output (parsed and stored)

```typescript
interface AIFeedback {
  atsScore: number; // 0–100 overall ATS compatibility score
  summary: string; // High-level resume assessment
  strengths: string[]; // List of resume strengths
  improvements: string[]; // Specific improvement suggestions
  keywordMatches: string[]; // Keywords found matching job description
  missingKeywords: string[]; // Important keywords absent from resume
}
```

### Storage

Feedback is serialized and stored alongside resume metadata in the key-value store:

```typescript
kv.set(`resume:${resumeId}`, {
  id: resumeId,
  companyName,
  jobTitle,
  jobDescription,
  pdfPath,
  imagePath,
  feedback: parsedAIFeedback,
  createdAt: Date.now(),
});
```

---

## 📄 PDF Processing Pipeline

PDF-to-image conversion is handled entirely **client-side** using `pdfjs-dist`.

### Flow (`app/lib/pdf2image.ts`)

```
PDF File (File object)
    ↓
Dynamic import of pdfjs-dist
    ↓
Set workerSrc → /pdf.worker.min.js
    ↓
pdfjsLib.getDocument(arrayBuffer)
    ↓
pdf.getPage(1)             ← First page only
    ↓
page.render({ canvasContext, viewport })
    ↓
canvas.toBlob("image/png") ← Convert to PNG blob
    ↓
Return PNG Blob for upload and preview
```

### Worker Files

The PDF.js worker is served statically from `public/` to avoid CORS issues:

```
public/pdf.worker.min.js     ← CommonJS (used in dev)
public/pdf.worker.min.mjs    ← ESM (used in production builds)
```

---

## 💾 Storage Architecture

All persistence is handled through two custom abstractions exposed by `usePuterStore`:

### File System (`fs`)

```typescript
fs.upload(file: File | Blob, path?: string): Promise<string>  // returns file path
fs.read(path: string): Promise<Blob>
```

Used to store:

- Raw uploaded PDF files
- Generated PNG preview images

### Key-Value Store (`kv`)

```typescript
kv.set(key: string, value: any): Promise<void>
kv.get(key: string): Promise<any>
kv.list(prefix: string): Promise<Record<string, any>>
kv.delete(key: string): Promise<void>
```

**Key naming convention:**

```
resume:<uuid>       ← Individual resume record
```

**Example record:**

```json
{
  "id": "a1b2c3d4-...",
  "companyName": "Acme Corp",
  "jobTitle": "Frontend Engineer",
  "jobDescription": "We are looking for...",
  "pdfPath": "/files/resumes/a1b2c3d4.pdf",
  "imagePath": "/files/previews/a1b2c3d4.png",
  "feedback": { "atsScore": 82, "summary": "...", ... },
  "createdAt": 1712345678900
}
```

---

## 🗺️ Routing

Resumer uses **Remix file-based routing**. All routes live in `app/routes/`.

| Route     | File         | Description                                   |
| --------- | ------------ | --------------------------------------------- |
| `/`       | `home.tsx`   | Resume gallery dashboard                      |
| `/auth`   | `auth.tsx`   | Login page (redirects to `?next` after login) |
| `/upload` | `upload.tsx` | Upload form and AI analysis trigger           |
| `/resume` | `resume.tsx` | Individual resume detail + full feedback      |
| `/wipe`   | `wipe.tsx`   | Dev utility — wipes all kv/fs data            |

### Auth Guard Pattern

Every protected route checks auth at the top of the component:

```typescript
const { auth, isLoading } = usePuterStore();
const navigate = useNavigate();

useEffect(() => {
  if (!isLoading && !auth.isAuthenticated) {
    navigate(`/auth?next=${encodeURIComponent(location.pathname)}`);
  }
}, [auth, isLoading]);
```

---

## ⚠️ Error Handling

| Scenario                          | Handling Strategy                                          |
| --------------------------------- | ---------------------------------------------------------- |
| PDF.js worker load failure        | Try/catch in `pdf2image.ts`, error surfaced to upload form |
| PDF conversion error              | Error state shown in upload UI, form remains interactable  |
| File upload failure (`fs.upload`) | Error state displayed, upload can be retried               |
| Image upload failure              | Non-blocking; resume still saved without preview           |
| AI feedback failure               | Error state displayed; user can re-submit                  |
| Auth failure                      | Redirect to `/auth` with `next` param                      |
| KV store failure                  | Console error logged, UI shows fallback state              |

---

## 🐳 Docker Deployment

A `Dockerfile` is included for containerized deployment.

### Build and Run

```bash
# Build the Docker image
docker build -t resumer .

# Run the container
docker run -p 3000:3000 resumer
```

### Example Dockerfile Structure

```dockerfile
FROM node:20-alpine

WORKDIR /app

COPY package*.json ./
RUN npm ci

COPY . .
RUN npm run build

EXPOSE 3000
CMD ["npm", "start"]
```

> Update the `Dockerfile` to inject your environment variables via `--env-file` or Docker secrets.

---

## 🤝 Contributing

Contributions are welcome! To get started:

1. **Fork** this repository
2. **Create** a feature branch: `git checkout -b feature/your-feature-name`
3. **Commit** your changes: `git commit -m "feat: add your feature"`
4. **Push** to your fork: `git push origin feature/your-feature-name`
5. **Open** a Pull Request

### Code Style

- TypeScript strict mode is enabled — ensure no `any` without justification
- Tailwind CSS only — avoid writing raw CSS unless extending `app.css`
- Components go in `app/components/`, utilities in `app/lib/`
- Follow existing naming conventions (PascalCase for components, camelCase for utils)

### Commit Convention

This project follows [Conventional Commits](https://www.conventionalcommits.org/):

```
feat:     New feature
fix:      Bug fix
refactor: Code change (no feature/fix)
style:    Formatting/styling only
docs:     Documentation updates
chore:    Build/tooling changes
```

---

<div align="center">
  <strong>Built with ❤️ using Remix, React, Tailwind CSS, and AI</strong>
  <br />
  <sub>Resumer — Smart feedback for your dream job</sub>
</div>
