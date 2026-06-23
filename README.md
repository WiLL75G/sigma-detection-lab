# Sigma Detection Lab

A working record of detection contributions I make to [SigmaHQ](https://github.com/SigmaHQ/sigma) **each one validated against real telemetry in my own lab before submission.**

Most detection rules get submitted straight from a blog post, untested. Everything here is different: I run it against live logs on my own endpoints, confirm the true positive fires, characterise the false-positive profile, and only then contribute upstream. Every folder is a self-contained case study with the evidence to back it.

> **The loop:** reproduce in lab → capture telemetry → confirm TP / characterise FP → document → submit lean upstream → track status honestly.

## Lab environment

| Component | Detail |
|-----------|--------|
| SIEM | Splunk (macOS host) + Microsoft Sentinel (KQL) |
| Endpoints | Windows 11 (`JAMES-VM`, 192.168.64.17), Ubuntu Server (`wazuh-manager`), Kali (`Attacker-Tier4`, 192.168.64.15) UTM VMs |
| Telemetry | Sysmon, Windows Security + PowerShell Script Block (4104), Linux auth.log, AMA → DCR |
| Validation | `sigma-cli` for schema/lint, manual TP/FP testing per contribution |

## Contribution index

| # | Contribution | Type | ATT&CK | Telemetry | Status | Evidence | Upstream |
|---|-------------|------|--------|-----------|--------|----------|----------|
| 1 | Local user creation via ADSI/WinNT missed by `posh_ps_create_local_user.yml` | Coverage gap | T1136.001 | Win 4104 | Reported · fix PR by community | [repo](https://github.com/WiLL75G/sigma-rule-validation-create-local-user) | [#6057](https://github.com/SigmaHQ/sigma/issues/6057) · [PR #6064](https://github.com/SigmaHQ/sigma/pull/6064) |
| 2 | PowerShell net-connection FP on legitimate Azure traffic | False positive | T1071.001 | Sysmon EID 3 | Reported | [repo](https://github.com/WiLL75G/soc-sigma-powershell-network-connection) | [#6056](https://github.com/SigmaHQ/sigma/issues/6056) |
| 3 | _next contribution…_ | | | | | | |

**Status legend:** `Draft` · `Tested` (validated in lab) · `Reported` (issue filed) · `PR Open` · `Merged` · `Needs revision`

**Type legend:** `New rule` · `Coverage gap` (existing rule misses a technique variant) · `False positive` (existing rule fires on benign activity)

## How each folder is organised

```
<contribution>/
├── README.md       # the writeup what, why it matters, the engineering reasoning
├── results.md      # lab evidence: TP proof, FP characterisation, sigma check output
├── rule.yml        # the rule (new) OR the proposed fix (gap/FP)
└── test-logs/      # the actual events used to validate it
```

Copy the matching `_TEMPLATE-*` folder for each new contribution.

## Why this repo exists

This is the portfolio I point recruiters and maintainers to. It's not a logbook of "I did things" each folder teaches the detection and shows the validation behind it. The status column tracks the real lifecycle, revisions and maintainer pushback included, because working *through* review is part of the engineering.

---
*William [Portfolio](https://will75g.github.io/-portfolio/) · [GitHub](https://github.com/WiLL75G) · [SigmaHQ contributions](https://github.com/SigmaHQ/sigma/issues?q=author%3AWiLL75G)*
