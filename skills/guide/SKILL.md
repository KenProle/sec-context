---
name: guide
description: "Secure-coding checklist and BAD/GOOD patterns from the sec-context AI code security anti-patterns, loaded section by section. Load this skill first, before writing any code, whenever the task writes or changes code that: handles login, passwords, sessions, cookies, JWT or OAuth; builds SQL, NoSQL, LDAP or XPath queries or templates; runs shell commands or subprocesses; builds file paths, handles uploads, downloads or temp files; renders HTML or sets innerHTML; hashes, encrypts, signs or generates tokens or keys; reads or stores secrets and API keys; adds API endpoints, CORS, security headers, error handlers or debug/config settings; or adds a new dependency."
argument-hint: "[topic, e.g. auth, sql, upload]"
allowed-tools: Read, Grep, Glob
---

# sec-context guide

Before you write the code, load the parts of the sec-context breadth guide that the task touches, then write
the code so it follows their GOOD patterns and avoids their BAD ones.

Topic (may be empty): `$ARGUMENTS`

## Where the guide is

`${CLAUDE_PLUGIN_ROOT}/ANTI_PATTERNS_BREADTH.md`. If that path still shows a literal `${CLAUDE_PLUGIN_ROOT}`,
Glob for `~/.claude/plugins/cache/sec-context/sec-context/*/ANTI_PATTERNS_BREADTH.md` and use the folder with
the highest version (compare version folder names numerically); only if that finds nothing, Glob
`**/ANTI_PATTERNS_BREADTH.md` under `~/.claude/plugins`.

The file is large (~65K tokens): never read it whole. Grep it for `^#{2,3} ` with line numbers to get the
headings, then Read only the ranges you need (`offset` = heading line, `limit` = up to the next heading of
the same or higher level). Locate everything by heading, never by remembered line numbers.

## What to load

1. Always read `## Pre-Generation Security Checklist` (about 60 lines) and keep its relevant boxes in mind.
2. For each kind of code the task involves, read the matching `### N.M` subsections (not the whole `## N.`
   section unless most of its subsections apply):

| The task involves | Read |
|---|---|
| API keys, passwords, tokens, config with credentials, front-end code that calls APIs | §1 (1.1–1.4); key or token lifetimes and rotation §1.5 |
| SQL / ORM raw queries | §2.1; NoSQL §2.5 |
| Shell commands, subprocesses | §2.2 |
| LDAP, XPath, server-side templates | §2.3, §2.4, §2.6 |
| HTML output, templates, JSX, `innerHTML`, Markdown rendering | §3.1–3.3, §3.5; CSP §3.4 |
| Login, password rules, reset flows, MFA | §4.1, §4.2, §4.6, §4.7 |
| Session or token generation, cookies, JWT | §4.3–4.5 |
| Hashing, encryption, random values, key derivation | §5 (pick the matching 5.x) |
| Parsing or accepting outside input, regexes on input, deserialization | §6 (pick the matching 6.x) |
| Server/framework setup, error handling, CORS, headers, debug flags, admin routes | §7 (pick the matching 7.x) |
| Adding or upgrading a dependency, CI actions, base images | §8.1–8.6 (8.4 whether it is needed at all, 8.6 what it pulls in transitively) |
| New API endpoints or handlers | §9.1–9.4, §9.6; rate limits §9.5 |
| File paths, uploads, temp files, symlinks, permissions | §10 (pick the matching 10.x) |

The examples are pseudocode: apply the idea in the project's language and with its own established
libraries. For a new dependency, check that the package really exists under that exact name (§8.3) before
adding it.

For a deeper treatment of the six top patterns (secrets, injection, XSS, auth/sessions, crypto, input
validation), `${CLAUDE_PLUGIN_ROOT}/ANTI_PATTERNS_DEPTH.md` has a `# Pattern N:` deep-dive each; read only
its `## Edge Cases` or `## Common Mistakes` subsection when the task is squarely in that area and the
breadth subsection leaves a question open.

Keep this light: load what the task needs, write the code, and mention in one line which guide sections you
followed. To review finished changes, use `/sec-context:review` instead.
