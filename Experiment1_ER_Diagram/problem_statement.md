# Experiment 1: Entity-Relationship (ER) Diagram

## Objective:
To understand and apply the concepts of ER modeling by creating an ER diagram for a real-world application.

## Purpose:
The purpose of this workshop is to gain hands-on experience in designing ER diagrams that visually represent the structure of a database including entities, relationships, attributes, and constraints.

---

## Choose One Scenario:

### Scenario 1: University Database
Design a database to manage students, instructors, programs, courses, and student enrollments. Include prerequisites for courses.

**User Requirements:**
- Academic programs grouped under departments.
- Students have admission number, name, DOB, contact info.
- Instructors with staff number, contact info, etc.
- Courses have number, name, credits.
- Track course enrollments by students and enrollment date.
- Add support for prerequisites (some courses require others).

---

### Scenario 2: Hospital Database
Design a database for patient management, appointments, medical records, and billing.

**User Requirements:**
- Patient details including contact and insurance.
- Doctors and their departments, contact info, specialization.
- Appointments with reason, time, patient-doctor link.
- Medical records with treatments, diagnosis, test results.
- Billing and payment details for each appointment.

---

## Tasks:
1. Identify entities, relationships, and attributes.
2. Draw the ER diagram using any tool (draw.io, dbdiagram.io, hand-drawn and scanned).
3. Include:
   - Cardinality & participation constraints
   - Prerequisites for University OR Billing for Hospital
4. Explain:
   - Why you chose the entities and relationships.
   - How you modeled prerequisites or billing.

# ER Diagram Submission - Student Name

## Scenario Chosen:
University 

## ER Diagram:
<img width="800" height="740" alt="image" src="https://github.com/user-attachments/assets/40e8095c-ec62-425a-9614-63f9ba02eb9d" />


## Entities and Attributes:
| Entity                        | Attributes (PK, FK)                                                        | Notes                                                                                                  |
| ----------------------------- | -------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| **Department**                | **DNo (PK)**, DName, Location                                              | One department offers many courses; one department is headed by one instructor.                        |
| **Instructor**                | **INo (PK)**, IName, Designation, MobileNo, RoomNo, DNo (FK)               | Each instructor belongs to one department; teaches multiple courses.                                   |
| **Course**                    | **CNo (PK)**, CName, Duration, PreRequisite (FK referencing CNo), DNo (FK) | A course may have zero or one prerequisite; offered by one department; taught by multiple instructors. |
| **Student**                   | **SNo (PK)**, SName, Gender, DOB, Address, Email                           | Students enroll in many courses.                                                                       |
| **Enrollment**                | **SNo (FK)**, **CNo (FK)**, EnrollmentDate                                 | Composite relationship table for M:N between Student and Course.                                       |
| **DepartmentHead** (optional) | DNo (FK), INo (FK)                                                         | Represents the “headed by” relationship if you want it as table.                                       |



## Relationships and Constraints:
| Relationship                            | Cardinality       | Participation       | Notes                                                                           |
| --------------------------------------- | ----------------- | ------------------- | ------------------------------------------------------------------------------- |
| **Department – Instructor** (Has)       | 1 : N             | Total on Instructor | One department has many instructors; each instructor belongs to one department. |
| **Department – Course** (Offers)        | 1 : N             | Total on Course     | A department offers many courses.                                               |
| **Department – Instructor** (Headed By) | 1 : 1             | Partial             | One instructor heads a department; instructor may or may not be a head.         |
| **Instructor – Course** (Is taught by)  | M : N             | Partial             | Multiple instructors may teach a course; an instructor may teach many courses.  |
| **Course – Student** (Enrolled by)      | M : N             | Partial             | Students enroll in many courses; each course has many students.                 |
| **Course – Course** (Pre-requisite)     | 1 : N (Recursive) | Optional            | A course may require another course; some courses have no prerequisites.        |



## Extension (Prerequisite / Billing)

### Prerequisite Modeling
Prerequisites are handled using a **self-referencing relationship** inside the Course entity.

- A course may require another course.
- The `PreRequisite` attribute in Course stores a **foreign key referencing Course.CNo**.
- This allows simple and optional prerequisite tracking.

### Billing Modeling 
If billing is included, a separate Billing entity is used:

- BillID (PK), StudentID (FK), Amount, DueDate, Status
- One student can have many billing records (1:N)

---

## Design Choices

### 1. Choice of Entities
The database uses these main entities:

- Department
- Instructor
- Course
- Student
- Enrollment

These were chosen because they represent the core academic structure of a university.

---

### 2. Relationship Design

- **Department–Course (1:N):** One department can offer many courses.
- **Course–Instructor (M:N):** A course may be taught by multiple instructors.
- **Student–Course (M:N):** Students can enroll in multiple courses.
- **Department–Instructor (1:N):** A department has many instructors.
- **Course–Prerequisite (1:N Recursive):** A course may have one prerequisite.

---

### 3. Attribute Selection

Each entity includes a **primary key** for unique identification:

- SNo (Student)
- CNo (Course)
- INo (Instructor)
- DNo (Department)

Other attributes store descriptive and required information (name, email, designation, duration, etc.).

---

### 4. Assumptions

- A course may or may not have a prerequisite.
- A student can enroll in a course only once.
- A department may exist without courses.
- Not every instructor must be a department head.


## RESULT
The ER diagram for the University Database was created successfully. All required entities, relationships, and constraints were represented correctly according to the user requirements.
