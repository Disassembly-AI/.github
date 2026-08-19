# Disassembly.AI

**AI-driven penetration testing.** Disassembly.AI probes an app or API for security holes the way
a human pentester would — it plans, executes, and verifies an assessment, then hands back a
**SARIF report** you can drop straight into GitHub or GitLab code scanning. Run it by hand from the
CLI, or wire it into your CI/CD pipeline. You pay per run and per token used.

🌐 **Website:** https://disassembly.ai · 💳 **Get a key / pick a plan:** https://disassembly.ai/pricing

> ⚠️ **Authorized use only.** Disassembly.AI performs offensive-security actions. Only point it at
> systems you own or are explicitly contracted to test. Keep scope allow-lists and consent gates in place.

---

## Get started

**1. Get an API key.** Sign up and choose a plan (Starter / Team / Business) on the
[customer portal](https://disassembly.ai/pricing). You'll be issued an instance key — set it as
`DISASSEMBLY_API_KEY`.

**2. Run a scan.** Use the CLI, the container, or wire it into CI — whichever fits.

### Option A — CLI

```bash
pip install disassembly            # or: pipx install disassembly
export DISASSEMBLY_API_KEY=sk-...  # from the portal — never hard-code it

disassembly scan https://staging.example.com --ci > report.sarif
```

The `--ci` flag emits **SARIF 2.1.0** to stdout (plus a Markdown report). Upload the SARIF to your
platform's code-scanning surface and findings show up inline on the PR.

### Option B — Container

```bash
docker run --rm \
  -e DISASSEMBLY_API_KEY \
  ghcr.io/disassembly-ai/toolkit:latest \
  scan https://staging.example.com --ci > report.sarif
```

Also on Docker Hub as `disassemblyai/toolkit`.

### Option C — CI/CD (GitHub Actions)

```yaml
# .github/workflows/security-scan.yml
name: security-scan
on: { pull_request: {} }
jobs:
  scan:
    runs-on: ubuntu-latest
    permissions: { contents: read, security-events: write }
    steps:
      - uses: disassembly-ai/scan-action@v1
        with:
          target: https://staging.example.com
        env:
          DISASSEMBLY_API_KEY: ${{ secrets.DISASSEMBLY_API_KEY }}
      - uses: github/codeql-action/upload-sarif@v3
        with: { sarif_file: report.sarif }
```

---

## Wrapper apps & integrations

Everything above is one image and one CLI. Drop it into whatever you already run — all examples are
public and MIT-licensed in **[disassembly-examples](https://github.com/Disassembly-AI/disassembly-examples)**:

| You run… | We have a copy-paste example |
|---|---|
| **GitHub Actions / GitLab / Jenkins / CircleCI** | [`ci/`](https://github.com/Disassembly-AI/disassembly-examples/tree/main/ci) |
| **AWS CodeBuild / Azure Pipelines / Google Cloud Build** | [`ci/`](https://github.com/Disassembly-AI/disassembly-examples/tree/main/ci) |
| **Docker Compose / Kubernetes / AWS ECS** (sidecar) | [`sidecar/`](https://github.com/Disassembly-AI/disassembly-examples/tree/main/sidecar) |
| **Terraform** (AWS / Azure / GCP + reusable module) | [`terraform/`](https://github.com/Disassembly-AI/disassembly-examples/tree/main/terraform) |
| **Just the CLI or the Python library** | [`quickstart/`](https://github.com/Disassembly-AI/disassembly-examples/tree/main/quickstart) |

**Conventions across every example:** image `ghcr.io/disassembly-ai/toolkit:latest` · auth via
`DISASSEMBLY_API_KEY` from your secret store (never hard-coded) · target via `TARGET` · SARIF out to
`report.sarif`.

---

## Links

- 🌐 **Website & pricing:** https://disassembly.ai
- 📦 **Integration examples (public):** https://github.com/Disassembly-AI/disassembly-examples
- 📚 **Docs & quickstart:** https://github.com/Disassembly-AI/disassembly-examples#readme

Questions or a security concern? Reach us through https://disassembly.ai.
