# Research Paper Humanizer: A Claude Skill to Humanize AI-Assisted Academic Writing

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Claude Skill](https://img.shields.io/badge/Claude-Skill-orange.svg)](research-paper-humanizer/SKILL.md)
[![Academic Writing](https://img.shields.io/badge/For-IEEE%20%7C%20Q1%20Journals-green.svg)](#who-is-this-for)

**Research Paper Humanizer** is an evidence-based [Claude](https://claude.ai) skill. It rewrites AI-assisted research papers so they read like an expert human author wrote them. It lowers AI-detection scores on **Turnitin, iThenticate, GPTZero, Originality.ai and Copyleaks** without changing your meaning, numbers, citations, technical terms or formal academic register.

Generic "AI humanizers" add opinions, casual voice, typos or synonym swaps. In a research paper those changes damage quality and can get flagged as misconduct. This skill does the opposite. It applies the structural, discourse-level rewriting that corpus studies of LLM-written science show actually matters, and it keeps every fact frozen.

> Built and tested while preparing IEEE conference papers in machine learning and explainable AI, then rebuilt on a literature review of AI-text detection research.

---

## Table of Contents

1. [What it does](#what-it-does)
2. [Why it is different from other AI humanizers](#why-it-is-different-from-other-ai-humanizers)
3. [Who is this for](#who-is-this-for)
4. [Installation](#installation)
5. [How to use](#how-to-use)
6. [Before and after example](#before-and-after-example)
7. [What it will never do](#what-it-will-never-do)
8. [Ethics and AI disclosure](#ethics-and-ai-disclosure)
9. [Evidence base](#evidence-base)
10. [FAQ](#faq)
11. [Author](#author)

---

## What it does

1. **Removes AI writing patterns found in research.** It cuts excess style words (delve, underscore, showcase, pivotal, intricate, meticulous), trailing participle clauses (", highlighting..."), nominalization chains, stacked transitions and generic openers and closers.
2. **Rebuilds sentence structure.** It reorders information using Gopen and Swan's topic and stress positions, varies sentence length (burstiness) and varies paragraph shape.
3. **Freezes every technical token.** Numbers, statistics, p-values, hyperparameters, equations, citations, dataset names, model names, metric names and terms of art stay exactly as written.
4. **Verifies after every pass.** It checks claim direction and strength, quantifiers, model-family labels and word count. A humanization pass never makes the text longer.
5. **Gives section-by-section guidance** for the Abstract, Introduction (Swales CARS model), Related Work, Methodology, Results, Discussion, Limitations and Conclusion.
6. **Includes a plateau protocol.** It explains why scores often stall around 20% on Turnitin and how to fix only the flagged sentence clusters instead of rewriting the whole paper again.
7. **Has a check-only mode.** When you rewrite or translate a passage yourself, it fixes only factual and mechanical errors (reversed findings, wrong numbers, causal overclaims, citation format) and leaves your phrasing alone.

## Why it is different from other AI humanizers

| Typical AI humanizer | Research Paper Humanizer |
|---|---|
| Swaps synonyms, which can create "tortured phrases" | Freezes technical terms; changes structure first and words last |
| Adds opinions, humor, "I" and casual tone | Keeps formal research register ("we" and passive for procedures) |
| Ignores meaning drift | Verifies every claim, number and citation after each pass |
| Uses typos, homoglyphs or invisible characters | Bans all evasion tricks |
| Output is a training target for Turnitin's bypasser detection | Produces author-quality prose that improves the paper |
| No disclosure guidance | Gives IEEE, Elsevier, ACM and Springer Nature disclosure guidance |

## Who is this for

1. Researchers, PhD students and master's students writing for **IEEE conferences, Q1 journals, Elsevier, Springer, ACM** and similar venues.
2. Non-native English academic writers. Detectors flag them more often because constrained, predictable prose looks "AI-like" (Liang et al., *Patterns*, 2023).
3. Anyone who drafts with ChatGPT, Claude or Gemini and wants the final paper to read as their own polished scholarly writing.

## Installation

### Claude (web and desktop apps)

1. Download [`research-paper-humanizer.zip`](research-paper-humanizer.zip) from this repository.
2. Upload the zip in the Skills section of Claude's settings.
3. Make sure the skill is enabled.

### Claude Code

```bash
git clone https://github.com/noobarafat/research-paper-humanizer.git
mkdir -p ~/.claude/skills
cp -r research-paper-humanizer/research-paper-humanizer ~/.claude/skills/
```

For a project-level install, copy the folder into `.claude/skills/` inside your project instead.

### Other agents

The skill is a single Markdown file, [`research-paper-humanizer/SKILL.md`](research-paper-humanizer/SKILL.md). Any agent that supports Agent Skills, or any LLM given the file as instructions, can use it.

## How to use

Paste a section of your paper and ask Claude to humanize it, or invoke the skill by name:

```text
/research-paper-humanizer
<paste your Related Work section here>
```

By default you get the final rewrite, ready to paste, followed by one check line:

```text
Words: 214 -> 201 | Numbers, citations, terms: unchanged
```

Other modes:

| Mode | What it does |
|---|---|
| `audit` | Lists the remaining AI tells sentence by sentence, without rewriting |
| `native` | Writes the passage in your first language so you can translate it into English yourself |
| `check` | Fixes only factual and mechanical errors in text you wrote or translated |
| `trim to N` | Cuts the passage to N words or fewer with no content loss |

**Tip for stubborn scores:** Turnitin shows scores from 1% to 19% only as "*%". If your paper is stuck around 20%, send only the sentences Turnitin highlights rather than the whole section.

## Before and after example

**Before (57 words, AI-typical):**

> In recent years, explainable artificial intelligence has garnered significant attention, playing a pivotal role in high-stakes domains. Notably, SHAP has emerged as a comprehensive framework for interpreting model predictions, highlighting the contribution of each feature. Furthermore, the integration of SHAP with gradient boosting enables a deeper understanding of complex decision processes, underscoring its potential for real-world deployment.

**After (51 words, same claims):**

> Explainable artificial intelligence has drawn significant attention and plays a central role in high-stakes domains. SHAP has become a general framework for interpreting model predictions. It assigns each feature a contribution. Paired with gradient boosting, it clarifies how complex models reach decisions. This gives the combination potential for real-world deployment.

The rewrite removes the generic opener, three participle tails, two stacked transitions and four marker words. Sentence lengths now vary. Every claim keeps its original strength.

## What it will never do

1. Use homoglyphs, zero-width or invisible characters, or white text.
2. Insert deliberate typos or grammar errors.
3. Swap synonyms for technical terms ("deep learning" never becomes "profound learning").
4. Run text through commercial humanizers or paraphrasers.
5. Use automated machine-translation round trips.
6. Fabricate data, citations, hedges or examples.
7. Advise hiding AI use where a venue requires disclosure.

## Ethics and AI disclosure

This skill removes **false AI signals** from content the author owns and has verified. It does not replace your venue's AI-use policy.

1. **IEEE** requires AI-generated content to be disclosed in the Acknowledgments. The disclosure names the system, the sections and the level of use.
2. **Elsevier** requires a separate generative AI declaration placed before the references.
3. **ACM** and **Springer Nature** also require disclosure. Check your target venue's current author guide.

The skill offers ready-to-adapt disclosure text at the end of a full-paper job.

## Evidence base

1. Kobak et al., "Delving into LLM-assisted writing in biomedical publications through excess vocabulary," *Science Advances*, 2025.
2. Liang et al., "Monitoring AI-Modified Content at Scale," *ICML* 2024; "Mapping the Increasing Use of LLMs in Scientific Papers," *COLM* 2024.
3. Reinhart et al., "Do LLMs write like humans? Variation in grammatical and rhetorical styles," *PNAS*, 2025.
4. Gopen and Swan, "The Science of Scientific Writing," *American Scientist*, 1990.
5. Hyland, hedging and metadiscourse in research articles, 1998 and 2005.
6. Swales, *Genre Analysis* (CARS model), 1990.
7. Krishna et al., *NeurIPS* 2023, and Sadasivan et al., *TMLR* 2025, on paraphrase attacks and diminishing returns.
8. Dugan et al., "RAID," *ACL* 2024.
9. Liang et al., "GPT detectors are biased against non-native English writers," *Patterns*, 2023.
10. Cabanac et al., "Tortured phrases," 2021.
11. Turnitin AI writing detection model release notes; IEEE PSPB Operations Manual 8.2.1.B.10.

## FAQ

**Does this guarantee a 0% AI score?**
No. Technical papers keep a floor because terminology cannot change. In practice, structural rewriting brings many papers from about 60% to about 20%. The skill then targets only the remaining flagged clusters.

**Is humanizing a research paper allowed?**
Improving the language of your own verified work is normal academic editing. What matters is following your venue's AI disclosure policy, which this skill supports.

**Will it change my results or citations?**
No. Numbers, citations, equations and terms of art are frozen, and every pass is checked for meaning drift.

**Does it work with ChatGPT or Gemini drafts?**
Yes. It works on any AI-assisted draft. The skill itself runs in Claude, or in any agent that supports Agent Skills.

**Does it work for non-IEEE papers?**
Yes. IEEE is the default citation style. The house style section lets you switch to your venue's rules.

## Author

**K. M. Arafat Islam**, CSE graduate, Daffodil International University, Dhaka, Bangladesh. Research focus: explainable AI (SHAP, LIME) in socioeconomic domains.

If this skill helps your paper, star the repo so other researchers can find it.

---

**Keywords:** research paper humanizer, humanize AI text, AI humanizer for academic writing, reduce AI detection score, Turnitin AI detection, GPTZero, Originality.ai, iThenticate, academic writing, scientific writing, IEEE paper writing, journal paper editing, Claude skill, Claude Code skill, Agent Skills, ChatGPT academic writing, LLM writing detection, AI writing patterns, research integrity, AI disclosure.
