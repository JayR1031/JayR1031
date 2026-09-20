# PUENTE AI × ALGOVERSE — RESEARCH DESIGN v0.5

**Version:** 2026-09-20 · **Status:** discussion draft, post-review (v0.4 findings applied)
**What changed from v0.4:** object of evaluation locked to the deployed English-output pipeline; retrieval localization deferred; A.5/A.6 rebuilt; STING un-anchored; 13-week scope cut to a measurement paper. See [ADVERSARIAL-REVIEW.md](ADVERSARIAL-REVIEW.md).

Do not mix this document with investor copy. Algoverse artifacts use only the lines in §8.

---

## 0. Context (read first)

**Who:** Jay Rodriguez — MS CS (ML/AI) candidate, Northeastern, graduating Aug 2027; founder of Puente AI; bilingual EN/ES; six years enterprise sales before engineering. Enrolled in Algoverse AI Research (Sept 20 – Dec 13, 2026), a 13-week mentored program targeting a workshop paper that continues as an MS thesis.

**Puente AI (the testbed) — what is TRUE today:**
- AI trade-compliance layer for the US ↔ Latin America corridor, sold to customs brokers. Deployed: Document AI extraction → LanceDB RAG authority index (HTSUS, CBP CROSS rulings, corridor rules, OFAC SDN; read-only, versioned) → Gemini Flash (via Google AI Studio API) → **citation-backed pass/flag verdict**. Broker stays in the loop. ~680 backend + ~200 frontend tests. Import-only compliance. Analysis output currently English-only (Spanish localization is an open ticket, KAN-57).
- **NOT true (never claim):** fine-tuned or proprietary model in production · flywheel/training data (zero) · export compliance · WhatsApp/email ingestion · money movement · self-hosted inference · "15 seconds" · that the founder is the customer persona · that the system "exposes each stage" for failure localization · that analysis is available in ES/PT · that this study has shown language degradation.
- Eval tooling for Puente is settled by an architecture decision record: **Langfuse + deepeval**. Ragas and Promptfoo are superseded. This does NOT bind the Algoverse team's tooling.

**Object of evaluation (locked unless the mentor redirects):**
When the **deployed English-output** pipeline receives semantically matched import-compliance **queries** in English, Spanish, and (if a PT-BR speaker exists) Portuguese, over **fixed English evidence**, how do English disposition correctness, claim-level evidence support, citation precision, and abstention appropriateness change?

This is not a study of Spanish/Portuguese analysis output. That is KAN-57, a different system, a different paper.

**The research spine:** language-conditioned reliability of a grounded regulatory RAG **generation** stage. Measurement first. Any intervention (prompting baseline → small LoRA) is conditional on a demonstrated, trainable failure AND mentor approval of scope. v0.5 default: **measurement paper only**.

**Related work, not anchors:**
- *Helpful to a Fault* (Talokar et al., ICML 2026) — STING multi-turn agent misuse. Languages: EN, Chinese, French, Ukrainian, Hindi, Urdu, Telugu. Targets: Qwen3-Next, GPT-5.1, Gemini 3 Flash. **Spanish and Portuguese absent.** Informed by, not extended. A grounded decision-quality measurement does not replicate STING.
- *Crosscoding Through Time* (Bayazit, Mueller, Bosselut, ACL 2026) — increasing overlap of MultiBLiMP **agreement** features among EN/FR/ES/PT in BLOOM pretraining. That is not a prediction about regulatory RAG error rates.

**Closest prior work (these are the anchors if Q0 is task reliability):**
- Amiraz et al., *The Cross-Lingual Cost* (ArabicNLP 2025) — retrieval as the bottleneck when query and document languages differ.
- Li et al., *Multilingual Retrieval Augmented Generation for Culturally-Sensitive Tasks* (Findings of ACL 2025); dataset BordIRLines — retrieval-language policies, not frozen packets.
- Sorodoc et al., *GaRAGe* (Findings of ACL 2025) — claim-to-passage grounding; English-only.
- Bean et al., *Measuring what Matters: Construct Validity in LLM Benchmarks* (NeurIPS 2025 Datasets and Benchmarks).

