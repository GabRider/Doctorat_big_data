
# Use Case: University Timetable Scheduling Solver

## Objective
Develop a scheduling solver to optimize university timetables by allocating classes to rooms, instructors, and times while minimizing conflicts and maximizing resource utilization. The solver must account for complex constraints, such as room capacities, student schedules, and instructor availability, while ensuring feasible travel distances for back-to-back classes.

---

## Key Features

### 1. Problem Decomposition
- **Large Lecture Rooms**:  
  Schedule large classes (800+ students) into 55 large lecture rooms, ensuring high room utilization (70%-97%) and accommodating shared resources between departments.
- **Departmental Scheduling**:  
  Handle independent scheduling for ~70 academic departments, each managing 10-500 classes, while addressing cross-departmental interactions.
- **Computer Labs**:  
  Allocate 450 smaller classes (20-45 students) into 36 computer labs, considering equipment availability and time preferences.

---

### 2. Standardized Time Patterns
- Classes must follow fixed time blocks (e.g., 1-hour x 3 days or 1.5-hour x 2 days).
- Ensure that all sessions of a course are taught in the same location and time slot whenever possible.

---

### 3. Constraints
#### Hard Constraints
- No overlapping classes for instructors, rooms, or students.
- Rooms must accommodate the number of enrolled students.
- Back-to-back classes for instructors must be within 200 meters.
- Students must be able to travel between back-to-back classes within 10 minutes (670 meters or less).

#### Soft Constraints
- Preferences for specific times, rooms, or equipment (e.g., computers).
- Minimize unused time slots in rooms that are too short for other classes.
- Avoid scheduling classes in rooms with more than 1/3 of seats unused.
- Balance time and room usage across departments.

---

### 4. Student Sectioning
- Divide students into class sections to minimize conflicts.
- Support re-sectioning after timetable generation to further reduce conflicts.
- Handle complex parent-child relationships between classes (e.g., lecture to lab).

---

## Functional Requirements

### 1. Solver Algorithm
- Implement a constraint-based optimization solver using techniques like constraint programming or mixed-integer programming.
- Incorporate dynamic adjustments to address inter-departmental conflicts.

### 2. User Features
- Allow departmental schedule managers to create, store, and compare multiple solutions.
- Enable centralized conflict resolution by committing solutions iteratively.

### 3. Metrics for Success
- Minimize student conflicts.
- Maximize room utilization.
- Ensure fair allocation of time and room preferences among departments.

---

## Example Workflow

### 1. Input
- Class data (enrollment, duration, instructor, room/equipment requirements).
- Room data (capacity, location, equipment).
- Instructor and student availability.
- Time patterns and departmental preferences.

### 2. Processing
- Decompose the problem into subproblems: large lecture rooms, departmental schedules, and computer labs.
- Solve each subproblem iteratively, ensuring compatibility with committed solutions.

### 3. Output
- Optimized timetable with assigned rooms, times, and instructors for each class.
- Detailed reports on student conflicts, room utilization, and soft constraint satisfaction.

---

This use case supports large-scale university scheduling, ensuring efficient resource allocation while adhering to diverse constraints and preferences.
