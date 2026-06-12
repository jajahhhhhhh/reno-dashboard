# Reno Dashboard Plugin

A Claude plugin for managing a multi-site home renovation project from chat. Works with the static HTML dashboard files (Kanban tasks, payment ledger, QR-coded inventory) — Claude reads them, updates them, and produces Thai/English progress reports.

Built originally for a two-site Koh Samui renovation (Lipa Noi + Chaweng) with a single contractor, but designed to be shareable for any owner running 1–3 reno sites.

---

## What it does

| Skill | What it does | Trigger |
|---|---|---|
| `reno-setup` | First-run setup — captures site names, contractor, dashboard file paths, currency, language. Writes `CONFIG.md` the other skills read. | "setup reno", "เริ่มใช้งาน" |
| `reno-status` | Read-only snapshot across tasks/payments/stock, split by site. The "what's going on right now" skill. | "ภาพรวม", "status", "ตอนนี้เป็นยังไง" |
| `reno-update-tasks` | Add / move / edit Kanban tasks. Writes back to `dashboard-final.html`. | "เพิ่มงาน", "move to done", "add task" |
| `reno-log-money` | Log a paid or pending payment; can ingest a bill (HomePro, Global House) and add it. | "บันทึกจ่าย", "log payment" |
| `reno-stock-move` | Stock in / out / adjust on inventory; flags low stock; can ingest a bill into new SKUs. | "เบิก", "ซื้อเข้า", "ของหมด" |
| `reno-ingest-chat` | Parse LINE chat or pasted text into structured tasks/payments/stock — JSON ready to import. | "อ่าน LINE", "parse this" |
| `reno-weekly-brief` | Weekly recap per site — tasks done, money paid, what's slipping, top 3 for next week. | "สรุปสัปดาห์", "weekly brief" |

---

## Requirements

- A copy of the renovation dashboard HTML files (`index.html`, `dashboard-final.html`, `inventory-system.html`). Bundled in the original repo as `reno-dashboard.zip`.
- Run `reno-setup` once on install to point the plugin at your dashboard files.

No external services required. No API keys. Everything is local file edits.

---

## Customization

This plugin uses `~~placeholders` for tool/site references so it can be adapted to different projects without code changes. See `CONNECTORS.md` for the full list (site names, contractor name, currency, language).

When you run `reno-setup`, it generates a `CONFIG.md` in your working directory that resolves these placeholders for your project. All other skills read from `CONFIG.md` automatically.

---

## Data model

The plugin works with three data structures embedded in the HTML files:

- `TASKS` (in `dashboard-final.html`) — Kanban cards: `{id, t, n, s, p, type, cost, date, pri, by}`
- `PAYS` (in `dashboard-final.html`) — Payment log: `{id, vendor, amount, status, proj, date, note}`
- `ITEMS` + `TXS` (in `inventory-system.html`) — Stock items and transactions

Edits are made to the inline `SAMPLE_TASKS` / `SAMPLE_PAYMENTS` / `SAMPLE_ITEMS` arrays. The user refreshes the browser to see updates. localStorage is preserved unless the user clears it.

---

## License

MIT
