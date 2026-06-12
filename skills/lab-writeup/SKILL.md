---
name: lab-writeup
description: Turn a completed lab or project into documentation. Use when the user finishes a lab/project and asks to document it, create an Obsidian note, write a LinkedIn post about it, or summarize what was built and learned.
---

# Lab Write-up Generator

When the user finishes a lab or project and wants documentation, produce up to three artifacts (ask which ones are needed, default = Obsidian note):

## 1. Obsidian note (markdown, wiki-link friendly)
```
# <Lab Name>
**Date**: | **Topic**: | **Tools**: [[Tool1]], [[Tool2]]

## Goal
One sentence: what problem this lab solves.

## What I built (steps)
Numbered steps with the key commands/configs (short, copy-pasteable).

## Problems & fixes
Table: problem | root cause | fix. This is the most valuable section — never skip it.

## Key takeaways
3–5 bullets: what to remember.

## Links
Related notes via [[wiki-links]], official docs.
```

## 2. LinkedIn post (if requested)
- Hook in line 1 (a problem or surprising result, no "I'm excited to share").
- 3–5 short paragraphs: what, why, one concrete obstacle and how it was solved, one lesson.
- Plain language, no buzzword stuffing; 3–5 relevant hashtags at the end.
- Length: 600–1100 characters. Voice: practitioner sharing, not bragging.

## 3. CV bullet (if requested)
One line, action verb + technology + measurable outcome:
"Built X using Y, achieving/demonstrating Z."

## Rules
- Pull real details from the conversation/files; never invent results or numbers.
- Keep commands exactly as they were run (correct flags, real resource names sanitized if sensitive).
- Sanitize: remove subscription IDs, tenant names, IPs, and secrets before output intended for LinkedIn/public.
