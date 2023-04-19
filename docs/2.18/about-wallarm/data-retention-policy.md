# Data retention policy

This policy outlines retention periods for different datasets collected by Wallarm and stored in the Wallarm Cloud.

| Dataset                                                                                                                                                                                                                                | Retention period |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------|
| Data on attacks, hits, and incidents detected by the filtering nodes                                                                                                                                                                         | 12 months        |
| Data on vulnerabilities detected by the filtering nodes or the **Active threat verification** module                                                                                                                                                                  | 12 months        |
| Data on vulnerabilities detected by the Vulnerability Scanner                                                                                                                                                                          | 24 months        |
| Statistics on processed and blocked requests displayed on the [dashboards](../user-guides/dashboard/intro.md)                                                                                                                          | 12 months        |
| [Company's exposed assets](../user-guides/scanner.md) detected by Exposed asset Scanner                                                                                                                                            | 24 months        |
| History of [denylisted IP addresses](../user-guides/denylist.md)                                                                                                                                                                     | 6 months         |
| Automatically generated or manually created [rules](../user-guides/rules/intro.md) for proccessing traffic by Wallarm nodes                                                                                                              | ∞                |
| Wallarm account configuration: [users](../user-guides/settings/users.md), [applications](../user-guides/settings/applications.md), [integrations](../user-guides/settings/integrations/integrations-intro.md), [triggers](../user-guides/triggers/triggers.md) | ∞                |
| [Audit log](../user-guides/settings/audit-log.md) records                                                                                                                                                                           | 12 month         |
