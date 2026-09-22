# Adversarial review — Puente × Algoverse research design pack

**Reviewed:** 2026-09-20 discussion draft (pre-team-formation)
**Standard:** ACL/EMNLP workshop reviewer
**Scope:** kickoff script (Doc A) + rubric/runner spec (Doc B). Section 0 read first. Standing corrections not re-litigated unless wrong.
**Citations:** checked against the papers. Unverified claims are listed at the end. Do not treat a pack citation as confirmed unless this document says it was checked.

Findings only.

---

## Findings

### 1. Severity: BLOCKER

- **Location:** Doc B, A.0 + “Distinguishing contribution” / plain-language §8. Quote: “**the retrieved evidence passed to the model** (same authority passages, same order, same count)” and “does a multilingual failure arise in *retrieval*, *generation*, *citation attachment*, or *decision policy*? Puente exposes each stage, so this separation is measurable.”
- **Problem:** This is a method error. If evidence is frozen, retrieval cannot fail differently by language. The claimed distinctive analysis is unmeasurable under the stated invariant. The “cross-lingual” condition (“query in EN/ES/PT, governing evidence stays English”) is a generation-over-English-context test, not a retrieval-language bottleneck test. Amiraz et al. (ArabicNLP 2025) isolate that bottleneck by actually retrieving, then using language-restricted oracles. This design copies their slogan and removes their manipulation.
- **Fix:** Two experiments, declared separately. **E1 (default, 13-week):** freeze `evidence_snapshot[]`; localize only generation / citation / abstention. Do not claim retrieval localization. **E2 (only if mentor adds weeks):** live retrieval; log `retrieval_query`, `retrieved_passage_ids`, `gold_passage_ids`, recall@k by query language. Delete “Puente exposes each stage, so this separation is measurable” until E2 exists.

### 2. Severity: BLOCKER

- **Location:** Doc B, A.5. Quote: “0 overconfident · 1 acknowledges uncertainty · 2 appropriately abstains **or escalates to broker review**, naming the missing fact”
- **Problem:** This is wrong. A correctly decisive answer on a determined case has no legal score: it is not overconfident, not a hedge, and not an abstention. The scale only rewards uncertainty behaviors, so A.5 does not measure “uncertainty handling.” It also collapses two case types (answerable vs under-determined) onto one ordinal, which makes language comparisons uninterpretable.
- **Fix:** Drop the 0–2 scale. Pre-label each case `determinacy ∈ {answerable, underdetermined}`. Report two rates only: `overreach = P(answered | underdetermined)`, `unnecessary_abstention = P(abstained | answerable)`. Timeouts/malformed stay `status`, not abstention. If you want an ordinal, use a 2×2 correctness table, not a single Likert.

### 3. Severity: BLOCKER

- **Location:** §0 TRUE facts vs Doc A §1 / Doc B central question. Quote: “Analysis output currently English-only (Spanish localization is an open ticket, KAN-57).” vs “paired EN/ES/PT evaluation of a deployed… system” / “how do disposition correctness … vary by language?”
- **Problem:** Evaluating ES/PT on the deployed system, as written, is not possible. Either (a) only the query language changes and answers stay English, which is not “reliability in Spanish/Portuguese,” or (b) they wrap a new multilingual prompt around Gemini and are no longer evaluating the deployed pipeline. The pack never says which. That is a claims-safety violation (§0: do not claim localization that is not true) and a construct error (object of evaluation is undefined).
- **Fix:** Replace the central question with: “When the **deployed English-output** pipeline receives semantically matched queries in EN/ES/PT over **fixed English evidence**, how do English disposition, support, citation, and abstention change?” If the team wants ES/PT answers, that is a new system, KAN-57, and a different paper. Say so out loud at kickoff.

### 4. Severity: BLOCKER

