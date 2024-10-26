# Banco de Dados e SQL - Guia Prático

## Sumário
1. [Tipos de Linguagem](#1---tipos-de-linguagem)
2. [Processo Desenvolvimento Banco de Dados](#2---processo-desenvolvimento-banco-de-dados)
3. [Conceitos de Banco de Dados](#3---conceitos-de-banco-de-dados)
4. [Tipos de Atributos](#4---tipos-de-atributos)
5. [Relacionamentos](#5---relacionamentos)
6. [Normalizacao e Desnormalizacao](#6---normalizacao-e-desnormalizacao)
7. [DML - Data Manipulation Language](#7---dml---data-manipulation-language)
8. [DQL - Data Query Language](#8---dql---data-query-language)


## 1 - Tipos de Linguagem

### 1.1 - DDL (Data Definition Language) - Linguagem de Definição de Dados
Define a estrutura do banco de dados (criação, alteração ou exclusão de tabelas e outros objetos).

**CREATE** – Cria objetos (tabela, índice, view).
```
CREATE TABLE alunos (
  id INT PRIMARY KEY,
  nome VARCHAR(100),
  idade INT
);
```
**ALTER** – Modifica objetos existentes.
```
ALTER TABLE alunos ADD email VARCHAR(100);
```
**DROP** – Exclui objetos.
```
DROP TABLE alunos;
```
**TRUNCATE** – Remove todos os registros de uma tabela.
```
TRUNCATE TABLE alunos;
```

### 1.2 - DML (Data Manipulation Language) - Linguagem de Manipulação de Dados
Manipula os dados armazenados nas tabelas (inserir, atualizar ou excluir registros).

**INSERT** – Insere novos registros.
```
INSERT INTO alunos (id, nome, idade) VALUES (1, 'Ana', 20);
```
**UPDATE** – Atualiza registros existentes.
```
UPDATE alunos SET idade = 21 WHERE id = 1;
```
**DELETE** – Exclui registros.
```
DELETE FROM alunos WHERE id = 1;
```

### 1.3 - DCL (Data Control Language) - Linguagem de Controle de Dados
Controla os privilégios de acesso e permissões no banco de dados.

**GRANT** – Concede permissões.
```
GRANT SELECT, INSERT ON alunos TO usuario_teste;
```
**REVOKE** – Revoga permissões concedidas.
```
REVOKE INSERT ON alunos FROM usuario_teste;
```

### 1.4 - DTL (Data Transaction Language) - Linguagem de Transação de Dados
Gerencia as transações no banco de dados, garantindo consistência e controle.

**BEGIN** – Inicia uma transação.
```
BEGIN;
```
**COMMIT** – Confirma as alterações realizadas durante a transação.
```
COMMIT;
```
**ROLLBACK** – Desfaz as alterações realizadas na transação.
```
ROLLBACK;
```

### 1.5 - DQL (Data Query Language) - Linguagem de Consulta de Dados
Realiza consultas e busca de informações no banco de dados.

**SELECT** – Consulta dados nas tabelas.
```
SELECT nome, idade FROM alunos WHERE idade > 18;
```


## 2 - Processo Desenvolvimento Banco de Dados
A criação de um banco de dados envolve várias etapas, que vão desde o planejamento até a implementação e uso. Abaixo está um processo detalhado, passo a passo:

### 2.1 - Planejamento
Antes de começar a implementar, é essencial entender as necessidades do projeto. Algumas perguntas importantes:

- Quais dados serão armazenados?
- Como os dados se relacionam entre si?
- Qual será o volume de dados e o crescimento esperado?
- Quem terá acesso ao banco de dados?

#### Atividades:
- Definição de requisitos funcionais e não funcionais.
- Escolha do tipo de banco de dados (relacional, NoSQL, distribuído, etc.).
- Modelagem conceitual usando diagramas ER (Entidade-Relacionamento).


### 2.2 - Modelagem do Banco de Dados

### Modelagem Conceitual:
Utiliza um **Diagrama Entidade-Relacionamento (ER)** para identificar as entidades, atributos e seus relacionamentos.
- Exemplo de Entidades: Aluno, Curso.
- Relacionamento: Aluno pode estar matriculado em vários Cursos.

### Modelagem Lógica:
Transforma o diagrama ER em **tabelas e colunas de um banco relacional**. Nessa etapa, define-se:
- **Chave Primária (PK)**: Identifica unicamente um registro na tabela.
- **Chave Estrangeira (FK)**: Relaciona registros entre tabelas.

Exemplo de Tabelas:

```
Tabela: Aluno
----------------------
id_aluno | nome  | idade

Tabela: Curso
----------------------
id_curso | nome_curso | carga_horaria

Tabela: Matricula
----------------------
id_aluno | id_curso | data_matricula
```

### Modelagem Física:
Define a estrutura do banco de dados para um sistema específico (por exemplo, MySQL, PostgreSQL). Essa etapa considera índices, triggers e otimizações.


### 2.3 - Escolha do SGBD (Sistema de Gerenciamento de Banco de Dados)
Com base nas necessidades do projeto, é feita a escolha do SGBD. Exemplos:
- **Relacionais**: MySQL, PostgreSQL, Oracle, SQL Server.
- **NoSQL**: MongoDB, Redis, Cassandra.

### 2.4 - Criação do Banco de Dados e Estrutura de Tabelas
Nesta fase, é implementada a estrutura no SGBD escolhido. Usamos **DDL** para **criar o banco e suas tabelas**.

Exemplo de Comandos SQL (MySQL):
```
-- Criação do banco de dados
CREATE DATABASE escola;

-- Uso do banco recém-criado
USE escola;

-- Criação das tabelas
CREATE TABLE aluno (
  id INT PRIMARY KEY AUTO_INCREMENT,
  nome VARCHAR(100) NOT NULL,
  idade INT NOT NULL
);

CREATE TABLE curso (
  id INT PRIMARY KEY AUTO_INCREMENT,
  nome VARCHAR(100) NOT NULL,
  carga_horaria INT NOT NULL
);

CREATE TABLE matricula (
  id_aluno INT,
  id_curso INT,
  data_matricula DATE,
  PRIMARY KEY (id_aluno, id_curso),
  FOREIGN KEY (id_aluno) REFERENCES aluno(id),
  FOREIGN KEY (id_curso) REFERENCES curso(id)
);
```

### 2.5 - Inserção de Dados (DML)
Depois de definir a estrutura, podemos inserir dados de exemplo nas tabelas.

```
-- Inserção de alunos
INSERT INTO aluno (nome, idade) VALUES ('Ana', 20);
INSERT INTO aluno (nome, idade) VALUES ('Carlos', 22);

-- Inserção de cursos
INSERT INTO curso (nome, carga_horaria) VALUES ('Matemática', 60);
INSERT INTO curso (nome, carga_horaria) VALUES ('Física', 40);

-- Inserção de matrículas
INSERT INTO matricula (id_aluno, id_curso, data_matricula) VALUES (1, 1, '2024-10-01');
INSERT INTO matricula (id_aluno, id_curso, data_matricula) VALUES (2, 2, '2024-10-02');
```

### 2.6 - Definição de Usuários e Permissões (DCL)
Controla o acesso de usuários ao banco de dados.

```
-- Criação de usuário e concessão de permissões
CREATE USER 'usuario_teste'@'localhost' IDENTIFIED BY 'senha123';
GRANT SELECT, INSERT ON escola.* TO 'usuario_teste'@'localhost';
```

### 2.7 - Testes e Validação
Nesta etapa, realiza-se a validação para garantir que:
- A estrutura das tabelas está correta.
- Os relacionamentos entre as tabelas estão funcionando.
- As operações básicas de **CRUD (Create, Read, Update, Delete)** funcionam como esperado.


### 2.8 - Otimização e Indexação
Para melhorar o desempenho, pode ser necessário:

- Criar índices nas colunas frequentemente usadas em consultas.
- Implementar normalização (para evitar redundância) ou desnormalização (para melhorar a performance de leitura).
```
-- Criação de índice na coluna 'nome' da tabela aluno
CREATE INDEX idx_nome_aluno ON aluno(nome);
```

### 2.9 - Backup e Recuperação
Configuração de backups regulares para evitar perda de dados em caso de falhas.

```
mysqldump -u usuario -p escola > backup_escola.sql
```

### 2.10 - Manutenção Contínua e Monitoramento
Após a implantação, é necessário:

- Monitorar o desempenho do banco.
- Fazer ajustes de índices ou estrutura conforme o banco cresce.
- Realizar backups e atualizações regulares.


## 3 - Conceitos de Banco de Dados

### 3.1 - Banco de Dados (BD)
Um banco de dados é uma coleção organizada de dados que permite armazenamento, gerenciamento e recuperação eficiente das informações. Pode ser utilizado por sistemas para registrar dados de forma estruturada.

### 3.2 - SGBD (Sistema de Gerenciamento de Banco de Dados)
É o software que permite a criação, manutenção e controle dos bancos de dados. Exemplos: MySQL, PostgreSQL, MongoDB, Oracle, SQL Server.

**Funções do SGBD**:
- Gerenciar acesso aos dados.
- Garantir integridade e segurança.
- Facilitar consultas e manipulação de dados.


### 3.3 - Entidade e Relacionamento
- **Entidade**: Representa um objeto do mundo real (ex.: Aluno, Produto).
- **Relacionamento**: Define como as entidades estão conectadas. Exemplo: "Aluno está matriculado em Curso".


### 3.4 - Tabela (ou Relação)
No modelo relacional, uma tabela é uma coleção de dados organizados em linhas e colunas. Cada linha é um registro e cada coluna representa um campo (atributo).

```
Tabela: Alunos
--------------
id | nome  | idade
1  | Ana   | 20
2  | João  | 22
```


### 3.5 - Chave Primária (Primary Key - PK)
É um **identificador único** para cada registro em uma tabela. Não pode haver duas linhas com o mesmo valor de chave primária.

```
id INT PRIMARY KEY
```


### 3.6 - Chave Estrangeira (Foreign Key - FK)
É um campo que referencia a chave primária de outra tabela, estabelecendo uma relação entre duas tabelas.

```
FOREIGN KEY (id_aluno) REFERENCES aluno(id)
```

### 3.7 - Consultas SQL (DQL)
SQL (Structured Query Language) é a linguagem usada para consultar e manipular dados. Exemplo de consulta:

```
SELECT nome, idade FROM aluno WHERE idade > 18;
```


### 3.8 - Índices
Índices são estruturas que aceleram a recuperação de dados. São criados em uma ou mais colunas para facilitar as buscas.

```
CREATE INDEX idx_nome ON aluno(nome);
```


### 3.9 - Transações (ACID)
Uma transação é uma unidade de trabalho que deve ser completada por inteiro ou não ser executada. As propriedades ACID garantem:

- **Atomicidade**: A transação é indivisível (tudo ou nada).
- **Consistência**: O banco mantém um estado consistente antes e depois da transação.
- **Isolamento**: Uma transação não interfere em outra.
- **Durabilidade**: Os dados confirmados são persistentes mesmo em caso de falha.


### 3.10 - Integridade Referencial
Garante que os dados relacionados entre tabelas estejam coerentes. Por exemplo, não é possível inserir uma matrícula se o aluno não existe na tabela de alunos.


### 3.11 - Normalização
Processo de organização das tabelas para minimizar redundâncias e dependências indesejadas. A normalização melhora a integridade dos dados.


### 3.12 - Denormalização
É o processo de reverter a normalização para melhorar a performance de leitura, geralmente replicando dados em diferentes tabelas.


### 3.13 - Backup e Recuperação
Processo de salvar uma cópia do banco de dados para restaurar em caso de falha. Ferramentas como mysqldump permitem backups frequentes.


### 3.14 - Controle de Concorrência
Garante que várias transações possam ocorrer simultaneamente sem comprometer a integridade dos dados. Isso é gerenciado por bloqueios (locks) ou controle de versão.


### 3.15 - Tipos de Banco de Dados
- **Relacional**: MySQL, PostgreSQL (usam tabelas e SQL).
- **NoSQL**: MongoDB, Redis (orientado a documentos, chave-valor ou grafos).
- **Data Warehouse**: Armazena dados históricos para análise e BI (ex.: Amazon Redshift).


### 3.16 - Backup Incremental e Completo
- **Backup Completo**: Salva toda a base de dados.
- **Backup Incremental**: Salva apenas as alterações desde o último backup.


### 3.17 - Triggers
São gatilhos que disparam automaticamente quando uma ação específica ocorre no banco (como um INSERT ou UPDATE).

```
CREATE TRIGGER atualiza_estoque AFTER INSERT ON venda
FOR EACH ROW
BEGIN
  UPDATE produto SET quantidade = quantidade - NEW.quantidade_vendida WHERE id = NEW.id_produto;
END;
```


### 3.18 - Vistas (Views)
Uma view é uma consulta armazenada como uma tabela virtual. Ajuda a simplificar consultas complexas e a proteger dados sensíveis.

```
CREATE VIEW alunos_maiores AS
SELECT nome, idade FROM aluno WHERE idade >= 18;
```


### 3.19 - Backup e Recuperação de Desastres
Estratégia para recuperar dados após falhas críticas. Pode incluir replicação de dados para outro servidor.


### 3.20 - Sharding e Replicação
- **Sharding**: Divide o banco de dados em várias partes para escalabilidade.
- **Replicação**: Cria cópias de dados em vários servidores para garantir disponibilidade.


## 4 - Tipos de Atributos
Em bancos de dados, os atributos são as características ou propriedades que descrevem uma entidade (como nome ou idade de um aluno). Eles são os campos de uma tabela no modelo relacional. Abaixo, explico os diferentes tipos de atributos e suas categorias.


### 4.1 - Atributos Simples e Compostos
- **Atributo Simples**: Não pode ser dividido em partes menores.
  - Exemplo: idade = 20 (um único valor).

- **Atributo Composto**: Pode ser subdividido em partes menores que fazem sentido individualmente. 
  - Exemplo: endereço → Rua, Cidade, CEP


### 4.2 - Atributos Monovalorados e Multivalorados
- **Atributo Monovalorado**: Armazena um único valor por registro.
  - Exemplo: nome = "Ana".

- **Atributo Multivalorado**: Pode armazenar múltiplos valores para um mesmo registro.
  - Exemplo: telefone = ["(11) 1234-5678", "(11) 9876-5432"].


### 4.3 - Atributos Derivados
O valor do atributo pode ser calculado a partir de outros atributos.
  - Exemplo: idade pode ser calculada a partir da data de nascimento (data_nascimento).
```
idade = ano_atual - ano_nascimento
```


### 4.4 - Atributos Nulos e Não Nulos
- **Atributo Nulo**: Pode não ter valor em determinados casos.
  - Exemplo: o atributo data_fim de um curso pode ser nulo enquanto o curso estiver em andamento.

- **Atributo Não Nulo**: Deve ter um valor sempre.
  - Exemplo: id de um aluno não pode ser nulo (chave primária).


### 4.5 - Atributos Chave
- **Chave Primária (PK)**: Identifica de forma única cada registro em uma tabela.
  - Exemplo: id_aluno = 1.
```
id_aluno INT PRIMARY KEY
```

- **Chave Estrangeira (FK)**: Relaciona uma tabela com outra.
  - Exemplo: id_curso em uma tabela de matrícula é uma chave estrangeira que referencia a tabela curso.


### 4.6 - Atributos Armazenados e Calculados
- **Atributo Armazenado**: O valor é mantido diretamente no banco.
  - Exemplo: nome = "Carlos".

- **Atributo Calculado**: O valor é gerado dinamicamente durante a consulta.
  - Exemplo: salario_anual pode ser calculado como salario_mensal * 12.


### 4.7 - Atributos Identificadores
São usados para identificar unicamente uma instância de entidade.
  - Exemplo: cpf em uma tabela de pessoas é um identificador único.


### 4.8 - Atributos de Domínio
Representam o conjunto de valores possíveis para um atributo.
  - Exemplo: o atributo sexo pode ter como domínio: {Masculino, Feminino, Outro}.


### 4.9 - Atributos Dependentes e Independentes
- **Atributo Independente**: Pode existir sem depender de outros atributos.
  - Exemplo: nome de um aluno é independente.

- **Atributo Dependente**: Seu valor faz sentido apenas quando associado a outro.
  - Exemplo: media_final faz sentido apenas se estiver associado a um aluno e a uma disciplina.


### Resumo em Tabela

| Tipo de Atributo | Descrição | Exemplo |
| ---------- | ----------- |  ------------ |
| Simples       | Não pode ser dividido | `idade` = 20  |
| Composto       | Pode ser dividido em partes menores | `endereço` = "Rua, Cidade"  |
| Monovalorado      | Armazena um único valor | `nome` = "Ana"  |
| Multivalorado      | Armazena múltiplos valores | `telefone` = ["123", "456"]  |
| Derivado      | Calculado a partir de outros atributos | `idade` = ano_atual - nascimento  |
| Nulo      | Pode não ter valor | `data_fim`  |
| Não Nulo      | Deve ter um valor sempre  | `id` = 1  |
| Chave Primária (PK)      | Identifica um registro de forma única	  | `id_aluno`  |
| Chave Estrangeira (FK)      | Relaciona duas tabelas  | `id_curso` refere curso(id)  |
| Armazenado      | Valor mantido no banco | `nome` = "Carlos"  |
| Calculado      | Gerado dinamicamente |  `salario_anual` = salario * 12 |
| Identificador      | Atributo único por registro | `cpf`  |
| Domínio      | Conjunto de valores possíveis | `sexo` = {Masculino, Feminino}  |
| Dependente      | Depende de outro atributo | `media_final`  |


## 5 - Relacionamentos
No contexto de bancos de dados, relacionamentos representam como as entidades (ou tabelas) se conectam entre si. Eles são fundamentais para garantir a integridade e a coerência dos dados. Abaixo, explico os principais tipos de relacionamentos, o conceito de cardinalidade e exemplos práticos.

### 5.1 - Relacionamento 1:1 (Um-para-Um/One-to-One)
Cada instância de uma entidade se relaciona com apenas uma instância de outra entidade, e vice-versa.

- **Exemplo**:
  - Pessoa ↔ RG
  - Cada pessoa possui um RG único, e cada RG pertence a uma única pessoa.

```
Pessoa (id_pessoa, nome)
RG (id_rg, numero_rg, id_pessoa)  -- id_pessoa é FK
```

- **Implementação**:

```
CREATE TABLE pessoa (
    id_pessoa INT PRIMARY KEY,
    nome VARCHAR(100)
);

CREATE TABLE rg (
    id_rg INT PRIMARY KEY,
    numero_rg VARCHAR(20),
    id_pessoa INT UNIQUE,
    FOREIGN KEY (id_pessoa) REFERENCES pessoa(id_pessoa)
);
```


### 5.2 - Relacionamento  1:N (Um-para-Muitos/One-to-Many)
Uma instância de uma entidade pode se relacionar com múltiplas instâncias de outra entidade, mas cada instância da segunda entidade só se relaciona com uma da primeira.

- **Exemplo**:
  - Professor → Turma
  - Um professor pode lecionar para várias turmas, mas cada turma tem apenas um professor.

```
Professor (id_professor, nome)
Turma (id_turma, nome_turma, id_professor)  -- id_professor é FK
```

- **Implementação**:
```
CREATE TABLE professor (
    id_professor INT PRIMARY KEY,
    nome VARCHAR(100)
);

CREATE TABLE turma (
    id_turma INT PRIMARY KEY,
    nome_turma VARCHAR(50),
    id_professor INT,
    FOREIGN KEY (id_professor) REFERENCES professor(id_professor)
);
```


### 5.3 - Relacionamento N:N (Muitos-para-Muitos/Many-to-Many)
Múltiplas instâncias de uma entidade podem se relacionar com múltiplas instâncias de outra entidade. Esse tipo de relacionamento é implementado por meio de uma **tabela associativa**.

- **Exemplo**:
  - Aluno ↔ Curso
  - Um aluno pode se matricular em vários cursos, e cada curso pode ter vários alunos.
```
Aluno (id_aluno, nome)
Curso (id_curso, nome_curso)
Matricula (id_aluno, id_curso)  -- Tabela associativa
```

Implementação:
```
CREATE TABLE aluno (
    id_aluno INT PRIMARY KEY,
    nome VARCHAR(100)
);

CREATE TABLE curso (
    id_curso INT PRIMARY KEY,
    nome_curso VARCHAR(100)
);

CREATE TABLE matricula (
    id_aluno INT,
    id_curso INT,
    PRIMARY KEY (id_aluno, id_curso),
    FOREIGN KEY (id_aluno) REFERENCES aluno(id_aluno),
    FOREIGN KEY (id_curso) REFERENCES curso(id_curso)
);
```


### Cardinalidade
A cardinalidade define o número de instâncias que uma entidade pode se relacionar com outra. Ela pode ser expressa como:

- **1:1**: Cada entidade tem no máximo uma correspondência.
- **1:N**: Uma entidade se relaciona com várias na outra (e vice-versa).
- **N:N**: Várias instâncias de ambas as entidades se relacionam entre si.

Exemplo de Cardinalidade em um Diagrama ER:
- **1:N**: Um cliente pode fazer vários pedidos, mas cada pedido pertence a um único cliente.
- **N:N**: Um aluno pode fazer vários cursos, e cada curso pode ter vários alunos.


### Resumo em Tabela
| Relacionamento | Descrição | Exemplo |  Implementação  |
| ---------- | ----------- |  ------------ |   ------------ |
|  1:1 (Um-para-Um)  | Cada entidade se relaciona com apenas uma da outra | Pessoa ↔ RG  |  FK com UNIQUE |
|  1:N (Um-para-Muitos)  | Uma entidade se relaciona com várias da outra | Professor → Turma  |  FK simples |
|   N:N (Muitos-para-Muitos)     | Várias instâncias de ambas as entidades se relacionam entre si | Aluno ↔ Curso  | Tabela associativa  |


## 6 - Normalizacao e Desnormalizacao

### O que é Normalização?
A normalização é um processo de **organização de tabela**s em um banco de dados para:

- Reduzir redundância/excesso de dados.
- Evitar anomalias (problemas) nas operações de inserção, atualização e exclusão.
- Melhorar a integridade dos dados.
- **Objetivo principal**: garantir que cada fato ou informação seja armazenado apenas uma vez e esteja em seu lugar correto.

### O que é Desnormalização?
A desnormalização é o processo **inverso da normalização**. Consiste em **adicionar redundância controlada** para:
- Melhorar a performance em leituras e consultas.
- Reduzir o tempo de acesso aos dados em sistemas que priorizam leitura (por exemplo, relatórios e dashboards).

### 6.1 - Primeira Forma Normal (1FN)
Uma tabela está na 1FN se:
- Todos os campos contêm apenas valores atômicos (ou seja, indivisíveis).
- Não há grupos repetidos ou colunas que armazenem listas de valores múltiplos.
- Ou seja, **atributos simples e monovalorados**

#### Exemplo - Não Normalizado: 
A coluna Produto armazena vários valores.
| Pedido | Produto |
| ---------- | ----------- |
|  1  | TV, Notebook  |
|  2  | Celular  |


#### Tabela em 1FN:
| Pedido | Produto |
| ---------- | ----------- |
|  1  | TV  |
|  1  | Notebook  |
|  2  | Celular  |

Agora, cada campo tem apenas um valor atômico.


### 6.2 - Segunda Forma Normal (2FN)
Uma tabela está na 2FN se:

- Está na 1FN.
- Todos os atributos não-chave são totalmente dependentes da chave primária (ou seja, não há dependência parcial).

#### Exemplo - Não Normalizado (violação da 2FN):
O atributo Nome Cliente depende apenas do Pedido, e não de todos os campos da chave composta (Pedido, Produto).
| Pedido | Produto |  Preço Total  |  Nome Cliente  |
| ---------- | ----------- |  ----------- |  ----------- |
|  1  | TV | 3000 | Ana |
|  1  | Notebook | 3000 | Ana |

#### Tabelas em 2FN:
Pedido:
| Pedido | Nome Cliente |
| ---------- | ----------- |
|  1  | Ana |

Itens_Pedido:
| Pedido | Produto |
| ---------- | ----------- |
|  1  | TV  |
|  1  | Notebook  |


### 6.3 - Terceira Forma Normal (3FN)
Uma tabela está na 3FN se:
- Está na 2FN.
- Atributos não-chave são independentes entre si e só dependem da chave primária.

#### Exemplo - Não Normalizado (violação da 3FN):
A cidade e o estado são dependentes entre si, e não apenas da chave primária (CPF).

| Cliente | CPF |  Cidade  | Estado  |
| ---------- | ----------- |  ----------- |  ----------- |
|  Ana  | 123.456.789 | São Paulo | SP |
|  João  | 987.654.321 | Belo Horizonte | MG |


#### Tabelas em 3FN:
Cliente:
| Cliente | CPF | Cidade  |
| ---------- | ----------- | ----------- |
|  Ana  | 123.456.789  | São Paulo  |
|  João  | 987.654.321  |  Belo Horizonte |

Localidade:
| Pedido | Produto |
| ---------- | ----------- |
|  São Paulo | SP |
|  Belo Horizonte | MG |

Agora, a relação entre cidade e estado foi separada em outra tabela.


### Objetivo da Normalização
- **Minimizar redundância**: Evitar duplicação de dados em diferentes lugares.
- **Evitar anomalias**: Corrigir problemas ao inserir, atualizar ou excluir dados.
- **Melhorar integridade**: Garantir que as relações entre os dados estejam corretas.

### Quando Usar Desnormalização?
- Quando performance de leitura é mais importante que a integridade.
- Em data warehouses ou sistemas de BI onde consultas precisam ser rápidas.
- Para reduzir joins em consultas muito complexas.


### Resumo das Formas Nominais
| Forma Normal | Critério | Exemplo de Problema Resolvido  |
| ---------- | ----------- | ----------- |
|  `1FN`  | Todos os campos contêm valores atômicos e não há listas.  | Colunas com múltiplos valores.  |
|  `2FN`  | `Não há dependências parciais` (todos os atributos dependem da chave primária).  |  Atributos que dependem apenas de parte da chave composta. |
|  `3FN`  | `Não há dependências transitivas` (atributos não-chave dependem apenas da chave primária).  |  Atributos que dependem de outros atributos não-chave. |



## 7 - DML - Data Manipulation Language
A DML (Data Manipulation Language) é uma subcategoria da SQL usada para **manipular dados em bancos de dados**. Ela inclui comandos para **inserir, atualizar, excluir e consultar registros**.

### 7.1 - INSERT
Insere um registro em uma tabela especificando valores para todas as colunas.

#### 7.1.1 - Inserindo um Registro Simples
Insere um registro em uma tabela especificando valores para todas as colunas.
```
INSERT INTO aluno (id, nome, idade) 
VALUES (1, 'Ana', 20);
```

#### 7.1.2 - Inserindo Múltiplos Registros
Insere mais de um registro de uma vez.
```
INSERT INTO aluno (id, nome, idade) 
VALUES 
    (2, 'Carlos', 22), 
    (3, 'Beatriz', 19);
```

#### 7.1.3 - Inserindo Registro com Colunas Opcionais
Insere um registro informando apenas algumas colunas. As demais colunas devem aceitar valores nulos ou ter um valor padrão (default).
```
INSERT INTO aluno (id, nome) 
VALUES (4, 'João');
```

#### 7.1.4 - Inserindo Dados com Valores Gerados Automaticamente
Em algumas tabelas, a coluna id é gerada automaticamente (auto-increment). Nesse caso, não é necessário inserir manualmente o valor.
```
INSERT INTO aluno (nome, idade) 
VALUES ('Fernanda', 21);
```

#### 7.1.5 - Inserindo Registros Usando Subconsulta
Insere dados em uma tabela usando resultados de uma consulta (SELECT).
```
INSERT INTO aluno_backup (id, nome, idade)
SELECT id, nome, idade 
FROM aluno 
WHERE idade > 20;
```
**Resultado**: Os registros com idade superior a 20 serão copiados para a tabela aluno_backup.


#### 7.1.6 - Inserindo Dados com Valores Padrão
Se uma tabela tiver valores padrão (DEFAULT) definidos, esses valores serão usados automaticamente se você não especificar.
```
CREATE TABLE aluno (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    idade INT DEFAULT 18
);

INSERT INTO aluno (nome) 
VALUES ('Gabriel');
```

#### 7.1.7 - Inserindo Dados em Tabela com Chave Estrangeira
Ao inserir dados em uma tabela com chaves estrangeiras, é necessário que o registro correspondente já exista na tabela referenciada.
```
INSERT INTO curso (id_curso, nome_curso) 
VALUES (1, 'Matemática');

INSERT INTO matricula (id_aluno, id_curso, data_matricula) 
VALUES (1, 1, '2024-10-26');
```

#### 7.1.8 - Inserindo Dados com Tratamento de Erros (Ignorar Conflitos)
Em alguns SGBDs, você pode ignorar conflitos com a cláusula IGNORE (MySQL) ou tratar com ON CONFLICT (PostgreSQL).

**Exemplo no MySQL:**
```
INSERT IGNORE INTO aluno (id, nome, idade) 
VALUES (1, 'Ana', 21);  -- Ignorado, pois o id já existe
```

**Exemplo no PostgreSQL**:
```
INSERT INTO aluno (id, nome, idade) 
VALUES (1, 'Ana', 21) 
ON CONFLICT (id) DO NOTHING;
```


### 7.2 - UPDATE
O comando UPDATE é utilizado para modificar dados existentes em uma ou mais linhas de uma tabela.

#### 7.2.1 - Sintaxe Básica
```
UPDATE tabela
SET coluna1 = valor1, coluna2 = valor2, ...
WHERE condição;
```
- **tabela**: A tabela onde os dados serão atualizados.
- **SET**: Define quais colunas e valores serão modificados.
- **WHERE**: Condição para identificar quais linhas serão atualizadas (evita atualizar tudo acidentalmente).

#### 7.2.2 - Atualizando um Registro Específico
Altere a idade de um aluno com id = 1.
```
UPDATE aluno 
SET idade = 21 
WHERE id = 1;
```

#### 7.2.3 - Atualizando Várias Colunas
Atualize tanto o nome quanto a idade de um aluno com id = 2.
```
UPDATE aluno 
SET nome = 'Carlos Silva', idade = 23 
WHERE id = 2;
```

#### 7.2.4 - Atualizando Todos os Registros
Aumente a idade de todos os alunos em 1 ano.
```
UPDATE aluno 
SET idade = idade + 1;
```

#### 7.2.5 - Atualizando com Condição no WHERE
Aumente a idade apenas dos alunos com idade menor que 21.
```
UPDATE aluno 
SET idade = idade + 1 
WHERE idade < 21;
```

#### 7.2.6 - Atualização com Subconsulta
Atualize a idade do aluno com o maior id para 25.
```
UPDATE aluno 
SET idade = 25 
WHERE id = (SELECT MAX(id) FROM aluno);
```

#### 7.2.7 - Atualizando Dados em Tabelas Relacionadas
Suponha que cada aluno está matriculado em um curso. Agora, remova um aluno e atualize sua matrícula para NULL.
```
UPDATE matricula 
SET id_aluno = NULL 
WHERE id_aluno = 3;
```

#### 7.2.8 - Atualização Condicional com CASE
Se um aluno tiver idade maior que 23, reduza a idade em 1 ano; caso contrário, aumente a idade em 1 ano.
```
UPDATE aluno 
SET idade = CASE 
               WHEN idade > 23 THEN idade - 1
               ELSE idade + 1
           END;
```

#### 7.2.9 - UPDATE com Tratamento de Erros e Transações
Use transações para garantir que uma atualização só seja confirmada se tudo der certo.
```
BEGIN;

UPDATE aluno 
SET idade = 30 
WHERE id = 1;

-- Se algo der errado, desfaça as mudanças.
ROLLBACK;

-- Se tudo estiver certo, confirme as mudanças.
COMMIT;
```


#### 7.2.10 - Atenção ao Usar UPDATE
- **Cuidado com o WHERE**: Se você esquecer o WHERE, todas as linhas da tabela serão atualizadas!

```
UPDATE aluno 
SET idade = 30;  -- Atualiza todos os registros!
```
- **Verifique antes de executar**: Use um SELECT com a mesma condição para garantir que você está atualizando os registros corretos.


### 7.3 - DELETE
O DELETE remove um ou mais registros de uma tabela. Ele pode ser usado com uma cláusula WHERE para remover registros específicos ou, se usado sem WHERE, apaga todos os registros da tabela.

#### 7.3.1 - Sintaxe Básica
```
DELETE FROM tabela 
WHERE condição;
```
- **tabela**: A tabela de onde os registros serão excluídos.
- **WHERE**: A condição que determina quais registros serão removidos.


#### 7.3.2 - Deletando um Registro Específico
Exclui o registro do aluno com id = 2.
```
DELETE FROM aluno 
WHERE id = 2;
```


#### 7.3.3 - Deletando Múltiplos Registros
Exclui todos os alunos com idade maior que 23.
```
DELETE FROM aluno 
WHERE idade > 23;
```


#### 7.3.4 - Deletando Todos os Registros
Remove todos os registros da tabela, mas mantém sua estrutura intacta.
```
DELETE FROM aluno;
```

- **Resultado**: a tabela aluno agora está vazia, mas ainda existe.


#### 7.3.5 - DELETE com Subconsulta
Exclui alunos matriculados no curso com id_curso = 1.
```
DELETE FROM aluno 
WHERE id IN (SELECT id_aluno FROM matricula WHERE id_curso = 1);
```


#### 7.3.6 - DELETE com Chaves Estrangeiras e Restrição de Integridade
Se houver uma chave estrangeira associada, pode ser necessário lidar com restrições de integridade.

- **Exemplo**: Ao tentar excluir um aluno que está referenciado na tabela matricula como chave estrangeira, o banco pode gerar um erro. Nesse caso, você pode usar:
  - ON DELETE CASCADE: Apaga também os registros relacionados automaticamente.

  - Exemplo de Criação da Tabela:

```
CREATE TABLE matricula (
  id_aluno INT,
  id_curso INT,
  FOREIGN KEY (id_aluno) REFERENCES aluno(id) ON DELETE CASCADE
);
```
  - Agora, ao excluir um aluno, suas matrículas também serão apagadas automaticamente:

```
DELETE FROM aluno WHERE id = 1;
```


#### 7.3.7 - Usando Transações para DELETE
Com o uso de transações, você pode desfazer uma exclusão acidental com ROLLBACK.
```
BEGIN;

DELETE FROM aluno 
WHERE id = 3;

-- Se precisar desfazer a exclusão:
ROLLBACK;

-- Para confirmar a exclusão:
COMMIT;
```


#### 7.3.8 - Atenção ao Usar DELETE
- **Esquecer o WHERE**: Se você não especificar uma condição, todos os registros da tabela serão apagados.

```
DELETE FROM aluno;  -- Apaga todos os registros!
```
- **Verificação Prévia**: É uma boa prática executar um SELECT com a mesma condição antes de realizar o DELETE para verificar quais registros serão afetados.


#### 7.3.9 - Alternativa: TRUNCATE
- **TRUNCATE** também remove todos os registros de uma tabela, mas é mais rápido que DELETE porque não registra cada remoção individualmente no log de transações.
```
TRUNCATE TABLE aluno;
```
- **Diferença**:
  - **DELETE**: Pode ser usado com WHERE.
  - **TRUNCATE**: Remove todos os registros sem condições e não pode ser revertido com ROLLBACK.


### 7.4 - MERGE
O comando MERGE é usado em alguns bancos de dados SQL para **combinar operações de inserção (INSERT), atualização (UPDATE) e, em alguns casos, deleção (DELETE) em uma única instrução**. Ele é útil para sincronizar dados entre duas tabelas, atualizando registros existentes e inserindo novos, dependendo de uma condição.

A sintaxe do MERGE **varia ligeiramente entre os bancos de dados**, mas os mais comuns a suportarem esse comando são **Oracle**, **SQL Server** e **PostgreSQL** (como UPSERT).


#### 7.4.1 - Sintaxe Básica do MERGE
```
MERGE INTO tabela_destino AS destino
USING tabela_origem AS origem
ON destino.id = origem.id
WHEN MATCHED THEN
    UPDATE SET destino.nome = origem.nome, destino.idade = origem.idade
WHEN NOT MATCHED THEN
    INSERT (id, nome, idade) VALUES (origem.id, origem.nome, origem.idade);
```
- **tabela_destino**: A tabela na qual os registros serão inseridos ou atualizados.
- **tabela_origem**: A tabela ou consulta que fornece os dados a serem comparados.
- **ON**: Define a condição de correspondência entre os registros (geralmente com uma chave primária).
- **WHEN MATCHED**: Especifica a ação de atualizar quando há uma correspondência.
- **WHEN NOT MATCHED**: Especifica a ação de inserir quando não há correspondência


#### 7.4.2 - Exemplo Prático de MERGE (SQL Server)
- **Cenário**:
  - Tabela de Alunos Atualizados (aluno_atualizado): Contém novas informações sobre alunos (origem).
  - Tabela de Alunos Principal (aluno): Onde queremos atualizar ou inserir os registros (destino).
```
MERGE INTO aluno AS destino
USING aluno_atualizado AS origem
ON destino.id = origem.id
WHEN MATCHED THEN
    UPDATE SET destino.nome = origem.nome, destino.idade = origem.idade
WHEN NOT MATCHED THEN
    INSERT (id, nome, idade) VALUES (origem.id, origem.nome, origem.idade);
```

- **Comportamento**:
  - Quando os ids correspondem (já existem na tabela aluno), o comando atualiza o nome e a idade.
  - Quando não há correspondência (o aluno ainda não existe na tabela aluno), o comando insere um novo registro.


#### 7.4.3 - Exemplo com UPSERT no PostgreSQL
O PostgreSQL não possui MERGE, mas o mesmo comportamento pode ser alcançado com INSERT ON CONFLICT para realizar um UPSERT (inserir ou atualizar).

```
INSERT INTO aluno (id, nome, idade)
VALUES (1, 'Ana', 21)
ON CONFLICT (id) DO 
UPDATE SET nome = EXCLUDED.nome, idade = EXCLUDED.idade;
```
- **ON CONFLICT**: Define como tratar um conflito (com base no id).
- **DO UPDATE**: Atualiza o registro se houver conflito.
- **EXCLUDED**: Refere-se aos valores que foram enviados na tentativa de inserção.


#### 7.4.4 - Variações do MERGE no SQL Server
- **Usando DELETE no MERGE** = também é possível excluir registros que não correspondem a uma condição.
```
MERGE INTO aluno AS destino
USING aluno_atualizado AS origem
ON destino.id = origem.id
WHEN MATCHED THEN
    UPDATE SET destino.nome = origem.nome, destino.idade = origem.idade
WHEN NOT MATCHED BY SOURCE THEN
    DELETE;  -- Exclui registros que não estão mais na origem
```

#### 7.4.5 - Exemplo Oracle
- **Estrutura das Tabelas:**
```
-- Tabela de destino: Alunos existentes
CREATE TABLE aluno (
    id NUMBER PRIMARY KEY,
    nome VARCHAR2(100),
    idade NUMBER
);

-- Tabela de origem: Novas informações de alunos
CREATE TABLE aluno_atualizado (
    id NUMBER PRIMARY KEY,
    nome VARCHAR2(100),
    idade NUMBER
);
```

- **Inserindo Dados nas Tabelas:**
```
-- Dados iniciais na tabela de destino (aluno)
INSERT INTO aluno (id, nome, idade) VALUES (1, 'Ana', 20);
INSERT INTO aluno (id, nome, idade) VALUES (2, 'Carlos', 22);

-- Dados novos e atualizados na tabela de origem (aluno_atualizado)
INSERT INTO aluno_atualizado (id, nome, idade) VALUES (2, 'Carlos Silva', 23); -- Atualização
INSERT INTO aluno_atualizado (id, nome, idade) VALUES (3, 'Beatriz', 21); -- Novo registro
```

- **Exemplo de MERGE no Oracle**
```
MERGE INTO aluno dest  -- Tabela de destino
USING aluno_atualizado src  -- Tabela de origem
ON (dest.id = src.id)  -- Condição de correspondência
WHEN MATCHED THEN
    UPDATE SET dest.nome = src.nome, dest.idade = src.idade  -- Atualiza registros correspondentes
WHEN NOT MATCHED THEN
    INSERT (id, nome, idade)  -- Insere novos registros
    VALUES (src.id, src.nome, src.idade);

```

- **Comportamento**:
  - **id = 2 (Carlos)**: Como o registro já existe, ele será atualizado para "Carlos Silva", com idade 23.
  - **id = 3 (Beatriz)**: Como esse aluno não existe na tabela de destino, ele será inserido.


- **Verificando o Resultado**:
```
SELECT * FROM aluno;
```

| id | nome | idade |
| ---------- | ----------- | ----------- |
|  1 | Ana | 20 |
|  2 | Carlos Silva | 23 |
| 3 | Beatriz | 21 |


- **Explicação do Código**:
  - **MERGE INTO aluno dest**: A tabela aluno é a tabela de destino.
  - **USING aluno_atualizado src**: A tabela aluno_atualizado é a tabela de origem.
  - **ON (dest.id = src.id)**: Define a condição de correspondência com base na coluna id.
  - **WHEN MATCHED THEN**: Se a correspondência for encontrada, o registro será atualizado.
  - **WHEN NOT MATCHED THEN**: Se não houver correspondência, o registro será inserido.





#### 7.4.6 - Vantagens do MERGE
- **Eficiência**: Reduz a necessidade de múltiplos comandos (INSERT, UPDATE, DELETE).
- **Legibilidadev: Agrupa operações relacionadas em uma única instrução.
- **Consistência**: Garante que inserções e atualizações sejam tratadas em um único passo.

#### 7.4.7 - Limitações e Cuidados
- **Performance**: O MERGE pode ser pesado em grandes conjuntos de dados.
- **Condições Complexas**: O uso inadequado de condições ON pode causar problemas de duplicação ou exclusão indesejada.
- **Log de Transações**: Gera um grande volume de log, o que pode impactar o desempenho em alguns SGBDs.


## 8 - DQL - Data Query Language
