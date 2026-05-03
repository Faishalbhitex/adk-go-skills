# ADK-Go Skills

Kumpulan modul "Skills" untuk pengembangan agen menggunakan **ADK-Go** (Agent Development Kit). Repositori ini berisi panduan modular dan pola implementasi untuk mempercepat pembuatan agen AI yang cerdas dan fungsional.

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

## 📂 Manajemen Proyek
- **`.gitignore`**: Mencegah file sampah masuk ke repositori.
- **`.geminiignore`**: Mengoptimalkan pembacaan konteks oleh AI.

## 📤 Prosedur Kontribusi
1. Lakukan perubahan pada folder `skills/`.
2. Pastikan `SKILL.md` mengikuti format YAML frontmatter.
3. Gunakan Gemini CLI untuk memvalidasi dan melakukan push ke branch `main`.

---
## Modul Tersedia
- **adk-go-harness**: Pengaturan dasar core runner, session, dan loop eksekusi.
- **adk-go-skills**: Implementasi SkillToolset untuk memuat instruksi (`SKILL.md`) secara dinamis.
- **adk-go-tools**: Pembuatan tool kustom dan sistem konfirmasi Human-in-the-Loop (HITL).
- **adk-go-mcp**: Integrasi toolset berbasis Model Context Protocol (MCP).
- **adk-go-a2a**: Implementasi komunikasi antar agen (Agent-to-Agent) menggunakan JSON-RPC.

---
*Dikelola menggunakan ADK-Go & Gemini CLI.*
