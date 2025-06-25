llms.txt – AI‑Friendly Index for Your Docs

Purpose: Give large‑language‑model clients (IDEs, chatbots, autonomous agents) a single entry point so they can find your most relevant documentation and code samples without crawling your whole site.

This repository stores the canonical llms.txt file for mcpo (MCP‑to‑OpenAPI proxy) plus helper scripts and publishing recipes. The proxy itself lives in a separate repo—here we focus purely on the index file.

📋 What is llms.txt?

llms.txt plays the same role for AI that robots.txt plays for web crawlers. It is a short Markdown document, placed at the web root (https://<host>/llms.txt), that lists links—and only links—to richer docs. Keeping the index lean lets tools decide what to fetch next and avoids blowing past token limits.

Key points

Element

Required

Example

Title (# Project Name)

✔

# mcpo – MCP‑to‑OpenAPI Proxy

One‑line summary (> …)

✔

> Exposes any MCP server as REST & Swagger

Section headers (## Guides)

✖



Bullet links (- [Label](URL): note)

✖

- [Quick Start](README.md): local dev

See the draft file for a real example.

🚀 Quick Start (creating the file)

# 1) Copy the template
cp samples/llms-skeleton.txt llms.txt

# 2) Tweak links & blurbs in your editor

# 3) Preview what an LLM will see
uvx lint-llms llms.txt   # simple regex sanity checks

Tip: For large doc sites generate the index automatically:

./scripts/build-index.sh ./docs > llms.txt

🌐 Publishing Options

Scenario

Command

Result

GitHub Pages

Enable Settings → Pages

https://<user>.github.io/<repo>/llms.txt

Docker volume

docker cp llms.txt open-webui:/app/backend/data/

Served by Open WebUI at /llms.txt

Sidecar Nginx

docker run -d -p 8088:80 -v $(pwd)/llms.txt:/usr/share/nginx/html/llms.txt nginx:alpine

http://<host>:8088/llms.txt

Local test

python -m http.server

http://localhost:8000/llms.txt

🔍 Who Consumes llms.txt Today?

Cursor IDE – Auto‑imports project docs for its AI chat window.

Open WebUI – Shows linked docs in the sidebar and forwards them to the underlying model.

JetBrains AI Assistant – Parses on project open to seed its context DB.

Claude Desktop – Fetches external sources on first mention.

If you use any of these tools you can test instantly:

Cursor ▸ Resources ▸ Add URL … ▸ https://<host>/llms.txt

✏️ Updating the Index

Keep it short. Aim for ≤ 25 links total; move verbose content to separate .md files.

Use stable URLs. Prefer latest/ docs or permalinks.

Version with your code. Bump links when tagging a new release.

Run ./scripts/check-links.py in CI to fail the build if any URL 404s.

🤝 Contributing

Pull requests are welcome! Please open an issue first if you plan a major schema change. We follow the simple rule: one concept per line, no nested lists, so parsing stays trivial.

📄 License

llms.txt and helper scripts are released under the MIT License. See LICENSE.