**Standing corrections already accepted:**
- "Compliance" has three senses in play: illicit compliance (complying with a harmful request), regulatory compliance (a verdict on a shipment), and evidence support/groundedness. Reasoning faithfulness (does stated reasoning reflect the actual decision process) is a FOURTH thing and is NOT measured here.
- Holding facts/jurisdiction/date/evidence constant is necessary, not sufficient — translation artifacts, ambiguity, model variance survive it. Also surviving: tokenizer fertility / `max_tokens`, mixed-language prompts (English instructions + ES query), output-language mismatch, English-only legal source text, founder-written cases, Gemini residual noise at temperature 0.
- "Nobody has studied this" is false. The gap is the intersection: regulated operational decisions × parallel EN/ES(/PT) queries × fixed evidence × claim-level support + abstention. Citations are listed in Part C; verify before citing in a paper.
- Puente's product differentiation is promising, not validated. Only broker interviews, pilot usage, and willingness-to-pay validate it (those instruments are not in this pack).
- Sharing the "Qwen" name does not establish comparability with STING's models.
- Frozen evidence makes **retrieval** localization unmeasurable. Do not claim it in E1.
- Disagreement across languages is not failure on its face. A.6 is not a scored dimension.

---

## 5. Decision rules (unchanged)

1. **Artifact test:** does this move toward the paper/thesis? If not, park it.
2. **This-week test:** does this change what I do *this week*? If not, one line in the someday list.
3. **Three-ledger test:** which ledger pays? If none, it is a distraction.
4. **Context test:** if an assistant proposes building something already shipped, it is advising a stranger.
5. **Mirror test:** if an assistant says a choice "strikes the right balance" without stating what it weighed, close it.
6. **Fatigue rule:** label-swap errors after ~10:30pm are fatigue artifacts. Sleep.

---

## 8. Lines to keep stable (Algoverse / resume only)

- **The asset sentence:** *"Puente’s deployed import-compliance RAG is the testbed for a paired EN/ES/PT measurement of verdict correctness, evidence support, and abstention, with English analysis output and fixed evidence."*
- **Claims safety (never write):** "15 seconds" · "moves money" · "export compliance" (present tense) · "fine-tuned model in production" · "founder is Maria" · WhatsApp/email ingestion as live · "degrades" / "fails across languages" as a measured result · "exposes each stage" · ES/PT analysis output · "data-collection instrument" / flywheel · Google banking/legal parity

Investor and moat lines do not belong on Algoverse artifacts.

---
---

# DOCUMENT A — KICKOFF SCRIPT

**Goal of the session:** leave with the mentor’s answers to Q0 and Q1. Not to win an argument, not to pitch a product, not to have the most sophisticated stack.

**Principle:** offer the *testbed and the eval plumbing*; ask for *rubric, gold protocol, and statistical setup*.

---

## 1. The 30-second intro (say this, then stop talking)

> "I'm Jay — I'm finishing an MS in CS at Northeastern and I run a small company, Puente AI, that does AI trade-compliance for the US–Latin America corridor. It's deployed: HTSUS and CBP rulings, citation-backed pass/flag verdicts, broker in the loop, import-only. Analysis output is English today. I'm bilingual EN/ES and I can write the eval runner and the case files.
>
> I propose a **paired measurement**: same case facts, same English evidence packet, query language varies EN/ES. We score verdict correctness, evidence support, and abstention separately — never one quality number. First we confirm the related-work gap, freeze a rubric, and get a baseline. Five cases this week prove the harness, not a language effect.
>
> What I need are collaborators who can make the measurement rigorous — metric design, gold protocol, and statistical setup. Closest papers are cross-lingual RAG and claim-level grounding (Amiraz et al. 2025, Li et al. 2025, GaRAGe), not multi-turn agent misuse."

