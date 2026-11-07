# Security Policy

## Project status and support

PotholeMap is in engineering foundation, not a live public-beta service. There are no supported production releases yet. Security reports about the current default branch and active PRs are welcome; no guaranteed response time, bounty or uptime is offered.

## Report vulnerabilities privately

Use the repository's **Security → Advisories → Report a vulnerability** channel:

[Submit a private vulnerability report](https://github.com/LaurensiusDani/PotholesMap/security/advisories/new)

Private vulnerability reporting must be enabled on the public repository as part of M0-01 setup. Its actual state and verification evidence are recorded in the [governance runbook](docs/runbooks/REPOSITORY_GOVERNANCE.md). If the form is unavailable, do not post exploit details or credentials publicly; a public issue may state only that the private channel is unavailable and request maintainer help, without describing the vulnerability.

Include the affected branch/commit or URL, expected and actual behavior, impact, minimal reproduction using synthetic data and any safe mitigation. Redact tokens, reporter identities, private image links and database contents. Coordinate remediation before public disclosure.

## Testing boundaries

- Use local or explicitly authorized isolated environments and synthetic images/reports.
- Do not access other people's data, attack provider infrastructure, run destructive/bulk tests or exhaust free-tier quotas.
- Do not upload real sensitive images or credentials to public issues, PRs or CI artifacts.
- Ordinary product bugs without security/private-data implications use the bug issue template.

## Maintainer response

Triage reports privately, reproduce safely, assess affected versions/data, prepare scoped fixes and regression checks, and coordinate disclosure. Rotate confirmed exposed credentials through authorized operations; pause intake or roll back when necessary. Detailed operational incident/recovery runbooks are delivered by their owning later milestones.
