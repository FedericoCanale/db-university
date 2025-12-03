# DB_Università

## Entità: 
- departments
- degree_courses
- courses
- teachers
- course_teacher (tabella pivot: molti-a-molti tra courses e teachers)
- students
- exam_calls
- exam_registrations (tabella pivot: molti-a-molti tra students e exam_calls)


## Departments (Dipartimenti):
- id PK BIGINT UNSIGNED AI
- name VARCHAR(100)
- code VARCHAR(10)
- created_at DATETIME DEFAULT (NOW())
- updated_at DATETIME DEFAULT (NOW())
[1 Dipartimento → molti Corsi di Laurea], (uno a molti con Degree_Courses)

## Degree_Courses (Corsi di Laurea):
- id PK BIGINT UNSIGNED AI
- department_id BIGINT UNSIGNED FK
- name VARCHAR(100)
- code VARCHAR(10)
- level VARCHAR(20)      – es. “Triennale”, “Magistrale”
- created_at DATETIME DEFAULT (NOW())
- updated_at DATETIME DEFAULT (NOW())
[1 Corso di Laurea → molti Corsi; 1 Dipartimento → molti Corsi di Laurea], (molti a uno con departments, uno a molti con courses e uno a molti con students)

## Courses (Corsi):
- id PK BIGINT UNSIGNED AI
- degree_course_id BIGINT UNSIGNED FK
- name VARCHAR(100)
- code VARCHAR(20)
- credits TINYINT        
- year TINYINT           
- created_at DATETIME DEFAULT (NOW())
- updated_at DATETIME DEFAULT (NOW())
[1 Corso di Laurea → molti Courses; ogni Course ha molti Teachers via tabella pivot (course_teachers)], (molti a uno con degree_courses, uno a molti con con exam calls, molti a molti con teachers tramite Course_teachers)

## Teachers (Insegnanti):
- id PK BIGINT UNSIGNED AI
- name VARCHAR(50)
- lastname VARCHAR(50)
- email VARCHAR(100)
- office VARCHAR(100) NULL
[1 Teacher può tenere molti Courses, e 1 Course può avere molti Teachers → many-to-many], (molti a molti con con courses tramite course_teachers)

## Course_Teacher (PIVOT Course_Teacher):
- id PK BIGINT UNSIGNED AI
- course_id BIGINT UNSIGNED FK
- teacher_id BIGINT UNSIGNED FK
(Tabella ponte many-to-many tra courses e teachers)

## Students (Studenti):
- id PK BIGINT UNSIGNED AI
- name VARCHAR(50)
- lastname VARCHAR(50)
- matricola VARCHAR(20)
- degree_course_id BIGINT UNSIGNED FK
- enrollment_year SMALLINT
- date_of_birth DATE NULL
[1 Degree_Course → molti Students; ogni Student può iscriversi a molti appelli], (molti a uno con con degree_courses, molti a molti con exam_calls tramite exam_registration)

## Exam_Calls (Appelli d’esame):
- id PK BIGINT UNSIGNED AI
- course_id BIGINT UNSIGNED FK
- exam_date DATETIME
- session VARCHAR(20) NULL
- classroom VARCHAR(50) NULL
[1 Course → molti Exam_Calls], (molti a uno con courses, uno a molti con exam_registrations, molti a molti con students tramite Exam_registration)

## Exam_Registrations (PIVOT Studenti–Appelli):
- id PK BIGINT UNSIGNED AI
- exam_call_id BIGINT UNSIGNED FK
- student_id BIGINT UNSIGNED FK
- grade TINYINT NULL            
- passed BOOLEAN DEFAULT 0 (Booleano 0 = non passato, 1 = passato)     
- created_at DATETIME DEFAULT (NOW())
(Tabella ponte many-to-many tra students e exam_calls)

