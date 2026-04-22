<h1 align="center">mcp-server-weread</h1>

<p align="center">
  <em>An MCP server for WeRead (微信读书) — exposes your bookshelf, highlights, notes, reviews, and <strong>booklists</strong> to MCP-aware LLM clients (Claude Desktop, Cursor, etc.).</em>
</p>

<p align="center">
  <a href="https://opensource.org/license/MIT"><img src="https://img.shields.io/github/license/frankliu20/mcp-server-weread" alt="MIT License"></a>
  <img src="https://img.shields.io/github/stars/frankliu20/mcp-server-weread?style=flat" alt="Stars">
</p>

> 🪐 This is a community-maintained variant of the original
> [`freestylefly/mcp-server-weread`](https://github.com/freestylefly/mcp-server-weread) by [@苍何 (canghe)](https://github.com/freestylefly).
> Distributed under the **MIT License** (preserved as-is). All credit for the original
> WeRead MCP design and implementation goes to the upstream author.

---

## ✨ What's different in this variant

The upstream server already covers **bookshelf · search · notes & highlights · best reviews**.
This variant adds **booklists** (微信读书"书单"), which lets your LLM reason about
the curated lists you've built or follow — not just individual books:

| Tool | What it does |
|---|---|
| `get_booklists` | Get the user's created booklists, with summary info and preview books |
| `get_booklist_detail` | Get the full content (all books) of a specific booklist by `booklistId` |

Use cases this enables:

- *"Summarize the books I put in my 'AI engineering' booklist."*
- *"Across all my booklists, which themes show up most?"*
- *"Find duplicates between my 'must-read' and 'productivity' lists."*

---

## 🚀 Quick start

### Prerequisites

- Node.js ≥ 16
- A WeRead account + a valid login Cookie (or a CookieCloud setup — see below)

### Use with Claude Desktop / Cursor

Add this to your MCP config:

```json
{
  "mcpServers": {
    "mcp-server-weread": {
      "command": "npx",
      "args": ["-y", "mcp-server-weread"],
      "env": {
        "CC_URL": "https://cc.chenge.ink",
        "CC_ID": "<your CookieCloud UUID>",
        "CC_PASSWORD": "<your CookieCloud password>"
      }
    }
  }
}
```

Or, if you prefer to paste the cookie directly:

```json
"env": {
  "WEREAD_COOKIE": "<your WeRead cookie string>"
}
```

> 💡 CookieCloud is recommended because WeRead cookies expire often. The MCP server
> tries CookieCloud first, then falls back to `WEREAD_COOKIE`.

### Get your WeRead cookie

1. Open https://weread.qq.com in Chrome and log in.
2. F12 → Network tab → refresh.
3. Find a `weread.qq.com` request → Headers → copy the entire `Cookie` value.
4. Paste it into `WEREAD_COOKIE`.

### CookieCloud setup (optional but recommended)

CookieCloud is an open-source, self-hostable cookie sync tool. See
[CookieCloud](https://github.com/easychen/CookieCloud).

1. Install the browser extension
   ([Edge](https://microsoftedge.microsoft.com/addons/detail/cookiecloud/bffenpfpjikaeocaihdonmgnjjdpjkeo) ·
   [Chrome](https://chromewebstore.google.com/detail/cookiecloud/ffjiejobkoibkjlhjnlgmcnnigeelbdl)).
2. Server URL: use the public default `https://cc.chenge.ink` or self-host.
3. Click **Auto-generate password**.
4. Add `weread` to the synced-domains keyword field.
5. Save → Sync → put the UUID + password into the MCP env block above.

---

## 🧰 All available tools

From the upstream:

- `get_bookshelf` — every book on your shelf
- `search_books` — fuzzy/exact search by title, author, translator, category
- `get_book_notes_and_highlights` — notes & highlights, optionally grouped by chapter
- `get_book_best_reviews` — popular reviews of a book

Added in this variant:

- `get_booklists` — your created booklists (with preview books)
- `get_booklist_detail` — full contents of a booklist

---

## 📜 License

MIT — see [`LICENSE`](./LICENSE).

Original copyright © 2025 [@苍何 (canghe)](https://github.com/freestylefly).
Modifications © 2025–2026 frankliu20.