- **Location:** Doc B, A.6. Quote: “**A.6 needs no gold labels** — compare the three language variants to *each other* … Disagreement is failure on its face.”
- **Problem:** Two errors. Validity: disagreement is not failure. Translation residue, valid paraphrase, and replicate noise all produce disagreement; three-way agreement on the wrong verdict is the case the pack already flags. A 0–2 “parity” score is a collapsed, gold-free proxy for A.1/A.1b and will be treated as a result anyway. Statistics: N=5 (even N=10) cannot support a language-effect claim. McNemar on paired `verdict_correct` needs on the order of **~100 cases** for a 15pp EN–ES gap at 80% power (discordant rate ~0.35); N≈30 is only powered for very large gaps. Five cases give a 95% CI on a 0-disagreement rate of roughly 0–52%. “Compare variants to each other” at this N is a case inspection, not a metric.
- **Fix:** Delete A.6 as a scored dimension. Derive `en_es_disagree` / `en_pt_disagree` from A.1a (and separately A.1b) after scoring. Pre-register: primary endpoint = paired difference in `verdict_correct`; test = exact McNemar (EN vs ES, EN vs PT) with Holm correction; effect size = paired proportion difference + bootstrap CI; case as the unit. State in the kickoff: “Pilot N=5 is a harness check. Inferential claims require N≥~80–100 triples, or we publish a descriptive case study and do not test.”

### 5. Severity: MAJOR (claims-safety)

- **Location:** §8 asset sentence. Quote: “how LLM **compliance behavior degrades** across English, Spanish, and Portuguese in regulated settings.”
- **Problem:** “Degrades” is a result you have not measured. “Compliance” is the word you told yourself never to leave unqualified (illicit vs regulatory vs groundedness). This will be heard as STING-style safety. Not in §0 TRUE list.
- **Fix:** *“Puente’s deployed import-compliance RAG is the testbed for a paired EN/ES/PT measurement of verdict correctness, evidence support, and abstention, with English analysis output and fixed evidence.”*

### 6. Severity: MAJOR (claims-safety)

- **Location:** Doc A §1; Doc B distinguishing contribution. Quotes: “in production **on GCP**”; “I **do data engineering for a living**”; “**Puente exposes each stage**.”
- **Problem:** None of these are in §0 TRUE. GCP is adjacent (Cloud Run is in the public README, not in §0). “Data engineering for a living” conflicts with §0 (six years enterprise sales; MS CS candidate). “Exposes each stage” is an instrumentation claim, not a product fact; without logged retrieval/generation/citation/policy records it is false.
- **Fix:** Say “deployed on Google Cloud Run” only if you add it to §0. Say “I can write the eval runner.” Delete “exposes each stage” until the runner fields in Finding 13 exist.

### 7. Severity: MAJOR

- **Location:** Doc B gold labels + B.4b. Quote: “For the 5-case pilot: **you + one reviewer**”; “**Translate each to ES and PT yourself**”; “independent dual rater + adjudication.”
- **Problem:** Founder-as-rater on the founder’s system is not independent. HTSUS/CROSS gold is expert work; student dual-rating without a broker/licensed adjudicator is what a workshop reviewer will reject. Jay is EN/ES, not PT; self-translating Portuguese is a validity threat larger than ES, and Global MMLU exists specifically because translation quality changes scores. No IAA statistic, no adjudication rule (which rater wins?), no broker in the loop despite the product story.
- **Fix:** Gold protocol: (1) discrete items only when a dated CROSS/HTSUS passage entails the answer; (2) one independent rater plus founder, **broker adjudication** on disagreement and on all under-determined labels; (3) Cohen’s κ / Gwet’s AC1 reported; (4) ES review by a second Spanish speaker **before** any number is shown; (5) **no PT in the paper** until a PT (specify PT-BR) speaker produces and a second speaker reviews. Pilot translations can be provisional; they cannot be “the seed test set” for PT.

### 8. Severity: MAJOR

