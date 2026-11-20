# Workspace instructions — Superpowers with local independent review

This workspace uses the **Superpowers** framework pattern with local independent review.

## Mandatory workflow for development tasks

1. **Capture requirements.** Preserve the user's original request verbatim, list acceptance criteria, authorized scope, prohibitions, and what must remain unchanged. Do not weaken or invent requirements.
2. **Implement** only the authorized changes. Track every modified file.
3. **Self-verify.** Read back modified files, run relevant tests and runtime checks, and fix confirmed issues.
4. **Discover the reviewer model.** Run `node tools/local-reviewer.mjs discover` if the tool exists. Otherwise, use the configured reviewer model from `.opencode/opencode.json`.
5. **Independent review.** Dispatch a fresh `subagent` (`agent: "reviewer"`) using the local model, with the original request and the actual implementation as evidence. Require two stages: specification compliance and technical quality. It must not edit files or dispatch further subagents.
6. **Correction loop.** Return confirmed findings to the primary model, fix them, re-run self-verification, and dispatch a fresh reviewer pass on the corrected work. Repeat while confirmed issues remain.
7. **Finalize status.** Report the verification status at the end of the response.

## Verification status

- `✓ Verified` - self-verification passed, reviewer returned complete valid result, no confirmed issues
- `◐ Partially Verified` - incomplete checks, insufficient evidence
- `◐ Local AI Unavailable` - reviewer endpoint down or model unusable
- `✗ Verification Failed` - confirmed unresolved issues or failed tests

## Notes

- Ordinary conversational questions get no audit, no reviewer dispatch.
- The reviewer model is `polystrata/gemma-4-26b-a4b-qat-ud-q4_k_xl` configured in `.opencode/opencode.json`.