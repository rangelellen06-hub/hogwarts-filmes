# ⚡ Hogwarts Filmes

> Sistema web de catálogo de filmes com tema Harry Potter, desenvolvido com Spring Boot, Java 21, MySQL e integração com a API TMDB.

![Java](https://img.shields.io/badge/Java-21-orange?style=flat-square&logo=java)
![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.2.5-brightgreen?style=flat-square&logo=springboot)
![MySQL](https://img.shields.io/badge/MySQL-8.0-blue?style=flat-square&logo=mysql)
![Maven](https://img.shields.io/badge/Maven-3.9-red?style=flat-square&logo=apachemaven)
![REST API](https://img.shields.io/badge/REST-API-yellow?style=flat-square)

---

## 📋 Sobre o Projeto

O **Hogwarts Filmes** é um sistema web completo que permite o cadastro de clientes e o acesso a um catálogo de filmes com tema Harry Potter. O sistema consome a API pública do **The Movie Database (TMDB)** para buscar filmes em tempo real e persiste todos os dados em banco de dados **MySQL**.

Desenvolvido como trabalho avaliativo da disciplina de **Backend Frameworks** — Curso de ADS.

---

## ✨ Funcionalidades

- ✅ Cadastro de clientes com nome, CPF, data de nascimento e e-mail
- ✅ Busca de filmes em tempo real via API TMDB
- ✅ Salvamento de filmes no banco de dados MySQL
- ✅ Registro de acessos (cliente ↔ filme)
- ✅ CRUD completo nas 3 entidades (Cliente, Filme, Acesso)
- ✅ API REST com endpoints documentados
- ✅ Frontend SPA com tema Harry Potter
- ✅ Validação de CPF e e-mail duplicados
- ✅ Modal de informações com dados do banco de dados

---

## 🏗️ Arquitetura em 4 Camadas

```
Controller  →  Service  →  Repository  →  MySQL
    ↕                           ↕
Frontend                    TMDB API
```

| Camada | Pacote | Responsabilidade |
|--------|--------|-----------------|
| **Model** | `com.hogwarts.filmes.model` | Entidades JPA mapeadas para o banco |
| **Repository** | `com.hogwarts.filmes.repository` | Acesso ao banco via Spring Data JPA |
| **Service** | `com.hogwarts.filmes.service` | Regras de negócio e integração TMDB |
| **Controller** | `com.hogwarts.filmes.controller` | Endpoints REST da API |

---

## 🛠️ Tecnologias Utilizadas

| Tecnologia | Versão | Função |
|------------|--------|--------|
| Java | 21 | Linguagem principal |
| Spring Boot | 3.2.5 | Framework web |
| Spring Data JPA | 3.2.5 | Persistência ORM |
| Hibernate | 6.4.4 | Implementação JPA |
| MySQL | 8.0+ | Banco de dados |
| Lombok | 1.18+ | Redução de boilerplate |
| Jackson | 2.15.4 | Serialização JSON |
| TMDB API | v3 | Catálogo de filmes |
| Maven | 3.9+ | Gerenciador de dependências |

---

## 🗄️ Banco de Dados

O banco `hogwarts_db` é criado automaticamente pelo Hibernate. Estrutura das tabelas:

```sql
-- Tabela de clientes
CREATE TABLE clientes (
    id               BIGINT       NOT NULL AUTO_INCREMENT,
    nome             VARCHAR(100) NOT NULL,
    cpf              VARCHAR(14)  NOT NULL UNIQUE,
    data_nascimento  DATE         NOT NULL,
    email            VARCHAR(150) NOT NULL UNIQUE,
    PRIMARY KEY (id)
);

-- Tabela de filmes
CREATE TABLE filmes (
    id               BIGINT       NOT NULL AUTO_INCREMENT,
    tmdb_id          BIGINT       UNIQUE,
    titulo           VARCHAR(200) NOT NULL,
    sinopse          TEXT,
    poster_url       VARCHAR(300),
    nota_media       FLOAT(53),
    ano_lancamento   VARCHAR(255),
    PRIMARY KEY (id)
);

-- Tabela de acessos (relacionamento N:N)
CREATE TABLE acessos (
    id           BIGINT    NOT NULL AUTO_INCREMENT,
    cliente_id   BIGINT    NOT NULL,
    filme_id     BIGINT    NOT NULL,
    data_acesso  DATETIME(6),
    PRIMARY KEY (id),
    FOREIGN KEY (cliente_id) REFERENCES clientes(id),
    FOREIGN KEY (filme_id)   REFERENCES filmes(id)
);
```

---

## 🔌 Endpoints da API REST

Base URL: `http://localhost:8081/api`

### Clientes
| Método | Endpoint | Descrição |
|--------|----------|-----------|
| `POST` | `/api/clientes` | Cadastrar novo cliente |
| `GET` | `/api/clientes` | Listar todos os clientes |
| `GET` | `/api/clientes/{id}` | Buscar cliente por ID |
| `PUT` | `/api/clientes/{id}` | Atualizar cliente |
| `DELETE` | `/api/clientes/{id}` | Remover cliente |

### Filmes
| Método | Endpoint | Descrição |
|--------|----------|-----------|
| `GET` | `/api/filmes/buscar?titulo=` | Buscar filmes na TMDB |
| `POST` | `/api/filmes` | Salvar filme no banco |
| `GET` | `/api/filmes` | Listar filmes salvos |
| `GET` | `/api/filmes/{id}` | Buscar filme por ID |
| `PUT` | `/api/filmes/{id}` | Atualizar filme |
| `DELETE` | `/api/filmes/{id}` | Remover filme |

### Acessos
| Método | Endpoint | Descrição |
|--------|----------|-----------|
| `POST` | `/api/acessos` | Registrar acesso |
| `GET` | `/api/acessos` | Listar todos os acessos |
| `GET` | `/api/acessos/cliente/{id}` | Filmes de um cliente |
| `DELETE` | `/api/acessos/{id}` | Revogar acesso |

---

## ▶️ Como Executar

### Pré-requisitos
- Java 21+
- MySQL 8.0+
- Eclipse IDE (ou IntelliJ)
- Chave gratuita da [API TMDB](https://www.themoviedb.org/settings/api)

### Passo a passo

**1. Clone o repositório**
```bash
git clone https://github.com/seu-usuario/hogwarts-filmes.git
```

**2. Configure o `application.properties`**
```properties
spring.datasource.url=jdbc:mysql://localhost:3306/hogwarts_db?createDatabaseIfNotExist=true
spring.datasource.username=root
spring.datasource.password=SUA_SENHA_MYSQL
tmdb.api.key=SUA_CHAVE_TMDB
server.port=8081
```

**3. Execute o projeto**

No Eclipse: botão direito em `FilmesApplication.java` → **Run As → Spring Boot App**

**4. Acesse o sistema**
```
http://localhost:8081
```

---

## 📁 Estrutura do Projeto

```
hogwarts-filmes/
├── pom.xml
└── src/main/
    ├── java/com/hogwarts/filmes/
    │   ├── FilmesApplication.java
    │   ├── config/AppConfig.java
    │   ├── controller/
    │   │   ├── ClienteController.java
    │   │   ├── FilmeController.java
    │   │   └── AcessoController.java
    │   ├── service/
    │   │   ├── ClienteService.java
    │   │   ├── FilmeService.java
    │   │   └── AcessoService.java
    │   ├── repository/
    │   │   ├── ClienteRepository.java
    │   │   ├── FilmeRepository.java
    │   │   └── AcessoRepository.java
    │   └── model/
    │       ├── Cliente.java
    │       ├── Filme.java
    │       └── Acesso.java
    └── resources/
        ├── application.properties
        └── static/index.html
```

---

## 👥 Autora

| Nome | 
|------|
| Ellen Silva Reis Rangel |

---

## 📚 Disciplina

**Backend Frameworks** — Curso de ADS  
Turma: GCE0400103NNB | Entrega: 03/06/2026

---

## 📄 Licença

Este projeto foi desenvolvido para fins educacionais.