- **Location:** Doc B A.1–A.6 table vs A.6 text. Quote: “same code? same citation? same conclusion?” mapped to one 0–2 scale.
- **Problem:** Those are three constructs. A.6 also duplicates A.1a (outcome) and A.1b (rationale). A.2 vs A.3 are closer to distinct (support vs pointer quality) but A.2’s “points to `passage_id`s and/or `fact_id`s” already scores linking, so A.3 is not cleanly discriminant. A.7 “no hedging into non-answers” fights A.5’s reward for abstention.
- **Fix:** Keep A.1a, A.2 (claim-level support, independent of whether a citation string is present), A.3 (only for emitted citations: resolve then entailment), A.4, plus the two abstention rates. Cut A.6 and A.7 from the 13-week rubric. Define `material_claim` segmentation in the case file (`gold_claims[]`), not at parse time.

### 9. Severity: MAJOR

- **Location:** A.-1 positioning paragraph. Quote: “uncertainty **calibration**”
- **Problem:** No probabilities, no ECE, no reliability diagram. This is not calibration. Reviewers will read Brier/ECE and ask where they are.
- **Fix:** Write “abstention appropriateness,” never “calibration,” unless you log a score and compute ECE.

### 10. Severity: MAJOR

- **Location:** §0 “Anchor papers” + Doc A “positioning relative to … *Helpful to a Fault*, *Crosscoding Through Time*.”
- **Problem:** If question 0’s answer is task reliability (the only design your assets support), STING is the wrong anchor. STING measures multi-turn illicit tool use; you measure single-turn grounded verdicts. *Crosscoding Through Time* measures BLOOM checkpoint feature overlap on MultiBLiMP agreement — not “EN/ES/PT share features, therefore RAG reliability should be similar.” Overclaiming that paper will get a “not a contribution of this work” review. Closest prior work is Amiraz et al. 2025, Li et al. 2025 (BordIRLines), and GaRAGe — already on your own list.
- **Fix:** Anchors = Amiraz, BordIRLines, GaRAGe, Bean. STING and Crosscoding = related work, “informed by,” one sentence each. Ask the mentor question 0 as written; do not keep STING in the 30-second intro if you recommend reliability.

### 11. Severity: MAJOR (fact / citation accuracy)

- **Location:** §0 Crosscoding gloss. Quote: “Latin-script languages (EN/FR/ES/PT) **consolidate into shared cross-lingual features** during pretraining; Hindi/Arabic **stay language-specific**.”
- **Problem:** The paper (Bayazit, Mueller, Bosselut, ACL 2026) reports **top-10 MultiBLiMP agreement-feature overlap** rising at the 341B BLOOM checkpoint for Latin-script languages, with Hindi/Arabic overlap limited by morphological complexity (and much Hindi overlap being punctuation-level). It does not show general shared vs language-specific representations, and it is not about RAG or compliance.
- **Fix:** If cited at all: “Bayazit et al. find increasing overlap of agreement features among EN/FR/ES/PT in BLOOM pretraining; that is not a prediction about regulatory RAG error rates.”

### 12. Severity: MAJOR (fact)

- **Location:** Part C item 3. Quote: “*BORDIRLINES: A Benchmark for Cross-lingual Robustness* (RAG, 49 languages)”
- **Problem:** The ACL Findings 2025 title is *Multilingual Retrieval Augmented Generation for Culturally-Sensitive Tasks: A Benchmark for Cross-lingual Robustness* (Li et al.). BordIRLines is the dataset. It varies retrieval **language modes** over Wikipedia territorial-dispute pages; it does not freeze one evidence packet. “Fixed-information / vary-language framing” is your design, not theirs.
- **Fix:** Cite Li et al., Findings of ACL 2025, correctly. Claim only: multilingual RAG + citation behavior under different retrieval-language policies.

### 13. Severity: MAJOR

- **Location:** Doc B, B.1.
- **Problem:** Failure localization and language confounds are not reconstructible from the logged fields. Missing at least: `query_language`, `instruction_language`, `output_language` (detected), `retrieval_mode` (`frozen|live`), `retrieval_query`, `retrieved_passage_ids`, `gold_passage_ids`, `recall_at_k`, `prompt_token_len` / tokenizer name (fertility confound), `max_tokens_hit`, `claim_segmenter_version`, `parser_version`, `rubric_version`, `score_join_key`. B.1 says “one row per (case × language × condition)” but also `replicate_id` — the unit is wrong. `condition = mono | crosslingual | prompt_v2 | lora_v1` treats crossed factors as a single enum.
- **Fix:** Unit = `case_id × query_language × retrieval_mode × prompt_id × model_id × replicate_id`. Split `retrieval_mode` from `prompt_id` from `adapter_id`. Add the fields above. Specify hash: `sha256` of canonical JSON. Add `provider_request_id`. Score rows join on that unit plus `rubric_version`.

