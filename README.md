# scientific-ppt-skill

A [Claude](https://claude.com/claude-code) skill that creates and edits **scientific PowerPoint decks** in a *figure-first* chemistry-report style — the way a working chemist actually builds slides, not a clean "AI-template" dashboard.

> **Core idea:** every content slide is built around one or a few **large scientific figures** (reaction schemes, spectra, structures, mechanism diagrams, data plots), carried by a short conclusion-style **title**, **one red take-home line**, and a **citation**. Figures are dense; text is sparse.

## What's inside

```
ppt-jiantao-style/
├── SKILL.md       # the skill: philosophy, rules, workflow, verification
├── reference.md   # exact color/font/size tokens + python-pptx / pptxgenjs recipes
├── genres.md      # deck skeletons (talk / defense / lit-share / progress / self-intro / poster)
└── data-mode.md   # the rare exception: a clean native chart of your own numbers
```

## Install (Claude Code)

```bash
git clone https://github.com/Jiantao-Zhao/scientific-ppt-skill.git
cp -r scientific-ppt-skill/ppt-jiantao-style ~/.claude/skills/
```

Then just ask Claude Code things like *"make a group-meeting PPT about X"*, *"组会 PPT"*, or point it at a `.pptx` to restyle — the skill triggers automatically.

## The style in one screen

- **Figure-first.** Paste the science large; the figure carries the argument. Native objects only for the title, the red take-home line, the citation, annotation boxes/arrows, and the occasional table.
- **One conclusion title + one red take-home line.** No bullet walls, no KPI/stat cards, no thin accent rules, no tint boxes, no "Summary —" chrome.
- **16:9.** Fonts: **Arial** (+ 黑体 for Chinese, Times New Roman for citations).
- **Restrained palette:** navy `#002060` / deep blue `#000099` titles; red `#C00000` for the take-home line and to highlight a key species or draw a red box/arrow on a figure.

## How it was built (and why it matters)

The first version of this skill *failed* — it learned the "style" by parsing each `.pptx` for fonts, colors, and sizes, and never actually rendered a single slide. It produced clean, templated, sparse "data-dashboard" decks that looked nothing like the real thing.

The fix was to **render the reference decks to images and *look*** — drive LibreOffice from Python (handling Chinese/NFD filenames), tile slides into contact sheets, and study the actual layout and density. Only then did the real style — *figure-first* — become obvious.

**Lesson:** to learn a *visual* style, render and look. Metadata gives you a fingerprint, not a gestalt.

## License

MIT — see [LICENSE](LICENSE).
