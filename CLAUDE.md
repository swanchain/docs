# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is the **Swan Chain documentation repository** (github.com/swanchain/docs), a GitBook-based documentation site for the Swan Chain decentralized AI computing blockchain ecosystem. There is no build system, test suite, or linting — this is a pure Markdown documentation repo hosted via GitBook.

## Documentation Structure

- **SUMMARY.md** — The GitBook table of contents. This file defines the sidebar navigation structure. **Every new page must be added here** or it won't appear in the published docs.
- **README.md** — The landing page (Getting Started).
- **bulders/** — Builder guides (note: intentional spelling "bulders", not "builders"). Contains guides for DApp developers, App developers, Computing Providers (FCP/ECP), Storage Providers, Market Providers, Node Operators, and Developer Tools (SDK, Console, etc.).
- **core-concepts/** — Conceptual documentation: protocol stack layers, tokenomics, computing/storage layers, glossary.
- **network-reference/** — Chain IDs, RPC URLs, contract addresses, fees.
- **swan-chain-campaign/** — Campaign/event-specific docs (mainnet launch, testnets, accelerator programs).
- **resource/** — External links and brand kit.
- **computing-provider/** — Additional computing provider docs (ECP FAQ). Note this is a separate top-level directory from `bulders/computing-provider/`.
- **.gitbook/assets/** — Images and media referenced by documentation pages.

## GitBook Conventions

- Pages use **YAML frontmatter** (e.g., `description: ...`) at the top of files.
- GitBook-specific syntax is used throughout:
  - `{% hint style="info" %}...{% endhint %}` for callout boxes
  - `{% embed url="..." %}` for embedded content
  - `<figure><img src="..."><figcaption>...</figcaption></figure>` for images with captions
  - `<table data-view="cards">` and `<table data-header-hidden>` for special table layouts
  - Image paths use `.gitbook/assets/` (relative) or `https://docs.swanchain.io/~gitbook/image?url=...` (absolute GitBook CDN)
  - Anchor links use the format `<a href="#id" id="id"></a>`
- Internal links are **relative paths** to `.md` files (e.g., `../bulders/market-provider/`).

## Key Editing Workflows

- **Adding a new page**: Create the `.md` file in the appropriate directory, then add an entry in `SUMMARY.md` at the correct nesting level.
- **Updating navigation order**: Edit `SUMMARY.md` — the order of entries there determines sidebar order.
- **Adding images**: Place files in `.gitbook/assets/` and reference them with relative paths like `../.gitbook/assets/filename.png`.
- **Cross-referencing**: Use relative Markdown links between pages (e.g., `[link text](../path/to/page.md)`).

## Important Notes

- The `bulders/` directory name is intentionally misspelled (not "builders") — do not rename it as it would break all existing links and GitBook references.
- There are two separate computing provider directories: `bulders/computing-provider/` (main guides) and `computing-provider/` (top-level, contains ECP FAQ). The top-level one is referenced from SUMMARY.md as `computing-provider/edge-computing-provider-ecp/ecp-faq.md`.
- Swan Chain Mainnet Chain ID is **254**; Proxima Testnet Chain ID is **20241133**.
- **Swan 2.0 / Inference Cloud** documentation lives in `core-concepts/swan-2.0-inference-cloud.md` and `core-concepts/market-provider/inference-marketplace.md`. Swan 2.0 introduces a dual-token payment model (stablecoins + SWAN tokens), contribution-based rewards replacing UBI, and a unified Computing Provider (CP) role merging ECP/FCP. The existing UBI docs are kept intact and framed as the bootstrap phase; Swan 2.0 sections are additive, not replacements.
