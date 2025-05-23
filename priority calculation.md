# Patient Priority Classification System

This module computes patient priority levels based on:

- Number of symptoms
- Chest pain
- Age
- Waiting time since admission

It is intended to be used in a hospital triage system.

---

## Priority Classification Table

| Priority | Level     | Conditions |
|----------|-----------|------------|
| **1**    | Critical | - Has **Chest Pain**<br>OR<br>- **6 or more symptoms**<br>OR<br>- **Age ≥ 60** and **≥ 4 symptoms**<br>OR<br>- **Waited ≥ 2 hours** |
| **2**    | Urgent   | - 4–5 symptoms<br>OR<br>Age ≥ 70 and ≥ 2 symptoms<br>AND<br>No chest pain<br>AND<br>Wait < 2h |
| **3**    | Normal   | - 2–3 symptoms<br>AND<br>Age < 70<br>AND<br>No chest pain<br>AND<br>Wait < 2h |
| **4**    | Low      | - 0–1 symptom<br>AND<br>No critical signs |

---

## Priority Calculation Code

```c
#include <time.h>
#include <stdio.h>
#include <string.h>

typedef struct Patient {
    char ssn[20];
    int age;
    char dateIn[11];     // "dd/mm/yyyy"
    char timeIn[6];      // "hh:mm"
    int symptoms[7];     // [Headache, Fever, ..., ChestPain]
    int priority;
} Patient;

// Parse date and time to struct tm
void parse_datetime(const char* dateStr, const char* timeStr, struct tm* result) {
    int d, m, y, h, min;
    sscanf(dateStr, "%d/%d/%d", &d, &m, &y);
    sscanf(timeStr, "%d:%d", &h, &min);
    result->tm_mday = d;
    result->tm_mon = m - 1;
    result->tm_year = y - 1900;
    result->tm_hour = h;
    result->tm_min = min;
    result->tm_sec = 0;
    result->tm_isdst = -1;
}

// Returns wait time in hours
double get_wait_hours(const char* dateStr, const char* timeStr) {
    struct tm tm_admit;
    parse_datetime(dateStr, timeStr, &tm_admit);
    time_t admit_time = mktime(&tm_admit);
    time_t now = time(NULL);
    return difftime(now, admit_time) / 3600.0;
}

// Count how many symptoms patient has
int count_symptoms(int symptoms[], int n) {
    int count = 0;
    for (int i = 0; i < n; i++) {
        count += symptoms[i];
    }
    return count;
}

// Priority assignment based on defined rules
int calculate_priority(Patient* p) {
    int symptomCount = count_symptoms(p->symptoms, 7);
    int hasChestPain = p->symptoms[6]; // 7th symptom = Chest Pain
    double waitedHours = get_wait_hours(p->dateIn, p->timeIn);

    if (hasChestPain || symptomCount >= 6 || (p->age >= 75 && symptomCount >= 4) || waitedHours >= 2.0) {
        return 1;
    } else if ((symptomCount >= 4) || (p->age >= 70 && symptomCount >= 2)) {
        return 2;
    } else if (symptomCount >= 2) {
        return 3;
    } else {
        return 4;
    }
}
