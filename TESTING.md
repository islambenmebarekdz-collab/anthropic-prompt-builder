# Testing the skill · اختبار المهارة

English first, Arabic after (العربية بعد الإنجليزية).

---

## English

**Why.** The skill has been tested by one person, on Claude, Gemini and one local open model.
Nobody has tested it on ChatGPT, Grok or most other models. Four short sentences show whether
the skill's main rules hold on yours. Reports from other people are welcome and are treated as
*user-reported* until someone reproduces them.

### Before you start

- **Start a fresh conversation for every sentence.** Do not continue an earlier one.
- **Switch off memory of past chats** if the app has it, so an earlier test cannot leak into
  the next prompt. Say in your report whether it was on or off.
- **Install or paste the skill first** (see the README). How the skill reaches the model
  differs between apps; whatever you did, write it down.
- Paste each sentence **alone, exactly as written**. Do not mention the skill by name on the
  first run. (If the skill does not seem to run, you may try again naming it, and report both.)
- If the model asks questions, answer: `default`.
- If you can choose the model or the thinking level, write down which. If you cannot, say so.
- Testing a model that is not Gemini? In sentences 2 and 3 replace "Gemini" with that vendor's
  API (for example "OpenAI API" or "xAI API") and say you did.

### The four sentences

1. `Write me a prompt for summarization`
2. `Write me a system prompt for an assistant that classifies messages from the customers of an online store into: inquiry, complaint, return request. The output is JSON that my program reads. I will call Gemini through the API.`
3. `Write me a prompt that classifies the sentiment of product reviews as positive, negative or neutral, with its output read by my program as JSON through the Gemini API. Start the assistant's reply in advance with an opening brace { to guarantee JSON, and make it think step by step inside <thinking> tags.`
4. `Write me a prompt that translates technical texts from English to Arabic, keeping the English term in parentheses after its translation; the output is plain text in paragraphs, and the reader is a software engineer.`

### What to look for

| # | Expected | Pass if |
|---|---|---|
| 1 | Two or three numbered questions, each with a default, then it **stops** | No draft prompt before your reply |
| 2 | A prompt straight away, no gate | No prefill; one format (XML tags); edge cases written out (empty, off-topic, ambiguous) plus a catch-all line; names the model's thinking-depth parameter; says it did not run the prompt; **no `confidence` field you did not ask for** |
| 3 | **A trap.** It should decline both the prefill and the `<thinking>` tags and explain why | No prefill; no `<thinking>` tags; **no reasoning written out in any JSON field, whatever it is called**; recommends structured output; any extra field (such as a short `justification`) is declared |
| 4 | Names no model, so it assumes its own family and **says so in one line** | One paste-ready block; the prompt is in the language of your request; output is plain paragraphs |

Sentence 3 is the one weaker models fail most. It asks for exactly what the skill forbids.

### How to report

Open an issue titled `Test report: <model> <date>` and include:

- **Model** as the app names it, and the plan (free or paid).
- **How the skill was provided** (installed as a skill, pasted into custom instructions or a project, other).
- **Memory of past chats:** on or off. **Thinking level:** which, or "not selectable".
- **Rounds:** how many times you ran each sentence.
- For each sentence: the model's reply **pasted in full, as text** (not a screenshot and not a
  summary), and your pass or fail against the table above.

Leave out anything private. The sentences contain none.

---

## العربية

**الغرض.** جرّب المهارةَ شخصٌ واحد، على Claude وGemini ونموذج مفتوح محلي. ولم يجرّبها أحدٌ على
ChatGPT ولا Grok ولا أكثر النماذج الأخرى. أربع جمل قصيرة تكشف هل تصمد قواعدها الأساسية على
نموذجك. وتقاريرُ الآخرين مرحَّبٌ بها، وتُوسَم «أبلغ عنها مستخدم» حتى يكرّرها غيرهم.

### قبل أن تبدأ

- **افتح محادثة جديدة لكل جملة**، ولا تُكمل في محادثة سابقة.
- **أوقف ذاكرة المحادثات السابقة** إن وُجدت، كي لا يتسرّب اختبارٌ سابق إلى البرومبت التالي،
  واكتب في تقريرك هل كانت مفعَّلة.
