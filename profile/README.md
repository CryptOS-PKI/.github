<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="assets/cryptos-mark-dark.svg">
    <img src="assets/cryptos-mark-light.svg" alt="CryptOS mark" width="96">
  </picture>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="assets/fleetos-mark-dark.svg">
    <img src="assets/fleetos-mark-light.svg" alt="FleetOS mark" width="96">
  </picture>
</p>

# CryptOS-PKI™ 🛡️

> 🔐 An immutable, API-driven, high-assurance PKI operating system.

CryptOS-PKI is an open-source, Apache-2.0 licensed operating system for certificate
authorities. One signed, immutable image boots as a Root, Intermediate or Issuing CA.
The CA keys live in the TPM or on an encrypted state partition and never touch disk in
the clear, and there's no SSH and no shell: the only way in is an mTLS gRPC API, driven
by the `cryptosctl` CLI or the optional Fleet Manager. It's for teams that run their own
internal PKI without AD CS and without a general-purpose server holding the CA key. It's
alpha software, versioned 0.x until 1.0.0.

🏠 **Home:** [cryptos-pki.com](https://cryptos-pki.com)

## 🧩 Node and Fleet Manager

- [cryptos-node](https://github.com/CryptOS-PKI/cryptos-node): the PKI engine: the CA, the gRPC API, the enrolment and revocation endpoints, and the `cryptosctl` CLI.
- [cryptos-appliance](https://github.com/CryptOS-PKI/cryptos-appliance): the appliance image built around the engine: the hardened kernel, the signed Unified Kernel Image, the read-only SquashFS, the installer and A/B upgrades.
- [cryptos-manager](https://github.com/CryptOS-PKI/cryptos-manager): the Fleet Manager backend: node adoption, inventory and fleet topology over mTLS gRPC, an MCP endpoint for AI agents, and its own Helm chart.

## 🖥️ Web

- [cryptos-web](https://github.com/CryptOS-PKI/cryptos-web): the Fleet Manager web frontend, in React and TypeScript, served by cryptos-manager.

## 🧪 Lab

- [cryptos-lab](https://github.com/CryptOS-PKI/cryptos-lab): tooling for testing CryptOS on real and virtual hardware: VMware ESXi via `govc` today, bare metal planned.

## 📚 Docs

- [docs](https://github.com/CryptOS-PKI/docs): the source of the CryptOS documentation site.

## 🌐 Website

- [website](https://github.com/CryptOS-PKI/website): the source of the CryptOS product site (www.cryptos-pki.com).

## 🤝 Contributing

- [Contributing guide](https://github.com/CryptOS-PKI/.github/blob/main/CONTRIBUTING.md), with the DCO sign-off
- [Code of Conduct](https://github.com/CryptOS-PKI/.github/blob/main/CODE_OF_CONDUCT.md)
- [Security policy](https://github.com/CryptOS-PKI/.github/blob/main/SECURITY.md)

## 🙏 Acknowledgements

CryptOS was originally written by [@Bugs5382](https://github.com/Bugs5382).

## 📄 Licence

[Apache-2.0](https://github.com/CryptOS-PKI/.github/blob/main/LICENSE).

## ™️ Trademarks

CryptOS-PKI™, FleetOS™ and their logos are trademarks of the rabbit hole tech, llc. The code is licensed under Apache-2.0; the licence does not grant rights to the names or logos. See [TRADEMARKS.md](https://github.com/CryptOS-PKI/.github/blob/main/TRADEMARKS.md) for the usage policy. Applications to register these trademarks are being prepared; full registered (®) protection is pending until they are granted.
