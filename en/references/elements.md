# Prompt elements and their sources

Tags: **[tutorial ch. N]** is Anthropic's interactive tutorial (that chapter); **[docs]** is
Anthropic's current documentation (checked through Context7 on 2026-10-30). Where the two
disagree, the docs are what applies.

| Element | Tag | Omit when | Source |
|---|---|---|---|
| Role | `<role>` or the system prompt | only for a very simple task | tutorial ch. 3 |
| Context / goal | `<context>` | a task with no background | tutorial ch. 9 |
| Material and variables | one tag per piece (`<email>`…) | no inputs | tutorial ch. 4 |
| Instructions | `<instructions>` | never | tutorial ch. 2 |
| Constraints | `<constraints>` | no real constraints | tutorial ch. 9 |
| Examples | `<examples>` / `<example>` | a simple task with free-form output | tutorial ch. 7; docs: 3–5, varied |
| Output format | `<output_format>` | free-form output | tutorial ch. 5 |
| Thinking | a general instruction, or `<reasoning>` + `<answer>` | a single-step task | tutorial ch. 6, **corrected by the docs** |
| "I don't know" out + quotes first | inside the instructions | a task that does not rest on a document | tutorial ch. 8 |

## Corrections the current docs make to the tutorial

- **Prefill** (tutorial ch. 5, 7, 9 element 10): unsupported on Claude 4.6 and later; the
  request returns a 400. Alternatives: `<output_format>` instructions, or structured outputs
  through the API.
- **Thinking** (tutorial ch. 6): on current Claude (Sonnet 5.5, Opus 5/5.5, Fable 5/5.1)
  thinking is built in and tuned with `effort`; a request to *write out the reasoning* in any
  tag (the docs give `<thinking>` as the example) may be declined, so `<reasoning>` is not used
  there either. Manual `<reasoning>`/`<answer>` is for older Claude and for non-Claude models
  only. Self-checking stays in general and is removed for Opus 5 alone, which verifies its
  own work, so the instruction causes over-verification.
- **temperature** (tutorial ch. 8): the tutorial sets it to zero; it does not belong in the
  prompt, since it is an API parameter, mentioned to the user only if they ask about the call.

## What was not verified

The effect of this template on any particular model was not measured here beyond a handful
of sessions. The tutorial's examples were measured on older Claude 3 models; what was carried
over from them is backed by the current docs above, not by a test of mine.
