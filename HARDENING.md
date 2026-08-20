<!-- markdownlint-disable -->

# Hardening Report: anchore--sbom-action/v0.24.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **anchore--sbom-action/v0.24.0** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### github-env-injection (severity: high)

In update-syft-release.yml, the 'Get latest Syft version' step fetches LATEST_VERSION from an external GitHub API call (`gh release view --json name -q '.name' -R anchore/syft`) and writes it directly to $GITHUB_OUTPUT without sanitization: `echo "LATEST_VERSION=$LATEST_VERSION" >> $GITHUB_OUTPUT`. A maliciously crafted release name on the external repository containing newline characters could inject additional key=value pairs into $GITHUB_OUTPUT. The required sanitization step (`printf '%s' "$LATEST_VERSION" | tr -d '\n\r'`) is missing before the write.

Locations:

- `.github/workflows/update-syft-release.yml:29`

### unpinned-uses (severity: high)

The workflow uses a reusable workflow pinned to the mutable branch ref '@main' instead of a full 40-character commit SHA. This means the referenced workflow can change at any time without notice, enabling supply-chain attacks. Failing reference: `anchore/workflows/.github/workflows/oss-project-board-add.yaml@main`

Locations:

- `.github/workflows/oss-project-board-add.yaml:15`

### unpinned-uses (severity: high)

The workflow uses a reusable workflow pinned to the mutable branch ref '@main' instead of a full 40-character commit SHA. This means the referenced workflow can change at any time without notice, enabling supply-chain attacks. Failing reference: `anchore/workflows/.github/workflows/remove-awaiting-response-label.yaml@main`

Locations:

- `.github/workflows/remove-awaiting-response-label.yaml:12`

## Iteration Notes

### Iteration 1

**Fixes applied:** github-env-injection, unpinned-uses

**Notes:**

Fixed three security findings: (1) github-env-injection in update-syft-release.yml: sanitized LATEST_VERSION with `printf '%s' "$LATEST_VERSION" | tr -d '\n\r'` before writing to $GITHUB_OUTPUT to prevent newline injection attacks from maliciously crafted external release names; also quoted $GITHUB_OUTPUT. (2) unpinned-uses in oss-project-board-add.yaml: pinned anchore/workflows reusable workflow from mutable @main branch ref to full commit SHA @a71346be63db56f324237b6a35fd35ffcec6d737 with # main comment. (3) unpinned-uses in remove-awaiting-response-label.yaml: pinned anchore/workflows reusable workflow from mutable @main branch ref to the same full commit SHA @a71346be63db56f324237b6a35fd35ffcec6d737 with # main comment.

