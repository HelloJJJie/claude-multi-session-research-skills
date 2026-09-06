---
name: study-notes
description: Turn a self-study Q&A or course-screenshot session into organized Obsidian notes automatically. Renames and files any lecture/slide/course screenshot the user shares into an Obsidian vault, writes or updates one atomic markdown note per concept explained (frontmatter + [[wikilinks]] + a linking index/MOC note), and on request produces a review/quiz report summarizing what was covered so far. Use this whenever the user pastes or references a study/course screenshot and asks for an explanation, says things like "整理笔记" / "存到 obsidian" / "生成review报告" / "帮我复习" / "make notes for this" / "add this to my notes", or is clearly in a self-study or exam-prep session where turning the Q&A into durable notes would help — even if they don't use those exact words. Also trigger when the user explicitly runs /study-notes.
---

# Study Notes → Obsidian Workflow

The point of this skill: the user is studying something (a certification course, a class, a topic) partly through screenshots they show Claude and questions they ask. Every such exchange is throwaway unless it's captured. This skill makes capturing it the default, not something the user has to ask for each time.

Do this quietly, in parallel with actually answering the user's question well — the note-taking is a side effect, never a reason to shortchange the explanation itself.

## 0. Find or set up the vault + topic folder

Check whether this project already has a configured vault/topic. Look in this project's memory directory (`~/.claude/projects/<project-slug>/memory/`) for a memory file describing an Obsidian vault path for the current subject. If one exists, reuse it silently — don't re-ask.

If none exists yet (new project, or a new subject within a project that already has unrelated notes configured), ask the user once, briefly:
- Which Obsidian vault to use (a path), and what to name the topic folder inside it (e.g. "Google Cloud 学习", "西班牙语 DELE B1", etc.) — offer to create a new subfolder in an existing vault as the default suggestion.

Once decided, create `<vault>/<topic>/` and `<vault>/<topic>/attachments/` if they don't exist, and record the decision as a project memory file (frontmatter with `type: feedback`, body stating vault path, topic folder, and this note-taking convention) plus a line in `MEMORY.md` — so future sessions in this same project pick it up without asking again. Model the memory file on any existing one for this workflow if you find one.

Also create `<vault>/<topic>/概念解释/` up front (see step 3a below) — every topic folder gets this one folder from the start, even before there's anything to put in it.

## 0a. Folder structure within a topic (applies to every topic, not just this one)

The user has explicitly asked for this organization (2026-09-02), and it applies to every subject going forward, not just the one it was first requested for:

- **One single `概念解释/` folder** for general, vendor/subject-agnostic technical concepts explained along the way — things like "what is SSH", "what is NAT", "what is a database", "what is a control plane". These are reusable knowledge that would make sense even to someone not studying this specific subject. All such notes go in this one flat folder, regardless of which lesson prompted them.
- **Topic/domain subfolders** for the subject's actual curriculum content, one per logical domain (e.g. for a cloud certification: `IAM 与安全/`, `计算/`, `网络/`, `存储与数据库/`, `环境搭建与计费/`; for a different subject, use whatever domains that subject's own syllabus naturally breaks into). Create a new subfolder when a new domain of content starts, rather than cramming everything into one folder or over-fragmenting into a folder per note.
- **Folders can nest** (hierarchy) if a domain grows large enough to need it — don't force flatness once a subfolder itself accumulates enough notes to warrant sub-splitting.
- The **index/MOC note and the syllabus/outline tracker stay at the topic folder's root**, never inside a subfolder — they're navigational hubs referencing everything else, so they belong at the top level. `attachments/` also stays at the root (screenshots referenced from notes in any subfolder still resolve correctly — see note below).
- When creating a new note, decide up front: is this a generic concept (→ `概念解释/`) or subject-specific curriculum content (→ the matching domain subfolder, creating one if none fits yet)? Don't default everything into one flat pile once this structure exists.

**Why moving files around is safe:** Obsidian resolves `[[wikilinks]]` and `![[embeds]]` by filename, not full path, by default — so moving a note into a subfolder, or moving an image within `attachments/`, does not break any existing links elsewhere in the vault as long as filenames stay unique. Reorganize freely; there's no need to rewrite link text when relocating files.

## 1. Handle screenshots

Whenever the user shares or references a screenshot (a new file, or one already sitting in the project folder):

