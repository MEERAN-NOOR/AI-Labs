# Lab 01: Knowledge-Based Systems 

## Exercise 1: Degree Honors Rule

### Objective
Introduce an additional rule into `UNIVERSITY_KB["rules"]` to identify students who qualify for special degree honors.

### Eligibility Criteria
- Minimum CGPA of 3.70.
- Zero disciplinary warnings.

### Expected Result
Graduated with Magna Cum Laude Honors.

---
## Solution
### New Rule Entry Into KB

```python
{
    "id": "RULE_07",
    "conditions": {
        "cgpa_min": 3.70,
        "disciplinary_warnings": 0
    },
    "condition_type": "degree_honors",
    "grade": "A+",
    "status": "Graduated with Magna Cum Laude Honors",
    "action": "Eligible for Degree Honors."
}
```
We add the above new rule to Knowledge Base to achieve the first exercise

---
---
## Exercise 2: Tracing Forward Chaining
### Objective
Modify the `evaluate_student()` function to print each evaluated rule condition as the loop runs, demonstrating step-by-step logical deduction.
### Expected Result
Display the evaluated rule conditions and show whether each rule matches (TRUE/FALSE), along with the final student grade and academic status.

---
## Solution
### Update Inference Engine

```python

def evaluate_student(student_record, kb):
    """
    Evaluates student records against university rules.
    Prints each evaluated condition to demonstrate forward chaining.
    """

    print(f"\n--- EVALUATING STUDENT: {student_record['student_name']} (ID: {student_record['student_id']}) ---")

    # Step 1: Check attendance rule
    print("\nChecking RULE_01: Attendance Condition")

    if student_record["attendance"] < 75:
        print(f"  Attendance = {student_record['attendance']}%")
        print("  Condition: Attendance < 75%")
        print("  Result: TRUE")

        attendance_rule = next(r for r in kb["rules"] if r["condition_type"] == "attendance_below")

        print("  RULE_01 MATCHED!")
        return {
            "rule_id": attendance_rule["id"],
            "grade": attendance_rule["grade"],
            "status": attendance_rule["status"],
            "action": attendance_rule["action"]
        }

    print(f"  Attendance = {student_record['attendance']}%")
    print("  Condition: Attendance < 75%")
    print("  Result: FALSE")
    print("  Student is eligible for marks evaluation.")

    # Step 2: Check marks range rules
    marks = student_record["total_marks"]

    for rule in kb["rules"]:
        if rule["condition_type"] == "marks_range":

            minimum = rule["conditions"]["marks_min"]
            maximum = rule["conditions"]["marks_max"]

            print(f"\nChecking {rule['id']}: Marks Range")
            print(f"  Condition: {minimum} <= {marks} <= {maximum}")

            if minimum <= marks <= maximum:
                print("  Result: TRUE")
                print(f"  {rule['id']} MATCHED!")

                return {
                    "rule_id": rule["id"],
                    "grade": rule["grade"],
                    "status": rule["status"],
                    "action": rule["action"]
                }

            print("  Result: FALSE")

    # Step 3: No matching rule
    print("\nNo matching rule found!")

    return {
        "rule_id": "UNKNOWN",
        "grade": "N/A",
        "status": "Invalid Score Entry",
        "action": "Verify submitted marks with the exam department."
    }
```
by updating the previous code with the code above, we achieve the desired result for 2nd exercise.

### Sample Execution:

```text
--- EVALUATING STUDENT: Ali Khan (ID: BSCS-2024-012) ---

Checking RULE_01: Attendance Condition
  Attendance = 92%
  Condition: Attendance < 75%
  Result: FALSE
  Student is eligible for marks evaluation.

Checking RULE_02: Marks Range
  Condition: 85 <= 88 <= 100
  Result: TRUE
  RULE_02 MATCHED!
Grade: A | Status: Passed with Distinction
Advisory Action: Recommended for Merit Scholarship.
```
**Outcome:** The inference engine successfully demonstrates Forward Chaining by evaluating rules step by step, identifying the matching condition, and generating the student's final grade, academic status, and recommended action.

---

## Summary

Both practical exercises were successfully completed:
- Exercise 1: Added a new degree honors rule to the Knowledge Base to determine student eligibility for special academic honors.
- Exercise 2: Updated the Inference Engine to show the evaluation of rules and illustrate the Forward Chaining process step by step.
