# Dev Tools

> Collection of handy online tools for developers, with great UX.

[![License](https://img.shields.io/badge/license-GPLv3-blue.svg)](LICENSE)

A modern web-based toolbox for developers, featuring **90+ tools** across various categories — from encoding/decoding and format conversion to cryptography and network utilities. Built with Vue 3, Vite, Naive UI, and UnoCSS.

## ✨ Features

- 🔧 **90+ Developer Tools** — converters, generators, encoders, validators, and more
- 🎨 **Professional Blue Theme** — clean, modern UI with dark/light mode support
- 📱 **PWA Support** — installable on desktop and mobile devices
- 🌍 **Internationalization** — Chinese & English support
- ⚡ **Lightning Fast** — built with Vite for instant HMR and optimized builds
- 🎯 **Great UX** — keyboard shortcuts, copy-to-clipboard, searchable command palette

## 🧰 Tools Categories

| Category | Tools |
|----------|-------|
| **Crypto** | Token Generator, Hash Text, Bcrypt, UUID/ULID Generator, Encryption, BIP39, HMAC, RSA Key Pair, Password Strength, PDF Signature Checker |
| **Converter** | DateTime, Base, Roman Numeral, Base64 (String/File), Color, Case, NATO Alphabet, Text ↔ Binary/Unicode, YAML/JSON/TOML/XML, Markdown to HTML |
| **Web** | URL Encoder/Parser, HTML Entities, Device Info, Basic Auth, Meta Tags, OTP, MIME Types, JWT Parser, Keycode Info, Slugify, WYSIWYG Editor, User Agent, HTTP Status, JSON Diff |
| **Images & Videos** | QR Code/WiFi QR Generator, SVG Placeholder, Camera Recorder |
| **Development** | Git Memo, Port Generator, Crontab, JSON Viewer/Minify/CSV, SQL Formatter, Chmod Calculator, Docker Run → Compose, XML Formatter, YAML Viewer, Regex Tester |
| **Network** | IPv4 Subnet/Address/Range, MAC Address Generator/Lookup, IPv6 ULA |
| **Math** | Math Evaluator, ETA Calculator, Percentage Calculator |
| **Measurement** | Chronometer, Temperature Converter, Benchmark Builder |
| **Text** | Lorem Ipsum, Text Statistics, Emoji Picker, String Obfuscator, Text Diff, Numeronym, ASCII Drawer |
| **Data** | Phone Parser, IBAN Validator |

## 🚀 Getting Started

### Prerequisites

- [Node.js](https://nodejs.org/) >= 18
- [pnpm](https://pnpm.io/) >= 9

### Development

```bash
# Install dependencies
pnpm install

# Start dev server
pnpm dev

# Build for production
pnpm build

# Preview production build
pnpm preview
```

### Run Tests

```bash
# Unit tests
pnpm test:unit

# E2E tests
pnpm test:e2e

# Coverage report
pnpm coverage
```

## 🏗️ Tech Stack

| Technology | Purpose |
|------------|---------|
| [Vue 3](https://vuejs.org/) | UI framework (Composition API + `<script setup>`) |
| [Vite](https://vitejs.dev/) | Build tool & dev server |
| [TypeScript](https://www.typescriptlang.org/) | Type safety |
| [Naive UI](https://www.naiveui.com/) | Component library |
| [UnoCSS](https://unocss.dev/) | Atomic CSS engine |
| [Pinia](https://pinia.vuejs.org/) | State management |
| [Vue Router](https://router.vuejs.org/) | Routing |
| [vue-i18n](https://vue-i18n.intlify.dev/) | Internationalization |
| [Monaco Editor](https://microsoft.github.io/monaco-editor/) | Code editor |
| [PWA](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps) | Offline & installable |

## 📦 Deployment

### Docker

```bash
docker build -t dev-tools .
docker run -p 8080:80 dev-tools
```

### Static Hosting

The `dist/` directory after `pnpm build` can be deployed to any static hosting service (Vercel, Netlify, GitHub Pages, etc.).

## 📝 License

[GNU GPLv3](LICENSE)
