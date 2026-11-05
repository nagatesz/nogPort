# 🎮 nogPort — Ported Games in Your Browser

[![Live Site](https://img.shields.io/badge/Live_Site-nogport.vercel.app-7928CA?style=for-the-badge&logo=vercel)](https://nogport.vercel.app/)
[![GitHub](https://img.shields.io/badge/GitHub-nagatesz%2FnogPort-181717?style=for-the-badge&logo=github)](https://github.com/nagatesz/nogPort)

> **nogPort** is a web portal that brings legendary PC games straight to your browser using WebAssembly, WebGL, and modern web APIs. No local installation required — click and play.

---

## 🕹️ Ported Games Catalog

| Game | Status | Live URL | Engine / Technology |
|---|---|---|---|
| **Terraria** | 🟢 Live | [nogterraria.vercel.app](https://nogterraria.vercel.app/) | .NET / FNA / Wasm |
| **Geometry Dash** | 🟡 In Progress | [noggd.vercel.app](https://noggd.vercel.app/) | OpenGD (Axmol) / Emscripten C++ |
| **GTA III / San Andreas** | 🟡 In Progress | [noggta3.vercel.app](https://noggta3.vercel.app/) | re3 / librw / Emscripten WebGL |

---

## 🛠️ How Each Port Was Made

### 1. 🌲 Terraria (`nogterraria.vercel.app`)
- **Runtime:** Built by cross-compiling the C# / FNA XNA-reimplementation runtime into **WebAssembly** via .NET Wasm toolchains.
- **Rendering:** Translates XNA/FNA `SpriteBatch` and primitive drawing into a high-performance **WebGL 2.0** render loop.
- **Storage:** Uses the browser's IndexedDB to emulate the local `.plr` and `.wld` character and world save system.
- **Tip:** *If the game takes time on the initial load screen, keep clicking or waiting — the browser is allocating virtual memory for the C# runtime heap.*

---

### 2. 🔷 Geometry Dash — nogGD (`noggd.vercel.app`)
- **Engine:** Built on top of **OpenGD Remastered**, an open-source recreation of Geometry Dash built in modern C++ with the **Axmol Engine** (a fork of Cocos2d-x).
- **Compilation:**
  - Compiled using the **Emscripten SDK** (`emcc` / `em++`) targeting `wasm32`.
  - Enables POSIX threads via `SharedArrayBuffer` with mandatory security headers:
    ```http
    Cross-Origin-Opener-Policy: same-origin
    Cross-Origin-Embedder-Policy: require-corp
    ```
- **Automated Resource Delivery:**
  - Game resources (~150MB of high-definition sheets, plists, and audio) are split into two parallel streams: `resources_assets.zip` (sprites, fonts, level layouts) and `resources_audio.zip` (music, sound effects).
  - Streamed directly into the browser and extracted on-the-fly using the native Web **`DecompressionStream('deflate-raw')`** API.
  - Written directly into Emscripten's virtual filesystem (`/axmolPersistPath/GeometryDash/Resources`).
  - Cached permanently in **IndexedDB (IDBFS)** so subsequent loads are instantaneous.
- **Custom Level Support:**
  - Built-in decoder for Geometry Dash `.dat` save files (`CCLocalLevels.dat`) and exported `.gmd` / `.xml` level files using XOR key 11 deobfuscation and gzip decompression in JavaScript.

---

### 3. 🚗 GTA III & San Andreas (`noggta3.vercel.app`)
- **Engine:** Based on reverse-engineered source trees (**re3** and **reVC**), ported to run in a web canvas via Emscripten.
- **Audio & Render Pipeline:**
  - Utilizes **librw** mapped to WebGL 2.0 for 3D model transforms, texture mapping, and lighting.
  - Custom Web Audio API backend handles 3D positional audio and multi-channel vehicle radio streams.
  - Dynamic streaming pulls game archives into virtual filesystem memory as player travels across Liberty City and San Andreas.

---

## ⚡ Tech Stack & Architecture

- **Core:** WebAssembly (C++, C#, Emscripten, .NET Wasm)
- **Graphics:** WebGL 1.0 / 2.0
- **Audio:** Web Audio API, Ogg Vorbis & MP3 decoding
- **Storage:** IndexedDB Virtual Filesystem (`FS.syncfs` / IDBFS)
- **Deployment:** Vercel Global Edge Network with zero-auth public delivery

---

## 👥 Credits

Built by:
- **nog** ([@nagatesz](https://github.com/nagatesz))
- **rea** ([@morluto/rea](https://github.com/morluto/rea))

---

## 📜 License

Each game engine and port retains the licenses and copyrights of its respective original creators and reverse-engineering teams (Re-Logic, Axmol Team / OpenGD, Rockstar Games / GTA re3 project). nogPort is an educational web compilation.