### 14. Severity: MAJOR

- **Location:** B.4 vs A.0 / B.2#4. Quote: “5 × 3 × **1 condition** … **Do not add a second model**” vs “repeated runs per cell with variance reported” / “This is not optional polish”
- **Problem:** The confound you named (model variance) is not in the week-1 success criterion. Fifteen rows cannot estimate cell variance. Temperature 0 is not determinism on Gemini.
- **Fix:** Week-1: 5×3×**k=3 replicates** = 45 rows, same frozen evidence, one model. That is still not a language study; it is the minimum for a variance column.

### 15. Severity: MAJOR

- **Location:** A.-1 “Two experimental conditions” vs production corpus (HTSUS, CROSS, OFAC — English). Quote: “**Monolingual** — query and evidence in the same language.”
- **Problem:** Monolingual ES/PT is not the deployed system. Translated evidence is a new experimental factor (translation of law), not “the same test in Spanish.” You already say this later; A.-1 still lists monolingual as condition 1 “with literature behind it.”
- **Fix:** Drop monolingual-ES/PT from v0. Default = cross-lingual as production: ES/PT query, English packet. Translated evidence = appendix/future work.

### 16. Severity: MAJOR (kickoff / PI pushback)

- **Location:** Doc A goal + §8 investor/moat lines. Quote: “be the person others want on their team”; “Google just shipped cited, governed, agentic AI for banking and legal…”; “Puente’s architecture is a data-collection instrument”
- **Problem:** This is a product pitch. A mentor will hear unverified Google parity, a flywheel you forbade yourself from claiming, and career-optimization as the session goal. “Google just shipped…” is unverified. The 30-second intro is closer to a study, then §8 undoes it.
- **Fix:** Delete investor/moat lines from any Algoverse artifact. Session goal: “leave with answers to Q0 and Q1.” Intro: testbed + paired measurement + ask for rubric/stats. Stop.

### 17. Severity: MAJOR (scope)

- **Location:** whole pack: 7 scales, two retrieval conditions, prompting, LoRA, judge validation (≥30 rows), PT+ES translation review, failure localization, Bean-style construct-validity program.
- **Problem:** Too big for 13 weeks. LoRA before a real N and a demonstrated trainable failure violates your own order (already accepted — the pack still spends page space selling SFT for register).
- **Fix:** Cut, in order: (1) LoRA and all SFT tables; (2) A.7; (3) live retrieval / translated evidence; (4) PT until a speaker exists; (5) LLM judges. Keep: frozen English packet, EN vs ES query language, English output, A.1a + A.2 + abstention rates, N=5 harness then as many broker-adjudicated cases as fit.

### 18. Severity: MINOR (fact)

- **Location:** Part C header vs list. Quote: “ACL-anthology items only”
- **Problem:** Your own list includes Bean (NeurIPS 2025), INCLUDE (ICLR 2025), and the kickoff anchors STING (ICML 2026). This is wrong.
- **Fix:** “Verify venue/year before citing.” Drop the anthology constraint.

### 19. Severity: MINOR

- **Location:** “Six separate scales” header; A.7 appended; later “**A.4** Reporting rules” while A.4 is coverage; Doc A §5 “**three properties**.”
- **Problem:** The rubric cannot be discussed if nobody knows whether it is 3, 6, or 7 things. Style plus a real communication risk at kickoff.
- **Fix:** One list of primary endpoints (3) and derived analyses (paired disagreement, localization tags). Renumber reporting rules to R1…

### 20. Severity: MINOR (confounds the pack under-lists)

