# 🛡️ CryptOS-PKI

> An immutable, API-driven, high-assurance PKI operating system.
> Talos Linux philosophy applied to certificate authorities.
> No SSH. No shell. No interactive access. mTLS gRPC only.

## ✨ What it is

CryptOS-PKI runs your organization's certificate authorities on a hardened, immutable Linux image. The CA's private keys never touch disk in the clear. The only way in is an mTLS-authenticated gRPC API. Every operation is declarative, audited, and reproducible.

The project is built in the spirit of [Talos Linux](https://www.talos.dev/): a single static Go init, a read-only SquashFS rootfs, an encrypted state partition, no package manager, no `/bin/sh`. A single image boots into a Root, Intermediate, or Issuing CA role based on its machine config.

It is for teams that run their own internal PKI: TLS and mTLS for services and Kubernetes workloads, network devices, Active Directory domain controllers, vCenter and ESXi hosts, and code signing, without AD CS and without a general-purpose server holding the CA key.

## 📦 Repositories

| Repo | What it is |
|---|---|
| 🧠 [**cryptos**](https://github.com/CryptOS-PKI/cryptos) | The OS / engine. Builds the Unified Kernel Image (UKI). Hosts the gRPC API, embedded etcd, TPM operations, the enrolment and revocation endpoints, and the `cryptosctl` CLI. **No web UI in the image**, by design. |
| 📡 [**api**](https://github.com/CryptOS-PKI/api) | Shared `.proto` definitions and generated gRPC stubs. Consumed by `cryptos`, `manager`, and `web` (via generated TS stubs). |
| 🛰️ [**manager**](https://github.com/CryptOS-PKI/manager) | The Fleet Manager backend. Optional control plane: node adoption and linking, cross-node inventory, fleet topology, and an MCP endpoint for AI agents. Talks to nodes over the same mTLS gRPC API. Holds no private keys. Its own Helm chart is the supported way to install it. |
| 🎨 [**web**](https://github.com/CryptOS-PKI/web) | The Fleet Manager web frontend, and the only web UI in the project. React + TypeScript, built with Vite, embedded in and served by `manager`. |
| ⚓ [**helm**](https://github.com/CryptOS-PKI/helm) | Helm charts for the control plane on Kubernetes. Today it holds one Fleet Manager chart; the chart in `manager` is the supported install. |
| 📚 [**docs**](https://github.com/CryptOS-PKI/docs) | The CryptOS website and documentation: what CryptOS is, installing, running and managing it, the concepts behind it, and the API reference. |
| 🧪 [**lab**](https://github.com/CryptOS-PKI/lab) | Tooling for testing CryptOS on real and virtual hardware: VMware ESXi via `govc` today, bare metal planned. |
| 🏠 [**.github**](https://github.com/CryptOS-PKI/.github) | This profile, plus the organization-wide contributing guide, code of conduct and security policy. |

## 🚦 Status

**Alpha.** Versions are `0.x`, and `1.0.0` will be the first generally available release. CryptOS already runs in real production use. Until `1.0.0`, the API and the machine config can still change between releases.

☁️ The project plans to apply to the [CNCF Sandbox](https://github.com/cncf/sandbox).

### Works today

- 🪨 **One image, three roles.** A signed UKI boots as a Root, Intermediate or Issuing CA, installs to disk from maintenance mode, and upgrades in place. Bring your own Secure Boot key.
- 🔑 **First-boot ceremony.** Creates the Root on the node itself, RFC 5280 strict.
- 🌳 **CA hierarchy.** Root, Intermediate and Issuing CAs, subordinate signing (including vCenter VMCA), certificate profiles, and leaf issuance through `cryptosctl` or the Fleet Manager.
- 🚫 **Revocation.** CRL and OCSP (RFC 6960), with a preflight that blocks issuance while the revocation endpoints don't answer.
- 🔌 **Enrolment protocols.** ACME (RFC 8555, `http-01`) and EST (RFC 7030) on Intermediate and Issuing nodes, set up in the machine config.
- 🗝️ **Key protection.** CA keys in the TPM or on the encrypted state partition, whose key is protected by the TPM, the node's hardware identity or an external KMS, plus operator-held key escrow and CA re-key.
- 🧰 **Management.** `cryptosctl` over mTLS gRPC, a hash-chained audit log, and a read-only status console on the node.
- 🛰️ **Fleet Manager.** Node adoption, fleet topology, certificates, profiles, operators signed in with client certificates, and an MCP endpoint for AI agents with step-up approvals.

### Being built now

- 🔀 **Protocol switches.** ACME and EST turned on and off through `config apply` and the Fleet Manager, as a change that takes effect at the next reboot.
- 📟 **SCEP** (RFC 8894), so network devices can enrol a trustpoint.
- ⏱️ **RFC 3161 timestamps** for code signing.
- 🕰️ **Clock sync, RSA CA keys in the TPM, `cryptosctl` for Windows, and a bare-metal image.**

### Roadmap

- 🌐 **ACME `dns-01` and wildcard names.**
- 🪟 **Windows autoenrolment** through Group Policy (MS-XCEP and MS-WSTEP), with no AD CS.
- 📶 **Machine and user certificates** for VPN and 802.1X.
- ☸️ **An external CA for a Kubernetes cluster's own certificates.**
- 🤝 **Two-node HA pairs** with a shared VRRPv3 address, M-of-N quorum for Root operations, and signed late-binding extensions.

## 🧭 Guiding principles

- 🚫 **No interactive access.** No SSH, no shell, no usernames/passwords. Management is `cryptosctl` over mTLS gRPC, or the Fleet Manager (same mTLS gRPC). The OS image hosts no web frontend.
- 🪨 **Immutable rootfs.** SquashFS, read-only. Persistent state only on the encrypted partition, unsealed at boot by the local TPM, the node's hardware identity or an external KMS.
- 🔑 **Keys stay on the box.** CA keys live in the local TPM or on the encrypted state partition. Never on disk in the clear. No network HSM.
- 📜 **Declarative config.** Roles, CA hierarchy, issuance policies, protocols: all version-controlled YAML applied via API. No click-ops.
- 🦺 **Memory safety.** Go for the node and the Fleet Manager backend. `unsafe` only when crossing into kernel/TPM headers.
- 🧪 **Stdlib-only on the crypto path.** Key generation, signing, X.509 marshaling, TLS: Go stdlib + `golang.org/x/crypto` only. No `cfssl`, no `smallstep`, no PKI wrappers. Wire formats are written by hand on the stdlib too: the CMS for EST and SCEP, and the JWS for ACME.
- 📐 **RFC-strict on the wire.** Every protocol (TLS 1.3, X.509, ACME, SCEP, EST, OCSP, RFC 3161, VRRPv3, …) follows its RFC to the letter. MUST is MUST.
- ✂️ **Minimize maintenance.** When two designs solve a requirement equally well, the lower-maintenance one wins.

## 🤝 Get involved

Start with the [docs](https://github.com/CryptOS-PKI/docs), and ⭐ or watch the repos to follow along.

Opening issues and pull requests is limited to collaborators today. It will open to the public. When it does, a change starts as an issue from the repo's templates, and every commit is signed off under the [Developer Certificate of Origin](https://developercertificate.org/) (`git commit -s`).

- 📝 [**CONTRIBUTING.md**](https://github.com/CryptOS-PKI/.github/blob/main/CONTRIBUTING.md): the workflow and the DCO sign-off.
- 💬 [**CODE_OF_CONDUCT.md**](https://github.com/CryptOS-PKI/.github/blob/main/CODE_OF_CONDUCT.md): the CNCF Community Code of Conduct.
- 🔒 [**SECURITY.md**](https://github.com/CryptOS-PKI/.github/blob/main/SECURITY.md): report vulnerabilities privately, never in a public issue.

## 📄 License

Apache License 2.0. See each repo's `LICENSE` for details.