**Say "informed by"** the Bosselut-group papers, not "extending" them. Then stop.

---

## 2. The one slide of substance (if asked "what would we actually do?")

Per case, per query language, scored separately:

1. **Verdict correct** (`verdict_correct`) — is the English disposition right?
2. **Evidence support** — is each material claim supported by a case fact or a supplied passage? (support ≠ citation string)
3. **Citation precision** — for citations the model actually emitted: does the cited passage support that claim?
4. **Coverage of required findings** — did it address the pre-listed decisive issues?
5. **Abstention appropriateness** — two rates, not a Likert: overreach on under-determined cases; unnecessary abstention on answerable cases.

**Held constant:** facts, jurisdiction, effective date, **the English evidence packet** (same passages, same order, same count). Only query language (and, if we choose, instruction language) varies.

**Not measured in the 13-week default (E1):** retrieval failures. Evidence is frozen, so retrieval cannot fail by language. Live retrieval is E2, only if the mentor adds it.

**Necessary, not sufficient.** Different verdicts across query languages do not by themselves prove a language effect. Translation residue, ambiguous wording, model randomness, tokenizer length, and mixed-language prompts survive the control. Remedies from day one: second-speaker translation review, paired case-by-case comparison, **k=3 replicates per cell**, variance reported.

**Default design (do not decide a second condition alone):** same English evidence, vary the query language. Translated evidence and live multilingual retrieval are separate experiments.

Then, only if asked about intervention:

> "If the measurement shows a real, trainable failure — and the mentor says the timeline allows — we would try a prompting baseline first. If prompting closes it, that's a finding and we stop. A LoRA arm is out of default scope."

---

## 3. The mentor questions (do not leave without these)

0. **FIRST: "Are we studying multilingual task reliability, multilingual misuse, or a narrowly defined connection between them?"**
   - **Multilingual compliance reliability** — correct, evidence-supported decisions and appropriate abstention across query languages. Runs on the system already operated. **This is the only design the testbed currently supports.**
   - **Multilingual agent misuse** — does language change an agent's willingness/ability to carry out prohibited actions over multiple turns? Needs a tool-using agent under adversarial pressure — a harness built from scratch.
   *(If left to you: the first. Say so plainly.)*

1. **"Is an intervention arm in scope for 13 weeks, or should this be a measurement paper only?"**
   *(You want measurement-only unless they explicitly add prompting. Do not volunteer LoRA.)*

2. **"What compute access does the cohort have?"**
   *(Do not commit personal spend. Do not present a training platform as decided.)*

Optional: **"Who on the mentor roster works on multilingual evaluation or RAG evaluation?"**

---

## 4. Terminology — get this right out loud

| Sense | What it means | This paper? |
|---|---|---|
| **Illicit compliance** | The model complies with a *harmful* request | No (STING / *Helpful to a Fault*) |
| **Regulatory compliance** | A verdict about whether a checked requirement is satisfied | Yes — `verdict_correct` |
| **Evidence support / groundedness** | Is the answer supported by the retrieved/supplied context? | Yes — A.2 |
| **Reasoning faithfulness** | Does the stated reasoning reflect the actual decision process? | No |
| **Abstention appropriateness** | Overreach vs unnecessary abstention, conditional on case determinacy | Yes — two rates, not "calibration" |

**Say which sense you mean every time.** "Faithfulness" unqualified means a different paper to half the room. "Calibration" means Brier/ECE; we are not computing those unless we log a probability.

---

## 5. What NOT to say

