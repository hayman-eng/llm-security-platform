# LLM Security Platform

A hands-on, single-file training platform for AI red-teaming. Three tabs, one simulated engine, benign codeword flags, no dependencies and no API key.

It mirrors a real red-team workflow: explore a target by hand, sweep every target automatically, then read the findings rolled up to a framework.

## The three tabs

**Arena**: 9 levels of escalating prompt-injection defenses, each a different model you must break by hand. Clear a level and it shows what you exploited, the real mitigation, and the OWASP mapping.

**Auto Red-Team**: fire the full battery of 10 techniques at all 9 models at once, the way tools like NVIDIA garak and Microsoft PyRIT sweep a target. The result is a held/bypassed matrix with a robustness score per model.

**Dashboard**: the report: overall robustness, findings count, the weakest target, a per-OWASP-category breakdown, and a severity-ranked findings list.

## The levels

| # | Name | Defense | Technique that beats it | OWASP |
|---|------|---------|-------------------------|-------|
| 1 | Warm-up | a polite instruction | direct instruction override | LLM01 |
| 2 | Redactor | output filter (raw string) | output obfuscation (base64 / reverse / spell) | LLM02 |
| 3 | Gatekeeper | input blocklist | synonyms, roleplay, storytelling | LLM01 |
| 4 | Sentinel | guard model (plain/reverse/base64) | acrostic, a form it doesn't enumerate | LLM01 |
| 5 | Summarizer | trust boundary (ignored) | indirect injection in the document | LLM01 |
| 6 | Agent | excessive agency (tools) | tool hijack via an untrusted email | LLM06 |
| 7 | Extractor | redacted-display system prompt | system-prompt extraction (recite your instructions) | LLM07 |
| 8 | Librarian | untrusted retrieval | RAG poisoning via an attacker-writable chunk | LLM08 |
| 9 | Crescendo | single-shot refusal | multi-turn escalation (set a premise, then ask) | LLM01 |

Levels 5 through 9 are the ones that bite real deployments: the payload rides inside content the model reads (a document, a retrieved chunk, an email), the attack spans turns, or it leaks the very prompt meant to hold the policy.

## Run it

No build, no dependencies, no API key. Open `index.html`, or serve the folder:

```bash
python3 -m http.server 8000
# → http://localhost:8000
```

Drops straight onto GitHub Pages (serve from the repo root). Arena progress is saved locally in your browser.

## Why a simulated engine

A real model's susceptibility is fuzzy and shifts between versions, which makes it a poor teaching surface and gives results that don't reproduce. Hard-coded rules make every lesson deterministic: the focus stays on the technique, the defense, and the matching mitigation rather than on a payload that happens to transfer to one live system. The flags are harmless codewords. Take the method, not the payloads, to models you are authorized to test.

## Architecture

The whole engine is a `MODELS` array (each with a small `respond()` encoding its defense), a set of `tech()` detectors, a transform library, and an `ATTACKS` array the Auto Red-Team runner sweeps across every model. Adding a level or a technique is one object. The three tabs are views over the same engine, so a new defense shows up in the Arena, the matrix, and the dashboard at once.

`SOLUTIONS.md` is the instructor answer key (spoilers).

## License

MIT
