# Nur

**Nur** is a local AI assistant for everyday office work on Windows. It runs Gemma 4 through Ollama: no API keys, no cloud, your text never leaves the machine. The interface is in Russian, with quick tasks built for Russian and Uzbek speakers.

[Русская версия](README.ru.md)

## What it does

### AI tasks (local Gemma 4)
- **Chat**: free conversation with the model, history saved automatically
- **Emails**: drafts by topic, tone, recipient and language (RU / UZ / EN)
- **Summarize**: short versions of long texts, with length and format options
- **Translate**: RU ↔ UZ (Latin and Cyrillic) ↔ EN
- **Improve**: 7 modes: grammar, shorter, longer, more formal, friendlier, simpler, rewrite

### Local utilities (no AI, no internet)
- **Converter**: length, weight, volume, area, temperature, currencies (USD/EUR/RUB/UZS/KZT/GBP/TRY/CNY), file sizes, numbers in words (Russian and Uzbek)
- **Transliteration**: Russian Cyrillic → Latin, Uzbek Cyrillic ↔ Latin (with oʻ / gʻ)

### System
- **History**: every conversation, searchable, stored in `%AppData%/Nur/history.json`
- **Settings**: Gemma model management (E2B / E4B / 26B), Ollama launcher

## Stack

- Electron 39 + React 19 + TypeScript 5.9
- Ollama (local runtime) + Gemma 4 (E2B / E4B / 26B)
- Vite 7, Vitest, electron-builder (NSIS)

## Development

```sh
npm install
npm run dev
```

## Checks

```sh
npm run typecheck   # TypeScript
npm run test        # Vitest (38 tests)
npm run build       # production build
```

## Windows installer

```sh
npm run build:win
```

The installer is written to `release/` as `Nur-<version>-x64.exe`.

## First run

1. Start Nur
2. Open **Settings**
3. Install Ollama with the "Download Ollama" button if you do not have it
4. Start Ollama and press "Prepare Gemma": the model downloads locally (about 3 to 9 GB)
5. A short Russian check confirms the model answers correctly
6. Go back to **Chat** and start working

## Architecture

- `src/main/`: Electron main process: IPC handlers, Ollama HTTP client, file storage
- `src/preload/`: the `window.os1` bridge with a typed API
- `src/renderer/`: React UI
  - `components/`: shared pieces (Sidebar, BootScreen, AiOutputPanel, ...)
  - `views/`: screens (ChatView, EmailView, SummarizeView, ...)
  - `hooks/`: `useGemma` for AI tasks
  - `utils/`: converters, transliteration, numbers in words
- `src/shared/`: types and validation for IPC payloads

## Windows SmartScreen

Early builds are not code-signed, so SmartScreen may warn on first launch. Only use installers from this repository's GitHub Releases.

## Roadmap

- Streaming model answers word by word
- Uzbek interface through i18n
- Drag and drop files (.txt, .docx, .pdf) into Chat and Summarize
- Templates for recurring emails
- Voice input (local Whisper)
