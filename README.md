# IGCSE GP Exam Creator

A single educational skill for creating coordinated Cambridge IGCSE Global Perspectives (0457) practice exam sets. This repository contains the GP exam creator only, with Claude and OpenAI plugin manifests around the same seven skill and reference files.

## Current status

Initial public distribution package, version 0.1.0. The underlying skill is v30. Marketplace submission is pending; a repository or ZIP is not evidence of marketplace acceptance.

**Claude is currently the more accurate version and probably the better choice, especially for document formatting and presentation.** The OpenAI edition is an early migration. Equivalent behaviour and presentation across platforms have not been established. The original Claude skill instructions and reference templates are preserved byte for byte.

## What it creates

1. Source booklet
2. Question paper
3. Mark scheme
4. Exemplar answers
5. Annotated sources
6. Student writing support

The workflow uses four sources, readability checks, a 70-mark question structure, coordinated answers and specified document formatting. File generation requires the host application's document tools and suitable dependencies, including the `docx` JavaScript library for the provided templates. PDF export additionally requires a conversion tool. Availability varies by host.

## Use

Example prompt: “Create an IGCSE Global Perspectives practice exam on data centre water usage.”

Claude Code can load this folder with `claude --plugin-dir /path/to/igcse-gp-exam-creator`. OpenAI hosts use `.codex-plugin/plugin.json` and the `skills/` directory. Standalone skill import uses the folder `skills/igcse-gp-exam-creator-v30`, containing `SKILL.md` and `references/`. Follow the current host's installation instructions; a skill ZIP is not interchangeable with a plugin ZIP in every interface.

## Validation and limitations

A data-centre water-usage workflow was previously completed in Codex, producing six DOCX files and six PDFs; all 36 PDF pages were visually reviewed. This was a workflow check, not a controlled platform comparison. The public package excludes those generated documents and their metadata.

An inherited template inconsistency remains: Q1(d) specifies a maximum of two sentences, while an approved example uses three. Teachers should review outputs against the current syllabus and resolve this during review. The original instruction files have not been changed to hide that discrepancy.

These are independent practice materials, not official examination-board papers or an endorsed product. Check facts, source attribution, assessment criteria and layout before classroom use. Do not present invented practice sources as genuine published evidence.

## Privacy and support

The package contains no school-specific data, student records, credentials, telemetry, publisher backend, connectors or hooks. Your chosen AI host processes prompts and files under its own policies. See [privacy](PRIVACY.md), [terms](TERMS.md), and [support](SUPPORT.md).

License: [MIT](LICENSE).