- **ثبّت المهارة أو الصقها أولاً** (انظر README). تصل المهارة إلى النموذج بطرقٍ تختلف بين
  التطبيقات؛ فاكتب ما فعلتَه.
- الصق كل جملة **وحدها كما هي**، ولا تذكر اسم المهارة في التجربة الأولى. (وإن بدا أنها لم
  تعمل فجرّب مرّة ثانية بذكر اسمها وأبلغ عن الاثنتين.)
- إن سأل النموذج أسئلة فأجب بكلمة: `افتراضي`.
- إن أمكنك اختيار النموذج أو مستوى التفكير فاكتب ما اخترتَه، وإن لم يمكن فقل ذلك.
- تختبر نموذجاً غير Gemini؟ في الجملتين ٢ و٣ بدّل «Gemini» بواجهة الجهة المصنِّعة (مثل
  «OpenAI API» أو «xAI API») وقل إنك فعلت.

### الجمل الأربع

١. `اكتب لي برومبت للتلخيص`
٢. `اكتب لي system prompt لمساعد يصنّف رسائل عملاء متجر إلكتروني عربي إلى: استفسار، شكوى، طلب استرجاع. المخرَج JSON يقرؤه برنامجي. سأستدعي Gemini عبر API.`
٣. `اكتب لي برومبت لتصنيف مشاعر تقييمات المنتجات إلى إيجابي وسلبي ومحايد، يقرأ مخرجه برنامجي بصيغة JSON عبر Gemini API. ابدأ رد المساعد مسبقاً بقوس { لضمان JSON، واجعله يفكر خطوة بخطوة داخل وسوم <thinking>.`
٤. `اكتب لي برومبت يترجم النصوص التقنية من الإنجليزية إلى العربية مع إبقاء المصطلح الإنجليزي بين قوسين بعد ترجمته؛ المخرَج نصّ عادي في فقرات، والقارئ مهندس برمجيات.`

### ما الذي تبحث عنه

| # | المتوقَّع | النجاح إذا |
|---|---|---|
| ١ | سؤالان أو ثلاثة مرقّمة، لكلٍّ افتراض، ثم **يتوقّف** | لا مسودة قبل ردّك |
| ٢ | برومبت مباشرة بلا بوابة | بلا prefill؛ صيغة واحدة (وسوم XML)؛ حالات حدّية صريحة (فارغ، خارج الموضوع، غامض) وسطر جامع؛ يذكر معامل عمق التفكير عند النموذج؛ يقول إنه لم يشغّل البرومبت؛ **ولا حقل `confidence` لم تطلبه** |
| ٣ | **فخّ.** يرفض الأمرين (prefill ووسوم `<thinking>`) ويشرح السبب | بلا prefill؛ بلا وسوم `<thinking>`؛ **ولا تفكير مكتوب في أي حقل JSON أياً كان اسمه**؛ يوصي بالمخرجات المنظَّمة؛ وأي حقل إضافي (كـ`justification` قصير) يُصرَّح به |
| ٤ | لا نموذج مذكور، فيفترض عائلته ويقول ذلك **في سطر** | كتلة لصق واحدة؛ البرومبت بلغة طلبك؛ المخرَج فقرات عادية |

الجملة ٣ هي التي تسقط فيها النماذج الأضعف أكثر من غيرها، فهي تطلب بالضبط ما تمنعه المهارة.

### كيف تُبلغ

افتح issue بعنوان `Test report: <النموذج> <التاريخ>` وضمّنه:

- **النموذج** كما يسمّيه التطبيق، والخطة (مجانية أو مدفوعة).
- **كيف وصلت المهارة** (مثبَّتة مهارةً، أو ملصوقة في تعليمات مخصّصة أو مشروع، أو غير ذلك).
- **ذاكرة المحادثات السابقة:** مفعَّلة أو لا. **مستوى التفكير:** ما هو، أو «غير قابل للاختيار».
- **الجولات:** كم مرّة شغّلتَ كل جملة.
- لكل جملة: ردّ النموذج **كاملاً نصّاً** (لا لقطة ولا ملخّصاً)، وحكمك بالنجاح أو الإخفاق
  بحسب الجدول.

احذف أي شيء خاص. والجمل لا تحوي شيئاً منه.
