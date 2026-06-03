# 🛡️ CryptOS-PKI

> An immutable, API-driven, high-assurance PKI operating system.
> Talos Linux philosophy applied to certificate authorities.
> No SSH. No shell. No interactive access. mTLS gRPC only.

## ✨ What it is

CryptOS-PKI runs your organization's certificate authorities on a hardened, immutable Linux image. The CA's private keys live inside the TPM and never touch disk in the clear. The only way in is an mTLS-authenticated gRPC API. Every operation is declarative, audited, and reproducible.

The project is built in the spirit of [Talos Linux](https://www.talos.dev/) — single static Go init, read-only SquashFS rootfs, encrypted state partition unsealed by the local TPM, no package manager, no `/bin/sh`. A single image boots into a Root, Intermediate, or Issuing CA role based on its machine config.

## 📦 Repositories

| Repo | What it is |
|---|---|
| 🧠 [**cryptos**](https://github.com/CryptOS-PKI/cryptos) | The OS / engine. Builds the Unified Kernel Image (UKI). Hosts the gRPC API, embedded etcd, TPM operations, and the on-box `cryptosctl` CLI. **No web UI in the image** — by design. |
| 🛰️ [**manager**](https://github.com/CryptOS-PKI/manager) | The Fleet Manager backend. Optional control plane: multi-node aggregation, cross-node inventory, and chain-of-trust visualization. Talks to nodes via the same mTLS gRPC API. Holds no private keys. |
| 🎨 [**web**](https://github.com/CryptOS-PKI/web) | The Fleet Manager web frontend. React + TypeScript, built with Vite, served by `manager/`. Built only when a web UI is wanted. |
| 📡 [**api**](https://github.com/CryptOS-PKI/api) | Shared `.proto` definitions and generated gRPC stubs. Consumed by `cryptos/`, `manager/`, and `web/` (via generated TS stubs). |

## 🚦 Status

**Pre-alpha.** Phase 1 scaffolding has landed across `api/` and `cryptos/`. Subsystem implementation is in progress; the project is not yet usable.

The build phases:

1. 🪨 **Phase 1 — Core OS + single-node Root CA MVP.** Hardened kernel, Go PID 1, SquashFS rootfs, TPM-unsealed encrypted state, embedded etcd, mTLS gRPC API, first-boot ceremony, `cryptosctl`. Validates the full crypto path is RFC 5280 strict by self-signing a Root cert in QEMU + `swtpm`.
2. 🔌 **Phase 2 — Role-aware API + protocol adapters + Fleet Manager.** Root / Intermediate / Issuing role split. ACME (RFC 8555), SCEP (RFC 8894), EST (RFC 7030), WSTEP, RFC 3161 timestamping, OCSP (RFC 6960), CRL distribution. Fleet Manager backend + web frontend.
3. 🛡️ **Phase 3 — Pool, HA, extensions, isolation, recovery.** 2-node HA pairs (Infoblox-style failover, VRRPv3 shared VIP, `TPM2_Duplicate` key sharing). Multi-Root / multi-Intermediate fleet topology with configurable depth (default cap 3). Fleet Manager linkage protocol (mutual-consent enrollment via TPM EK attestation). Talos-style signed late-binding extensions. Explicit threat model. Disaster-recovery escrow (Shamir, paper, smart-card, KMS).

## 🧭 Guiding principles

- 🚫 **No interactive access.** No SSH, no shell, no usernames/passwords. Management is `cryptosctl` over mTLS gRPC, or the Fleet Manager (same mTLS gRPC). The OS image hosts no web frontend.
- 🪨 **Immutable rootfs.** SquashFS, read-only. Persistent state only on the encrypted partition, unsealed by the local TPM.
- 🔑 **TPM-bound identity.** Private keys are wrapped by the TPM. Never on disk in the clear. No network HSM.
- 📜 **Declarative config.** Roles, CA hierarchy, issuance policies, rotation cadence — all version-controlled YAML applied via API. No click-ops.
- 🦺 **Memory safety.** Go for everything. `unsafe` only when crossing into kernel/TPM headers.
- 🧪 **Stdlib-only on the crypto path.** Key generation, signing, X.509 marshaling, TLS — Go stdlib + `golang.org/x/crypto` only. No `cfssl`, no `smallstep`, no PKI wrappers. Wire-format-only libraries (CMS, JOSE, SOAP/WS-Security) are permitted for protocol adapters and pinned like critical dependencies.
- 📐 **RFC-strict on the wire.** Every protocol (TLS 1.3, X.509, ACME, SCEP, EST, OCSP, RFC 3161, VRRPv3, …) follows its RFC to the letter. MUST is MUST.
- ✂️ **Minimize maintenance.** When two designs solve a requirement equally well, the lower-maintenance one wins.

## 🤝 Contributing

The project is in early Phase 1 implementation and currently driven by a small team. Issues and PRs follow Conventional Commits and the branch-per-change / squash-merge workflow documented in each repo. If you're interested in the project, ⭐ the repos and check back as Phase 1 lands.

## 📄 License

Apache License 2.0. See each repo's `LICENSE` for details.
