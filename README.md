# CSE321 – Database Systems: Lab Session Reports

## Overview

This repository contains the individual lab session reports for **CSE321 Database Systems** at UNIST. 
Each lab session involved an in-class design exercise followed by a written reflective report. The reports are not simply presentations of correct answers they document the original in-class work honestly, analyse the mistakes made, explain the reasoning behind them, and present a fully corrected design with step-by-step justification.

---

## Lab Session 1 — ER Diagram Design

**Topic:** Entity-Relationship (ER) Modelling in Chen's Notation

The in-class task required constructing an ER diagram from a written specification describing the data requirements of a university system. 
The system covers four groups of people (professors, staff, students, and stakeholders), two types of staff (tech staff and admin staff, with admin managers as a further subtype), two types of students (undergraduate and graduate), a laboratory system with equipment, courses, supervision relationships, and stakeholder comments.

**What the report covers:**

The report presents the original diagram submitted during the lab session and reconstructs the reasoning behind each design decision at the time, including the assumptions explicitly noted on the submitted paper. It then identifies six revisions made after carefully re-reading the specification:

1. **Removal of the UNIVERSITY entity; introduction of a PERSON supertype** — The University entity was drawn from a misreading of motivational language in the specification as a modelling instruction. The shared person attributes (Person_ID, Name, Address, Email, Phone, Schools, Nation, Zipcode) were incorrectly placed on Stakeholders only, rather than factored into a shared supertype accessible to all four person groups.

2. **Removal of the RESEARCH GRADUATE subtype** — A three-level student ISA hierarchy was drawn, with a Research Graduate subclass below Graduate. The assumption was that "graduates who do research" described a qualifying subset. The revision shows this was over-modelling: a subtype is only justified when it carries unique attributes or unique relationships, and all graduate students in this system are supervised and assigned to research laboratories.

3. **Removal of DAILY OPERATIONS as an entity** — Daily Operations was drawn as a separate entity connected to Staff via a manage relationship. The revision identifies this as a descriptive phrase in the specification, not a data object — it has no key, no attributes, and no independent relationships.

4. **Domain modelled as an attribute, not an entity** — Domain was drawn as a separate entity with a belong to relationship. The revision corrects this to a simple single-valued attribute of Stakeholder.

5. **Addition of missing total participation constraints** — No double lines appeared anywhere in the original diagram. The revision adds total participation on the Graduate side of the Supervised_By and Assigned_To relationships, and on the Comment side of the Provides relationship.

6. **Addition of the missing Hires relationship** — The specified 1:N relationship between Professor and Admin Manager was entirely absent from the original diagram and is restored in the revision.

---

## Lab Session 2 — Relational Schema Design and Normalisation

**Topic:** Relational Schema Translation and Normal Form Analysis (1NF, 2NF, 3NF, BCNF)

The in-class task required converting the same university ER diagram into a relational database schema. For each relation, the task involved specifying attributes, identifying candidate keys and primary keys, stating functional dependencies (FDs), and verifying whether the relation satisfies Third Normal Form (3NF).

**What the report covers:**

The report presents the exact schema submitted during the lab session — eight relations in total — and reconstructs the reasoning and assumptions behind each design decision. It then presents a fully corrected schema of seventeen relations, with precise keys, FDs, foreign keys, and a normal form analysis (3NF and BCNF) for every relation. Ten revisions are documented:

1. **Superkeys listed as candidate keys across all relations** — Non-minimal sets such as {Person_ID, Name} were listed as candidate keys throughout. Since candidate keys must be minimal, these were all superkeys. This error undermined the validity of every subsequent 3NF analysis.

2. **ISA hierarchies collapsed into a Type attribute** — Rather than creating separate subtype relations, the ISA distinctions (Tech_Staff vs. Admin_Staff, Undergrad vs. Graduate) were collapsed into a single Type attribute, losing all subtype-specific attributes and relationships.

3. **Multi-valued attributes violating 1NF** — `Course_list` was stored as an attribute of Student, and `teaching_courses` and `supervising_topics` were stored as attributes of Professor. All three are non-atomic, violating First Normal Form and making the 3NF conclusions for those relations invalid at their foundation.

4. **Equipment_ID placed inside Laboratory** — A foreign key was placed on the wrong side of a 1:M relationship. Since one laboratory has many pieces of equipment, the reference must live in the Equipment relation, not in Laboratory.

5. **Comments_Suggestion embedded in Stakeholder** — The Comments_Suggestion weak entity was collapsed into the Stakeholder relation as plain attributes, implying a one-to-one relationship between stakeholders and comments and contradicting the 1:M cardinality in the ER diagram.

6. **Missing junction tables for M:N relationships** — The Student–Course (Takes) and Undergrad–Teaching_Laboratory (Uses) many-to-many relationships were not translated into separate relations.

7. **Equipment incorrectly flagged as not in 3NF** — Given the FDs stated in the original submission, Equipment was actually in BCNF. The revision corrects this with an explicit partial- and transitive-dependency analysis.

8. **Laboratory's DeptNum dependency and decomposition** — Under the assumption that Dept_Name → DeptNum, the Laboratory relation violates 3NF (and BCNF): a non-prime attribute is determined by a non-superkey. The fix decomposes Laboratory into two relations: `Department(Dept_Name, DeptNum)` and `Laboratory(Lab_Name, Dept_Name, Capacity, Location, Lab_Type)`.

9. **Missing Person_ID foreign keys in subtype relations** — The ISA connection back to the Person supertype was missing from all subtype relations, making it impossible to join on shared person attributes.

10. **Unjustified Dept_Name attribute in Professor** — An attribute not present in the ER diagram was added to Professor without a stated assumption or justification.

---

## Repository Structure

```
CSE321-Lab-Session/
│
├── README.md
│
├── CSE321-lab-session1-manual.pdf
│
└── CSE321-lab-session2-manual.pdf
│
├── CSE321_Lab1_report.pdf
│
└── CSE321_Lab2_report.pdf

```

---

## Course Information

| Field | Detail |
|---|---|
| Course | CSE321 — Database Systems |
| Institution | UNIST (Ulsan National Institute of Science and Technology) |
| Semester | Spring 2026 |
| Student | Aliona Pirozhenko |
| Student ID | 20232008 |