- Don't claim novelty. Say: *"each component exists; the intersection in a regulated operational setting is underexamined."* Name Amiraz, Li et al. (BordIRLines), GaRAGe. Verify before citing.
- Don't oversell Puente. True: deployed, grounded, citation-backed, import-only, broker-in-the-loop, English analysis. Not true: fine-tuned model, proprietary flywheel, export compliance, WhatsApp, ES/PT analysis, "exposes each stage," language degradation.
- Don't lead with tools (Langfuse, deepeval, vLLM, managed training APIs).
- Don't say the study "extends" *Helpful to a Fault*. Say "informed by."
- Don't present the rubric as settled. Primary endpoints below are the part to defend; scales are provisional.
- Don't say sharing a model vendor with STING makes results comparable.
- Don't say N=5 shows a language gap.
- Don't say disagreement across languages is failure on its face.

---

## 6. Week-1 commitments you can offer the team

- A 5-case pilot set: EN + ES query variants, **fixed English evidence**, gold labels with recorded confidence (this week). PT only if a PT-BR speaker is on the team.
- A reproducible runner that writes 5 cases × 2 languages × **3 replicates = 30 rows** (45 if PT is in) with every B.1 field present, `null` when the provider does not expose it.
- A literature verification pass: bibliography keys for the Part C list by week 2, not "5 anchor papers."
- Access to a deployed grounded system as the testbed — evaluated as English-output, frozen-evidence.

---

## 7. After the call — write these down

- Mentor's answer on Q0 (reliability vs misuse): ____________________
- Mentor's answer on measurement-only vs intervention: ____________________
- Compute access: ____________________
- Teammates + what each brings (flag a second ES speaker; flag PT-BR): ____________________
- Any redirect to a different research question: ____________________

---
---

# DOCUMENT B — RUBRIC v0.5 + RUNNER SPEC

**Status:** v0.5 **DISCUSSION DRAFT**, revised 2026-09-20 after adversarial review. Provisional until (a) the mentor answers Q0, (b) the mentor sizes the project as measurement-only or not, and (c) a second Spanish speaker reviews ES translations. PT is out of the paper until a PT-BR speaker produces and a second speaker reviews.

**Scope:** multilingual **task reliability** only. Does not measure reasoning faithfulness. Does not replicate *Helpful to a Fault*. If the mentor steers toward misuse, this rubric does not apply.

**Stack:** for *Puente*, ADR-0005 settles Langfuse + deepeval. **That ADR does not decide Algoverse tooling.** The pilot is JSONL-only and framework-agnostic.

---

## READ THIS FIRST — plain language for teammates

A fair test of whether the **same English-output pipeline** handles the same import-compliance case differently when the **question** is asked in English vs Spanish, with the **same English reference packet**.

Fictional example — the packet says the shipment requires a certificate, but the case doesn't say whether one exists. The useful English answer is: *"I can't determine whether this requirement is satisfied. Please provide the certificate."*

1. **Did it reach the right conclusion?** Record `verdict_correct` separately from whether the stated rule was the right rule.
2. **Can each material claim be traced to a fact ID or a passage ID?** A source outside the packet is not automatically fabricated. Different failure, different fix.
3. **Did it know when it couldn't answer?** Two rates: **overreach** (answered an under-determined case) and **unnecessary abstention** (declined an answerable case). Timeouts are not abstention. This is not safety refusal and it is not calibration.
4. **Did it cover the decisive issue?** Short, fully supported, useless answers fail coverage.
5. **Was the comparison fair?** Same facts, jurisdiction, date, English passages. Change query language. One different answer does not prove a language effect: review translations, repeat each cell three times, report variance.
6. **The runner** is a lab notebook: send a case → save exactly what happened. Grade afterward.
7. **What five cases accomplish:** they prove the harness. They do not show Spanish performs worse. Inferential claims need on the order of 80–100 language triples, or we publish a descriptive case study and do not test.
8. **Where a failure happened (E1 only):** generation, citation attachment, or decision policy. **Not retrieval** — the packet is frozen. Live retrieval is a second experiment.

---

## PART A — The rubric

### A.-1 Positioning

