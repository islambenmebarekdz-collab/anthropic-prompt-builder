# What changes with the target model

One skill for every model; this file collects what differs between families. Tags:
**[docs]** from the vendor's documentation through Context7 on 2026-10-30, **[measured]**
tried by me on one model, **[inference]** not verified. These docs change as models change;
update this file when it goes stale.

| | Claude (current) | OpenAI (reasoning models) | Gemini 3.x | Open models (Gemma, Llama…) |
|---|---|---|---|---|
| Built-in thinking | yes, adaptive [docs] | yes [docs] | yes [docs] | varies: Gemma 4 12B yes; Llama unknown |
| Setting depth | `effort` | `reasoning.effort` | `thinking_level` (`minimal`/`low`/`medium`/`high`) [docs] | `reasoning_effort` in LM Studio [measured] |
| Asking for written-out reasoning | may be declined [docs] | unnecessary [docs] | short and direct preferred [docs] | manual if thinking is off or absent |
| System message | `system` | `developer` [docs] | System Instruction [docs] | depends on the chat template |
| Delimiters | XML tags [docs] | Markdown and XML together [docs] | XML or Markdown, **one format** [docs] | XML tags [inference] |
| Examples | 3–5, varied [docs] | try with none first [docs] | identical format across examples [docs] | as needed |
| Prefill | unsupported since 4.6 [docs] | generally unavailable [inference] | generally unavailable [inference] | not portable |

## Notes per family

**Claude.** Anthropic's docs: consistent descriptive tags, three to five examples, do not ask
for written-out reasoning on current models (it may be declined), and Opus 5 verifies its own
work so verification instructions are removed with it.

**OpenAI.** Reasoning models take a `developer` message. The docs advise simple, direct
instructions, avoiding "think step by step", clear delimiters (Markdown or XML), trying zero
examples before few, and explicit limits on what is wanted. The docs also say reasoning models
avoid Markdown formatting by default and that the string `Formatting re-enabled` on the first
line of the developer message turns it back on [docs; possibly dated].

**Gemini 3.x.** Google's docs: give precise, direct instructions (tricks built for older
models can push it to over-analyze); set thinking depth with `thinking_level`, not
`thinking_budget`: the latter is legacy and not recommended on Gemini 3.x, deprecated on
Gemini 4, and cannot be combined with `thinking_level` in one request (it returns a 400)
[Google's docs through Context7, 2026-10-31]; put the essential constraints, the role and the output
format in the System Instruction or at the very start of the prompt; put **long material first
and the question last**, with a bridging phrase such as "Based on the information above…"; and
say so explicitly if you want a detailed reply, since the default is terse.

**Open models.** The criterion is whether thinking is built in. My measurements on
`google/gemma-4-12b-qat` in LM Studio (October 2026): thinking is built in by default and is
turned off with `"reasoning_effort": "none"` in a `/v1/chat/completions` request (reasoning
tokens fell from 402 to 0). `chat_template_kwargs` with `enable_thinking: false` and
`"reasoning": "off"` changed nothing [measured]. With thinking off, manual
`<reasoning>`/`<answer>` still works. On an empty input: with thinking on, the model spun for
about 4,000 tokens without answering; with thinking off it replied with an ordinary sentence
outside the format. A line for "empty" fixed both; a catch-all line fixed symbols, random
characters and a greeting but not the truly empty input [measured: one model].

## What was not verified

- The skill has not been run hosted inside ChatGPT. Gemini it has: hosted in its app
  (2026-10-31, the four test sentences, one or two rounds per model) [measured]. Argon and
  Spark kept all its rules; 3.5 Flash Lite wrote the reasoning out in both rounds (a tag, then a
  JSON field), did a real prefill once and a softened text constraint once, and switched the
  prompt's language; 3.8 Flash sometimes did not answer, or answered with no
  trace of the skill. All of them stopped after the questions on the vague sentence, as they
  should. So expect the rules to weaken on smaller models.
- No produced prompt was run on OpenAI or Gemini models. What is said above about them comes
  from their docs, not from measurement.
- **Gemini 4:** the Google docs I reviewed for guidance cover 3.x; of the Gemini 4 docs I
  reviewed only the thinking parameter (`thinking_level`; `thinking_budget` is deprecated).
  Antigravity reported in my test (2026-10-30)
  that its model is "Gemini 4 Argon (Medium)". If the target or host is Gemini 4, treat the 3.x
  notes as a hypothesis and tell the user you did not review the Gemini 4 docs [inference].
- Chat-app features (custom instructions, GPTs, Gems, Projects) and their length limits were
  not checked.
- Llama itself was not run.
