---
name: "research-paper-humanizer"
description: "Humanize AI-assisted research paper text (IEEE, Q1 journals) to lower AI-detection scores while keeping meaning, numbers, citations, terms and formal register. Use for any paper section or humanize request on academic text."
---

# Research Paper Humanizer

This skill rewrites AI-assisted academic prose so it reads as if an expert author wrote it. The rewrite should lower scores on Turnitin/iThenticate, GPTZero, Originality.ai and Copyleaks. The research must come out unchanged: the same claims, numbers, citations and terminology, with the same strength of evidence. The writing should get better, never worse.

The goal is to remove false AI signals from content the author owns and has verified. It does not replace the venue's AI-use disclosure (Section 12).

For academic text, this skill replaces generic humanizers. Generic advice like "add opinions", "use I", "add humor" or "let some mess in" is wrong for research papers. Never apply it.

## 1. Hard constraints (never break)

1. **Freeze every technical token.** This covers numbers, statistics, units, p-values, confidence intervals, hyperparameters, seeds, equations, variable names, figure and table references, citation numbers, author names, dataset names, model names, metric names and terms of art. Copy each one exactly. Never swap a technical term for a synonym: "deep learning" never becomes "profound learning", and "SHAP values" stays "SHAP values". Synonym-spun terms are "tortured phrases". Integrity screeners such as the Problematic Paper Screener flag them, and they have led to mass retractions.
2. **Meaning is fixed.** Add no claims, examples, citations, data or limitations. Delete nothing unless asked. Keep each claim at the same strength: a hedge ("may", "suggests") stays a hedge, a finding stays a finding, and correlation never becomes causation. Filler words can go, but a claim cannot.
3. **Word count never goes up** on a humanization pass. Cut words only when asked, or by removing filler. Count before and after.
4. **Formal research register throughout.** No casual phrasing, contractions, rhetorical questions, humor or personal opinion. Use "we" for decisions and claims. Use passive voice for procedures, where the agent does not matter.
5. **Leave structure alone.** Do not touch headings, section numbering, figure and table identifiers, captions or formatting.
6. **Never use evasion tricks** (Section 9).

## 1b. House style defaults (change these if your venue or team differs)

1. **No em dashes.** Use a full stop, comma, colon or parentheses. Practitioners widely treat heavy em dash use as an AI tell, but no peer-reviewed study has measured it.
2. **No first person singular "I".** Most multi-author papers use "we".
3. **Citation format exactly as the venue requires.** For IEEE, write [1], [2], [3], never [1,2,3]. Keep citation ranges exactly as the source writes them.

## 2. What detectors measure (why the levers work)

1. **Predictability (perplexity).** LLM text uses high-probability tokens, so a reference model finds it predictable.
2. **Uniformity (burstiness).** LLM sentences are similar in length and shape. Human writing varies.
3. **Learned style features.** The evidence comes from corpus studies.
   1. LLM text uses excess style words such as delves, underscores, showcasing, pivotal, intricate and meticulous (Kobak et al. 2025; Liang et al. 2024).
   2. Instruction-tuned LLMs use present-participle tails (", highlighting...") at 2 to 5 times the human rate. They also use about 2 times as many nominalizations and about 1.9 times as much phrasal coordination (Reinhart et al., PNAS 2025).
   3. LLMs use agentless passive at about half the human rate, so legitimate passive in Methods reads as human.
4. **Turnitin behavior** (vendor-reported).
   1. It scores segments in context and needs at least 300 words of prose.
   2. It flags the generic first and last sentences of a document more often.
   3. It displays scores from 1 to 19% as "*%".
   4. False-positive sentences cluster next to sentences that are actually AI-written.
   5. It detects AI-paraphraser output (since December 2023) and humanizer or bypasser tool output (since August 2025).

**Consequence:** sentence structure and information order move the score. Synonym swaps do not, and in benchmarks (RAID 2024) synonym attacks sometimes raised detection. Technical vocabulary is predictable and cannot change, so technical papers likely have a detection floor. The levers left are syntax, information order and paragraph shape.

## 3. Procedure for every passage

1. **Lock.** Identify every frozen token from constraint 1 and note the original word count.
2. **Diagnose.** Scan the passage for:
   1. marker words (Section 5) and template phrases (Section 6)
   2. participle tails, nominal chains and stacked transitions
   3. uniform sentence lengths and identical paragraph templates
   4. generic openers or closers, and unsupported boosters
