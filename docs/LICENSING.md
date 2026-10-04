<!-- writing-profile: research -->
# Licensing and provenance

Checked 3 October 2026. Text: CC BY 4.0. Code: MIT.

Every claim below carries a label. **Verified** means the source was read on that date and the quote or record is cited. **Inferred** means a conclusion drawn from verified facts that no licence text states directly. None of this is legal advice.

## Result

1. **The two seed algorithms have clear provenance and may ship under MIT.** mulberry32 carries a CC0 1.0 public domain dedication from its first revision in 2017 (verified). xmur3's author releases his code to the public domain, with MIT as the stated fallback (verified). No replacement seed function is needed, and the site's seeded plates keep their streams.
2. **An MIT file can be vendored into every consumer**: the site (code reserved), the AGPL-3.0 repos, the FSL-1.1 repos and BuildLang Fair-Source. The FSF lists the Expat (MIT) licence as GPL-compatible (verified). For the other three the conclusion is inferred: none of their licence texts restricts what third-party permissive files a repository may contain, and the MIT file keeps its own terms inside them.
3. **One formula has an unknown first author.** `raw-pixel-hash/1` is raw-native's code, but the same four operations appear in many public snippets, and nobody has been traced as their origin (unverified). Section 2.4 explains why this is low risk.

## 1. This repository

| Part | Licence | Basis |
|---|---|---|
| `superstack.py`, `superstack.mjs`, `superstack.hpp` | MIT; each file carries the full notice | The author's grant, `LICENSE` |
| `vectors/`, `tests/`, `tools/`, code and data under `examples/` | MIT | Same |
| Prose: `SPEC.md`, `README.md`, `VENDORING.md`, `docs/`, `examples/README.md` | CC BY 4.0 | `LICENSE-TEXT` |

Each implementation carries the whole MIT notice and a list of the algorithms by others that it contains, so a vendored copy is self-contained.

## 2. Provenance of what the contract carries

### 2.1 mulberry32 (verified)

- Author: Tommy Ettinger. Source: the gist "Mulberry32 PRNG", https://gist.github.com/tommyettinger/46a874533244883189143505d203312c, created 2017-11-04 (GitHub API, `created_at`).
- The file header reads: "Written in 2017 by Tommy Ettinger ... To the extent possible under law, the author has dedicated all copyright and related and neighboring rights to this software to the public domain worldwide," with a link to CC0 1.0. The same header is present in the first of the gist's two revisions (2017-11-04 and 2017-11-06, read through the GitHub API).
- The 32-bit JavaScript form the site and this contract use follows bryc's port in the same `PRNGs.md` file as xmur3 (section 2.2), which is also public domain.
- A note on quality, not licence: in a comment of 2022-11-10 on the gist, the author writes that mulberry32 cannot produce about a third of all 32-bit outputs and recommends a SplitMix-style generator. v0 keeps mulberry32 because the site's seeded plates depend on its exact stream; a later rule id can change the generator without breaking old artefacts (SPEC.md section 4.4).

### 2.2 xmur3 (verified)

- Author: bryc. Source: `jshash/PRNGs.md` in https://github.com/bryc/code.
- xmur3 first appears in that file at commit `89aaa1d448` (2020-02-08). The file has opened with "License: Public domain" since commit `c67b4736d7` (2019-12-05), so xmur3 entered under that statement.
- The repository's `LICENSE.md` (created 2024-04-12, last changed 2024-04-13 at `1c370c1240`) reads: "Public domain." It continues: "If for whatever reason you cannot use works in the public domain, as a fallback license, I approve the use of MIT", followed by the MIT text under "Copyright (c) 2024 bryc".
- xmur3 is built on MurmurHash3's mixing constants. MurmurHash3's source states: "MurmurHash3 was written by Austin Appleby, and is placed in the public domain. The author hereby disclaims copyright to this source code." (`src/MurmurHash3.cpp` in aappleby/smhasher, verified).
- The same function was also posted on Stack Overflow, where posts carry CC BY-SA (inferred; that post was not read). This contract relies on the repository's grant, which allows MIT use, and not on the Stack Overflow copy.
- Use in this contract: the first output of xmur3's return function, computed over UTF-16 code units exactly as bryc's JavaScript does. Each implementation file credits bryc and repeats his fallback copyright line, which covers jurisdictions without a public domain.

### 2.3 OKLab (verified)

- Author: Bjorn Ottosson, "A perceptual color space for image processing", https://bottosson.github.io/posts/oklab/ (2020).
- The post states: "The code is available in public domain, feel free to use it any way you please. It is also available under an MIT licensee if you for some reason can't or don't want to use public domain software." The contract uses the post's matrix constants, which are the same numbers the site's `sense-core` module carries.

### 2.4 raw-pixel-hash/1 (provenance partly unknown)

