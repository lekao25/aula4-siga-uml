classDiagram
    %% Etapa 1 e 2: Entidades, visibilidade e Herança
    class Pessoa {
        #String nome
        #String email
    }

    class Aluno {
        -String matricula
        -double media
        +getMatricula() String
    }

    class Professor {
        -String titulacao
        +lecionar() void
    }

    Pessoa <|-- Aluno : herda
    Pessoa <|-- Professor : herda

    %% Etapa 1: Disciplina e Matricula
    class Disciplina {
        -String nome
        -int cargaHoraria
    }

    class Matricula {
        -double valorBase
        +calcularMensalidade() double
        +salvar() void
    }

    %% Etapa 3: Turma contendo Aluno (Agregação com multiplicidade)
    class Turma {
        -String codigo
        -List~Aluno~ alunos
        +adicionarAluno(Aluno) void
    }
    Turma o--"1..*" Aluno : contem

    %% Relacionamentos adicionais básicos do domínio
    Matricula --> Aluno : pertence a
    Turma --> Disciplina : referente a
    Turma --> Professor : ministrada por

    %% Etapa 4: Interface Desconto e implementações (Realização)
    class Desconto {
        <<interface>>
        +aplicar(double valorBase) double
    }

    class DescontoBolsista {
        +aplicar(double valorBase) double
    }

    class DescontoConvenio {
        +aplicar(double valorBase) double
    }

    Desconto <|.. DescontoBolsista : realiza
    Desconto <|.. DescontoConvenio : realiza
    Matricula --> Desconto : usa



    sequenceDiagram
    actor Cliente
    participant M as :Matricula
    participant D as :Desconto (Bolsista)

    Cliente->>M: calcularMensalidade()
    activate M
    M->>D: aplicar(valorBase)
    activate D
    D-->>M: valorCalculado (ex: 50%)
    deactivate D
    M-->>Cliente: retorna valorFinal
    deactivate M