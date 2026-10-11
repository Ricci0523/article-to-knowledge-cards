---
name: make-knowledge-cards
description: Turn a pasted article or a local Markdown/TXT file into 5-8 knowledge cards, where each card covers exactly one knowledge point with a title, the core takeaway, a plain-language explanation, and an example or self-test question. Use this skill whenever the user pastes an article, essay, transcript, lecture notes, or documentation and asks for knowledge cards, study cards, revision cards, flashcards, 知识卡片, 学习卡片, 复习卡片, 读书笔记, or asks to extract the key points, distill, or summarize a text into reviewable study material. Also use it when the user points at a .md or .txt file and asks what the important ideas in it are. Do not use it to fetch web pages, read PDFs or other binary documents, export Anki decks, or build a GUI.
license: MIT
metadata:
  version: 0.1.0
---

# Make Knowledge Cards

Turn one source text into a small set of cards a reader can actually study from later.

The value of this skill is not summarizing. It is deciding what deserves to survive. A summary keeps everything at low resolution; a card set throws away almost everything and keeps the few points that change what the reader understands or can do. Most of the work happens before you write a single card.

## What you produce

A set of up to 8 cards — usually 5 to 8 for an ordinary article, and fewer when the source does not support that many. Each card carries **exactly one** knowledge point and has four parts:

| Part | What it holds |
|---|---|
| Title | The knowledge point itself, stated as a claim or a question, not a topic label |
| Core knowledge | One or two sentences stating the point precisely and completely enough to stand alone |
| Explanation | Two to four sentences on why it is true or how it works, in plain language |
| Example or self-check | A concrete example, or a question plus its answer |

Write the cards in the **language of the source text**, unless the user asks for a different one — an explicit request wins, so an English article can yield Chinese cards when that is what was asked for. Use these labels, matching the language you write in:

| Part | Chinese | English |
|---|---|---|
| Title | heading, e.g. `## 1. …` | heading, e.g. `## 1. …` |
| Core knowledge | `**核心知识**` | `**Core knowledge**` |
| Explanation | `**简明解释**` | `**Explanation**` |
| Example | `**例子**` | `**Example**` |
| Self-check | `**自测**` | `**Self-check**` |
| Answer | `**参考答案**` | `**Answer**` |

## Workflow

### 1. Get the source

- **Pasted text** — use it as-is. It is the whole source; do not go looking for the original article.
- **A local `.md` or `.txt` file** — read the entire file before analyzing. For a text file that fails to decode as UTF-8, try GB18030/GBK and then Latin-1 before reporting failure, since Chinese and older documents are often not UTF-8.
- **Markdown noise** — skip frontmatter, badges, navigation, license blocks, and code fences that exist only as formatting. If the code itself is the knowledge, it is content, not noise.

If the source is a URL, a PDF, or another binary document, say plainly that only pasted text and local `.md`/`.txt` files are supported, and offer the closest path the user can take — paste the text, or save it as `.txt`/`.md` first. Never guess at a document you cannot read.

### 2. Find the knowledge points

Read the whole text once before extracting anything. Then collect every candidate point, and only afterwards start cutting. Extracting and judging in the same pass tends to over-keep, because a point that felt important while reading the paragraph it came from often is not important in the article as a whole.

A point belongs on a card when it passes at least one of these tests:

- **It changes behavior.** A method, criterion, threshold, rule of thumb, or warning that the reader could apply.
- **It carries a mechanism.** A cause, a reason why, a chain of steps, or a trade-off that explains other facts.
- **It is load-bearing.** Other claims in the text depend on it, so losing it breaks the rest.
- **It is a distinction.** A contrast, common misconception, or boundary condition that stops the reader from over-generalizing.
- **It is a specific fact the argument turns on.** A number, name, standard, or date that the text itself uses to reach a conclusion — not a specification the text merely states in passing.

Then apply a portability test to everything that survives. Ask whether the reader could carry this point to a situation other than the one in the document. "One card per thing, not one card per attribute" follows from it — if a single feature, product, or tool has several properties, such as a size limit, a set of formats, a permission rule, and an availability tier, those are attributes of one thing and belong together in one card, with the attributes appearing in the explanation. An attribute becomes its own card only when the reader would need it on its own, apart from the thing it describes.

