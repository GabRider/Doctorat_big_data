# Dataset
The data used for this project comes from [UniTime](https://www.unitime.org/exam_datasets.php), an open-source application dedicated to generating exam schedules. The dataset selected for this work, _`pu-exam-fal12`_, is an XML file containing information about a week of exams at Purdue University during the fall of 2012. 

To  understand correctly  the data, this XML file will be convert into multiple CSV files.
- Parameters
- Periods
- Rooms
- Exams
- Students
- Instructors
- Constraints



```python
import xml.etree.ElementTree as ET
import csv
def retrieve_xml_root(xml_file):
    return ET.parse(xml_file).getroot()
```

# Parameters
Essential constants used inside the program

| Property Name                                | Value       |
|---------------------------------------------|-------------|
| backToBackConflictWeight                    | 10.0        |
| backToBackDistance                          | -1.0        |
| directConflictWeight                        | 1000.0      |
| distanceBackToBackConflictWeight            | 25.0        |
| distanceToStronglyPreferredRoomWeight       | 1.0E-4      |
| distributionWeight                          | 1.0         |
| examRotationWeight                          | 1.0E-4      |
| instructorBackToBackConflictWeight          | 0.0         |
| instructorDirectConflictWeight              | 10.0        |
| instructorDistanceBackToBackConflictWeight  | 0.0         |
| instructorMoreThanTwoADayWeight             | 0.0         |
| isDayBreakBackToBack                        | false       |
| largePeriod                                 | 0.86        |
| largeSize                                   | 600         |
| largeWeight                                 | 2500000.0   |
| maxRooms                                    | 4           |
| moreThanTwoADayWeight                       | 100.0       |
| mpp                                         | false       |
| periodIndexWeight                           | 1.0E-7      |
| periodSizeWeight                            | 0.5         |
| periodWeight                                | 1.0         |
| perturbationWeight                          | 0.0010      |
| roomPerturbationWeight                      | 0.1         |
| roomSizeFactor                              | 1.1         |
| roomSizeWeight                              | 0.0010      |
| roomSplitDistanceWeight                     | 0.01        |
| roomSplitWeight                             | 10.0        |
| roomWeight                                  | 1.0         |



```python
xml_file = "./raw_data/parameters.xml"  
csv_file = "./csv_data/parameters.csv"  

root = retrieve_xml_root(xml_file)
with open(csv_file, mode='w', newline='', encoding='utf-8') as file:
    writer = csv.writer(file)

    # Write the CSV header
    writer.writerow(['Property Name', 'Value'])

    # Iterate over each property in the XML
    for property in root.findall('property'):
        name = property.get('name')
        value = property.get('value')
        writer.writerow([name, value])

```

# Periods
 ```XML
<periods>
    <period id="1" length="120" day="07/12/10" time="8:00am-10:00am" penalty="0"/>
    <period id="2" length="120" day="07/12/10" time="10:20am-12:20pm" penalty="0"/>
    <period id="3" length="120" day="07/12/10" time="1:00pm- 3:00pm" penalty="0"/>
    <period id="4" length="120" day="07/12/10" time="3:20pm- 5:20pm" penalty="0"/>
    <period id="5" length="120" day="07/12/10" time="7:00pm- 9:00pm" penalty="0"/>
    <period id="6" length="120" day="07/12/11" time="8:00am-10:00am" penalty="0"/>
    <period id="7" length="120" day="07/12/11" time="10:20am-12:20pm" penalty="2"/>
    ...
</periods>
```
- **period**: definition of an examination period
- **id**	period unique id
- **length**	period length (in minutes)
- **day**	period date (string)
- **time**	period time (string)
- **penalty**	penalization of using this period unless changed on an exam, if an exam is assigned to a period with non-zero penalty, this penalty is added to the overall period penalty of the solution


```python
xml_file = "./raw_data/periods.xml"  
csv_file = "./csv_data/periods.csv"  
root = retrieve_xml_root(xml_file)
with open(csv_file, mode='w', newline='', encoding='utf-8') as file:
    writer = csv.writer(file)

    # Write the CSV header
    writer.writerow(["Period ID", "Length (minutes)", "Day", "Time", "Penalty"])

    # Loop through each period in the XML
    for period in root.findall('period'):
        period_id = period.get('id', 'N/A')
        length = period.get('length', 'N/A')
        day = period.get('day', 'N/A')
        time = period.get('time', 'N/A').replace('a',"AM").replace('p',"PM")
        penalty = period.get('penalty', '0')  # Default penalty is 0
        writer.writerow([period_id, length, day, time, penalty])

```

# Rooms

 ```XML
<rooms>
    <room id="1" size="58" alt="32" coordinates="447,392"/>
    <room id="2" size="50" alt="25" coordinates="447,392"/>
    <room id="3" size="60" alt="30" coordinates="450,463"
        <period id="3" available="false"/>
        <period id="5" penalty="2"/>
    </room>
    ...
</rooms>
 ```
- **room**: definition of an examination room
    - **id :**	room unique id
    - **size :**	room size (number of seats in the room)
    - **alt :**	alternative room size (number of seats in the room if an exam requests alternate seating)
    - **coordinates :**	room location coordinates X,Y
Distance between rooms in meters is: $10 * ((x_2-x_1)^2 + (y_2-y_1)^2)^{1/2}$
Location attribute might not be present (e.g., for non-campus locations), distance between such a room and any other room are considered as infinite in such a case

Room period preferences:
- **period :**	definition of a period preference of a room
    - **id :**	period unique id
    - **available :**	if false, room is not available during this period (default is true) penalty	penalization of using the room during this period (default is zero)

Room can be unavailable for certain periods. Unless changed on an exam, if an exam is assigned to a room in a period with non-zero penalty, this penalty is added to the overall room penalty of the solution.


```python
xml_file = "./raw_data/rooms.xml"  
csv_file = "./csv_data/rooms.csv"  
root = retrieve_xml_root(xml_file)
with open(csv_file, mode='w', newline='', encoding='utf-8') as file:
   writer = csv.writer(file)

# Write the CSV header
   writer.writerow(["Room ID", "Size", "Alt Size", "Coordinates", "Period ID", "Available", "Penalty"])

   # Loop through each room in the XML
   for room in root.findall('room'):
      room_id = room.get('id', 'N/A')
      size = room.get('size', 'N/A')
      alt_size = room.get('alt', 'N/A')
      coordinates = room.get('coordinates', 'N/A')

      # Process periods associated with the room
      periods = room.findall('period')
      tmp=[]
      if periods:
  
         for period in periods:
            period_id = int(period.get('id', 'N/A'))
            tmp.append(period_id)
            available = period.get('available', True)  # Default is true
            penalty = period.get('penalty', 0)  # Default is 0
            writer.writerow([int(room_id), int(size), int(alt_size), coordinates, int(period_id), available, int(penalty)])
         # Write room info if no periods are defined
      for i in range(29):
          if i+1 in tmp :
              continue
          writer.writerow([int(room_id), int(size), int(alt_size), coordinates, i+1, True, 0])
```

# Exams

 ```XML
<exams>
    <!--Begin object Exam-->
    <exam id="1" length="120" alt="true" minSize="10" maxRooms="2" average="14">
        <!--Begin listing the periods during the exam can be scheduled -->
        <period id="1"/>
        <period id="3" penalty="2"/>
        <period id="4" penalty="-2"/>
        ...
        <!--End listing periods-->
        <!--Begin listing of avaiable rooms for the exam -->
        <room id="1"/>
        <room id="5" penalty="2"/>
        <room id="12" penalty="-1"/>
        ...
        <!--End listing rooms-->
         <!--This object appear only when a solution is found-->
        <assignment>
            <period id="1"/>
            <room id="5"/>
            <room id="12"/>
        </assignment>
        <!--End assignement-->
    </exam>
    <!--End object Exam-->
    <exam id="2" length="60" alt="false">
        <period id="2"/>
        <period id="4" penalty="2"/>
        ...
        <room id="1"/>
        <room id="4"/>
        ...
        <assignment>
            <period id="4"/>
            <room id="4"/>
        </assignment>
    </exam>
    <exam id="3" length="60" alt="false" maxRooms="0">
        <period id="2"/>
        <period id="5" penalty="2"/>
        ...
        <assignment>
            <period id="2"/>
        </assignment>
    </exam>
    ...
</exams>
 ```

- **exam :** definition of an examination
    - **id :**	examination unique id
    - **length :**	length (in minutes)
    - **alt :**	alternate seating requested
    - **minSize :**	minimal size of the requested room (zero if not present)
    - **maxRooms :**	maximal number of rooms (4 if not present)
    - **average :**	index of average period (for rotation penalty)
  
List of available periods and rooms is included in the exam element. If an exam is assigned to period and room(s), assignment element is also present. The following hard constraint must be met

- Only one exam can be placed in a room at any period.
- Only available periods and rooms may be used (periods/rooms listed under the exam).
- A room cannot be used on periods during which it is not available.
- An exam must be placed in a room (or a set of rooms) so that the overall seating capacity of the rooms equals or is greater than the number of students attending the exam, with respect to the requested seating type (e.g., each room has a normal seating (size attribute) and alternate seating capacities defined (alt attribute), an exam can request either normal (alt=false) or alternate seating (alt=true)). Maximal number of rooms into which an exam can be split (maxRooms attribute) cannot be exceeded as well.
- If maximal number of rooms is set to zero, an exam is to be placed only in a period (without any room assignment).
- Required distribution constraints must be satisfied(see bellow).


```python
xml_file = "./raw_data/exams.xml"  
csv_file = "./csv_data/exams.csv"  
csv_headers = [
    "ExamID", "Length", "AltSeating", "MinSize", "MaxRooms", "Average",
    "PeriodID", "PeriodPenalty", "RoomID", "RoomPenalty", "AssignedPeriod", "AssignedRooms"
]
root = retrieve_xml_root(xml_file)
with open(csv_file, mode="w", newline="", encoding="utf-8") as file:
    writer = csv.writer(file)
    writer.writerow(csv_headers)

    # Process each exam
    for exam in root.findall("exam"):
        exam_id = exam.get("id")
        length = exam.get("length")
        alt_seating = exam.get("alt")
        min_size = exam.get("minSize", "0")
        max_rooms = exam.get("maxRooms", "4")
        average = exam.get("average", "")

        # Extract periods
        for period in exam.findall("period"):
            period_id = period.get("id")
            period_penalty = period.get("penalty", "0")

            # Extract rooms
            for room in exam.findall("room"):
                room_id = room.get("id")
                room_penalty = room.get("penalty", "0")

                # Extract assignment details
                assignment = exam.find("assignment")
                assigned_period = ""
                assigned_rooms = ""
                if assignment is not None:
                    assigned_period = assignment.find("period").get("id", "") if assignment.find("period") is not None else ""
                    assigned_rooms = ",".join([r.get("id") for r in assignment.findall("room")])

                # Write data to the CSV file
                writer.writerow([
                    exam_id, length, alt_seating, min_size, max_rooms, average,
                    period_id, period_penalty, room_id, room_penalty, assigned_period, assigned_rooms
                ])

```

# Students
 ```XML
<students>
    <student id="1">
        <exam id="2048"/>
        <exam id="1976"/>
        <exam id="1978"/>
        <period id="3" available="false"/>
    </student>
    <student id="2">
        <exam id="389"/>
        <exam id="1924"/>
        <exam id="2103"/>
    </student>
    ...
</students>
```
Each student has a unique id and a list of examinations into which he/she is enrolled.
A student might be not available at some periods (direct conflict occurs when an exam with such a student is placed into such a period)


```python
xml_file = "./raw_data/students.xml"  
csv_file = "./csv_data/students.csv"  

root = retrieve_xml_root(xml_file)
with open(csv_file, 'w', newline='', encoding='utf-8') as file:
    writer = csv.writer(file)

    # Write the header
    writer.writerow(['Student ID', 'Exam IDs'])

    # Parse and write data
    for student in root.findall('student'):
        student_id = student.get('id')
        for exam in student.findall('exam'):
            writer.writerow([student_id, int(exam.get('id'))])

```

# Instructors
```XML
<instructors>
    <instructor id="1">
        <exam id="123"/>
        <exam id="235"/>
        <exam id="1235"/>
    </instructor>
    <instructor id="2">
        <exam id="389"/>
        <exam id="1234"/>
        <period id="7" available="false"/>
    </instructor>
    ... 
</instructors> 
```
Each instructor has a unique id and a list of examinations into which he/she is enrolled.
An instructor might be not available at some periods (direct conflict occurs when an exam with such a student is placed into such a period) 


```python
xml_file = "./raw_data/instructors.xml"  
csv_file = "./csv_data/instructors.csv"  

root = retrieve_xml_root(xml_file)
with open(csv_file, 'w', newline='', encoding='utf-8') as file:
    writer = csv.writer(file)

    # Write the header
    writer.writerow(['Instructor ID', 'Exam IDs', 'Unavailable Periods'])

    # Parse and write data
    for instructor in root.findall('instructor'):
        instructor_id = instructor.get('id')
        exams = [exam.get('id') for exam in instructor.findall('exam')]
        unavailable_periods = [
            period.get('id') for period in instructor.findall('period') if period.get('available') == 'false'
        ]
        writer.writerow([instructor_id, ', '.join(exams), ','.join(unavailable_periods)])

```

# Constraints
 ```XML
<constraints>
    <different-period id="1">
        <exam id="123"/>
        <exam id="345"/>
        <exam id="2334"/>
    </different-period>
    <different-room id="2" hard="false" weight="2">
        <exam id="12"/>
        <exam id="157"/>
    </different-room>
    ...
</constraints>
 ```
Distribution constraints are constraints that take place between two or more exams. Each instructor has a unique id and a list of examinations into which he/she is enrolled.
Soft constraints have hard attribute set to false (default is true), and have a weight defined. This weight is added to overall distribution penalty of a solution if the constraint is not satisfied.

Following constraints are defined:
- **same-room** ... exams are required/preferred to take place in the same room or rooms.
- **different-room** ... exams are required/preferred to take place in different rooms (no two - exams of the constraint are to be placed in the same room).
- **same-period** ... exams are required/preferred to take place during the same periods.
- **different-period** ... exams are required/preferred to take place during different period.
- **precedence** ... exams are required/preferred to take place in the order they are listed in the constraint. For instance if exam A is in the precedence constraint before exam B, it is to be placed in a period that precedes the period in which exam B is placed.


```python
xml_file = "./raw_data/constraints.xml"  
csv_file = "./csv_data/constraints.csv"  

root = retrieve_xml_root(xml_file)
csv_headers = ['Constraint Type', 'Constraint ID', 'Hard', 'Weight', 'Exam IDs']

# Open the CSV file for writing
with open(csv_file, 'w', newline='', encoding='utf-8') as file:
    writer = csv.writer(file)
    writer.writerow(csv_headers)

    # Iterate over all constraint types
    for constraint in root:
        constraint_type = constraint.tag  # e.g., "different-period", "same-room"
        constraint_id = constraint.get('id')
        hard = constraint.get('hard', 'true')  # Default is true
        weight = constraint.get('weight', '0')  # Default is 0

        # Collect all exam IDs in the constraint
        exam_ids = [exam.get('id') for exam in constraint.findall('exam')]

        # Write the row to the CSV file
        writer.writerow([constraint_type, constraint_id, hard, weight, ','.join(exam_ids)])

```


```python

```
