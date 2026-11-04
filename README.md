# World of Warships OCR + YOLO Tournament Discord Bot

WoWS battle-result screenshot recognition and tournament operations bot. This repository is the primary project home.

## Project status

- Package version: 2.5.0
- Runtime baseline: Node.js 22.16.0 (see `.nvmrc`); package engine requires >=22.16.0.
- The nine required battle fields remain exact-match and in this order: `result`, `player`, `map`, `mode`, `battleStart`, `credits`, `freeXp`, `damage`, `xp`. Missing, conflicting, invalid, uncertain or incorrect values must not be silently accepted.
- Tesseract.js remains the primary OCR engine. OpenCV is opt-in and supplies crop/geometry proposals only. Local PaddleOCR is disabled by default pending paired evaluation.
- OCR.space is implemented as an optional secondary crop recognizer. It receives localized field crops, not the original screenshot; its candidates still pass the strict existing parsers/consensus rules.
- OCR.space live verification is pending: this work session has no configured OCR.space key and the execution container cannot resolve `api.ocr.space`. Adapter offline tests passed 17/17; this does not establish live service availability or improved accuracy.
- The complete production release remains NO-GO until outstanding OCR acceptance, native Windows/Tesseract and Discord smoke-test gates are separately verified.

## Install and run

```sh
npm ci
cp .env.example .env
# Edit .env locally; never commit secrets.
npm test
npm start
```

Use Node.js 22.16.0 or newer. On Windows, use the supplied PowerShell setup and test scripts in `scripts/`.

## Key commands

| Command | Purpose |
|---|---|
| `npm test` | Offline automated test suite |
| `npm run test:ocrspace-live:dry` | Validate the local crop/dry-run path without a network call |
| `npm run test:ocrspace-live` | Explicit live OCR.space smoke/engine comparison (requires a free API key configured locally) |
| `npm run test:expanded-corpus:integrity` | Verify expanded screenshot corpus integrity |
| `npm run test:opencv` | OpenCV crop-isolation tests |
| `npm run test:paddleocr-fallback` | PaddleOCR fallback contract tests |

## Accuracy and safety principles

1. OCR output is candidate evidence, not authority.
2. Never loosen the nine-field exact-value contract to improve a pass rate.
3. Do not fuzzy-correct player names, ship names, identifiers or numbers. Abstain and route ambiguity to review.
4. Preserve raw OCR, parsed candidates, accepted values, human corrections and provenance separately.
5. External OCR is optional. Only localized bounded crops may be sent, and only after the operator configures the free OCR.space key. Quotas are conservatively reserved in a local ledger.
6. Keep event scoring policy separate from screenshot extraction; the tournament contains multiple event formats and must not use a universal scoring formula.
7. Keep tokens, API keys, `.env`, databases, runtime state and private user data out of Git.

## Repository layout

- `src/`: Discord bot, battle-record schema, parsers, OCR supervision, CV and provider adapters.
- `services/`: supporting OCR pipeline components.
- `scripts/`: regression runners, diagnostics, setup helpers and live smoke tests.
- `tests/`: unit and integration-contract tests.
- `docs/`: architecture, iteration records, acceptance reports and operator guidance.
- `fixtures/`, `testdata/`, `provenance/`: test images, source provenance and corpus materials (subject to package-transfer and data-size limits).

## Privacy

Never commit a real Discord token or OCR.space API key. Use local `.env` only. The OCR.space route is disabled unless explicitly enabled with a key; selected field crops are transmitted to the provider when enabled. Review the provider's current terms and privacy policy before enabling external OCR in a deployment.

## Current working checkpoint

The detailed current status and pending gates should be kept in `OCRSPACE_LIVE_PROVIDER_GATE_STATUS_2026-11-04.md` and `GITHUB_TRANSFER_STATUS.md`. Do not infer release readiness from a successful package upload.
