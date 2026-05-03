# ADK-Go Skills

Kumpulan modul "Skills" untuk pengembangan agen menggunakan **ADK-Go** (Agent Development Kit). Repositori ini berisi panduan modular dan pola implementasi untuk mempercepat pembuatan agen AI yang cerdas dan fungsional.

## 🛠 Peran Gemini CLI (Staging & Gatekeeper)
Dalam repositori ini, Gemini CLI berperan sebagai **Staging Environment** dan **Gatekeeper** sebelum perubahan di-push ke GitHub:
1. **Validasi**: Mengecek sintaks `SKILL.md` dan struktur folder.
2. **Review**: Memastikan instruksi dalam skill logis dan mengikuti standar ADK-Go.
3. **Automasi**: Mengelola file `.gitignore` dan `.geminiignore` untuk menjaga efisiensi konteks.

## 🚀 Cara Registrasi Skills
Anda dapat menambahkan skill dari repositori ini ke agen Anda (seperti Claude Code atau Gemini CLI) menggunakan **Skills CLI**:

### Menggunakan GitHub URL
```bash
npx skills add https://github.com/Faishalbhitex/adk-go-skills/tree/main/skills/nama-skill
```

### Menggunakan Local Path
Jika Anda sudah men-clone repositori ini secara lokal:
```bash
npx skills add ./skills/nama-skill
```

## 📂 Manajemen File Ignore
- **`.gitignore`**: Standar Git untuk mencegah file sampah (seperti `node_modules`) masuk ke repositori.
- **`.geminiignore`**: Khusus untuk Gemini CLI. File yang terdaftar di sini tidak akan dibaca oleh AI, berguna untuk menghemat token dan menjaga privasi data sensitif.

## 📤 Prosedur Push Project
Gemini CLI melakukan push menggunakan protokol **SSH** untuk keamanan tanpa password:
1. Pastikan SSH Key sudah terdaftar di GitHub.
2. Remote URL menggunakan format `git@github.com:...`.
3. Gemini CLI akan melakukan `git add`, `git commit`, dan `git push` setelah mendapatkan konfirmasi dari Anda.

---
## Modul Tersedia
- **adk-go-harness**: Pengaturan dasar core runner, session, dan loop eksekusi.
- **adk-go-skills**: Implementasi SkillToolset untuk memuat instruksi (`SKILL.md`) secara dinamis.
- **adk-go-tools**: Pembuatan tool kustom dan sistem konfirmasi Human-in-the-Loop (HITL).
- **adk-go-mcp**: Integrasi toolset berbasis Model Context Protocol (MCP).
- **adk-go-a2a**: Implementasi komunikasi antar agen (Agent-to-Agent) menggunakan JSON-RPC.

---
*Dibuat dan dikelola oleh Gemini CLI.*
