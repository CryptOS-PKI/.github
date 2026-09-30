# Security Policy

CryptOS-PKI is a certificate authority platform, so a vulnerability in it can put every
certificate it issues at risk. Please report problems privately and give the maintainers
time to fix them before anything is made public.

## Reporting a vulnerability

Report a vulnerability through GitHub private vulnerability reporting on the repository
the problem is in:

1. Open the repository on GitHub, for example
   [CryptOS-PKI/cryptos](https://github.com/CryptOS-PKI/cryptos).
2. Go to the **Security** tab and choose **Report a vulnerability**.
3. Describe the problem and send the report. Only you and the maintainers can see it.

If you aren't sure which repository is affected, report it on
[CryptOS-PKI/cryptos](https://github.com/CryptOS-PKI/cryptos/security/advisories/new).

> [!CAUTION]
> Never report a vulnerability in a public issue, pull request, discussion or commit. A
> public report tells attackers about the problem before a fix exists. If you opened one
> by mistake, close it and send the details through a private report instead.

A useful report includes:

- the affected repository, and the version or commit;
- what an attacker can do, and what they need first (network access, a credential, a
  particular role or configuration);
- the steps to reproduce it, or a proof of concept;
- any fix or mitigation you already have in mind.

Expect an acknowledgement within a few business days. The maintainers confirm the problem,
work on a fix in a private security advisory, and agree a disclosure date with you. Once
the fix is released the advisory is published, with a CVE where one applies, and you are
credited unless you ask not to be.

## Supported versions

CryptOS-PKI has not reached 1.0.0 yet. Until it does, only the latest `0.x` release of each
repository gets security fixes; older releases don't, so upgrade to the latest release to
pick up a fix.

| Version | Security fixes |
| --- | --- |
| Latest `0.x` release | Yes |
| Older `0.x` releases | No |

This table is updated when 1.0.0 ships.
