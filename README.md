# Anthropic Prompt Builder · باني البرومبت بمنهج Anthropic

An [Agent Skill](https://docs.claude.com/en/docs/agents-and-tools/agent-skills) that writes a
professional, copy-ready prompt (or system prompt, or prompt template) **for any LLM** —
Claude, ChatGPT/GPT, Gemini, or an open model — in two phases:

1. **Clarification gate.** If the request is vague, it asks two to three numbered questions,
   each with a default you accept by replying "default", then stops. If the request is
   already complete it skips the gate. One round only.
2. **Build.** It assembles the prompt with XML tags (`<role>`, `<context>`, `<instructions>`,
   `<constraints>`, `<examples>`, `<output_format>`), `{{variables}}`, labelled illustrative
   examples, explicit edge cases, and a reasoning instruction **matched to the target model**.
   The prompt arrives in one block with its assumptions, a variable list, and an honest note
   that it was not run on your model.

It is **one skill for all models**, not one per model: the structure is shared, and what
changes with the target (thinking, parameters, delimiters) lives in
[`references/models.md`](en/references/models.md).

مهارة واحدة لكل النماذج تكتب برومبتاً احترافياً جاهزاً للنسخ على مرحلتين: بوابة استيضاح قصيرة
(سؤالان إلى ثلاثة، لكل سؤال افتراضي، ثم توقّف) إن كان الطلب غامضاً، ثم بناءٌ بوسوم XML
ومتغيّرات وأمثلة وحالات حدّية صريحة وتعليمة تفكير تناسب النموذج المستهدف.

## What it does differently · ما الذي تفعله بخلاف الدليل القديم

Anthropic's tutorial predates current models. This skill follows current documentation where
they differ:

- **No prefill.** Seeding the assistant's reply is unsupported on Claude 4.6 and later (it
  returns a 400) and is not portable. It uses format instructions or structured outputs.
- **Reasoning depends on whether the target thinks internally**, not on its brand. For targets
  with built-in thinking (current Claude, OpenAI reasoning models, Gemini 3.x) it writes a
  general instruction and does not ask for reasoning in a tag or a JSON field; depth is set by
  the call parameter (`effort`, `reasoning.effort`, `thinking_level`). Manual
  `<reasoning>`/`<answer>` is for targets without thinking, or with it switched off.
- **Edge cases are stated explicitly**, plus a catch-all line, because a measured open model
  either spun on an empty input or left the format.
- **Three separate languages:** the prompt's language (your request wins, never switched
  silently), the model output's language (stated explicitly), and the language it talks to you
  in.
- **Chat app or API?** For ChatGPT/Gemini/Claude.ai it gives one block to paste; for an API it
  gives a system (or developer) block plus a user template.

## Install · التثبيت

Copy one folder (`en/` for English, `ar/` for Arabic) into your agent's skills directory under
the name `anthropic-prompt-builder`.

```powershell
# Claude Code on Windows (PowerShell)
Copy-Item -Recurse .\en "$env:USERPROFILE\.claude\skills\anthropic-prompt-builder"
```

```bash
# Claude Code on macOS / Linux
cp -r ./en ~/.claude/skills/anthropic-prompt-builder
```

**Antigravity.** A folder under `~/.agents/skills` is *not* auto-discovered; Antigravity's own
guide lists only `.agents/` in a workspace root, `GEMINI.md`/`AGENTS.md` rules, and
`~/.gemini/config/`. What worked here (verified: the skill appeared in the agent's `<skills>`
list and was activated from its description alone) is a plugin layout in the global config:

```powershell
$p = "$env:USERPROFILE\.gemini\config\plugins\anthropic-prompt-builder"
New-Item -ItemType Directory -Force "$p\skills" | Out-Null
Copy-Item -Recurse .\en "$p\skills\anthropic-prompt-builder"
Set-Content "$p\plugin.json" '{"name": "anthropic-prompt-builder", "description": "Build professional prompts for any LLM."}' -Encoding ascii
```

Then restart Antigravity and open a new conversation. It is a **copy**, so re-copy after you
update the skill. A `skills.json` declaration and a junction were also tried and showed no
effect in one session; the cause was not determined (they may need a restart). A workspace
`.agents/skills/` folder is documented by Antigravity but I did not test it.

Other agents that read `SKILL.md` skills use their own folder; check theirs (not verified
here). Chat apps have no skill install: the nearest is pasting the body of `SKILL.md` into
custom instructions (a Gem, a GPT, a Project); its length limits and how those apps follow it
are **not verified**, and the file is long and technical for casual use.

## Status and known limits · الحالة والحدود

- **Tested, with Claude as the host:** vague, complete and English-for-Arabic-users requests;
  a prefill trap; non-Claude and OpenAI-reasoning and Gemini-API and ChatGPT-app targets (the
  outputs were read by hand against each vendor's docs); a request naming no model. About a
  dozen non-interactive runs of the skill on Sonnet 5.5. Defects found this way were fixed and
  re-run:
  silent language switching, a `reasoning` JSON field used to get around the thinking rule,
  and missing edge-case rules.
- **Tested prompts it produced, on the target model:** a customer-support assistant and a JSON
  sentiment classifier on Claude Sonnet 5.5 (nine inputs: off-topic, injection, empty,
  sarcastic — all as specified), and a sentiment prompt on **Gemma 4 12B** locally with
  thinking on and off. Findings: an empty input made the thinking-on model spin for ~4,000
  tokens, and with thinking off it left the format; a rule for the case fixed both, a
  catch-all line fixed most other odd inputs but not the truly empty one. **No prompt has been
  run on OpenAI or Gemini models, or on Llama.**
- **Tested hosted in Antigravity** (model reported by the app: "Gemini 4 Argon (Medium)"), four
  fresh conversations, skill never named by the user, transcripts read from the app's own
  database: it was activated from its description in all four; a vague request got three
  questions with defaults and then a built prompt; a request with a prefill-plus-thinking-tags
  trap was corrected (no prefill, no thinking tags, structured outputs recommended, a short
  justification field instead); a request naming no model assumed Gemini and said so. One
  deviation: a fairly complete request still got two clarifying questions. The assistant
  assumed "Gemini 3.x" because that is all `models.md` covers; the Gemini 4 docs were not
  reviewed.
- **Not tested:** the skill hosted in ChatGPT or in the Gemini app; the request to "think inside
  `<thinking>` tags" *through Claude* (the platform's safeguard rejected that test message
  before the skill ran); use with a screen reader.
- The model-specific rules reflect vendor documentation as of **2026-10-30** and will go stale
  as models change. Treat `references/models.md` as the place to update.
- Every prompt it produces is a hypothesis until you try it on your own inputs, and no prompt
  guarantees a format: validate the reply in your program.

## Sources and credit · المصادر

- Anthropic's [prompt-eng-interactive-tutorial](https://github.com/anthropics/prompt-eng-interactive-tutorial)
  (chapters 1–9) is the basis of the method. That repository has no license file, so
  **nothing is copied from it**: the skill paraphrases its ideas, and every example here is
  original. Chapter references like `[ch. 4]` point to it.
- Current prompting documentation from Anthropic, OpenAI and Google, for the corrections and
  the per-model notes.
- This project is **not affiliated with or endorsed by Anthropic, OpenAI or Google.**

## License · الرخصة

MIT — see [LICENSE](LICENSE).