3. **Rewrite** with the levers in Section 4, in priority order. Fix structure first and swap words last.
4. **Verify** (mandatory):
   1. Every frozen token is present and identical.
   2. Every claim keeps its direction and strength. No comparison is reversed. Quantifiers are still accurate: "most" is not "all", "some" is not "every".
   3. Nothing new was added and no claim was dropped.
   4. Word count is no higher than the original. Use a code tool to count when one is available.
   5. No house-style violation (Section 1b), no contractions and no new marker words.
   6. Technical labels are still correct after restructuring. An RBF-kernel SVM is not a "linear model", and a single Decision Tree is not an "ensemble".
5. **Output** in the format in Section 11.

## 4. Rewrite levers (in priority order)

**L1. Information order (Gopen and Swan 1990).** Start each sentence with old, linking information and end it on the new result or number, the stress position. Sentence openings then vary naturally, so not every sentence starts with "Author et al. [n] found...".
Before: "Smith et al. [5] proposed LOF, which detects anomalies through local density."
After: "LOF, proposed by Smith et al. [5], detects anomalies through local density."

**L2. Remove present-participle tails.** Turn ", highlighting X", ", demonstrating X", ", enabling X" or ", underscoring X" into a separate sentence with a real subject. Fold it into the main verb, or delete it if it only restates.
Before: "RF reached 84.35% accuracy, highlighting its strength on tabular data."
After: "RF reached 84.35% accuracy. This is a strong result for tabular data."

**L3. Turn nominalizations back into verbs.** "performed the optimization of hyperparameters" becomes "optimized the hyperparameters". "for the evaluation of" becomes "to evaluate".

**L4. Choose voice by function.** Use passive for procedures ("Features were standardized using z-scores"). Use "we" with active voice for choices and claims ("We set c = 0.05 because..."). Do not convert every passive to active, because LLMs underuse agentless passive.

**L5. Vary sentence length.** In each paragraph, mix a short declarative sentence (about 6 to 10 words) with longer sentences that carry subordinate clauses. Do not force strict short-long alternation. Split semicolon chains into full stops. Break up rhetorical three-item lists ("X, Y, and Z") that are padding rather than fact.

**L6. Vary paragraph shape.** Do not repeat claim, citation, claim, citation in every paragraph. Open some paragraphs with evidence, some with the gap and some with a number. End each paragraph on its last substantive point, never on a summary or wrap-up sentence.

**L7. Cut transition stacking.** Remove Moreover, Furthermore, Additionally, In addition, Notably, Indeed and Importantly. Let the topic position do the linking ("This gap...", "Under class imbalance, ..."). Keep a plain connector (However, Because, Thus) only where the logic needs it, and never at the start of two sentences in a row.

**L8. Use calibrated hedges, not boosters (Hyland).** Write "significantly" only when a statistical test supports it. Replace "remarkable" or "impressive" with the number. Match hedges to the evidence: "suggests", "is consistent with", "in our experiments". Never weaken a supported claim or strengthen a tentative one.

**L9. Be specific, using only what is already in the passage.** "the proposed approach" becomes "the majority-voting ensemble". "various metrics" becomes the named metrics, if the passage names them. Never import facts from outside the passage.

**L10. Change vocabulary last.** Swap a Section 5 marker only after the structure is fixed. Choose one plain word, not a fancier synonym.

**L11. Turn argumentative lists into prose.** Contribution and motivation lists become prose, with each sentence shaped differently. This is for style. Turnitin may not score lists at all, so it may not lower the score.

## 5. Marker vocabulary (evidence-backed)

| Marker | Plain replacement |
|---|---|
| delve(s) into | examine, analyze, study |
| underscore(s) | shows, indicates |
| showcase, showcasing | shows, reports |
| pivotal, crucial | central, key, necessary, or state why |
| intricate | complex, detailed |
| meticulous(ly) | careful(ly), or describe the step |
| commendable, noteworthy, remarkable | delete, or give the number |
| realm, landscape | field, area |
| notably, additionally | delete |
| comprehensive | full, general, or quantify ("all 12 datasets") |
| enhance, enhancing | improve, increase |
| garnered | received, drew |
| grappling with | addressing |
| aligns with, aligning with | is consistent with, matches |
| excels | outperforms (with the number) |
| insights | findings, or the specific finding |
| novel, innovative | new, or say what is new |
| versatile, seamless, holistic | flexible, direct, overall, or delete |
| robust (unqualified) | say robust to what |
| leverage, utilize, harness, foster, elevate, empower, facilitate | use, build, support, allow |
| tapestry, palpable, testament | delete |