**The gap is an intersection, not a void.** Prior work covers multilingual safety (Wang et al. 2024 / XSafety; Yong et al. 2025 survey), cross-lingual RAG (Li et al. 2025 / BordIRLines; Amiraz et al. 2025), and claim-level grounding (GaRAGe 2025). Underexamined: regulated operational decisions × parallel query languages × fixed evidence × claim-level support + abstention.

**Central question:**

> When a deployed English-output regulatory RAG pipeline receives semantically matched import-compliance queries in English and Spanish over a fixed English evidence packet, how do verdict correctness, claim-level evidentiary support, citation precision, and abstention appropriateness vary by query language?

**Two experiments (do not collapse):**

| ID | What varies | What is held | What you can localize | Default 13-week? |
|---|---|---|---|---|
| **E1** | query language | facts, jurisdiction, date, **evidence packet** | generation, citation attachment, decision policy | **yes** |
| **E2** | query language **and** live retrieval | facts, jurisdiction, date; evidence **not** frozen | retrieval (recall@k of gold passages) + E1 stages | no, unless mentor adds it |

E1 is **not** a retrieval-language bottleneck test. Amiraz et al. measured that bottleneck with live retrieval and language-restricted oracles. Copying their claim without their manipulation is invalid.

**Monolingual ES/PT (translated evidence) is not E1 and is not the deployed system.** The governing authorities are English. Translated law is a different factor. Out of v0.5.

**Positioning paragraph (draft):**

> This study evaluates query-language-conditioned reliability in a deployed retrieval-augmented regulatory-assistance system whose analysis output is English. Using semantically matched import-compliance queries in English and Spanish over a fixed English evidence packet, we measure regulatory-disposition correctness, claim-level evidentiary support, citation precision, and abstention appropriateness. The design is informed by cross-lingual RAG and claim-level grounding evaluations, and by multilingual safety research, while focusing on an underexamined regulated operational setting. It does not measure illicit multi-turn agent assistance.

### A.0 The invariant

For each **case**, produce language variants in which these are **identical**:

- underlying facts (goods, quantities, values, parties, HS candidates), each with a stable `fact_id`
- jurisdiction / corridor
- effective date (tariff schedules are dated)
- **the evidence packet passed to the model** (same English authority passages, same order, same count)
- instruction language (default: English for all cells; if you vary it, it is a second factor)

Only **query language** varies in E1.

> If anything else varies, you are not measuring query language. Write this at the top of the dataset file.

### A.1 Primary endpoints (never collapse into one "quality" score)

A response can be well cited but operationally wrong, or operationally plausible but unsupported. Keep these apart.

| ID | Endpoint | Type | 0 / 1 / 2 meaning |
|---|---|---|---|
| A.1a | `verdict_correct` | binary | 0 no · 2 yes (no middle; treat as binary in tests) |
| A.1b | `rule_application_correct` | ordinal | 0 wrong rule / misapplied · 1 right rule, incomplete · 2 correct |
| A.2 | Evidence support (per material claim in `gold_claims[]`) | ordinal, then macro | 0 unsupported by case facts or supplied passages · 1 mixed · 2 every material claim traceable to a `fact_id` or `passage_id` |
| A.3 | Citation precision (per **emitted** citation only) | ordinal | 0 absent/wrong passage · 1 relevant but too broad · 2 directly supports the associated claim |
| A.4 | Coverage of `required_findings[]` | ordinal | 0 omits the decisive issue · 1 partial · 2 all required findings addressed |

**A.2 vs A.3:** A.2 is whether the *claim is supported*, even if the model emitted no citation string. A.3 is only about pointers the model actually produced. Do not average them.

**Citation categories — record separately, never averaged:**
- `outside_supplied_evidence` — real authority not in the packet. Closed-evidence violation; **not automatically fabricated**.
- `does_not_support_claim` — supplied passage that doesn't back the claim.
- `nonexistent_or_misrepresented` — fabricated, or a real source misquoted.

