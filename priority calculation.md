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

#include <time.h>
#include <string.h>
#include <stdio.h>

// Define constants
#define SYMPTOM_COUNT 10

// Symptom indices
enum Symptom {
    ANAPHYLAXIS = 0,
    SEIZURE,
    UNCONSCIOUSNESS,
    FRACTURES,
    BROKEN_BONE,
    ALLERGIC_REACTION,
    FEVER,
    COUGH,
    NAUSEA,
    HEADACHE
};

// Patient struct
typedef struct Patient {
    char ssn[20];
    int age;
    char dateIn[11];     // dd/mm/yyyy
    char timeIn[6];      // hh:mm
    int symptoms[SYMPTOM_COUNT];
    int priority;
    struct Patient* next;
} Patient;

// Utility function to count symptoms (if needed later)
int count_symptoms(int symptoms[], int n) {
    int count = 0;
    for (int i = 0; i < n; i++) {
        count += symptoms[i];
    }
    return count;
}

// Assign priority based on 3-level severity
int calculate_priority_by_symptoms(Patient* p) {
    // Priority 1: Life-threatening or severe trauma
    if (p->symptoms[ANAPHYLAXIS] ||
        p->symptoms[SEIZURE] ||
        p->symptoms[UNCONSCIOUSNESS] ||
        p->symptoms[FRACTURES] ||
        p->symptoms[BROKEN_BONE]) {
        return 1;
    }

    // Priority 2: Moderate symptoms or allergic reaction
    if (p->symptoms[ALLERGIC_REACTION] ||
        p->symptoms[FEVER] ||
        p->symptoms[COUGH] ||
        p->symptoms[NAUSEA]) {
        return 2;
    }

    // Priority 3: Mild symptoms or no symptoms
    return 3;
}

// Optional: DateTime parser and wait time calculator
void parse_datetime(const char* dateStr, const char* timeStr, struct tm* result) {
    int day, month, year, hour, minute;
    sscanf(dateStr, "%d/%d/%d", &day, &month, &year);
    sscanf(timeStr, "%d:%d", &hour, &minute);

    result->tm_mday = day;
    result->tm_mon = month - 1;
    result->tm_year = year - 1900;
    result->tm_hour = hour;
    result->tm_min = minute;
    result->tm_sec = 0;
    result->tm_isdst = -1;
}

double get_wait_hours(const char* dateStr, const char* timeStr) {
    struct tm admit_tm;
    parse_datetime(dateStr, timeStr, &admit_tm);
    time_t admit_time = mktime(&admit_tm);
    time_t now = time(NULL);
    return difftime(now, admit_time) / 3600.0;
}