Words like findings, potential, within, across and particularly are normal scientific vocabulary. Use them less densely, but do not ban them. The sources are Kobak et al. 2025, Liang et al. ICML 2024 and COLM 2024, and Reinhart et al. 2025. The leverage, utilize and harness group is a practitioner heuristic, not a corpus finding.

## 6. Template phrases to remove

| AI template | Human move |
|---|---|
| "In recent years, X has garnered significant attention" | Start with the specific problem |
| "plays a crucial/pivotal role in" | State the actual function, or use "central" |
| "In today's rapidly evolving..." | Delete |
| "This paper proposes a novel..." | "We propose" plus what it does |
| "To address this gap/these challenges" | Name the gap in the previous sentence, then continue |
| "The results demonstrate the effectiveness of" | Report the comparison with its number |
| ", highlighting/underscoring/demonstrating..." | A separate sentence, or delete |
| "It is worth noting that", "It is important to note" | Delete and state the fact |
| "not only X but also Y", "X rather than merely Y" | A plain statement |
| "serves as", "stands as" | "is" |
| "a wide range of", "various", "a variety of" | The actual items or the count |
| "Overall, these findings underscore...", "In conclusion, this study..." | End on the most specific forward point |
| "Despite its contributions, this study has some limitations" | Open with the first concrete limitation |
| "paving the way for", "opens new avenues" | The specific next step |

## 7. Section playbook

1. **Abstract.**
   1. Write one paragraph that follows: problem, specific gap, method, headline number, implication.
   2. Put the headline number in a stress position.
   3. Respect the word cap.
   4. Take extra care with the first sentence, since Turnitin flags generic openers more often.
2. **Introduction (Swales CARS).**
   1. Compress the territory move to one or two sentences.
   2. Make the gap concrete: name a method, dataset or metric that fails.
   3. Write contributions as prose with varied sentence shapes.
3. **Related Work.**
   1. Group studies by approach, not author by author.
   2. Open sentences with the method or its limitation, and put the citation after.
   3. Use "Author et al. [n]" at most once per paragraph.
   4. End each paragraph on the gap the next paragraph picks up.
4. **Methodology.**
   1. Use passive voice for procedures, and "we" plus a reason for decisions.
   2. Use verbs rather than nominal chains.
   3. Never touch equations, hyperparameters, seeds or data splits.
5. **Results.**
   1. Lead with the comparison and its number.
   2. After a dense numeric sentence, add one short interpretive sentence.
   3. Do not write "remarkable" or "impressive".
   4. Keep every figure and table reference.
6. **Discussion.**
   1. Hedge in proportion to the evidence.
   2. Contrast with specific prior results that are already cited.
   3. Use no participle tails.
7. **Limitations.** Open directly with the first concrete limitation, such as dataset size, a single source or a proxy label.
8. **Conclusion.**
   1. Do not write "In conclusion".
   2. Do not restate the whole paper.
   3. End on the most specific forward-looking item already in the text.
   4. Take extra care with the final sentence, since Turnitin flags generic closers more often.

## 8. Plateau protocol

1. **Expect diminishing returns.** In practice, structural passes bring a paper from about 60% to about 20%. More whole-document passes after that give diminishing returns and start to damage the prose. Published paraphrase studies show the same tapering (Krishna et al. 2023; Sadasivan et al. 2025).
2. **Target the flagged clusters.** Turnitin shows 1 to 19% as "*%", so a paper stuck at 20 to 21% usually has only a few flagged clusters left. Ask for the highlighted sentences. Rewrite only those and the sentences next to them, because flags cluster.
3. **Move to untouched sections.** Prioritize sections not yet rewritten, often the Abstract, Introduction and Conclusion. Do not rewrite the same paragraph a fourth time.
4. **Stop after 2 to 3 targeted passes** on the same text, and say so plainly.
5. **Use the author-composition route at the plateau.** Claude writes the passage in natural prose in the author's first language. The author translates it into English themselves. Claude then runs check-only mode (Section 10). This route is legitimate because the final English is the author's own composition. Automated machine-translation round trips are different: they are detectable and they mangle terminology. Do not use them.
6. **Set the right target.** In practical experience (not a published finding), 20 to 30% is common and workable for technical ML papers. No score cutoff appears in the published AI policies of IEEE, Elsevier, Springer Nature or ACM, which rely on disclosure instead. The venue's policy decides what is acceptable, not the detector score.

## 9. Never do

1. Use homoglyphs, zero-width or invisible characters, white text, or odd Unicode spacing.
2. Insert deliberate typos or grammar errors.
3. Spin synonyms for technical terms (tortured phrases).
4. Pass text through commercial humanizers or paraphrasers (StealthGPT, HIX, Undetectable AI, QuillBot). Turnitin trains on their output.
5. Use automated multi-language machine-translation round trips.
6. Add opinions, humor, a casual voice or "I".
7. Fabricate hedges, citations, data, limitations or examples to add "human detail".
8. Advise hiding AI use where the venue requires disclosure.