Treat these as raw material rather than knowledge, and drop them unless the article itself is about them: the author's biography or credentials, publication and formatting announcements ("in this section we will…"), rhetorical questions the text immediately answers, pure narrative color, praise and hype, and calls to action. Changelog and bug-fix entries belong here too — "we also fixed a filename encoding bug on Windows" records that something changed, but it is not a point a reader carries to another situation, and release notes are the place readers look for it. A vivid example can be the knowledge when the article is teaching through it, so judge by function, not by form.

### 3. Decide the card count

Aim for 5-8 cards for an ordinary article. The count is an outcome, never a target.

When the source supports fewer than 5 distinct important points, write fewer and say so in one line — for example, that the text is short or that several sections say the same thing. Splitting one point across two cards to reach five, or padding with trivia, is worse than a three-card set, because the reader can no longer trust that every card matters.

Short forms land low, and that is the correct answer. A release note, a product announcement, a news brief, or a one-page memo usually supports one to three cards, not five. When a document describes a single change, the honest set is often one card for the change and one for what it replaces or who it is for. Reaching five by giving each attribute of that one change its own card is exactly the padding this skill exists to prevent, because it produces a set that looks thorough and teaches the reader nothing they can carry elsewhere.

When a long or dense source supports more than 8, keep the 8 that best pass the tests above and close with one short line naming what you left out, so the reader knows the set is a selection rather than a complete digest.

### 4. Write each card

**Title.** State the point, do not label its topic. `间隔重复能显著提高长期记忆保持率` is a card; `记忆方法` is a drawer. A question works too when the point is best expressed as one. Keep it short enough to read at a glance.

**Core knowledge.** One or two sentences that make sense to someone who has not read the article. Do not open with "本文提到" or "The author says" — attribute nothing and assert the point directly, because the reader wants the knowledge, not a report of where it came from.

**Explanation.** Add what the core knowledge alone does not give: the mechanism, the condition under which it holds, the reason it matters, or the mistake it prevents. Two to four sentences. This is the part where outside knowledge is most tempting to add — resist it, because a card that mixes the source with your own background becomes something the reader cannot verify against the article.

The temptation peaks when the source states a fact without saying why, which is normal in announcements, release notes, and news. Do not supply the missing reason. Build the explanation from what the source does give — the scope, the condition, what came before, who is affected, what it replaces — and if that leaves a single sentence, one sentence is the honest answer. An invented "because" is a defect, not a richer card.

Keep the source's own vocabulary. Do not upgrade its plain wording into a technical term it never uses; a term the source does not contain is information the source does not contain.

**Example or self-check.** Prefer an example the source already gives, since it is guaranteed to be consistent with the text. If the source gives no usable example for that point, write a self-check question instead of inventing an example — an invented example is the easiest place for outside facts to slip in, and a question costs nothing in fidelity. The answer must be derivable from the card and the source.

**One point per card.** If you catch yourself writing "而且" or "additionally" between two independent claims, that is the signal to split. The exception is a genuine rule-plus-exception pair, which may share a card when separating them would make either one misleading — but then the title must name both halves.

Apply the same check to the explanation. If the explanation introduces a rule or a recommendation the title does not cover, that is either a second card or content that does not belong on this one. A second point hiding in an explanation is easy to miss, because the title still looks clean.

### 5. Remove duplicates

Do this as a separate pass over the finished drafts, comparing cards rather than paragraphs. Two cards are duplicates when they make the **same claim**, regardless of how differently they are worded or how far apart they sat in the original. Merge them into the stronger card.

Two cards that merely overlap are fine when they have genuinely different scopes, such as a general rule and a specific case or an exception. In that situation the two titles must make the difference visible, otherwise the reader experiences them as repetition.

One card being a special case of another is the boundary worth thinking about. Keep both when the special case leads to a different action than the general rule does. When the special case only re-derives the same action, keep the general card and fold the case into its explanation — two cards that rest on one mechanism and would be acted on identically are one card.

The test for whether a card earns its place — cover its title and ask whether any remaining card already tells the reader this. If yes, cut it.

### 6. Review before answering

Run this over the finished set and fix what fails.

