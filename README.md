# Anthropic Prompt Builder · باني البرومبت بمنهج Anthropic

An [Agent Skill](https://docs.claude.com/en/docs/agents-and-tools/agent-skills) for Claude
that writes a professional, copy-ready prompt (or system prompt, or prompt template) for any
LLM, in two phases:

1. **Clarification gate.** If the request is vague, it asks two to three numbered questions,
   each with a default you can accept by replying "default", then stops. If the request is
   already complete it skips the gate. One round only.
2. **Build.** It assembles the prompt with XML tags (`<role>`, `<context>`, `<instructions>`,
   `<constraints>`, `<examples>`, `<output_format>`), `{{variables}}`, 3–5 labelled
   illustrative examples, and a reasoning instruction **matched to the target model**. The
   prompt arrives in one block, with its assumptions, a variable list, and an honest note that
   it was not run on your model.

مهارة لـClaude تكتب برومبتاً احترافياً جاهزاً للنسخ على مرحلتين: بوابة استيضاح قصيرة
(سؤالان إلى ثلاثة، لكل سؤال افتراضي، ثم توقّف) إن كان الطلب غامضاً، ثم بناءٌ بوسوم XML
ومتغيّرات وأمثلة وتعليمة تفكير تناسب النموذج المستهدف.

## What it does differently · ما الذي تفعله بخلاف الدليل القديم

Anthropic's tutorial predates current models. This skill follows the current documentation
where they differ:

- **No prefill.** Seeding the assistant's reply is unsupported on Claude 4.6 and later and
  returns a 400. It uses format instructions or structured outputs instead.
- **Reasoning depends on the model family.** On current Claude it writes a general "think
  carefully" instruction and leaves depth to `effort`; it does not ask for reasoning to be
  written in a tag or a JSON field. Manual `<reasoning>`/`<answer>` is kept for older Claude
  and non-Claude targets.
- **Three separate languages:** the prompt's language (the user's request wins, never
  switched silently), the model output's language (stated explicitly), and the language it
  talks to you in.

## Install · التثبيت

Copy one folder (`en/` for English, `ar/` for Arabic) to your skills directory under the name
`anthropic-prompt-builder`.

```powershell
# Windows (PowerShell)
Copy-Item -Recurse .\en "$env:USERPROFILE\.claude\skills\anthropic-prompt-builder"
```

```bash
# macOS / Linux
cp -r ./en ~/.claude/skills/anthropic-prompt-builder
```

Then ask Claude for a prompt: "write me a system prompt that …".

## Status and known limits · الحالة والحدود

- **Tested:** eight non-interactive Claude Code runs on Sonnet 5.5 — vague request, complete
  request, English prompt for Arabic-speaking users, prefill trap, non-Claude target — read
  by hand. Two defects found that way were fixed and the two affected cases re-run.
- **Not tested:** the request to "think inside `<thinking>` tags" — the platform's safeguard
  rejected that test message before the skill ran. No prompt it produced was run on its
  intended target model, and it was not tested with a screen reader.
- The model-specific rules reflect Anthropic's documentation as of **2026-10-30** and will go
  stale as models change. Treat `references/elements.md` as the place to update.
- Every prompt it produces is a hypothesis until you try it on your own inputs.

## Sources and credit · المصادر

- Anthropic's [prompt-eng-interactive-tutorial](https://github.com/anthropics/prompt-eng-interactive-tutorial)
  (chapters 1–9) is the basis of the method. That repository has no license file, so
  **nothing is copied from it**: the skill paraphrases its ideas, and every example here is
  original. Chapter references like `[ch. 4]` point to it.
- Anthropic's current prompt-engineering documentation, for the corrections above.
- This project is **not affiliated with or endorsed by Anthropic.**

## License · الرخصة

MIT — see [LICENSE](LICENSE).