## 10. Check-only mode (author's own translation or edit)

When the author returns English they wrote or translated themselves, fix only factual and mechanical errors. Do not restyle the author's phrasing. Check for:
1. **Reversed findings:** a weaker model said to outperform a stronger one, or a correlation with the wrong sign or direction.
2. **Wrong numbers:** values missing from the original, altered values, or numbers that differ between the Abstract and the Results.
3. **Wrong terminology:** for example "Layer" vs "Level", "return reduction" vs "diminishing returns", or transposed framework names.
4. **Wrong model-family labels:** for example calling an RBF-kernel SVM linear, or a Decision Tree an ensemble.
5. **Quantifier errors:** "most" when the arithmetic says "all".
6. **Causal language** where only association was shown.
7. **Citation and punctuation errors:** IEEE needs [1], [2], [3], not [1,2,3]. Watch for stray double periods.
8. **House-style breaks** (Section 1b) and contractions.

Return the corrected text. List only the changes that affect meaning or numbers, one line each.

## 11. Output format

Default, for each passage:
1. The final rewritten text only, ready to paste. Include no draft unless asked.
2. One check line, for example: `Words: 214 -> 201 | Numbers, citations, terms: unchanged`.
3. A flag only when something needs the author's decision, such as an ambiguous meaning in the source, a possible factual error in the original, or how to read a figure. One line each.

Talk to the user in the language they use with you. Paper content is always formal English.

Other modes, on request:
1. `audit`: list the remaining AI tells sentence by sentence, without rewriting.
2. `native`: output the passage in natural prose in the author's first language (for example Bangla) for the author to translate.
3. `check`: Section 10.
4. `trim to N`: reduce the passage to N words or fewer with no content loss.

## 12. Disclosure

IEEE (PSPB 8.2.1.B.10) requires AI-generated content to be disclosed in the Acknowledgments. The disclosure names the AI system, the sections and the level of use. Pure grammar and editing help generally falls outside the policy, but disclosure is still recommended. Elsevier wants a separate declaration placed before the references. ACM and Springer Nature also require disclosure, but their current wording varies, so check the target venue's guide.

At the end of a full-paper job, mention disclosure once (not on every passage) and offer this IEEE text:
"The authors used [tool, version] to improve the language and readability of Sections [..] of an author-written draft. All technical content, analyses, results and citations were produced and verified by the authors, who take full responsibility for the article."

## 13. Worked example

Before (57 words):
"In recent years, explainable artificial intelligence has garnered significant attention, playing a pivotal role in high-stakes domains. Notably, SHAP has emerged as a comprehensive framework for interpreting model predictions, highlighting the contribution of each feature. Furthermore, the integration of SHAP with gradient boosting enables a deeper understanding of complex decision processes, underscoring its potential for real-world deployment."

After (51 words):
"Explainable artificial intelligence has drawn significant attention and plays a central role in high-stakes domains. SHAP has become a general framework for interpreting model predictions. It assigns each feature a contribution. Paired with gradient boosting, it clarifies how complex models reach decisions. This gives the combination potential for real-world deployment."

What changed:
1. The generic opener "In recent years" was removed.
2. Three participle tails became main clauses.
3. "the integration of" became "Paired with" (de-nominalized).
4. Two stacked transitions were cut.
5. Marker words were replaced: garnered, pivotal, comprehensive, underscoring.
6. Sentence lengths now vary (15, 10, 6, 11 and 9 words).
7. Every claim survives at the same strength: "significant attention", the role in high-stakes domains, and the hedged "potential".
8. No term or number changed, and the word count went down.

## Evidence base

Kobak et al., Science Advances 2025 (excess vocabulary). Liang et al., ICML 2024 and COLM 2024 (LLM-modified scientific text). Reinhart et al., PNAS 2025 (grammatical features). Gopen and Swan, American Scientist 1990. Hyland 1998 and 2005 (hedging, metadiscourse). Swales 1990 (CARS). Krishna et al., NeurIPS 2023 and Sadasivan et al., TMLR 2025 (paraphrase attacks, diminishing returns). Dugan et al., ACL 2024 (RAID). Liang et al., Patterns 2023 (false positives on non-native writing). Cabanac et al. 2021 (tortured phrases). Turnitin AI writing model release notes. IEEE PSPB Operations Manual 8.2.1.B.10.