1. Read the image to understand its content.
2. Rename the original file in place to a short, descriptive name (2–6 words, no "Screenshot 2026-...", match the language the user is working in) — do this every time, the user has explicitly asked for this.
3. Copy it into `<vault>/<topic>/attachments/<descriptive-name>.png` (keep the extension; convert an odd format to png/jpg only if Obsidian wouldn't render it).
4. Later, embed it in the relevant concept note with `![[descriptive-name.png]]`, placed near the section it illustrates — not just dumped at the top.

Skip this step for screenshots that aren't course/study material (e.g. an unrelated bug screenshot in a coding session).

## 2. Answer the question

Explain things normally, at whatever depth the question calls for. This skill changes what happens *after* the explanation, not how you explain.

## 3. Write or update atomic notes

For every distinct concept covered in the explanation, create or update `<vault>/<topic>/<domain-subfolder-or-概念解释>/<Concept Name>.md` — see step 0a above for which folder a given note belongs in:

- Frontmatter: `title`, `tags`, the course/subject name, `created` (today's date), and `source` if a screenshot backs it.
- Body: **carry over the full depth of the chat explanation, not a condensed summary of it.** The user has explicitly said notes must stay detailed even though the concept was already explained in chat — the note is meant to stand alone later without them needing to remember or re-read the conversation. Keep the concrete examples, the comparison tables, the "how to choose" reasoning, the exam-relevant caveats — everything that made the chat explanation useful, reorganized with clear headers rather than trimmed down. Only actually cut something if it was truly redundant within the note itself (e.g. don't say the same sentence twice), never just to make the note shorter.
- Right after the frontmatter, **before the H1 title** — the very first thing in the note body, ahead of everything else — put a **"相关笔记" (or "Related notes")** block linking every other concept note this one relates to, as a vertical bullet list (one `[[wikilink]]` per line), not a single inline line separated by · or commas:
  ```
  **相关笔记:**
  - [[A]]
  - [[B]]
  - [[C]]
  ```
  Not under the title, not at the bottom, not inline: the user wants it as the literal first thing in the file, in list form.
- Cross-link related concepts with `[[wikilink]]` syntax **every time the keyword appears in the body**, not just on first mention — the user wants to be able to jump to the related note from wherever the term shows up (prose, table cells, bullet points), not hunt for the one linked instance. Exception: don't self-link a note's own subject to itself, and links inside headings are optional (fine either way).
- Embed any screenshot that illustrates this note (see step 1).
- If the note already exists, Read it first and extend/refine it rather than overwriting — treat existing content the user or a past session wrote as something to build on, not replace. If a later, richer explanation of the same concept comes up in chat, enrich the note to match — don't leave it thinner than what was actually discussed.

Keep one note per concept (atomic notes), not one giant note per session — this is what makes the `[[wikilink]]` graph in Obsidian useful. Atomic means "one topic per note," not "short note" — an atomic note on a rich topic can and should be long.

## 4. Keep the index (MOC) current

Maintain one `<Topic> 学习索引.md` at the root of the topic folder (never inside a subfolder — see step 0a):
- A bullet list linking every concept note that exists so far, with a one-line description each — group this list under headings that mirror the subfolder structure (概念解释 / each domain subfolder) so the index stays navigable as it grows, rather than one long flat list.
- A dated "学习记录" section — append one line per session summarizing what was covered that day (convert relative dates using today's actual date).

## 5. Generate a review report (on request)

When the user asks to review, self-test, or get a summary (e.g. "生成review报告", "帮我复习一下", "来几道自测题"), create `<vault>/<topic>/Review <date>.md`:

- A short "本次复习知识点" list linking to the relevant atomic notes (don't restate their full content).
- A compact self-quiz: 3–6 question/answer pairs covering concepts learned so far in this topic, pitched at a level useful for exam prep if this is certification study. Put the answer right after each question (Obsidian doesn't have native flashcard hide/reveal without a plugin) unless the user says they want a no-answer quiz to test themselves first.
- Link back to the index note.

Don't generate a review report unprompted every turn — only when asked, or when the user finishes a clear study session and might want one (use judgment; if unsure, ask once).

## Notes on scope

- This skill is subject-agnostic — it's already been used for a Google Cloud certification, but applies equally to any course/topic the user studies through screenshots and Q&A. Each subject gets its own topic folder (and its own memory file recording that folder), so unrelated subjects never mix notes.
- Every topic folder gets the `概念解释/` + domain-subfolder structure from step 0a, not just the first subject this was set up for — apply it by default to any new subject/vault, without the user having to ask again.
- If the recorded vault path no longer exists (moved, renamed, external drive unmounted), say so and ask where to put notes instead — don't silently create a new vault elsewhere.