- The formula is raw-native's CPU AO hash, written by this repository's author. raw-native is FSL-1.1-MIT from 0.3.0, read from its `LICENSE` on GitHub and in the 0.4.0 tree (verified). The author can also offer their own code under MIT here (inferred: a sole copyright holder may license the same work on several terms).
- Three of its four constants are xxHash32 primes: 374761393 is `XXH_PRIME32_5`, 668265263 is `XXH_PRIME32_4` and 2246822519 is `XXH_PRIME32_2` (verified against `xxhash.h` in Cyan4973/xxHash). The fourth, 1274126177, is not.
- The same expression appears in many public snippets for seeding from coordinates. Nobody has been traced as its first author (unverified).
- Risk (inferred, low): the formula is four arithmetic operations on published constants, the kind of short functional expression that copyright generally does not protect, and it already ships in raw-native and on the site. If a first author turns up with terms that conflict with MIT, a new rule id (`raw-pixel-hash/2`) can replace it without breaking old artefacts.

### 2.5 BS.1770 K-weighting (verified)

- The 48 kHz coefficients are the table in ITU-R BS.1770-4, used as printed.
- For other rates the filters are derived with the constants in libebur128 (https://github.com/jiixyj/libebur128, MIT): f0 1681.974450955533, G 3.999843853973347, Q 0.7071752369554196, Vb exponent 0.4996667741545416, and f0 38.13547087602444, Q 0.5003270373238773. Each was read in `ebur128/ebur128.c`. At 48 kHz the derivation reproduces the standard's table to within 1e-15, which the contract checks.
- These are numeric constants of a published standard and a published derivation (inferred: facts, not expression). The implementation files credit libebur128.

### 2.6 Risk tokens, WCAG contrast, sRGB transfer

- The token values, grounds and verdict words come from the site's `system/media-engine/colour.mjs` at `origin/main` 13618f0 (verified read). The site's code is all rights reserved and its author is this repository's author, who asked for the tokens to be part of the contract (inferred authority to publish them here under MIT).
- The WCAG 2.x contrast formula and the IEC 61966-2-1 sRGB transfer function are published standards; the files implement them from their published formulas.

### 2.7 Examples and recorded reference data

- `examples/` is ported from the author's private proof of 3 October 2026. The proof also ran the site's own media engine; that code is reserved and is **not** included. `examples/pixels/gl2.mjs` was written new for this repository and stands in for it.
- `examples/reference/raw-native-0.4.0/` holds four files written by raw-native 0.4.0 (FSL-1.1-MIT) on the author's workstation: `frame.ppm`, `mask.pgm`, `ao_rt.pfm` and `certificate.json`. They are program output, not raw-native's source. The author publishes them here under MIT (inferred authority, as in 2.4). `certificate.json` was checked for local paths and holds none.

## 3. Vendoring into each consumer

The question: may a project copy one MIT file into its tree and ship it under the project's own licence?

| Consumer | Its licence (verified from the file named) | Vendoring an MIT file | Obligation |
|---|---|---|---|
| Site (`HarperZ9.github.io`) | Code all rights reserved; written works CC BY 4.0 (`LICENSE` at `origin/main` 13618f0) | Allowed. The MIT grant permits use, copy, modification, sublicensing and sale, in any project, on the condition that the notice stays (verified, MIT text) | Keep the file's header |
| studio-engine | AGPL-3.0 (`LICENSE`) | Allowed. The FSF lists Expat (MIT) as "a lax, permissive non-copyleft free software license, compatible with the GNU GPL" (verified, gnu.org licence list). AGPL-3.0 shares GPL-3.0's terms apart from its network clause, so the same holds (inferred) | Keep the header; the combined work is distributed under AGPL |
| studio-libs | Each package is `AGPL-3.0-or-later` (`package.json` of render-nd, sense-core, render-sound, studio-perception-mcp, viable-viz); the workspace root says it is not one licensed package | Same as studio-engine | Same |
| brender-archival | AGPL-3.0 (`LICENSE`) | Same as studio-engine | Same |
| reconcile | AGPL-3.0 (`LICENSE`) | Same as studio-engine | Same |
| raw-native | FSL-1.1-MIT (`LICENSE` on GitHub and in the 0.4.0 tree) | Allowed (inferred). FSL's grant and its Competing Use limit apply to the licensor's Software; nothing in it forbids including third-party permissive files, and an MIT file inside keeps its own terms | Keep the header |
| telos | FSL-1.1-ALv2 (`LICENSE`) | Same as raw-native (inferred) | Same |
| BuildLang | BuildLang Fair-Source 1.0 (`LICENSE`) | Allowed (inferred). Its grant covers "the BuildLang compiler and toolchain"; section 5 requires keeping notices and nothing restricts included third-party files | Keep the header |

## 4. Before the repository goes public

| Check | State |
|---|---|
| mulberry32 provenance | Verified, CC0 |
| xmur3 provenance | Verified, public domain with MIT fallback |
| MIT vendoring into each consumer | Verified for AGPL (FSF list); inferred for reserved, FSL and Fair Source |
| Reserved site code excluded | Verified: no site snapshot is in the tree; `gl2.mjs` is new |
| `raw-pixel-hash/1` first author | Unknown; inferred low risk (2.4) |
| Secrets, keys, local paths in the tree | Searched before each release: no keys, tokens or credential files, and no absolute local paths |
