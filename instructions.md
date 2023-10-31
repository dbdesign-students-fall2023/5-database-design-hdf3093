# Data Normalization and Entity-Relationship Diagramming

## Original Table
The following table, representing students' grades in courses at a university, is already in [first normal form](https://knowledge.kitchen/A_Simple_Guide_to_Five_Normal_Forms_in_Relational_Database_Theory#FIRST_NORMAL_FORM) (1NF) - all records have the same number of fields, and there is only one value per field.

| assignment_id | student_id | due_date | professor | assignment_topic                | classroom | grade | relevant_reading    | professor_email   |
| :------------ | :--------- | :------- | :-------- | :------------------------------ | :-------- | :---- | :------------------ | :---------------- |
| 1             | 1          | 23.02.21 | Melvin    | Data normalization              | WWH 101   | 80    | Deumlich Chapter 3  | l.melvin@foo.edu  |
| 2             | 7          | 18.11.21 | Logston   | Single table queries            | 60FA 314  | 25    | Dümmlers Chapter 11 | e.logston@foo.edu |
| 1             | 4          | 23.02.21 | Melvin    | Data normalization              | WWH 101   | 75    | Deumlich Chapter 3  | l.melvin@foo.edu  |
| 5             | 2          | 05.05.21 | Logston   | Python and pandas               | 60FA 314  | 92    | Dümmlers Chapter 14 | e.logston@foo.edu |
| 4             | 2          | 04.07.21 | Nevarez   | Spreadsheet aggregate functions | WWH 201   | 65    | Zehnder Page 87     | i.nevarez@foo.edu |
| ...           | ...        | ...      | ...       | ...                             | ...       | ...   | ...                 | ...               |

## Assumptions

This data represents information about students' grades in courses at a university. The dependencies reflect the reality of how courses work in most universities.

- each course can be taught by multiple professors in different sections
- each professor might teach multiple sections of the same course
- each section meets in a specific classroom with a specific professor
- different sections of the same course may meet in different classrooms, even if the professor is the same
- professors give assignments, with specific due dates
- professors give readings that are relevant and helpful to a given assignment.
- a professor might give the same assignment to different sections of the same course, but with different due dates
- professors give readings to help with the assignments
- students complete assignments and receive a grade

Further assumptions:

- Assume there are many rows of data following this structure... only 5 sample rows have been shown for brevity. The number of rows is not important to this assignment.


### Convert to 4NF: Existing Issues

This data set has many issues. There are non-key fields that only correlate to a cetain aspect of the whole key. To articulate this better, take this for example: assignment_topic, relevant_reading, and due_date all depend on/are facts only about assignment_id, not student_id. In this way, it is not 2NF compliant.

Similarly, it is not 3NF compliant because there are non-key fields about other non-key fields. professor_email and classroom depend on professor, and are only facts about the professor with different emails and classrooms.


## Making the Data 4NF Compliant

I made separate tables with separate IDs and data in order to account for non-key fields, making sure that every non-key is a fact about the key field. I think it is logical to separate the tables in this way below. Based on the original table, I found it confusing that the assignment_id/due_date/classroom existed without a variable that accounted for the specific section of the class. Especially with the amount of assumptions regarding varying sections of the same course taught by one professor, various professors teaching many various sections, etc. This is clearly important information and it makes sense to add a variable to account for sections, hence the creation of section_id. In turn this will make this table 4NF compliant and more easy to understand.

| assignment_id | student_id | grade |
| :------------ | :--------- | :----- |
| ...          | ...        | ...   |

| section_id   | assignment_id | due_date |
| :------------ | :--------- | :----- |
| ...          | ...        | ...     |

| assignment_id   | assignment_topic | relevant_reading |
| :------------ | :--------- | :----- |
| ...          | ...        | ...               |

| section_id   | classroom   | professor      | professor_email |
| :------------ | :--------- | :-----         | :---------      |
| ...          | ...        | ...            | ...            |



This ensures that our data does not contain more than one multi-valued fact about an entity in the same table.



### Draw an Entity-Relationship Diagram

 My [diagram](./images/diagram.svg).



