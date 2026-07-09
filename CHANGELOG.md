# Changelog

All notable changes to Groundwork are recorded here. The format follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and the project
follows [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.1.0] - 2026-07-09

First public release. The pipeline is built and validated end-to-end through
Airtable persistence. Email sending is intentionally left unwired — a documented
scope boundary, not an omission.

### Added
- Five-agent SMYKM cold-email pipeline as an importable, credential-scrubbed n8n
  workflow (`workflows/groundworksmykmpipeline.json`): 31 nodes across
  intake/sourcing, checkpoint/dedup, research/synthesis, a bounded
  generator-critic loop, and persistence.
- Research, Synthesis, Generator, Critic, and an orchestrating SMYKM agent, each
  on a separate model (Qwen 3.7 Max, DeepSeek V4 Pro, Claude Sonnet 4.6 via AWS
  Bedrock, Gemini 3.5 Flash via Google Vertex, Mistral Medium 3.5) so the model
  that writes an email is never the one that judges it.
- Three-table Airtable data model (`Lead_Profiles`, `Leads`,
  `Lead_Research_Events`) keyed on the lead's LinkedIn URL, with a re-runnable
  checkpoint on `pipeline_status` that skips any lead already finalized.
- Full case study (`docs/Groundwork - Case Study.PDF`): the architecture, the
  reasoning behind each model seat, what broke during the build, and the measured
  results.
- Three architecture diagrams (`assets/`): the system map, the generator-critic
  loop, and the Airtable data model.
- README with setup steps, a placeholder-ID reference table, and the tradeoffs
  made on purpose.
- MIT License.

### Known limitations
- The two-round revision cap is enforced by agent instruction, not a
  deterministic loop-counter node. It has behaved correctly in testing, including
  a live run that reached round two, but it is not yet structurally guaranteed.
  Documented in the case study (§7.5); slated for a later hardening pass.
- Sending is not wired. The final node emits a clean `ready_to_send` payload for
  a human to review or a downstream SMTP / SendGrid / Gmail node to pick up.

[0.1.0]: https://github.com/tesfa12michael/groundwork/releases/tag/v0.1.0
