# Connectors

## How tool references work

Plugin files use `~~category` as a placeholder for whatever tool, site, or value the user configures. Plugins are project-agnostic — they describe workflows in terms of categories rather than specific names.

When you run `reno-setup`, the skill generates a `CONFIG.md` in your working directory mapping each placeholder to your real values. All other skills read from `CONFIG.md` and substitute at runtime.

## Placeholders used by this plugin

| Category         | Placeholder         | Example values                                       |
| ---------------- | ------------------- | ---------------------------------------------------- |
| Primary site     | `~~site-a`          | "Lipa Noi", "Site A", "House 1"                      |
| Secondary site   | `~~site-b`          | "Chaweng", "Site B", "House 2" (omit if single site) |
| Contractor       | `~~contractor`      | "MR.HOME", "Khun Somchai", "ABC Builders"            |
| Currency         | `~~currency`        | "THB", "USD", "EUR"                                  |
| Currency symbol  | `~~currency-symbol` | "฿", "$", "€"                                        |
| Output language  | `~~language`        | "Thai", "English", "Bilingual TH/EN"                 |
| Dashboard folder | `~~dashboard-path`  | Absolute path to folder containing the HTML files    |

## Single-site mode

If you only have one site, leave `~~site-b` blank in `CONFIG.md`. The plugin will skip per-site splits and report everything as a single project.

## Multi-contractor mode (not yet supported)

The current plugin assumes one primary contractor. If you have several, list them comma-separated in `~~contractor` and the skills will route work by inferring from context (vendor name in payments, assignee in tasks). Full multi-contractor support is on the roadmap.
