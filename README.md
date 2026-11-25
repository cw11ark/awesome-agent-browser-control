# Awesome Agent Browser Control (updated 2026-11-25)

> Pick the right browser-control layer for an AI agent — MCP server, SDK, CDP API, or a purpose-built headless browser.

Every agent that touches the web needs a control layer, and the wrong one costs you either tokens or determinism. This page sorts the 2026 field by **what actually drives the browser**: an accessibility tree over MCP, a typed SDK that mixes natural-language steps with Playwright code, a raw CDP/WebSocket API, or a browser written from scratch for automation. Each entry states the control surface, the token cost model, and the case where it is the wrong pick. Star counts and activity dates are as of 2026-11-25; nothing here is a benchmark we ran ourselves.

> [!NOTE]
> Compiled from public repository documentation — no independent performance testing of the linked projects.

## Contents

- [🧭 Pick by control surface, not by star count](#pick-by-control-surface-not-by-star-count)
- [🏗️ Hosted or self-hosted browser infrastructure](#hosted-or-self-hosted-browser-infrastructure)
- [⚙️ Deterministic automation underneath the agent](#deterministic-automation-underneath-the-agent)
- [🔗 Related notes in this series](#related-notes-in-this-series)

## 🧭 Pick by control surface, not by star count

Four different things get called a "browser agent". Decide which one you need before comparing repositories.

| Project | What it is | Tags |
|---|---|---|
| [browser-use](https://github.com/browser-use/browser-use) | browser-use gives the model one persistent Python REPL over a real Chrome session through CDP, with helper bindings for clicks, JS evaluation and tabs. Right pick when the navigation path is unknown at build time; wrong pick when the page is a static table you could scrape deterministically. | `agent-loop`, `python` |
| [Playwright MCP](https://github.com/microsoft/playwright-mcp) | Playwright MCP exposes Playwright as an MCP server over accessibility snapshots, so the model reads structure instead of pixels. Its own README argues CLI+skills beat MCP for high-throughput coding agents — read that trade-off before wiring it in. | `mcp`, `accessibility-tree` |
| [Stagehand](https://github.com/browserbase/stagehand) | Stagehand is an SDK where individual actions can be either declared in code or delegated to a model, so the deterministic parts stay deterministic. Right pick when only a few steps are genuinely ambiguous. | `hybrid`, `typescript` |
| [Lightpanda](https://github.com/lightpanda-io/browser) | Lightpanda is a browser written in Zig for automation rather than a Chromium fork; the project claims 16x lighter and 9x faster than Chromium. Relevant when browser memory, not model calls, is the bottleneck — verify the claim on your own pages. | `headless`, `zig` |

## 🏗️ Hosted or self-hosted browser infrastructure

When the problem is not "which model drives the page" but "where do the browser sessions run".

| Project | What it is | Tags |
|---|---|---|
| [Steel Browser](https://github.com/steel-dev/steel-browser) | Steel wraps a managed browser session behind an HTTP API, so your agent talks to a session endpoint instead of spawning processes. Right pick when you need many concurrent sessions and want to own the runtime. | `self-hosted`, `session-api` |
| [browserless](https://github.com/browserless/browserless) | browserless ships headless browsers as a deployable container with connection limits and queueing. Right pick when the constraint is concurrency and isolation, not agent reasoning. | `docker`, `pool` |
| [Skyvern](https://github.com/Skyvern-AI/skyvern) | Skyvern targets browser-based workflows rather than single-page extraction, with the model handling page variation inside a defined flow. Right pick when the task is a repeatable business process, not a one-off read. | `workflow`, `automation` |

## ⚙️ Deterministic automation underneath the agent

The layer an agent should call when the page is stable. Cheap, auditable, and the thing most "browser agent" comparisons skip.

| Project | What it is | Tags |
|---|---|---|
| [Puppeteer](https://github.com/puppeteer/puppeteer) | Puppeteer is the JavaScript API over CDP. If your locators are stable, this is the cheapest control layer per action — no model in the loop and no token cost per step. | `cdp`, `javascript` |
| [WebdriverIO](https://github.com/webdriverio/webdriverio) | WebdriverIO is the automation framework when the requirement is a maintained grid across browsers and devices. Right pick when the organization already has Selenium/Appium infrastructure to reuse. | `webdriver`, `grid` |

## 🔗 Related notes in this series

Adjacent write-ups that answer the questions this page deliberately does not cover.

| Project | What it is | Tags |
|---|---|---|
| [API relay signup walkthrough](https://github.com/cw11ark/ai-relay-guide) | Step-by-step signup and key-management notes for API relay services, for the case where the agent's model calls have to go through a relay rather than a first-party endpoint. | `relay`, `setup` |
| [Relay outage log](https://github.com/cw11ark/relay-outage-log) | Dated record of relay outages and the symptoms that showed up first, so a failing agent run can be checked against a known upstream incident instead of being debugged as a browser bug. | `uptime`, `incident` |
| [Node availability checks](https://github.com/cw11ark/jichang-node-availability) | How to measure whether a proxy node is actually reachable from your network before blaming the automation layer for a timeout. | `network`, `diagnostics` |

---

## About this list

- **12** entries across **4** sections.
- Compiled from public sources; every linked project carries its own license.
- Last updated: **2026-11-25**.
- Data lives in `data/content.json` — this README is generated, do not edit by hand.

## License

The text and curation in this repository are licensed under CC BY 4.0. Linked projects keep their own licenses.
