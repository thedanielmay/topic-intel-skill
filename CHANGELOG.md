# Changelog

## 2.4.0 - 2026-11-23

- Layer 4 (Playwright) is opt-in. Name the source and why before running it, and run it only after the user says yes for that source.
- Log a gap with the reason when the user says no or Playwright is not installed, and skip Layer 4 when Playwright is missing.
- Removed the headed login capture and headless session reuse template, and every instruction to save or load a browser session.
- Removed browser evasion and its install check. Layer 4 now runs as a normal browser that identifies as automation.
- LinkedIn is not a Layer 4 target. Use only public content from Layers 1 to 3, or content the user pastes.
- Replaced the hard-coded Python path with one detection step: `python3 -c "import playwright"`. No machine-specific paths remain.
- The access rule now respects each site's terms and robots rules.

## 2.3.0 - 2026-11-23

- Bundles the three shared reference files (intelligence standards, web access layers, layer 4 Playwright protocol), so the skill works without files outside the repo.

## 2.2.0 - 2026-11-23

- Moved the seven topic type modules and the multi-module chaining protocol into references/topic-modules.md.
- Moved the Phase 11 synthesis and expert review procedure into references/topic-synthesis.md.
- Cut the SKILL.md body below 500 lines and added one-line pointers to the new reference files.

## 2.1.3 - 2026-11-23

- Adds the marketplace icon approved by Daniel (thedanielmay/agent-tools#2).

## 2.1.2 - 2026-11-23

- Marketplace file gains a description, so the full check passes again (thedanielmay/agent-tools#10).

## 2.1.1 - 2026-11-22

- Added the marketplace files: marketplace.json, LICENSE and CHANGELOG.md.
- Added license and metadata.version to the SKILL.md frontmatter.
- Removed the email address from plugin.json, added the author url, homepage, repository and license, and set the plugin name to topic-intel.
- Added the plugin install line to README.md and removed the version number from its prose.
- Added .gitignore.

## 2.1.0

- added a dependency check for fact-check-workflow, literature-review, document-skills:pdf, data-visualization and expert-panel.

## 2.0.0

- added the anti-hallucination protocol (Cite-Before-You-Write, Numeric Quarantine, Fabrication Detection Pass, SCI Citation Manifest, POL Verbatim Provision Rule, Module Contamination Firewall).
