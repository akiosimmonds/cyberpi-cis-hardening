# System Inventory and Hardening Summary

## Status

The selected hardening phase was completed on September 26, 2026. Repository documentation and evidence packaging are in progress.

This summary reflects the recorded project conversation and command outputs. It is not a new live assessment.

## System Details

- Hostname: cyberpi
- Hardware: Raspberry Pi 5, as recorded in the project discussion
- Operating system: Debian GNU/Linux 13 (trixie)
- Architecture: arm64
- Kernel at evidence capture: Linux 6.18.50+rpt-rpi-2712
- Role: Security lab endpoint running Pi-hole and Tailscale, monitored by Wazuh
- Wazuh agent: Version 4.14.7; enabled and active at verification
- Evidence capture date: September 26, 2026

Memory capacity and storage specifications are not established in the reviewed evidence.

## Assessment Method and Results

Assessment tool: Wazuh Security Configuration Assessment (SCA).

Policy: CIS Debian Linux 13, using /var/ossec/ruleset/sca/cis_debian13.yml.

The exact benchmark revision and profile must be confirmed from the policy metadata before being stated.

| Assessment stage | Passed | Failed | Not applicable | Reported score |
| --- | ---: | ---: | ---: | ---: |
| Initial baseline | 77 | 107 | 23 | 41% |
| Intermediate | 100 | 89 | 18 | 52% |
| Final | 115 | 79 | 13 | 59% |

The reported score improved by 18 percentage points, with 38 additional checks passing. Applicability counts also changed between assessments.

The final scan completed on September 26, 2026. These are Wazuh policy results, not a claim of full CIS compliance or certification.

## Recorded Hardening and Validation

### SSH

Effective configuration was inspected using sshd -T.

- Root login prohibited
- Password authentication disabled
- Public-key authentication enabled
- Allowed user restricted to akio
- Maximum authentication attempts: 4
- Login grace time: 60 seconds
- Client alive interval: 15 seconds
- Client alive count maximum: 3
- Login banner configured
- SSH forwarding retained as an intentional exception

### Host Firewall

UFW was recorded as active with:

- Default deny incoming
- Default allow outgoing
- Default deny routed
- Explicit rules supporting local SSH, Pi-hole DNS/web access, and Tailscale functionality

The exact rules will be preserved in the firewall evidence.

### Password and Account Controls

The recorded PAM configuration included:

- pam_pwquality
- Password history of 24 passwords, with enforce_for_root
- yescrypt password hashing

The recorded aging settings for the administrator account were:

- Minimum password age: 1 day
- Maximum password age: 365 days
- Expiration warning: 7 days
- Inactivity period after password expiration: 45 days

### Logging and Monitoring

- Wazuh agent enabled and running
- SCA scan completion verified in the agent log
- Journald configured with ForwardToSyslog=no, Compress=yes, and Storage=persistent
- Journald configuration precedence checked and service confirmed active
- Privileged command activity recorded in /var/log/sudo.log

## Exceptions and Troubleshooting

The project conversation records these decisions and findings:

- Outbound traffic remained allowed; a tested egress allowlist was deferred.
- SSH forwarding and the graphical interface were retained.
- Partitioning changes were deferred.
- Journald was selected instead of the alternative rsyslog configuration.
- Remote journald controls were not implemented under the selected monitoring design; Wazuh monitoring does not itself demonstrate compliance with those controls.
- A pam_pwquality scanner discrepancy was recorded: the configuration line and an independent matching test were present, while the scanner still reported failure.
- SSH configuration precedence, PAM profile selection, and journald drop-in ordering required troubleshooting.

Remaining failed checks require individual classification. They are not all verified false positives or accepted risks.

## Evidence Status

The earlier conversation records completed captures for:

- System identification
- Wazuh agent operation
- Final SCA results
- Journald configuration
- Effective SSH configuration
- UFW firewall rules
- PAM password configuration
- Account aging
- Sudo activity logging

The evidence files still need to be organized, reviewed for sensitive information, and linked in this repository.

## Details to Recover from Supporting Records

- Memory and storage specifications
- Exact benchmark revision and profile
- Update status at assessment time
- Complete service and listening-port inventory
- Time synchronization status
- Backup and recovery arrangements

These are documentation gaps in the reviewed material, not assertions that the work was never performed.