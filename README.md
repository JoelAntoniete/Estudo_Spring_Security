# 🛡️ Spring Security Deep Dive

## 🔐 Autenticação JWT e Autorização em API REST

------------------------------------------------------------------------

## 📌 Sobre o Projeto

Este repositório contém uma implementação robusta e didática de
segurança para APIs Java utilizando Spring Security 3 com autenticação
baseada em JWT (JSON Web Token).

O objetivo principal foi compreender profundamente o funcionamento
interno do Spring Security e transformar seu comportamento padrão em uma
configuração stateless, personalizada e escalável.

------------------------------------------------------------------------

# 🚀 Tecnologias e Ferramentas

-   ☕ Java 17+
-   🌱 Spring Boot 3.5.6
-   🔐 Spring Security
-   🗄️ Spring Data JPA
-   🐘 PostgreSQL
-   🛠️ Flyway Migration
-   🎟️ Java JWT (Auth0)
-   🧩 Lombok
-   ✅ Bean Validation
-   📦 Maven

------------------------------------------------------------------------

# 🧠 Arquitetura de Segurança

## 1️⃣ O Contrato `UserDetails`

A entidade `User` implementa a interface `UserDetails`, permitindo que o
Spring Security:

-   👤 Identifique o usuário
-   🔑 Acesse a senha criptografada
-   🏷️ Gerencie permissões (Authorities)
-   🔍 Controle status da conta

Configuração adotada:

-   ✔ Conta não expirada\
-   ✔ Conta não bloqueada\
-   ✔ Sempre habilitada

------------------------------------------------------------------------

## 2️⃣ Configuração Stateless e Filtros Customizados

A aplicação foi configurada para ser totalmente stateless (sem uso de
sessões).

### 🔒 CSRF

Desativado, pois a autenticação via token já protege APIs REST.

### 🌍 CORS

Configurado para integração com front-ends modernos (React, Angular,
etc.).

### 🧵 Security Filter Chain

-   `/auth/register` → Público\
-   `/auth/login` → Público\
-   Demais rotas → 🔐 Protegidas

------------------------------------------------------------------------

## 3️⃣ Filtro Customizado

Implementado com `OncePerRequestFilter`.

Responsabilidades:

-   Interceptar requisições
-   Validar header Authorization
-   Validar token JWT
-   Autenticar usuário no contexto do Spring

Retorno em caso de erro:

HTTP 403 Forbidden

------------------------------------------------------------------------

# 🔄 Fluxo Interno de Autenticação

1️⃣ AuthenticationManager recebe credenciais\
2️⃣ Delegação ao UserDetailsService\
3️⃣ DaoAuthenticationProvider valida senha com BCrypt\
4️⃣ TokenConfig gera JWT assinado com HMAC256

------------------------------------------------------------------------

# 🛠️ Banco de Dados com Flyway

Script inicial:

``` sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    email VARCHAR(255) NOT NULL UNIQUE,
    password VARCHAR(255) NOT NULL
);
```

Benefícios:

-   Versionamento de banco
-   Histórico de migrações
-   Integridade de schema

------------------------------------------------------------------------

# 🛣️ Endpoints

  Método   Endpoint         Acesso      Descrição
  -------- ---------------- ----------- ------------------
  POST     /auth/register   Público     Registra usuário
  POST     /auth/login      Público     Retorna JWT
  GET      /test            Protegido   Valida token

------------------------------------------------------------------------

# 🧪 Como Testar

1.  Registrar usuário: POST /auth/register

2.  Realizar login: POST /auth/login

3.  Enviar token no header:

Authorization: Bearer `<seu_token_jwt>`{=html}

Se inválido: HTTP 403 Forbidden

------------------------------------------------------------------------

# ▶ Como Executar

``` bash
mvn clean install
mvn spring-boot:run
```

A aplicação estará disponível em: http://localhost:8080

------------------------------------------------------------------------

# 🎯 Objetivo do Projeto

Aprofundamento em:

-   Segurança de APIs
-   Arquitetura Stateless
-   Spring Security interno
-   JWT manual
-   Flyway

------------------------------------------------------------------------

# 🚀 Roadmap

-   Roles (USER / ADMIN)
-   Refresh Token
-   Testes automatizados
-   Dockerização
-   Deploy cloud

------------------------------------------------------------------------

# 👨‍💻 Autor

Joel Neto\
Graduando em Engenharia de Software (conclusão em 2026)\
Foco em Backend Java
