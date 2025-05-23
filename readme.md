## Patient Priority Classification

The system assigns a priority level (1 to 4) to each patient based on symptom severity, age, presence of chest pain, and total wait time.

### Escalation & Priority Rules

| **Priority** | **Level Name** | **Conditions**                                                                                                                     |
| ------------ | -------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| **1**        | Critical    | - Has **Chest Pain**<br>OR<br>- **6 or more symptoms**<br>OR<br>- **Age ≥ 75** AND **4+ symptoms**<br>OR<br>- **Waited ≥ 2 hours** |
| **2**        | Urgent      | - **4–5 symptoms**<br>OR<br>- **Age ≥ 70** AND **2+ symptoms**<br>AND<br>No chest pain<br>AND<br>Wait time < 2 hours               |
| **3**        | Normal      | - **2–3 symptoms**<br>AND<br>Age < 70<br>AND<br>No chest pain<br>AND<br>Wait time < 2 hours                                        |
| **4**        | Low         | - **0–1 symptom**<br>AND<br>No critical risk factors                                                                               |

---

### Priority Escalation (Over Time)

If a patient has waited for **2 or more hours**, their priority is immediately escalated to **Priority 1**, regardless of initial classification.

This helps ensure that all patients are treated in a timely and fair manner, even in long queues.

---

### Code Logic

```c
if (hasChestPain || symptomCount >= 6 || (age >= 75 && symptomCount >= 4) || waitedHours >= 2) {
    priority = 1;
} else if ((symptomCount >= 4) || (age >= 70 && symptomCount >= 2)) {
    priority = 2;
} else if (symptomCount >= 2) {
    priority = 3;
} else {
    priority = 4;
}