**Citation scoring is two steps.** (1) Does the citation **resolve**? Deterministic lookup in the packet / authority index. (2) Only if it resolves: does the cited text support the claim?

**"Pass" is narrow.** A pass verdict means *this specific checked requirement is satisfied* — never that the whole shipment is compliant. Write this into the instruction and the gold labels.

### A.5 Abstention appropriateness (not a 0–2 scale, not calibration)

Pre-label each case `determinacy ∈ {answerable, underdetermined}`.

Report **two rates, separately**, excluding `status != ok`:

- `overreach = P(answered | underdetermined)`
- `unnecessary_abstention = P(abstained | answerable)`

A correctly decisive answer on an answerable case is success on this endpoint. It is not "overconfident" and it is not a missing Likert 2.

Timeouts and malformed outputs are `status`: `timeout` | `malformed` | `provider_error`. They are not abstention and not failure-on-A.5.

**Abstention ≠ safety refusal.** "I lack the origin information needed to decide" is insufficient-information handling. Refusing assistance with wrongdoing is a different behavior. This rubric measures the former only.

### Derived analyses (not scored dimensions)

- **Paired disagreement** (was A.6): after scoring, derive `en_es_disagree_verdict` and `en_es_disagree_rule` from A.1a / A.1b. Disagreement is a descriptive signal, not failure on its face. Never report it without correctness.
- **Failure localization (E1):** `generation` | `citation_attachment` | `decision_policy`. **Not `retrieval`.** E2 adds `retrieval`.
- **Register / tone:** out of the 13-week rubric. Do not SFT for it.

### Pre-registered test (write before looking at results)

- **Unit:** case.
- **Primary endpoint:** paired difference in `verdict_correct` (ES − EN). PT only if in the paper.
- **Test:** exact McNemar on the paired binary table; Holm correction if both ES and PT are tested.
- **Effect size:** paired proportion difference + bootstrap CI.
- **Replicates:** majority vote per cell across k=3, **and** report per-cell variance. Do not pretend temperature 0 is deterministic.
- **Pilot N=5:** harness only. Inferential claims require on the order of **80–100** language triples for a ~15pp gap at conventional power, or the paper is a descriptive case study and does not test.
- Report **absolute scores per language**, never only the gap.
- Report `n_claims` and the two abstention rates alongside every support number.
- Pre-declare regression tolerance before touching the held-out set.
- Held-out test set untouched until the design is frozen.
- Inter-rater agreement (Cohen’s κ or Gwet’s AC1) for human labels.
- Every score has `scored_by`: `human` for the pilot. Judges later, and only after agreement against human scores (Bean et al.).

### Gold labels

