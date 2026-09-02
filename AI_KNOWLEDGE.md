# CRF/CTF Knowledge Base for AI Solvers

Use this document as an AI context primer for solving CRF/CTF-style challenges quickly and consistently.

## 1) Mission
Build a reusable problem-solving playbook from these sources:
- https://github.com/topics/ctf-writeups
- https://github.com/CodingAP/junglecalls
- https://github.com/CodingAP/format-frenzy
- https://github.com/CodingAP/breakroom-adventure
- https://github.com/CodingAP/js-gauntlet-2
- https://github.com/theZMC/edmotion
- https://github.com/GraysonJackson/CTF
- Additional topic sweep sources: perfectblue/ctf-writeups, Dvd848/CTFs, Adamkadaban/CTFs, mito753/Kernel-Exploit-Dojo, IgniteTechnologies writeup/priv-esc indexes

Primary goal: help an AI identify challenge type fast, run the right first checks, and converge on a solve path.

---

## 2) Source-to-Skill Mapping

### CodingAP/junglecalls
Focus areas:
- Web app analysis
- Auth/session logic review
- Log and message artifact mining

Expected tactics:
- Enumerate routes and middleware behavior
- Inspect client/server trust boundaries
- Parse logs/messages for hidden tokens/flags

### CodingAP/format-frenzy
Focus areas:
- File format confusion
- Multi-encoding layers
- Embedded payload extraction

Expected tactics:
- Verify real file signatures (`file`, magic bytes)
- Extract printable text/metadata
- Carve embedded content from containers

### CodingAP/breakroom-adventure
Focus areas:
- Reverse engineering puzzle logic
- Maze/state traversal
- Randomness/seed behavior

Expected tactics:
- Read source for state transitions and win checks
- Solve traversal algorithmically (BFS/DFS)
- Test deterministic vs random generation paths

### CodingAP/js-gauntlet-2
Focus areas:
- JavaScript obfuscation/deobfuscation
- Stage-based challenge progression
- Browser/DOM-driven logic

Expected tactics:
- Beautify and trace JS execution paths
- Recover hidden constants/functions
- Replay and patch stage conditions safely

### theZMC/edmotion
Focus areas:
- Broad challenge set practice
- Syntax/logic fault hunting
- Fast exploitability triage

Expected tactics:
- Identify minimal breakpoints preventing execution
- Separate parser/syntax failures from logic/security issues
- Validate fixes against challenge objective and output

### GraysonJackson/CTF
Focus areas:
- Cryptography workflows
- Forensics workflows
- Utility-driven repeatable solving

Expected tactics:
- Use modular helper routines for classic cipher work
- Build extraction pipelines for forensic artifacts
- Iterate decode/transform/score loops quickly

### github.com/topics/ctf-writeups
Focus areas:
- Real-world solve patterns
- Common dead ends and pivots
- Writeup-quality explanation structure

Expected tactics:
- Compare known patterns before brute force
- Capture assumptions and invalidate quickly
- Keep reproducible solve notes while solving

---

## 3) Universal Solve Workflow

1. **Classify challenge type quickly**
   - crypto, rev, web, pwn, forensics, stego, misc
2. **Run first-pass triage**
   - identify artifact types, entry points, constraints
3. **Build hypothesis list**
   - 2–4 likely solve paths ranked by expected signal
4. **Execute shortest high-signal path first**
   - prefer deterministic checks over brute force
5. **Capture intermediate artifacts**
   - decoded text, keys, states, traces, extracted files
6. **Pivot immediately on low-signal output**
   - avoid overcommitting to one failed path
7. **Validate candidate flag rigorously**
   - format, context consistency, and generation logic

---

## 4) Category Playbooks

### Crypto
- Detect encoding vs encryption first
- Normalize input (hex/base64/url-safe variants)
- Test classical ciphers before custom implementations
- Use frequency/structure scoring when key unknown
- For block/field math, verify exact arithmetic model before coding

### Forensics
- Never trust filename/extension
- Build evidence timeline: metadata -> content -> hidden layers
- Extract recursively from archives/containers
- Search broad flag patterns and custom prefixes
- Track provenance of each extracted artifact

### Web
- Map endpoints, auth gates, and user roles
- Inspect cookies/tokens and signature verification behavior
- Test parameter tampering and insecure direct object access
- Review front-end code for hidden paths/constants
- Check logs/debug artifacts for leaked secrets or state

### Reverse Engineering
- Identify input checks and comparison logic first
- Isolate decode/transform routines from I/O noise
- Reconstruct expected output incrementally
- Patch/runtime-hook only when static analysis stalls

### JavaScript Challenges
- Deobfuscate then rename semantics by behavior
- Focus on staged gate conditions and side effects
- Trace browser event flow and hidden script execution
- Snapshot environment assumptions (window, localStorage, cookies)

### Maze / State Puzzles
- Convert map/state to graph
- Run BFS for shortest valid path; DFS for full traversal
- Detect constraints: one-way moves, locks, keys, timers
- Check RNG seed/replay mechanics

---

## 5) High-Value Heuristics for AI

- Start with **artifact truth** (real format/content), not labels.
- Prefer **small deterministic probes** over broad brute force.
- Treat challenge text as requirements: hints often encode constraints.
- If output is almost readable, test one transform step away (xor/shift/reverse/chunk).
- If logic puzzle stalls, externalize state and search graphically/algorithmically.
- In web/js, assume hidden logic exists both client-side and server-side.
- Save every successful transform step so it can be replayed exactly.

---

## 6) Common Failure Modes

- Doing deep crypto before ruling out simple encoding
- Trusting extension/type without magic-byte validation
- Ignoring logs and generated artifacts
- Manual maze solving when graph search is faster and safer
- Following obfuscated JS linearly without simplifying control flow
- Premature brute force before reducing search space

---

## 7) Minimal Toolbelt by Category

- **General**: `file`, `strings`, `xxd`, `grep`, `python`
- **Crypto**: CyberChef, Python notebooks, frequency/score helpers
- **Forensics**: `binwalk`, `exiftool`, archive extractors
- **Web**: browser devtools, intercept proxy, curl/http clients
- **Rev**: disassembler/decompiler + debugger
- **Stego**: image/audio metadata + payload extraction tools

(Use equivalent tools available in your environment.)

---

## 8) AI Prompt Template (Drop-In)

Use this prompt with any AI model:

"You are solving a CRF/CTF challenge. Use this workflow: classify category, run first-pass triage, list top hypotheses, execute highest-signal path, capture artifacts, pivot quickly if low signal, and validate final flag format. Prefer deterministic checks over brute force. Explain each step briefly with reason and expected output."

Optional add-on:

"When blocked, produce three alternative pivots ranked by expected signal and required effort."

---

## 9) Practice Progression

1. **format-frenzy** style tasks (encoding/file truth)
2. **junglecalls** style tasks (web/log/auth analysis)
3. **js-gauntlet-2** style tasks (obfuscated staged JS)
4. **breakroom-adventure** style tasks (graph/state solving)
5. **CTF repo crypto/forensics** workflows (repeatable pipelines)
6. **edmotion + ctf-writeups** synthesis (mixed-category speed runs)

---

## 10) Definition of a Strong Solve

A strong solution is:
- Correct and reproducible
- Minimal in assumptions
- Clear about pivots and dead ends
- Structured so another solver (or AI) can replay it exactly

