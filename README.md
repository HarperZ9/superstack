<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/HarperZ9/superstack/main/docs/art/hero-dark.svg">
  <img src="https://raw.githubusercontent.com/HarperZ9/superstack/main/docs/art/hero-light.svg" alt="superstack: One contract for renderers and sound engines, checked byte for byte. A chain of small linked squares, each holding a few ruled lines, winds inward to a bright core." width="100%">
</picture>

# superstack

One contract for renderers and sound engines, checked byte for byte.

```
git clone https://github.com/HarperZ9/superstack && cd superstack
```

[![version: 0.2.0](https://img.shields.io/badge/version-0.2.0-e6e1d6?style=flat-square&labelColor=1a1712)](https://github.com/HarperZ9/superstack/releases/latest)
[![CI](https://github.com/HarperZ9/superstack/actions/workflows/ci.yml/badge.svg)](https://github.com/HarperZ9/superstack/actions/workflows/ci.yml)
[![license](https://img.shields.io/badge/license-FSL--1.1--MIT-e6e1d6?style=flat-square&labelColor=1a1712)](https://github.com/HarperZ9/superstack/blob/main/LICENSE)

<!-- writing-profile: readme -->

One contract for renderers and sound engines, so a frame or a sound made by one engine can be checked against another, byte for byte.

You get one file for your language. It writes a canonical hash of any scene, draws the same random numbers from the same seed string, keeps time on one integer clock, and seals a receipt that says two things: whether your bytes equal the reference (MATCH or DRIFT), and whether they fall within tolerance (verified, refuted or unverifiable). Every receipt also lists what it does not prove.

Text: CC BY 4.0. Code: FSL-1.1-MIT.

From v0.2.0, code is licensed FSL-1.1-MIT. Earlier releases remain under MIT. Each release turns MIT two years after it ships. [docs/LICENSING.md](docs/LICENSING.md) explains what that means for engines that vendor a file.

## Run it now

```
git clone https://github.com/HarperZ9/superstack && cd superstack
python tests/run_vectors.py                 # Python 3.11+, standard library only
node tests/run_vectors.mjs                  # Node 20+
cmake -S tests -B build && cmake --build build --config Release
./build/run_vectors .                       # build\Release\run_vectors.exe . on Windows
```

Each prints a line like `python: 383/383 checks passed`.

Use it from code:

```python
import superstack as ss
scene = {"kind": "superstack.sound/1", "seed": "folded-light", "rate": 48000, "channels": 1, "duration_samples": 4}
pcm = ss.quantize_s16([0.0, 0.25, -0.25, 0.5])
receipt = ss.make_receipt(producer="my-synth", version="1.0.0", backend="python", scene=scene, content=pcm,
                          media={"kind": "audio", "content": "music", "rate": 48000, "channels": 1, "format": "s16le",
                                 "frames": 4, "duration_flicks": 4 * ss.flicks_per_sample(48000),
                                 "access": {"autoplay": False, "captions": None, "transcript": False,
                                            "reduced_sound": "silent"}},
                          does_not_prove=["A PCM hash says nothing about how a device plays the sound."])
assert ss.verify_receipt(receipt) == []
```

```js
import * as ss from './superstack.mjs';
ss.canonical({ b: 1.0, a: 1e-7 });          // '{"a":0.0000001,"b":1}', the same bytes Python and C++ write
ss.rng('folded-light').nextFloat();         // the same stream in every language
```

## What the contract fixes

| Rule | In one line | Spec |
|---|---|---|
| Scene IR | `superstack.scene/1` for pixels, `superstack.sound/1` for sound, conventions spelled out | [2](SPEC.md#2-scene-and-frame-ir) |
| Canonical JSON v2 | Sorted keys, compact UTF-8, one number form that Python, JavaScript and C++ all write | [3](SPEC.md#3-canonical-json-v2) |
| Seed rule | A seed string through xmur3 into mulberry32, named sub-streams, a stateless pixel hash | [4](SPEC.md#4-seed-rule) |
| Clock | Integer flicks, 705,600,000 per second; common frame and sample rates land on whole numbers | [5](SPEC.md#5-flick-clock) |
| Receipt | Identity and tolerance as two verdicts, a required `does_not_prove`, a SHA-256 seal | [6](SPEC.md#6-receipt-superstackreceipt1) |
| Colour | OKLab and the liability risk tokens, one hot mark per view | [7](SPEC.md#7-colour) |
| Sound | s16le PCM receipts, a BS.1770 loudness meter, targets of -16 LUFS for speech and -14 for music, no autoplay, captions, a reduced-sound mode, narration backends that are never references | [8](SPEC.md#8-sound) |
| Exports | Canonical forms and delivery formats; byte-exact WAV, PPM and PGM writers | [9](SPEC.md#9-export-formats) |

## Evidence

The examples rebuild a proof from 3 October 2026: one scene, three pixel engines, two sound paths, one check. `python examples/run_all.py --ci` runs the parts that need no GPU or browser; CI runs it on Linux and Windows.

| Candidate, against its reference | Identity | Tolerance | Numbers |
|---|---|---|---|
| numpy port of raw-native, on Windows | MATCH | verified | 0 differing pixels |
| WebGL2 ray cast, RTX 4090 through ANGLE on Direct3D 11 | MATCH | verified | 0 differing pixels |
| WebGL2 ray cast, SwiftShader | DRIFT | verified | 9 of 65,536 pixels differ, at most 3 levels |
| Sound, Python and JavaScript float64 paths | MATCH | verified | 0 LSB |
| Sound, WebAudio `OfflineAudioContext` | DRIFT | verified | at most 1 LSB, 82.39 dB SNR |
| Control: AO radius 1.5 where the scene says 2 | DRIFT | refuted | AO RMSE 0.0379 |
| Control: one byte flipped | DRIFT | verified | 1 pixel, 1 level |
| Control: one voice one sample late | DRIFT | refuted | 15.95 dB SNR |

Observed on one Windows 11 workstation (Chromium 145, Python 3.12, Node 25). In CI on GitHub's Ubuntu 24.04 and Windows runners, the numpy port and both exact sound paths also gave the same hashes (`0282ef9c...` and `692ead20...`). It does not show equality across GPU vendors or drivers, and the pixel reference's own certificate refutes its screen-space AO against its ray-traced AO on this view. [examples/README.md](examples/README.md) has the commands and the limits.

Conformance: 383 vector checks pass in each of the three languages, with equal check counts. 59 paired mutations, one-line breaks to a single rule, are each caught by the vector file named for that rule.

## Use it in your engine

Copy one file, pin its SHA-256, run the vectors in your CI: [VENDORING.md](VENDORING.md).

## Files

| Path | What it is |
|---|---|
| `SPEC.md` | The contract |
| `superstack.py`, `superstack.mjs`, `superstack.hpp` | The three implementations |
| `SHA256SUMS` | The hashes engines pin |
| `vectors/` | Conformance vectors, pinned by `vectors/MANIFEST.json` |
| `tests/` | Vector runners, the C++ build, the paired mutations |
| `tools/make_vectors.py` | Regenerates the vectors; `--check` fails when they drift |
| `tools/check_vendored.py` | Checks a vendored copy against its pin |
| `examples/` | The pixel and sound proof, run against the contract files |
| `docs/LICENSING.md` | Licence and provenance of everything carried here |