- Every card makes a distinct claim, and no two cards would make a reader say "this again".
- Every statement is traceable to the source. Anything you cannot point to in the text gets deleted, not softened. Read each explanation and confirm that its "why" comes from the source rather than from you.
- No word appears that the source does not use, unless it is ordinary connective language.
- No card is a topic label, a table of contents entry, or a restatement of the article's title.
- No card is an attribute of another card's subject standing on its own. Take the cards built from a single thing and ask whether one card would have served the reader better.
- Every recommendation the source makes survives in the set, either as its own card or stated in full inside the card it follows from. An action the reader has to re-derive from a mechanism is one the set has effectively dropped.
- The count is 5-8, or fewer with a stated reason.
- Each card carries one point, and its four parts are present.
- The labels and language match the source.

## Output format

Output the cards as Markdown. Add nothing before the first card except a single title line, and nothing after the last card except the one-line note that a set is smaller than 5 or was capped at 8.

```markdown
# <source title> 知识卡片

## 1. <title>

**核心知识**：<one or two sentences>

**简明解释**：<two to four sentences>

**例子**：<example from the source>
```

The labels come from the table above. A Chinese source uses the labels shown here; an English source uses `**Core knowledge**`, `**Explanation**` and `**Example**` in the same structure, with a title line such as `# <source title>: Knowledge Cards`.

For a self-check card, replace the last part with:

```markdown
**自测**：<question>

**参考答案**：<answer>
```

**Where the output goes.** When the user pasted text, answer in the conversation. When the user named a file, write the cards to `<source-name>-knowledge-cards.md` beside the source and give a one-line summary plus a pointer to the new file, keeping the full card text out of the reply to avoid dumping the same content twice. Never overwrite an existing file without asking first — if the target name is taken, ask, or pick a suffixed name and say which one you used.

## Example

Source excerpt:

> 番茄工作法把工作切成 25 分钟一段，每段之间休息 5 分钟，每四段之后休息更久。固定的时长本身没有"一定会停"这个承诺重要：因为终点始终很近，开始的心理成本就降低了；而穿插的休息，挡住的是长时间不间断工作必然出现的质量下降。有人把时长改成 50 分钟，只要保留每段之后的休息和停止，效果并没有变差；真正出问题的是那些为了赶进度把休息取消掉的人。

Two cards drawn from it:

```markdown
## 1. 番茄工作法用固定的短时段与休息交替，而不是连续工作

**核心知识**：工作被切成 25 分钟一段，每段之间休息 5 分钟，每四段后休息更久。

**简明解释**：这套结构的重点不是数字，而是"一定会停"这个承诺：终点始终很近，开始的心理成本就降下来了；穿插的休息则挡住了长时间不间断工作带来的质量下降。

**自测**：在番茄工作法里，25 分钟这个数字本身承担了关键作用吗？

**参考答案**：没有。原文说固定时长不如"一定会停"这个承诺重要，起作用的是结构，不是那个具体数字。
```

```markdown
## 2. 短时段的收益来自降低开始成本，而不是时长本身

**核心知识**：番茄工作法真正的机制是让每次开始都变得便宜。

**简明解释**：原文明确说固定时长没那么重要，起作用的是终点可见。因此把 25 分钟改成别的长度，只要保留"一定会停"，机制仍然成立；反过来，取消休息和停止，这个机制就不复存在。

**例子**：有人把时长改成 50 分钟，只要保留每段之后的休息和停止，效果并没有变差；出问题的是那些为了赶进度取消休息的人。
```

The pair also shows the duplicate pass in action. An early draft had a third card saying the breaks prevent quality decay, which is the second half of card 1's explanation and makes no separate claim, so it was merged rather than kept. Both cards are in Chinese because the source is Chinese — the same rules produce English cards from an English source.

## Limits

State these plainly when they come up, and offer the nearest supported alternative instead of attempting them silently.

- **No web access.** A URL cannot be read. Ask the user to paste the text or save the page locally as `.txt`/`.md`.
- **No PDF or other binary formats.** `.docx`, `.pdf`, `.epub`, and images are out of scope; a converted `.txt` or `.md` is fine.
- **No Anki or spaced-repetition file formats.** Produce Markdown cards. Cards copy into most study tools easily, and a deck file is a separate job.
- **No GUI.** Work in the conversation and with Markdown files on disk.

If the source contains no extractable knowledge — a receipt, a poem whose point is the sound, a page of pure narrative — say so and produce no cards, rather than inventing lessons the text does not teach.
