# Project Jesse — n8n Agentic Workflows

This repo contains eight autonomous, Discord-integrated n8n workflows (JSON) that implement **Project Jesse** Phase I & II agent capabilities: harvesting, parsing, enrichment, alignment, QA, trend analysis, user interaction, and educational pathways. Every workflow emits **structured JSON artifacts**, stores summaries in the **n8n Data Store**, and posts **non-blocking FYIs to Discord**.

---

## Quick start

1. **Import** any `Jesse — *.json` file into n8n (`Workflows → Import from File`).
2. **Set credentials / env** you use (e.g., `DISCORD_BOT_TOKEN`, any API keys).
3. **Open the `Config` node** in each workflow to set URLs, thresholds, and channel IDs.
4. **Run** the `Start` or trigger node. Discord messages are informational and never block execution.

> Import safety: all JSON uses the **single-array fan-out** pattern in `connections` to avoid the n8n `undefined[0]` import bug (don't place links in `main[1]` for single-output nodes).

---

## Workflows

### 1) AI Job Posting Harvester

**File:** [Jesse — AI\_Job\_Posting\_Harvester.json](./Jesse_AI_Job_Posting_Harvester.json)

![AI Job Posting Harvester Workflow](https://raw.githubusercontent.com/Humanitariansai/Jesse/refs/heads/main/Agents/Jesse_AI_Job_Posting_Harvester.png)

**What it does**

* LLM **controller** plans queries and extraction instructions.
* Crawls job sources via search → fetch → **neural document understanding**.
* **Active learning** style retry: Parser A1 → assess → low-confidence fallback Parser A2.
* Emits normalized job JSON; posts a concise Discord FYI.

**Key inputs (Config)**
`SEED_QUERIES, REGION, LANGUAGE, CONFIDENCE_THRESHOLD, MAX_NEW_SOURCES, SEARCH_API_BASE, JSON_SINK_URL?, DISCORD_CHANNEL_ID`

**Output record (fields)**
`title, company, location, posted_date, employment_type, description, requirements[], skills[], source_url, soft_confidence, needs_review, reasons[], harvest_run_ts`

---

### 2) AI Syllabi Analysis Agent

**File:** [Jesse — AI\_Syllabi\_Analysis\_Agent.json](./Jesse_AI_Syllabi_Analysis_Agent.json)

![AI Syllabi Analysis Agent Workflow](https://raw.githubusercontent.com/Humanitariansai/Jesse/refs/heads/main/Agents/Jesse_AI_Syllabi_Analysis_Agent.png)

**What it does**

* Discovers + ingests syllabi (PDF/HTML/Docx) via search and seed URLs.
* Uses an OCR+layout API, then LLM **zero-shot** extraction of outcomes/competencies.
* **Self-supervised alignment** maps outcomes to SkillsLex taxonomy.
* **Knowledge distillation** produces lightweight edge rules for future parsing.
* Stores records & distilled rules; sends Discord FYIs.

**Key inputs (Config)**
`SEED_SYLLABI, SEED_QUERIES, REGION, LANGUAGE, CONFIDENCE_THRESHOLD, MAX_NEW_SOURCES, SEARCH_API_BASE, DOC_PARSER_URL, DOC_PARSER_KEY, SKILLS_TAXONOMY_URL, JSON_SINK_URL?, DISCORD_CHANNEL_ID`

**Output record (fields)**
`course_code, course_title, term, instructor, program, modality, workload, tools[], learning_outcomes[], competencies[], topics[], readings[], assessments[], mapped_skills[], unmapped_phrases[], alignment_notes[], source_url, soft_confidence, needs_review, reasons[], harvest_run_ts`

---

### 3) AI SkillsLex Enrichment Agent

**File:** [Jesse — AI\_SkillsLex\_Enrichment\_Agent.json](./Jesse_AI_SkillsLex_Enrichment_Agent.json)

![AI SkillsLex Enrichment Agent Workflow](https://raw.githubusercontent.com/Humanitariansai/Jesse/refs/heads/main/Agents/Jesse_AI_SkillsLex_Enrichment_Agent.png)

**What it does**

* LLM **few-shot** guided generation of full lexical entries per skill (definition, relations).
* Proposes **knowledge-graph completion** edges (requires/enables/part\_of/…).
* Optional **RLHF** loop: webhook ingests human feedback to update few-shot memory.
* Stores entries and KG suggestions; Discord FYIs enumerate edges.

**Key inputs (Config)**
`SEED_SKILLS, DOMAIN_HINTS, MAX_SKILLS_PER_RUN, CONFIDENCE_THRESHOLD, TAXONOMY_URL?, KG_SNAPSHOT_URL?, FEEDBACK_WEBHOOK_PATH, JSON_SINK_URL?, DISCORD_CHANNEL_ID`

**Output record (fields)**
`skill, entry{definition, classification, synonyms[], hypernyms[], hyponyms[], meronyms[], holonyms[], verb_relations[], antonyms[], relational_adjectives[]}, soft_confidence, quality_notes[], kg_nodes[], kg_edges[], needs_review, reasons[], run_ts`

---

### 4) AI Cross-Domain Alignment Agent

**File:** [Jesse — AI\_Cross\_Domain\_Alignment\_Agent.json](./Jesse_AI_Cross_Domain_Alignment_Agent.json)

![AI Cross-Domain Alignment Agent Workflow](https://raw.githubusercontent.com/Humanitariansai/Jesse/refs/heads/main/Agents/Jesse_AI_Cross-Domain_Alignment_Agent.png)

**What it does**

* LLM controller defines **multi-objective** weights & acceptance thresholds.
* Builds program↔career pairs; runs **neural similarity** + **attention** gap analysis.
* Generates **curriculum enhancement** recommendations to close critical gaps.
* Scores/accepts alignments and posts a summary to Discord.

**Key inputs (Config)**
`PROGRAMS_SOURCE_URL, CAREERS_SOURCE_URL, JOBS_SOURCE_URL, REGION, LANGUAGE, CONFIDENCE_THRESHOLD, MAX_PAIRS, JSON_SINK_URL?, DISCORD_CHANNEL_ID`

**Output record (fields)**
`program_id/title, career_id/title, similarity, coverage_rate, demand_score, salary_score, alignment_confidence, gap_penalty, objective_score, matched_skills[], gaps[], recommendations[], accepted, thresholds, run_ts`

---

### 5) AI Data Quality Assurance Agent

**File:** [Jesse — AI\_Data\_Quality\_Assurance\_Agent.json](./Jesse_AI_Data_Quality_Assurance_Agent.json)

![AI Data Quality Assurance Agent Workflow](https://raw.githubusercontent.com/Humanitariansai/Jesse/refs/heads/main/Agents/Jesse_AI_Data_Quality_Assurance_Agent.png)

**What it does**

* Runs LLM-aided **anomaly detection** against schema/content rules.
* **Self-supervised duplicate detection** (canonical signature + Jaccard) with a persistent index in the n8n Data Store.
* **Uncertainty estimation** (coverage-based fallback) → unified `soft_confidence`.
* **Temporal drift** checks for evolving definitions/skills.
* Outputs QA records and Discord FYIs (non-blocking).

**Key inputs (Config)**
`JOBS_SOURCE_URL, SYLLABI_SOURCE_URL, SKILLSLEX_SOURCE_URL, TEMPORAL_SNAPSHOT_URL, DUP_SIM_THRESHOLD, CONFIDENCE_THRESHOLD, MAX_ITEMS, LANGUAGE, REGION, JSON_SINK_URL?, DISCORD_CHANNEL_ID`

**Output record (fields)**
`item_type, item_id, signature{hash, canonical_text}, duplicate{is_duplicate, best_similarity}, anomalies[], schema_ok, uncertainty, soft_confidence, temporal{ok, drift_score, changed_terms[]}, thresholds{confidence, dup_similarity}, needs_review, run_ts, source_excerpt`

---

### 6) AI Trend Analysis Agent

**File:** [Jesse — AI\_Trend\_Analysis\_Agent.json](./Jesse_AI_Trend_Analysis_Agent.json)

![AI Trend Analysis Agent Workflow](https://raw.githubusercontent.com/Humanitariansai/Jesse/refs/heads/main/Agents/Jesse_AI_Trend_Analysis_Agent.png)

**What it does**

* Builds **per-skill time series** from postings (daily bins).
* Forecasts with robust linear fit; **neural topic modeling** clusters emerging groups.
* **Causal check** against baseline to discount seasonality.
* Computes an **emergence score**; marks skills as 🚀 emerging; Discord FYIs.

**Key inputs (Config)**
`JOBS_SOURCE_URL, SKILLSLEX_SOURCE_URL?, WINDOW_DAYS, FORECAST_HORIZON_DAYS, TOP_N_SKILLS, MIN_FREQ, CONFIDENCE_THRESHOLD, JSON_SINK_URL?, DISCORD_CHANNEL_ID`

**Output record (fields)**
`skill, topic_label, window_days, forecast_horizon_days, stats{mean,slope,…}, trend{label,r2,slope}, forecast[], causal{…}, emergence{growthScore, deltaScore, noveltyScore, causalScore, emergence_score}, accepted, run_ts`

---

### 7) AI User Interaction Agent

**File:** [Jesse — AI\_User\_Interaction\_Agent.json](./Jesse_AI_User_Interaction_Agent.json)

![AI User Interaction Agent Workflow](https://raw.githubusercontent.com/Humanitariansai/Jesse/refs/heads/main/Agents/Jesse_AI_User_Interaction_Agent.png)

**What it does**

* **Webhook** entrypoint for student queries.
* LLM **NLU** extracts intent/entities; **personalized recommender** selects careers/programs/courses; **XAI** layer explains evidence & alignment.
* **Dialogue manager** updates session state and proposes next questions.
* Persists interaction + session state; Discord FYIs summarize outcomes.

**Key inputs (Config)**
`WEBHOOK_PATH, PROFILE_SOURCE_URL, SKILLSLEX_SOURCE_URL, PROGRAMS_SOURCE_URL, CAREERS_SOURCE_URL, SESSION_TTL_DAYS, CONFIDENCE_THRESHOLD, JSON_SINK_URL?, DISCORD_CHANNEL_ID`

**Output record (fields)**
`session_id, student_id, query_text, nlu{intent,entities,…}, profile_summary, recommendations{careers[], programs[], courses[], skills_to_build[], pathway[]}, explanations{…}, dialogue{state_update, next_questions[], short_reply}, soft_confidence, needs_review, run_ts`

---

### 8) AI Educational Pathways Agent

**File:** [Jesse — AI\_Educational\_Pathways\_Agent.json](./Jesse_AI_Educational_Pathways_Agent.json)

![AI Educational Pathways Agent Workflow](https://raw.githubusercontent.com/Humanitariansai/Jesse/refs/heads/main/Agents/Jesse_AI_Educational_Pathways_Agent.png)

**What it does**

* LLM controller defines **multi-objective** weights and **RL-style** exploration (epsilon-greedy) over candidate course sequences.
* Enforces **prerequisites** and term credit caps; scores candidates on alignment, time, balance, cost, and risk.
* Generates **counterfactual** alternatives (swap electives, accelerate core, balance workload).
* Optional **feedback webhook** updates policy memory; Discord FYIs per candidate.

**Key inputs (Config)**
`WEBHOOK_PATH, FEEDBACK_WEBHOOK_PATH, PROFILE_SOURCE_URL, COURSES_SOURCE_URL, PREREQS_SOURCE_URL, PROGRAMS_SOURCE_URL, CAREERS_SOURCE_URL, SKILLSLEX_SOURCE_URL, K_CANDIDATES, DEFAULT_CREDITS_PER_COURSE, MAX_TERMS, MAX_CREDITS_PER_TERM, CONFIDENCE_THRESHOLD, JSON_SINK_URL?, DISCORD_CHANNEL_ID`

**Output record (fields)**
`session_id, student_id, career_goal, candidate_id, sequence[{term,courses[]}], scores{career_alignment,time_to_completion,workload_balance,cost,prereq_risk}, objective_score, soft_confidence, rationale[], alternatives[], constraints, weights, accepted, run_ts`

---

## Common conventions

* **Discord**: Set `DISCORD_BOT_TOKEN` credential in n8n and the `DISCORD_CHANNEL_ID` per workflow. Messages are **FYIs only**; pipelines continue automatically.
* **JSON sink (optional)**: If `JSON_SINK_URL` is set, a `POST` mirrors each record to your storage/analytics API.
* **Data Store**: Every workflow persists a compact record for downstream processing or audit. Some agents also maintain memory blobs (e.g., duplicate index, distilled rules, few-shot/policy memory).
* **Confidence & review**: Each agent computes/propagates `soft_confidence` and sets `needs_review` when below the configured threshold.
* **Import safety rule (n8n)**: Single-output nodes must fan out using **one inner array**:

  ```json
  "connections": {
    "NodeName": { "main": [[ { "node": "A" }, { "node": "B" } ]] }
  }
  ```

  Never add `main[1]` for single-output nodes—this is the cause of the classic **"Cannot read properties of undefined (reading '0')"** import error.

---

## Credentials & environment

* **Discord**: `DISCORD_BOT_TOKEN`
* **Document/OCR** (Syllabi agent): `DOC_PARSER_KEY`
* Any custom APIs you point `*_SOURCE_URL` to should allow anonymous GET or be fronted by n8n credentials you configure on the HTTP Request nodes.

---

## Testing tips

* Use small **seed sets** first (`MAX_*` / `TOP_N_*`), then scale.
* Point `JSON_SINK_URL` to a request bin during development to inspect payloads.
* For the **User Interaction** and **Pathways** agents, hit the configured `WEBHOOK_PATH` with sample payloads to simulate sessions & feedback.

---

## License & attribution

These workflows implement the architecture described in **Project Jesse** and honor Jesse B. Davis's guidance legacy by connecting education and careers with modern AI. Adjust, extend, or compose them as needed for your deployment.
