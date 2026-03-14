# MediaCrawler Documentation

## Overview
MediaCrawler is a **multi‑platform social‑media data crawler** written in Python. It supports a range of popular Chinese and global platforms (Xiaohongshu, Douyin, Kuaishou, Bilibili, Weibo, Tieba, Zhihu) and is capable of collecting posts, comments, creator profiles, and media assets.

The project is designed for **learning and research** purposes only – it adheres to a non‑commercial license and requires users to respect each platform’s terms of service and `robots.txt`.

---

## Directory Structure
```
MediaCrawler/
├─ api/                # FastAPI server, routers, schemas, services, and WebUI assets
├─ base/               # Abstract base classes (Crawler, Login, Store, API client)
├─ cache/              # Local file cache and optional Redis cache
├─ cmd_arg/            # CLI argument parsing
├─ config/              # Global and platform‑specific configuration files
├─ constant/           # Platform‑specific constants
├─ database/           # ORM models and DB session handling
├─ docs/               # Documentation and usage guides (Markdown, images)
├─ media_platform/     # Platform specific crawler implementations (xhs, dy, ks, …)
├─ README.md           # Existing project README (keep unchanged)
├─ DOCS_README.md      # **This file** – detailed developer documentation
└─ ...
```

---

## Quick Start
```bash
# 1. Install uv (recommended) and dependencies
pip install uv
uv sync                 # install Python dependencies
uv run playwright install   # install browser binaries

# 2. Configure the crawler (edit config/base_config.py)
#    - Choose platform, keywords, login method, etc.

# 3. Run a crawl (example: Xiaohongshu keyword search)
uv run python main.py --platform xhs --lt qrcode --type search \
    --keywords "AI,爬虫" --headless false

# 4. (Optional) Launch the Web UI to control the crawler
uv run uvicorn api.main:app --port 8080 --reload
# Open http://localhost:8080 in a browser.
```

---

## Architecture
- **Web UI** (Vite) → **FastAPI** (REST + WebSocket) → **Crawler Engine** (Playwright) → **Storage** (JSON / CSV / SQLite / MySQL / MongoDB).
- The engine is built on abstract base classes defined in `base/base_crawler.py`; each platform implements its own logic under `media_platform/<platform>/core.py`.
- `api/services/crawler_manager.py` runs the crawler as a separate subprocess, captures stdout, stores logs, and pushes them to WebSocket clients.
- Configuration lives in `config/base_config.py` with per‑platform overrides (`config/xhs_config.py`, etc.).

---

## Development
- **Run tests** (if you add any): `uv run pytest`
- **Code style** is enforced via pre‑commit (`black`, `isort`, `flake8`, `mypy`).
- Follow Conventional Commits for PRs.
- When adding a new platform:
  1. Create `media_platform/<new>/core.py` inheriting the abstract classes.
  2. Add a config file `config/<new>_config.py` and import it in `base_config.py`.
  3. Update the OpenAPI enums if needed.

---

## License & Disclaimer
This project is released under the **NON‑COMMERCIAL LEARNING LICENSE 1.1**. It is intended for educational and research use only. Users must not employ the crawler for commercial purposes or to violate any platform’s policies. See the full license text in the repository root (`LICENSE`).

---

## Contact & Contributions
- GitHub: https://github.com/NanmiCoder/MediaCrawler
- Issues & feature requests: open an issue on GitHub.
- Pull requests are welcome – please run the pre‑commit checks before submitting.

---

*Generated with Claude Code*