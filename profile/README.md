## Hi there 👋
Membros da Equipe

Iago Gonçalves de Meira - Scrum Master

Fabio Roberto Pereira Filho - Product Owner

Theo Penteado Zepponi - Desenvolvedor

Gregory Vinicius Clariano Silva - Desenvolvedor

```mermaid

classDiagram

    class Subject {
        +int id
        +String code
        +String name
        +List~ChangeHistory~ history
        +getDetails() String
        +editData(newName: String, newCode: String) void
        +delete() void
        +archive() void
    }

    class Position {
        <<enumeration>>
        PROFESSOR
        AGENTE_1
        AGENTE_2
        FUNCIONARIO_TERCEIRIZADO
        PROFESSOR_PEDAGOGICO
    }

    class Status {
        <<enumeration>>
        ATIVO
        ARQUIVADO
    }

    class OutsourcedEmployee {
        +String business <<opcional>>
    }

    class Employee {
        +int id
        +String name <<obrigatório>>
        +String cpf <<obrigatório>>
        +String rg <<obrigatório>>
        +Date birthDate <<obrigatório>>
        +String email <<obrigatório>>
        +String telephone <<obrigatório>>
        +String birthCity
        +String birthState
        +Address address
        +List~HigherEducation~ graduation
        +List~Specialization~ specialization
        +List~FunctionalLine~ functionalLine
        +List~Subject~ subjects
        +Position position
        +int Workload
        +Status status
        +List~ChangeHistory~ history
        +tieSubject(subject: Subject) void
        +getDetails() String
        +editData() void
        +delete() void
        +archive() void
    }

    class Address {
        +int id
        +String road
        +String number
        +String neighborhood
        +String cep
        +String municipality
    }

    class Cell {
        <<abstract>>
        +int id
        +int row
        +int column
    }

    class Schedule {
        +int id
        +int row
        +Time scheduleStart
        +Time scheduleEnd
        +String type <<Intervalo ou Aula>>
        +validateConflicts() Boolean
    }

    class WeekDay {
        +int row <<Sempre 1ª linha>>
        +String dayName
    }

    class Recess {
        +int id
        +int row
        +String recess
    }

    class Lesson {
        +Linking lesson
        +String class
        +getContent() Lesson
        +setContent(content: Lesson) void
    }

    class Timetable {
        +int id
        +int numberRows
        +int numberColumns
        +List~Lesson~ lessons
        +List~Schedule~ schedules 
        +List~WeekDay~ days
        +List~Recess~ recess
        +String classFilter
        +getCell(line: int, column: int) Cell
        +addContentCell(cell: Cell) void
        +removeContentCell(cell: Cell) void
        +createColumn(numberColumn: int) void
        +removeColumn(numberColumn: int) void
        +createLine(numberLine: int) void
        +removeLine(numberLine: int) void
    }

    class HigherEducation {
        +int id
        +String name
        +String location
        +Date conclusionDate
    }

    class SpecializationType {
        <<enumeration>>
        POS_GRADUACAO
        MESTRADO
        DOUTORADO
    }

    class Specialization {
        +SpecializationType type
    }

    class Linking {
        +int teacherId
        +int subjectId
        +saveLinking(teacherId: int, subjectId: int) void
    }

    class FunctionalLine {
        +int id
        +int lineNumber
        +String tie
        +String subject
    }

    class ChangeHistory {
        +int id
        +Date date
        +String description
    }

    class ChangeHistorySubject {
        +Subject originalSubjectData
    }

    class ChangeHistoryEmployee {
        +Employee originalEmployeeData
    }

    Employee "1..*" --> "1" Address
    Employee "1" --> "0..2" FunctionalLine
    Employee "1..*" --> "0..*" HigherEducation
    Specialization --> SpecializationType : tipo
    Specialization --|> HigherEducation : herda
    Specialization --> Employee
    Employee --> Status
    Employee --> Position : cargos
    Employee --> OutsourcedEmployee : "se cargo == FUNCIONARIO_TERCEIRIZADO"

    Subject "1" --> "0..*" ChangeHistorySubject : registra
    Employee "1" --> "0..*" ChangeHistoryEmployee : registra

    Employee "1" --> "0..*" Linking
    Subject "1" --> "0..*" Linking

    Linking --> Lesson

    Timetable "1" --> "0..*" Lesson
    Timetable "1" --> "0..*" Schedule
    Timetable "1" --> "0..*" WeekDay
    Timetable "1" --> "0..*" Recess

    Lesson --|> Cell
    WeekDay --|> Cell

    ChangeHistory <|-- ChangeHistorySubject
    ChangeHistory <|-- ChangeHistoryEmployee


