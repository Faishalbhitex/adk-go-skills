# ADK-Go Skills

Kumpulan modul "Skills" untuk pengembangan agen menggunakan **ADK-Go** (Agent Development Kit). Repositori ini berisi panduan modular dan pola implementasi untuk mempercepat pembuatan agen AI yang cerdas dan fungsional.

## 🚀 Instalasi via Skills CLI (`npx skills`)

Anda dapat mengelola skill dalam repositori ini menggunakan **Skills CLI**. CLI ini mendukung instalasi ke berbagai agen seperti Claude Code, Cursor, Gemini CLI, dan lainnya.

### 1. Lihat Daftar Skill Tersedia
Gunakan perintah ini untuk melihat semua modul yang ada di repositori ini tanpa menginstalnya:
```bash
npx skills add https://github.com/Faishalbhitex/adk-go-skills --list
```

### 2. Instalasi Semua Skill
Untuk menginstal seluruh modul ke semua agen yang terdeteksi:
```bash
# Instalasi ke level Proyek (lokal)
npx skills add https://github.com/Faishalbhitex/adk-go-skills --all

# Instalasi secara Global (user-level)
npx skills add https://github.com/Faishalbhitex/adk-go-skills --all -g
```

### 3. Instalasi Skill Tertentu
Jika Anda hanya membutuhkan modul spesifik (misalnya hanya `adk-go-mcp`):
```bash
npx skills add https://github.com/Faishalbhitex/adk-go-skills --skill adk-go-mcp
```

### 4. Instalasi dari Local Path
Jika Anda sudah men-clone repositori ini secara lokal:
```bash
npx skills add ./skills/adk-go-tools
```

## 📂 Manajemen Proyek
- **`.gitignore`**: Mencegah file internal (seperti `GEMINI.md`) masuk ke repositori publik.
- **`.geminiignore`**: Mengoptimalkan pembacaan konteks oleh AI dengan mengabaikan file yang tidak relevan.

## 📤 Prosedur Kontribusi
1. Lakukan perubahan pada folder `skills/`.
2. Pastikan setiap modul memiliki file `SKILL.md` dengan YAML frontmatter.
3. Gunakan Gemini CLI untuk validasi otomatis dan push ke branch `main`.

---
## Modul Tersedia
- **adk-go-harness**: Pengaturan dasar core runner, session, dan loop eksekusi.
- **adk-go-skills**: Implementasi SkillToolset untuk memuat instruksi (`SKILL.md`) secara dinamis.
- **adk-go-tools**: Pembuatan tool kustom dan sistem konfirmasi Human-in-the-Loop (HITL).
- **adk-go-mcp**: Integrasi toolset berbasis Model Context Protocol (MCP).
- **adk-go-a2a**: Implementasi komunikasi antar agen (Agent-to-Agent) menggunakan JSON-RPC.

---
*Dikelola menggunakan ADK-Go & Gemini CLI.*
