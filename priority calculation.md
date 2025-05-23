# Patient Priority Classification System

This module assigns medical priority to patients based on the presence of specific symptoms, ordered by clinical severity.

---

## Symptom-Based Priority Levels

| **Priority** | **Description** | **Symptoms** |
|--------------|-----------------|--------------|
| Priority 1 (Critical) | Life-threatening conditions | Anaphylaxis, Seizure, Unconsciousness |
| Priority 2 (Urgent)   | Severe trauma or allergic reaction | Fractures, BrokenBone, AllergicReaction |
| Priority 3 (Moderate) | Moderate infection symptoms | Fever, Cough, Nausea |
| Priority 4 (Low)      | Non-urgent mild symptoms | Headache |

---

## Priority Calculation Code

```c
int calculate_priority_by_symptoms(int symptoms[], int n) {
    // Symptom order:
    // 0: Anaphylaxis, 1: Seizure, 2: Unconsciousness,
    // 3: Fractures, 4: BrokenBone, 5: AllergicReaction,
    // 6: Fever, 7: Cough, 8: Nausea, 9: Headache

    // Priority 1: Any critical symptom present
    if (symptoms[0] || symptoms[1] || symptoms[2]) return 1;

    // Priority 2: Any trauma or severe reaction
    if (symptoms[3] || symptoms[4] || symptoms[5]) return 2;

    // Priority 3: Moderate symptoms
    if (symptoms[6] || symptoms[7] || symptoms[8]) return 3;

    // Priority 4: Only mild symptoms (e.g., Headache or none)
    return 4;
}
