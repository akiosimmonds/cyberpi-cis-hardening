# System Inventory and Hardening Summary

## Status

The selected hardening phase was completed on September 26, 2026. This document summarizes the results, decisions, and supporting evidence assembled for the portfolio. Remaining documentation gaps are listed below.

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

[View effective SSH configuration evidence](../screenshots/cyberpi-ssh-hardening-2026-09-26.png)

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

[View UFW firewall configuration evidence](../screenshots/cyberpi-ufw-firewall-2026-09-26.png)

UFW was recorded as active with:

- Default deny incoming
- Default allow outgoing
- Default deny routed
- Explicit rules supporting local SSH, Pi-hole DNS/web access, and Tailscale functionality

The linked screenshot records the exact firewall rules at evidence capture.

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

[View journald configuration evidence](../screenshots/cyberpi-journald-config-2026-09-26.png)

- Wazuh agent enabled and running
- SCA scan completion verified in the agent log
- Journald configured with ForwardToSyslog=no, Compress=yes, and Storage=persistent
- Journald configuration precedence checked and service confirmed active
- Privileged command activity recorded in /var/log/sudo.log

## Exceptions and Troubleshooting

### Decisions Made During This Hardening Phase

- **Outbound firewall policy:** Outbound traffic remained allowed to preserve required lab services. A default-deny policy was deferred until an explicit egress allowlist could be designed and tested.
- **SSH forwarding:** Forwarding was retained for lab functionality. This remains an intentional deviation from the assessed restriction.
- **Graphical interface:** The GUI was retained for the endpoint's lab use.
- **Partitioning:** Filesystem partitioning changes were deferred because they require separate planning and recovery preparation.
- **Logging architecture:** Journald was selected instead of rsyslog. Checks for the alternative logging approach require applicability review rather than installing another logging service solely to improve the score.
- **Remote journald:** Remote journald controls were not implemented. Wazuh provides security monitoring, but its presence does not demonstrate compliance with remote journald requirements.

These decisions describe this project's scope. They do not establish that every remaining failed check is acceptable or inapplicable.

### Troubleshooting Findings

- **SSH configuration precedence:** The investigation identified a conflict involving 50-cloud-init.conf. Effective settings were checked with sshd -T.
- **PAM profile selection:** A backup file unexpectedly appeared as a selectable PAM profile, requiring investigation of profile selection.
- **Journald drop-in ordering:** An earlier override lost precedence to syslog.conf. The final configuration used zz-cyberpi-hardening.conf, and configuration ordering was checked.
- **Password-quality scanner discrepancy:** Wazuh continued reporting a pam_pwquality failure despite the expected configuration line and a successful independent pattern match. This was recorded as an unresolved discrepancy; matching configuration text alone does not prove full enforcement or a scanner false positive.

### Remaining Review

The final assessment reported 79 failed checks. Each requires individual review before being classified as a deferred remediation, intentional exception, applicability issue, or confirmed scanner discrepancy.

## Evidence Index

The following screenshots document the selected hardening phase. Terminal captures record configuration and command output at capture time; they do not establish that every control passed a functional test.

- [Baseline SCA assessment — September 21, 2026](../screenshots/cyberpi-sca-baseline-2026-09-21.png)
- [Final SCA assessment — September 26, 2026](../screenshots/cyberpi-sca-final-2026-09-26.png)
- [Effective SSH configuration](../screenshots/cyberpi-ssh-hardening-2026-09-26.png)
- [UFW firewall configuration](../screenshots/cyberpi-ufw-firewall-2026-09-26.png)
- [Journald configuration](../screenshots/cyberpi-journald-config-2026-09-26.png)
- [PAM configuration, account aging, and sudo activity logs](../screenshots/cyberpi-authentication-sudo-evidence-2026-09-26.png)
- [Wazuh agent status and version](../screenshots/cyberpi-wazuh-agent-2026-09-26.png)

## Details to Recover from Supporting Records

- Memory and storage specifications
- Exact benchmark revision and profile
- Update status at assessment time
- Complete service and listening-port inventory
- Time synchronization status
- Backup and recovery arrangements

These are documentation gaps in the reviewed material, not assertions that the work was never performed.