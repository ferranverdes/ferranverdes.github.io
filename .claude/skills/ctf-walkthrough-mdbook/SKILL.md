---
name: ctf-walkthrough-mdbook
description: Write a CTF challenge walkthrough from user-supplied notes as a chapter of the PwnLog_ mdBook repo, in formal impersonal English with all flags and secrets redacted.
---

# CTF walkthrough for the PwnLog_ mdBook

Use this skill when the user supplies the text of a capture-the-flag challenge (notes, prompts, commands, tool output, task descriptions) and wants the walkthrough added to the book.

## Repository

- The repository root is an mdBook project titled "PwnLog_". All paths below are relative to the repository root.
- Chapters live in `src/`. `src/SUMMARY.md` is the table of contents.
- Do not touch `book/` (generated and gitignored) or `.github/workflows/`.
- `book.toml` sets `smart-punctuation = false`, so that quotes and dashes in quoted prompts and output stay as typed, and `no-section-label = true`, so that the sidebar shows no chapter numbers. Leave both settings in place and do not change the rest of the file.

## Layout

- One file per challenge: `src/<PLATFORM>/<challenge_name>.md`. The platform folder uses the platform's short name, for example `THM` for TryHackMe. The file name is the challenge name in lowercase snake_case.
- `THM` (TryHackMe) and `OffSec` are the platform names defined so far. For another platform (for example Hack The Box), follow the same pattern and confirm the short name with the user first.
- In `src/SUMMARY.md` each platform is a part title and its challenges are entries beneath it. Create the part title if it does not exist yet, and add new challenges under the existing one:

```markdown
# Summary

# THM

- [Challenge Name](./THM/challenge_name.md)
```

- The challenges under each platform are listed in alphabetical order of their titles, ignoring case. mdBook never sorts entries: the order of `src/SUMMARY.md` is the order of the sidebar, so a new entry is inserted at its alphabetical position.
- A platform without a walkthrough yet is listed as a part title followed by a draft chapter, an mdBook entry without a file that works as a TODO list: `- [TODO]()`. The first walkthrough of that platform replaces the draft chapter. The `OffSec` section currently holds such a draft chapter.
- The scaffold placeholder `src/chapter_1.md` (content: `# Chapter 1`) is replaced by the first walkthrough. Remove it and its `SUMMARY.md` entry. If the file cannot be deleted directly, ask for the delete permission or move it to a `_to_delete/` folder and tell the user.

## Source material

- Work only from the text the user provides. Never connect to targets, labs or VPNs.
- Do not invent commands, output, ports, usernames, paths or findings. If something needed is missing or unclear, ask the user or mark the gap explicitly.

## Redaction

- Always redact flags, passwords, hashes and keys (and any other secret credential) everywhere: prose, prompts, commands and output.
- Use consistent placeholders, for example `THM{REDACTED}` for flags and `<REDACTED>` for other secrets. When the flag is a value such as a dollar amount, use `$REDACTED`.
- Do not add a separate closing line with the flag. The redacted value already appears redacted in the answer where it was obtained.

## Language and tone

- Always write in English, in a formal and impersonal register.
- No first person ("I", "we") and no second person ("you") in the walkthrough text. No personal sentences or anecdotes.
- Write as a brief, concise guide for a reader who is facing the challenge and looking for a solution: impersonal, but oriented to the reader and to the reasoning behind each step. Prefer constructions such as "A good first step is to ask the assistant about its tools" or "The target event needs to exist first" over direct commands.
- Be straightforward and focus only on what was done in the challenge, step by step.
- Do not add background or theory, metadata tables, root-cause analysis, mitigations or any other extra section unless the user asks for it. Admonitions (see Callouts) are allowed inside a step and do not count as extra sections.

## Structure

- The challenge title, then a line in italics with the date on which the walkthrough is added, in ISO format (for example `*2026-10-26*`, using the current date of the environment and never a guessed one), then one short paragraph with the scenario and objective.
- One `##` heading per action performed. Headings are short and technical and start with the action, for example `Enumerating the assistant's tools and rules`, `Creating the target event` or `Injecting the payload`. Each step has a short lead-in sentence, the prompt, the answer and, when useful, a one-line comment that explains the finding. That comment may be written as an admonition.
- The chapter ends with the last comment. There is no closing flag line.
- Follow any structure the user specifies and keep new walkthroughs consistent with the existing ones in the book.

## Prompts, answers and output

- Show every prompt, question or command and every answer or output exactly as given in the supplied text. Only redactions may alter the words.
- Label the input of each step **Prompt** and its output **Answer**. Each label is on its own line, followed by a blank line and then the blockquote.
- Prompts typed into a chat and the answers of the chat are placed in blockquotes. A fenced code block never renders Markdown, not even with the `markdown` language tag, which only adds syntax highlighting (GitHub behaves the same), and the answers contain Markdown formatting that has to be rendered.
- Inside such a blockquote:
  - End with a trailing backslash every line that must stay on its own line and is followed by another line of the same paragraph (for example `**Title:** ...\` followed by `**Day:** ...`).
  - Leave a blank quoted line before any `---` that follows text, so that it is not read as a heading underline.
  - Write headings such as `### Tools Available:` as bold lines instead (`**Tools Available:**`, with a trailing backslash when text follows on the next line). Real headings would become headings of the page and entries in the sidebar.
- Leave a blank line after a blockquote before the next paragraph.
- Commands and plain terminal output go in fenced code blocks with the `text` language tag (or the matching language).

## Callouts

- Admonitions, the GitHub-style alerts supported by mdBook (https://rust-lang.github.io/mdBook/format/markdown.html#admonitions), are allowed in walkthroughs. An admonition is a blockquote whose first line is the tag (`> [!IMPORTANT]`), followed by the text on the next quoted lines. The five tags are `NOTE`, `TIP`, `IMPORTANT`, `WARNING` and `CAUTION`.
- mdBook enables admonitions by default (`output.html.admonitions`). Never disable that option in `book.toml`.
- Use them sparingly, at most one or two per chapter.
- `[!IMPORTANT]` marks the key finding that the rest of the attack depends on and that is easy to skim past.
- `[!NOTE]` and `[!TIP]` are used only for context or a hint that would otherwise break the flow of a step.
- `[!WARNING]` and `[!CAUTION]` are reserved for steps with real risk, such as destructive commands, account lockouts or rate limits.
- A callout is separated from the previous blockquote by a plain paragraph or heading, so that the two blockquotes do not merge.
- No callout is placed on the flag or the final result: the value is redacted and already shown in the answer.

## Boundaries

- Never commit, stage or push. Leave all changes uncommitted in the working tree, since a push to `main` publishes the site.
- Building the book (`mdbook build`) or checking the rendered output is not required.
- Make only the changes the user asks for. A question is answered without changing any file, and a pasted example is a reference that is not applied unless the user says so.
- Finish with a short note listing the files created or changed.
