# Use Case: [Exam Scheduling Solution](https://www.unitime.org/exam_description.php)

## Objective

The goal is to compare the resources used in the execution of a classical program versus a quantum program for a university's exam scheduling system. The system must adhere to strict constraints imposed by the institution while taking into account the availability of students and professors (flexible constraints).

---

## Entities

### Examination Periods
The system must manage distinct periods, each defined by a specific date, start time, and duration. These periods can include preferences to simplify scheduling, such as avoiding exams on the last day of the exam week. For example, five two-hour periods could be scheduled per day for final exams, while midterm exams might be placed at the beginning or end of the day on specific dates.


### Rooms

Rooms are critical resources with attributes such as seating capacity (for normal or exam seating) and availability during specific periods. Each room can have preferences for certain periods, including:
- **Prohibited Periods**: The room cannot be used during this period.
- **Strongly Discouraged Periods**: The room should only be used if the exam explicitly prefers it.
- **Discouraged Periods**: The room is used only if no other suitable room is available.  
Room coordinates help calculate distances for scheduling, particularly when managing consecutive exams in different locations.

### Exams

Each exam has attributes such as its duration, seating requirements, and the maximum number of rooms it can be allocated to. Exams are also linked to enrolled students, instructors, and specific preferences for periods or rooms. These preferences may include required or prohibited periods and rooms. If a room is not needed (e.g., for online exams), the system will assign only a period.

### Distribution Preferences

Relationships between exams often influence their scheduling. For example, some exams may need to take place in the same room, while others must occur in different rooms or periods. Precedence constraints might require one exam to be scheduled before another. These distribution preferences help the system create a coherent and optimized timetable.

---

## Constraints

### Hard Constraints

The scheduling system must strictly adhere to several rules:
- Only one exam can take place in a room during a given period.
- Exams cannot be scheduled in rooms or periods that are unavailable.
- The seating capacity of assigned rooms must be sufficient for the number of students enrolled.
- Prohibited periods or rooms cannot be assigned to exams.
- Required distribution constraints must always be satisfied.

### Soft Constraints

While hard constraints ensure feasibility, soft constraints focus on optimizing the timetable:
- **Student Conflicts**: The system minimizes direct conflicts (students scheduled for overlapping exams), back-to-back conflicts, and situations where a student has three or more exams in a single day. It also considers the distances between rooms for consecutive exams.
- **Instructor Conflicts**: Similarly, instructors should avoid overlapping assignments, back-to-back exams, or excessive exams in one day. Room distances for consecutive assignments are also optimized.
- **Other Optimization Criteria**: The system reduces penalties for non-preferred periods or rooms, excessive room splits, oversized rooms, and unsatisfied distribution preferences. Large exams scheduled late in the examination period are also penalized to promote balanced scheduling.

---

### Data Overview
This [link](./data_convertion.md) offers a comprehensive overview of the dataset, detailing its structure, properties, and attributes. The second [link](./csv_overview.md) focuses on the dataset after its conversion into CSV format.
