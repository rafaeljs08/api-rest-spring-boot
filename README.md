# Parcelamentos API

API REST em **Spring Boot** para controle de parcelamentos, com persistência em **MySQL** e migrações versionadas com **Flyway**. Projeto acadêmico do curso de Sistemas de Informação da FEPI.

> **Estado atual:** o cadastro de **clientes** está completo (CRUD com validação). A entidade `Parcelamento` ainda é só um esqueleto e será o próximo passo.

---

## Tecnologias

| Tecnologia | Uso |
|------------|-----|
| Java 21 | Linguagem |
| Spring Boot 3.5.5 | Framework (Web, Data JPA, Validation) |
| MySQL | Banco de dados relacional |
| Flyway | Migrações do banco |
| Lombok | Redução de código repetitivo |
| Jackson XML | Respostas também em XML (`Accept: application/xml`) |
| Maven (`mvnw`) | Build e dependências |

---

## Endpoints

Base: `http://localhost:8080`

| Método | Rota | Descrição | Resposta |
|--------|------|-----------|----------|
| `GET` | `/clientes` | Lista os clientes | `200` |
| `GET` | `/clientes/{id}` | Busca um cliente | `200` ou `404` |
| `POST` | `/clientes` | Cadastra um cliente | `201` |
| `PUT` | `/clientes/{id}` | Atualiza um cliente | `200` ou `404` |
| `DELETE` | `/clientes/{id}` | Remove um cliente | `204` ou `404` |

Uma regra de negócio violada devolve `400` com a mensagem no corpo.

### Cliente

| Campo | Regra |
|-------|-------|
| `nome` | obrigatório, até 60 caracteres |
| `email` | obrigatório, formato de e-mail válido, de 10 a 255 caracteres |
| `telefone` | obrigatório, até 20 caracteres |

```bash
curl -X POST http://localhost:8080/clientes \
  -H "Content-Type: application/json" \
  -d '{"nome": "Maria Silva", "email": "maria.silva@example.com", "telefone": "35999990000"}'
```

---

## Como executar

Requer **Java 21** e um **MySQL** em execução.

1. Confira a conexão em `src/main/resources/application.properties`. O padrão é `localhost:3306`, banco `parcelamentos` (criado automaticamente), usuário `root` e senha vazia. Ajuste ao seu ambiente.
2. Inicie a aplicação:

```bash
./mvnw spring-boot:run        # Windows: mvnw.cmd spring-boot:run
```

Na primeira execução o Flyway cria a tabela `cliente` e carrega dados de exemplo (`src/main/resources/db/migration`).

---

## Estrutura

```
src/main/java/br/fepi/si/parcelamentos/
├── ParcelamentosApplication.java
├── api/controller/      # Endpoints REST (ClienteController)
└── domain/
    ├── exception/       # Exceções de negócio
    ├── model/           # Entidades JPA (Cliente, Parcelamento)
    ├── repository/      # Repositórios Spring Data
    └── service/         # Regras de negócio (CadastroClienteService)
src/main/resources/
├── application.properties
└── db/migration/        # V001 a V003 (Flyway)
```
