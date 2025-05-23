# Patient Priority Escalation System

This module manages automatic patient priority escalation in a hospital queue system.

## Priority Rules

| Priority | Conditions |
|----------|------------|
| **1**    | - Has **Chest Pain**<br>OR<br>- 5+ symptoms<br>OR<br>- Age ≥ 70<br>OR<br>- Waited ≥ 3 hours |
| **2**    | - 3–4 symptoms, no chest pain, wait < 3h |
| **3**    | - 0–2 symptoms, no chest pain, wait < 3h |

## Example Code Snippet

```c
if (hasChestPain || symptomCount >= 5 || age >= 70 || waitedHours >= 3) {
    priority = 1;
} else if (symptomCount >= 3) {
    priority = 2;
} else {
    priority = 3;
}