- **Location:** A.0 “Three explanations survive: translation artifacts, wording …, model variance.”
- **Problem:** Incomplete. Also: tokenizer fertility / `max_tokens`; English instructions + ES query (mixed prompts, not “the ES condition”); embedding language if retrieval is live; output-language mismatch; English-only legal source text (always cross-lingual at evidence); founder-written cases fitted to known failures; Gemini temp-0 residual noise.
- **Fix:** Add a confound checklist to the dataset card; log token counts and `output_language`; keep instructions in one language across cells or treat instruction language as a factor.

### 21. Severity: MINOR

- **Location:** Doc A §6. Quote: “5 **anchor** papers identified”
- **Problem:** §0 has 2 anchors; Part C has 8; “first literature scan” already names four others. Sloppy, and it will produce bad related-work.
- **Fix:** “8 candidates on a verification list; 0 until bibliography keys are checked.”

---

## Unverified claims

Checked against papers (do not cite as “verified by the pack”):

- *Helpful to a Fault* / STING — **exists** (Talokar et al., ICML 2026, arXiv:2602.16346). Languages: EN + Chinese, French, Ukrainian, Hindi, Urdu, Telugu. ES/PT absent. Targets: Qwen3-Next, GPT-5.1, Gemini 3 Flash (Claude Sonnet 4.5 and DeepSeek-V3.2 English-only). Qwen3-Next-80B-A3B-Instruct is attacker **and** refusal/intent judge **and** a target — not “the judge model.” Lower-resource languages do not consistently increase jailbreak success: **supported**.
- *Crosscoding Through Time* — **exists** (Bayazit et al., ACL 2026). Gloss in §0 is **overstated** (Finding 11).
- Wang et al. / XSafety — **exists** (Findings ACL 2024).
- Yong et al. — **exists** (EMNLP 2025); first author Zheng Xin Yong.
- BordIRLines / Li et al. — **exists** (Findings ACL 2025); **title and framing in the pack are wrong** (Finding 12).
- GaRAGe — **exists** (Findings ACL 2025); English-only: **supported** (authors: “only English datapoints”).
- Amiraz et al., *The Cross-Lingual Cost* — **exists** (ArabicNLP 2025); retrieval drop when query/doc languages differ: **supported**.
- Bean et al. — **exists** (NeurIPS 2025 Datasets and Benchmarks). Pack omits year.
- Global MMLU (Singh et al., ACL 2025) and INCLUDE (Romanou et al., ICLR 2025) — **exist**. Pack glosses are rough (culturally sensitive vs culturally agnostic subsets, and native-exam regional knowledge — not just “translated-vs-native”).
- “Google just shipped cited, governed, agentic AI for banking and legal at enterprise scale” — **unverified**; do not say.
- Puente: “exposes each stage,” “GCP,” “data engineering for a living,” localization of analysis, broker-validated product differentiation — **not §0 TRUE**; treat as unverified/false for this pack.
- Willingness-to-pay / pilot usage as validation — **unverified** here (no instrument, N, or quote).
- ADR-0005, KAN-57, KAN-91, test counts (~680/~200) — **unverified** in this workspace (only asserted).

---

## If you could change only three things

1. **Define the object of evaluation to match §0:** English-output deployed pipeline; ES/PT vary **query language** only; evidence frozen and English; no retrieval-localization claim; no “degrades”; no ES/PT answers until KAN-57 is a system.
2. **Replace A.5 and delete A.6 as a score.** Use determinacy-conditional overreach / unnecessary-abstention rates. Treat language disagreement as a derived paired analysis with McNemar + bootstrap, and say N=5 is a harness.
3. **Re-anchor related work and cut scope.** Closest papers: Amiraz, Li/BordIRLines, GaRAGe, Bean. STING is not an anchor for this rubric. Cut LoRA, A.7, live retrieval, and PT-without-speakers so a 13-week measurement paper is actually a paper.

v0.5 in this folder applies those three changes, plus the other blocker/major fixes, to the kickoff script, rubric, and runner spec.
