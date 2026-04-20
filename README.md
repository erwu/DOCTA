# DOCTA — Documentation Test Analyser

> **Version:** 1.0 | **Author:** Radosław Wojciechowski  
> **Type:** Standalone offline tool | **Format:** Single HTML file, no installation required

---

## Overview

DOCTA is a standalone, offline tool for analysing software documentation and generating test artefacts using a local AI model (Ollama). No installation, no cloud connection, no subscription required — your documents never leave your computer.

---

## Key Features

- Accepts **text, PDF (.pdf), Word (.docx), and image input** (.jpg / .png / .gif / .webp)
- Generates:
  - Functional requirements (R-01, R-02...)
  - Test cases (TC-001 format) with categories: HAPPY PATH, NEGATIVE, BOUNDARY
  - Ambiguities & gaps
  - Questions for dev/analyst
  - Coverage matrix (requirements → test cases)
- Exports to **.txt, .md, .docx, .xlsx**
- **Batch mode** — analyse multiple files, export all results to a single Excel file
- Analysis **history** stored in browser localStorage (up to 50 entries)
- **Prompt templates** for consistent output style across projects
- **EN / PL** interface and output language toggle
- 100% offline — no API keys, no accounts, no internet required after setup

---

## Setup (one-time)

### 1. Install Ollama

Download and install from [https://ollama.com/download](https://ollama.com/download).  
Ollama starts automatically in the background after installation.

### 2. Start Ollama with CORS enabled

DOCTA runs in a browser and requires Ollama to allow cross-origin requests.

**Windows (Command Prompt):**
```cmd
set OLLAMA_ORIGINS=* && ollama serve
```

**Mac / Linux:**
```bash
OLLAMA_ORIGINS="*" ollama serve
```

> ⚠️ Run this command every time you restart Ollama. Consider creating a startup script or shortcut.

### 3. Pull a model

**Recommended for 8 GB RAM (text analysis):**
```bash
ollama pull qwen2.5:7b
```

**For image / screenshot analysis:**
```bash
ollama pull llava:7b
```

### 4. Open DOCTA

Open `docta.html` in any modern browser (Chrome, Firefox, Edge). No web server needed.

### 5. Verify connection

The status indicator in the top bar should show a **green dot** and _"Ollama running"_.  
If it shows red, check that Ollama is running with CORS enabled (step 2).

---

## Interface Overview

DOCTA uses a two-panel layout: sidebar navigation on the left, main content area on the right.

| Section | Description |
|---|---|
| **Analyser** | Main analysis tool — single document |
| **History** | Past analyses stored in browser localStorage |
| **Batch Analyser** | Analyse multiple files in sequence |
| **Settings** | Ollama URL, default model, prompt templates |
| **About** | Version and technical information |

---

## Usage

### Analyser Tab

Three input modes are available:

- **Paste text** — paste plain text from any source (Confluence, email, Notepad, etc.)
- **Upload file** — drag and drop or browse; supports `.txt`, `.pdf`, `.docx`
- **Image / screenshot** — upload `.jpg`, `.png`, `.gif`, `.webp`; requires a vision model (e.g. `llava:7b`)

**Steps:**
1. Paste or upload your documentation
2. Select output options (requirements, test cases, ambiguities, etc.)
3. Select output language (EN / PL)
4. Click **Analyse**
5. Export results as `.txt`, `.md`, `.docx`, or `.xlsx`

### Batch Analyser Tab

1. Drag and drop multiple `.txt`, `.pdf`, or `.docx` files
2. Select output options and language
3. Click **Analyse all**
4. Export all results to a single `.xlsx` file

### History Tab

- Every completed analysis is saved automatically
- Up to **50 entries** stored; each shows date, model, token count, and document preview
- Click any entry to reload it into the Analyser
- Export full history to `.xlsx`

---

## Prompt Templates

Templates append extra instructions to every analysis prompt for consistent output style.

| Template | Description |
|---|---|
| **API Testing** | Focus on REST API; includes HTTP status codes, schema validation notes |
| **Functional Spec** | IEEE 830 style; hierarchical numbering; flags conflicting requirements |
| **User Story** | Given/When/Then format; focuses on acceptance criteria |
| **GDPR / Security** | Highlights personal data, GDPR risks, auth/authorisation test cases |

Custom templates can be created and saved in localStorage. Only one template is active at a time.

---

## Model Reference

| Model | Size | RAM | Best for |
|---|---|---|---|
| `qwen2.5:7b` *(default)* | ~5 GB | 8 GB | Text analysis — best quality/speed balance |
| `mistral:7b` | ~4 GB | 8 GB | Fast, good English output |
| `llama3.1:8b` | ~5 GB | 8 GB | Strong reasoning |
| `llama3.2:3b` | ~2 GB | 4 GB | Fastest, lower quality |
| `phi3:mini` | ~2 GB | 4 GB | Lightweight, quick |
| `llava:7b` | ~5 GB | 8 GB | Image/screenshot analysis |
| `llava-phi3:3.8b` | ~3 GB | 6 GB | Faster vision analysis |
| `moondream:1.8b` | ~1 GB | 4 GB | Lightest vision model |
| `llava:13b` | ~8 GB | 12 GB | Best vision quality |

**Recommended for 8 GB RAM:**
- Text: `qwen2.5:7b`
- Images: `llava:7b` or `llava-phi3:3.8b`

---

## Troubleshooting

| Problem | Solution |
|---|---|
| **Red dot — Ollama not reachable** | Verify Ollama is running (`ollama list`); confirm CORS flag is set; check port 11434 is not blocked by firewall |
| **Analysis stops or returns empty** | Model may exceed available RAM — try `llama3.2:3b` or `phi3:mini`; split long documents into sections |
| **Image analysis fails** | Ensure a vision model is selected; pull it first with `ollama pull llava:7b` |
| **PDF text not extracted** | Scanned (image-only) PDFs cannot be read via Upload mode — use the Image tab instead |
| **History not persisting** | Do not use private/incognito mode; do not clear browser data between sessions |

---

## Privacy

All processing is performed **locally** via Ollama. No data is sent to any external server.  
No API keys, no accounts, no internet connection required after initial Ollama setup.

---

*DOCTA — Documentation Test Analyser | v1.0 | Author: Radosław Wojciechowski*