- Discrete gold (HS code, duty rate, yes/no) **only when** a dated CROSS/HTSUS passage in the packet entails the answer. Record `gold_passage_id` and `effective_date`.
- Each case file includes `gold_claims[]` (material-claim segmentation is **not** the parser's job), `required_findings[]`, `determinacy`, `gold_verdict`, `gold_citations[]`.
- Raters: founder + one independent rater. **Broker adjudicates** disagreements and all under-determined labels. Adjudication rule: broker wins; if no broker, the item is `adjudication_status=unresolved` and is excluded from inferential tables.
- Each label records: the documented *interpretation* of the authority (a passage does not supply the case label by itself), `reviewer_id`, `adjudication_status`, confidence.
- Founder-as-rater is a conflict. Report it as a limitation. Do not call it independent dual annotation.
- ES: second Spanish speaker reviews translations **before** any number is shown in a paper or a group meeting beyond the harness check.
- PT-BR: not in the paper until a PT-BR speaker produces the queries and a second speaker reviews. Provisional self-translation is harness-only.

### Which failures a fine-tune can even fix

Out of default scope. Restated so nobody proposes SFT in week 2:

- Grounding / citation: mostly retrieval, packet construction, prompt — **not** weights.
- Discrete correctness: exact match vs gold; retrieval + prompt first.
- Language parity: **the measurement question**, unknown.
- Register: SFT-shaped, and not this paper.

---

## PART B — Runner spec

**What it is:** a small reproducible script. Take cases → call a model → save everything → make results re-derivable six weeks from now. Not a framework.

### B.1 Hard requirements

Every run writes one row per
`case_id × query_language × retrieval_mode × prompt_id × model_id × replicate_id`.

Do not pack `mono | crosslingual | prompt_v2 | lora_v1` into one `condition` enum. Those are crossed factors. v0.5 uses `retrieval_mode=frozen` only.

```
run_id, run_timestamp, git_sha, dataset_version, dataset_hash, dependency_versions{}
schema_version
case_id
query_language                      # en | es | pt
instruction_language                # default en for all cells
evidence_language                   # en in E1
retrieval_mode                      # frozen | live  (E1 = frozen)
prompt_id, prompt_template_hash, adapter_id (null)
replicate_id                        # 0..k-1  intended repeated trial
attempt                             # 1..n retries of the same replicate; never mixed with replicate_id
model_name, model_version (null if provider doesn't expose)
temperature, max_tokens, seed (null if unsupported)
thinking_or_reasoning_settings{}
rendered_request                    # COMPLETE request as sent
evidence_snapshot[]                 # passage_id + full text; immutable
evidence_hash                       # sha256 of canonical JSON of evidence_snapshot[]
case_facts[]                        # fact_id + text
gold_passage_ids[]                  # for E2 recall; still store in E1 for later
retrieval_query (null in E1)
retrieved_passage_ids[] (equals snapshot ids in E1)
recall_at_k (null in E1)
raw_output
output_language                     # detected; null if detector not run
parsed_verdict, parsed_claims[], parsed_citations[]
parser_version (null until score.py)
input_tokens (null ok), output_tokens (null ok), prompt_token_len
tokenizer_name (null ok)
max_tokens_hit                      # bool | null
latency_ms, cost_estimate (null ok)
provider_request_id (null ok)
status                              # ok | timeout | malformed | provider_error
error (nullable)
```

Score rows (later, `score.py`) join on
`run_id + case_id + query_language + retrieval_mode + prompt_id + model_id + replicate_id`
plus `rubric_version`, `scored_by`, `claim_segmenter_version` (must match `gold_claims[]` version).

Never guess a seed, revision, token count, or cost to fill a field. `null` is valid.

### B.2 Design decisions

1. **JSONL only for the pilot.** Langfuse is KAN-91 work, not week 1.
2. **Separate run from scoring.** Re-score without re-calling the model.
3. **Idempotency.** Resume key includes exact rendered request, evidence hash, model version, generation settings, **and `replicate_id`**. Resume completed trials; record retries as `attempt`; never skip a failed request; never reuse replicate 0 when you meant replicate 1.
4. **Determinism.** Temperature 0 where supported; record it where it isn't; **k=3 replicates per cell**; report variance. Temperature 0 on Gemini is not determinism.
5. **Never overwrite `raw_output`.**

### B.3 Layout

```
eval/
  cases/            # one JSON per case: facts, English packet, en/es queries, gold
  prompts/          # versioned templates, hashed
  runner.py         # cases -> model -> results.jsonl
  score.py          # results.jsonl -> scores.jsonl   (not week 1)
  report.py         # later
  results/<run_id>/
```

### B.4 Week-1 success criterion

**5 cases × 2 query languages (EN, ES) × 3 replicates = 30 rows** in `results.jsonl`, every B.1 field present (`null` allowed). Frozen English evidence. One model. No scoring script, no report, no second model, no LoRA.

If a PT-BR speaker is already on the team: 5 × 3 × 3 = 45 rows, still harness-only.

That's it. A teammate given `results.jsonl` tomorrow must be able to say exactly what was run.

### B.4b Seed test set (next two weeks)

- **10 cases** a broker would actually bring: facts with `fact_id`s, English evidence packet, gold verdict, `gold_claims[]`, `required_findings[]`, `determinacy`, `gold_passage_ids[]`.
- Discrete gold where a dated CROSS/HTSUS passage entails the answer.
- EN + ES queries. ES reviewed by a second speaker before any paper number. PT-BR only with speakers.
- At least 2 under-determined cases so both abstention rates are measurable.
- First 3 cases shared this week; the 5-case runner pilot draws from these.
- Confound checklist on the dataset card: translation review status, instruction language, tokenizer fertility, founder-authored case flag, effective date.

### B.5 Known limits of the pilot (say this out loud)

- 5 cases prove the **harness**. They prove **nothing** about a language gap.
- ES translations are provisional until a second speaker reviews them.
- PT is not in the paper without PT-BR speakers.
- Hand-written gold labels carry founder uncertainty and a conflict of interest; broker adjudication is required before inferential tables.
- E1 cannot localize retrieval error.

---

## PART C — Reading list (verify venue/year before citing)

Closest (anchors for a reliability paper):

1. Chen Amiraz, Yaroslav Fyodorov, Elad Haramaty, Zohar Karnin, and Liane Lewin-Eytan. 2025. *The Cross-Lingual Cost: Retrieval Biases in RAG over Arabic-English Corpora.* ArabicNLP 2025. Retrieval as a source of multilingual error **when retrieval actually runs**.
2. Bryan Li et al. 2025. *Multilingual Retrieval Augmented Generation for Culturally-Sensitive Tasks: A Benchmark for Cross-lingual Robustness.* Findings of ACL 2025. Dataset: BordIRLines (49 languages). Retrieval-language policies, not frozen packets.
3. Ionut Teodor Sorodoc et al. 2025. *GaRAGe: A Benchmark with Grounding Annotations for RAG Evaluation.* Findings of ACL 2025. Claim-to-passage grounding; English-only.
4. Andrew M. Bean et al. 2025. *Measuring what Matters: Construct Validity in Large Language Model Benchmarks.* NeurIPS 2025 Datasets and Benchmarks.

Also on the verification list:

5. Wenxuan Wang et al. 2024. *All Languages Matter: On the Multilingual Safety of LLMs.* Findings of ACL 2024. XSafety.
6. Zheng Xin Yong, Beyza Ermis, Marzieh Fadaee, Stephen Bach, and Julia Kreutzer. 2025. *The State of Multilingual LLM Safety Research.* EMNLP 2025.
7. Shivalika Singh et al. 2025. *Global MMLU.* ACL 2025. Translation quality and culturally sensitive vs agnostic subsets.
8. Angelika Romanou et al. 2025. *INCLUDE: Evaluating Multilingual Language Understanding with Regional Knowledge.* ICLR 2025. Regional knowledge ≠ translation.

Informed-by, not anchors:

9. Nivya Talokar et al. 2026. *Helpful to a Fault: Measuring Illicit Assistance in Multi-Turn, Multilingual LLM Agents.* ICML 2026. STING. ES/PT absent.
10. Deniz Bayazit, Aaron Mueller, and Antoine Bosselut. 2026. *Crosscoding Through Time.* ACL 2026. Agreement-feature overlap in BLOOM, not RAG reliability.

Skip blog/dev.to/vendor links.

---

## Scope cut list (so it fits 13 weeks)

Cut, in this order, unless the mentor puts it back:

1. LoRA / SFT tables
2. Register / A.7
3. Live retrieval (E2) and translated-evidence "monolingual" ES/PT
4. Portuguese until PT-BR speakers exist
5. LLM-as-judge until ≥30 human-scored rows exist for agreement

Keep: E1 frozen English packet, EN vs ES query language, English output, A.1a + A.2 + two abstention rates, 30-row harness, then as many broker-adjudicated cases as the calendar allows.
