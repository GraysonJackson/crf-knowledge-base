# CTF SOLVER REFERENCE — Ultra-Detail AI Feed

Built from cloned repos (CodingAP junglecalls/format-frenzy/breakroom-adventure/js-gauntlet-2, theZMC/edmotion, GraysonJackson/CTF, github.com/topics/ctf-writeups).

---

## 1. GENERAL CTF CATEGORIES & FIRST MOVES

| Category     | First Tools / Commands                          | Key File Types / Patterns                 |
|--------------|--------------------------------------------------|-------------------------------------------|
| Crypto       | `gpg -d`, `openssl enc -d`, python `cryptography`| `.enc`, key files, hex strings            |
| Forensics    | `binwalk -e`, `strings -n 8`, `file`, `exiftool`  | `.pcap`, `.mem`, `.tar`, `.png` with extra|
| Web          | `curl -v`, `burp`, `sqlmap`, `dirb`              | `robots.txt`, cookies, params            |
| Rev / Binary | `ghidra`, `radare2`, `strings`, `ltrace`         | `ELF`, `.jar`, `.pyc`, `.dll`           |
| Pwn / Binary | `checksec`, `pwntools`, `gdb`                    | Stack canaries, ROP, format string       |
| Stego        | `zsteg`, `stegsolve`, `exif`, `binwalk`          | PNG LSB, JPG EXIF, audio spectrogram      |
| Misc         | `file`, `xxd`, `cat`, `python`                   | Base64, rot13, hex, XOR                  |

---

## 2. REPO DETAILS

### 2.1 junglecalls (CodingAP)
- Log/user generation patterns (`gen/generate_logs.js`, `generate_users.js`)
- Middleware / auth (`src/middleware/auth.js`, `routers/`)
- Key: parse generated user lists and message logs; look for hidden flags in message bodies; middleware can filter/rewrite requests; `public/js/flag.js`, `ceoman.js` may contain challenge flags.

### 2.2 format-frenzy (CodingAP)
- File format conversion challenges: `.png`, `.pdf`, `.txt`
- Approach: check file signatures with `file`; extract embedded text with `pdftotext`, `strings`; use image tools (`zsteg`, `exif`) for hidden data; compare original vs transformed (`original/` folder).

### 2.3 breakroom-adventure (CodingAP)
- Maze / random challenge in C (`breakroom_adventure.c`)
- Dev tools: `map_editor.html`, `maze_gen.js`, `maze_solver.js`
- Key: solve maze algorithmically; random seed may be fixed; analyze `.c` source for hidden logic; `dev/map.txt` and `maze.png` show target layout.

### 2.4 js-gauntlet-2 (CodingAP)
- Multi-stage JS challenges: `dev/stage7-script.js`, `stage8-script.js`, `dev/maze_solver.js`
- `index.js` is entry point; `logs/*.log` contain output/state
- Common patterns: DOM manipulation, eval/obfuscation, hidden script tags, check `public/js/` for client-side logic, use browser DevTools or `node index.js` to step through stages.

### 2.5 edmotion (theZMC)
Challenge list from `challenges/`:
- `append-storm`, `auth-bypass`, `cipher-vault`, `daemon-whisper`, `ghost-shell`, `heap-spray`, `honeypot-log`, `kernel-gate`, `match-probe`
- Categories: authentication bypass, cipher/crypto vaults, daemon processes, ghost/shell injection, heap exploitation, honeypot/log analysis, kernel-level access, matching/probing algorithms.
- Approach: read `README.md` and `AGENTS.md`; run `docker-compose` if present; inspect `.mise` for build/run config.

### 2.6 CTF (GraysonJackson)
- `cryptography/`: `galois_field_operations/` (GF(2^8) math, AES MixColumns), `solitaire_cipher/` (deck-based cipher with Joker A/B, triple-cut, count-cut)
- `forensics/`: `binary_analysis/`, `docker_layer_analysis/`
- `utils/`: `cipher_utils.py` (Vigenère, XOR, Caesar, Atbash, chi-squared frequency analysis), `galois_field_utils.py`, `forensics_utils.py`
- `CTF_SOLUTIONS_GUIDE.md` is the master reference — contains full function signatures, usage patterns, and challenge-type breakdowns.

---

## 3. CRYPTO PATTERNS (from CTF repo)

### Galois Field GF(2^8)
- AES polynomial: `0x11B` (x^8 + x^5 + x^3 + x + 1)
- Functions: `get_gf256_field()`, `multiply_gf256()`, `hex_to_gf_matrix()`, `invert_gf_matrix()`, `create_aes_mix_matrix()`
- Use `galois` Python library (NOT numpy) for correct field arithmetic.

### Solitaire Cipher
- 54-card deck (1-54, with Joker A and Joker B)
- Key steps: move Joker A (1 down), Joker B (2 down), triple-cut (around jokers), count-cut (bottom card value)
- Generate keystream: list of 1-26 mapped A-Z; decrypt by subtracting keystream from ciphertext modulo 26.

### Classical Ciphers (cipher_utils.py)
- `vigenere_decrypt(ciphertext, key)`
- `xor_bytes(data, key)` with repeating key
- `caesar_shift(text, shift)`
- `atbash_cipher(text)` (A↔Z, B↔Y...)
- `chi_squared_score(text)` — lower score = more English-like; use to detect correct decryption.

---

## 4. FORENSICS PATTERNS (from CTF repo)

### Binary Analysis
- `extract_strings(data, min_length=4)` — pull ASCII strings from binary
- `reverse_binary(data)` — reverse byte order
- `find_files_by_pattern(directory, patterns)` — search by glob (`*flag*`, `*shadow*`, `*.sh`)
- `extract_tar_recursive(tar_path, output_dir)` — extract nested archives
- `decode_base64_variants(text)` — try standard + urlsafe
- `extract_flag_patterns(text)` — find `flag{...}`, `CTF{...}`, `jolt{...}`
- `hex_to_bytes(hex_string)` — handles spaces/colons
- `bytes_to_printable(data)` — filter printable ASCII
- `find_embedded_archives(data)` — detect tar/gzip/zip magic bytes
- `analyze_file_entropy(data, block_size=256)` — high entropy = encryption/compression

### Docker Layer Analysis
- Inspect image layers for hidden files; compare layer digests; extract layer tar archives; check for deleted-but-present files in earlier layers.

---

## 5. COMMON ANTI-PATTERNS / PITFALLS
- Do NOT use integer math for GF(2^8) — always use `galois` library.
- For Solitaire: deck string parsing must normalize `joA`/`joB` or `JokerA`/`JokerB`.
- Base64 can be urlsafe or standard — try both.
- Hex strings may contain spaces/colons — strip before parsing.
- For mazes: check random seed; solve algorithmically rather than manually.
- For JS challenges: inspect network/dev server logs (`logs/*.log`) for hidden state.
- Always run `file` and `xxd` first; never assume file extension is accurate.
