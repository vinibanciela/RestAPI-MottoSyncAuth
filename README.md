# Faculdade de Informática e Administração Paulista - FIAP/SP  

**Referência:** Global Solution 2025 – 2º Semestre – [O Futuro do Trabalho]  

**Alunos:**  
- Guilherme Gonçalves – RM558475  
- Thiago Mendes – RM555352  
- Vinicius Banciela – RM558117  

**Turma:** 2TDSPW  
## 1. FuturoJobs.Api

### 📌 Visão Geral

**Tecnologias Utilizadas**

- C#, .NET 8  
- ASP.NET Core Web API (Controllers)  
- Entity Framework Core + Migrations  
- SQL Server Express (ambiente Development)  
- Azure SQL Database (ambiente Production)  
- Swagger / OpenAPI  
- HealthChecks  
- Application Insights (telemetria e tracing)  
- EF Core InMemory (para testes de integração, via projeto separado)

**Principais Funcionalidades**

- CRUD completo de **Empresas** e **Vagas**.  
- Relacionamento **1:N** entre as entidades  
- Uma **Empresa** pode possuir **várias Vagas**.  
- Paginação nas rotas de listagem, encapsulada em `PagedResponse<T>`.  
- Suporte a **HATEOAS** nas respostas paginadas (links `self`, `nextPage`, `prevPage`).  
- Versionamento da API com `Asp.Versioning` (versão padrão v1).  
- HealthChecks para verificar a conectividade com o banco de dados.  
- Projeto separado de **testes de integração**, utilizando EF Core InMemory.  

---

### 📘 Introdução

A **FuturoJobs.Api** é a API RESTful desenvolvida como parte da **Global Solution 2025 – 2º Semestre** do curso de Tecnologia em Análise e Desenvolvimento de Sistemas da FIAP, cujo tema central é **“O Futuro do Trabalho”**.

O problema identificado é a ausência de uma plataforma especializada que:

- centralize **empresas** e **vagas** voltadas para profissões emergentes;  
- apoie um **painel / blog** com conteúdos sobre tendências do mercado;  
- permita organizar e consultar, de forma estruturada, oportunidades ligadas ao futuro do trabalho.

Dentro desse contexto, a FuturoJobs.Api é o **núcleo tecnológico** responsável por expor os dados principais da plataforma (Empresas e Vagas) via HTTP, de forma padronizada e versionada, para ser consumida por aplicações web, painéis administrativos ou futuros aplicativos mobile.

---

### 🧩 Descrição da Solução

A solução proposta é uma **API RESTful** em .NET 8 que:

- gerencia o cadastro de **Empresas** (nome, site, setor, país, cidade, descrição);  
- gerencia o cadastro de **Vagas** associadas às empresas (título, descrição, modelo de trabalho, nível, faixa salarial, data de publicação, status, `empresaId`);  
- retorna listas paginadas com metadados e links de navegação (HATEOAS);  
- fornece endpoints claros e consistentes para criação, leitura, atualização e exclusão de registros (CRUD completo).

#### 🌍 Relação com a ODS da ONU

A FuturoJobs.Api se conecta diretamente à **ODS 8 – Trabalho Decente e Crescimento Econômico**, pois:

- facilita a divulgação de vagas relacionadas a profissões do futuro;  
- aproxima empresas e profissionais em busca de qualificação e novas oportunidades;  
- pode ser base para uma plataforma que incentive a formalização, o acesso à informação e a melhoria das condições de trabalho.

---

### 📁 Estrutura de Pastas da Solução

```plaintext
FuturoJobs/
│
├── FuturoJobs.Api/                 # Projeto principal da Web API
│   ├── Controllers/
│   │   ├── EmpresasController.cs
│   │   └── VagasController.cs
│   │
│   ├── Data/
│   │   ├── AppDbContext.cs
│   │   └── Migrations/
│   │       ├── 20251122065640_InitialCreate.cs
│   │       ├── 20251122065640_InitialCreate.Designer.cs
│   │       └── AppDbContextModelSnapshot.cs
│   │
│   ├── DTOs/
│   │   ├── CreateEmpresaDTO.cs
│   │   ├── CreateVagaDTO.cs
│   │   ├── EmpresaDetailDTO.cs
│   │   ├── EmpresaDTO.cs
│   │   ├── ErrorResponse.cs
│   │   ├── LinkDTO.cs
│   │   ├── PagedResponse.cs
│   │   ├── UpdateEmpresaDTO.cs
│   │   ├── UpdateVagaDTO.cs
│   │   ├── VagaDTO.cs
│   │   └── VagaResumoDTO.cs
│   │
│   ├── Models/
│   │   ├── Empresa.cs
│   │   └── Vaga.cs
│   │
│   ├── Services/
│   │   ├── EmpresaService.cs
│   │   └── VagaService.cs
│   │
│   ├── appsettings.json
│   ├── appsettings.Development.json
│   ├── FuturoJobs.Api.csproj
│   └── Program.cs
│
├── FuturoJobs.Api.Tests/           # Projeto de testes de integração
│   ├── EmpresaTests.cs
│   ├── VagaTests.cs
│   ├── HealthTests.cs
│   ├── IntegrationTestFactory.cs
│   └── FuturoJobs.Api.Tests.csproj
│
└── scripts/                        # Scripts auxiliares (referência)
│  ├── banco/
│  │   └── script-bd.sql          # Script SQL apenas para referência (não utilizado em runtime)
│   └── infra/ 
│       ├── script-infra-database.ps1
│       └── script-infra-webapp.ps1
│
├── .gitgnore
├── FuturoJobs.sln
└── READEME.md

```

---

### 🧱 Diagrama de Alto Nível (Visão de Componentes)
```plaintext
+-------------------------------------------------------------------------+
|                         FuturoJobs.Api                                  |
|                                                                         |
|  +-----------------------+       +-----------------------+              |
|  |  EmpresasController   |       |    VagasController    |              |
|  +-----------+-----------+       +-----------+-----------+              |
|              |                               |                          |
|  +-----------v-----------+       +-----------v-----------+              |
|  |   EmpresaService      |       |     VagaService       |              |
|  +-----------+-----------+       +-----------+-----------+              |
|              \______________________/                      HATEOAS,     |
|                        |                                     DTOs       |
|             +----------v-----------+                                    |
|             |    AppDbContext      |  (Entity Framework Core)           |
|             +----------+-----------+                                    |
|                        |                                                |
|          +-------------v---------------------------+                    |
|          |   SQL Server Express / Azure SQL        |                    |
|          +-----------------------------------------+                    |
+-------------------------------------------------------------------------+
```

Consumidores previstos:
- Aplicação Web / Painel FuturoJobs
- Aplicativos ou outros serviços que precisem consultar Empresas e Vagas


## 2. Estrutura de Endpoints

### 📂 Empresas

| Método | Rota                    | Descrição                                      | Respostas HTTP                                                                 | HATEOAS                            |
|--------|-------------------------|-----------------------------------------------|-------------------------------------------------------------------------------|------------------------------------|
| POST   | `/api/v1/empresas`      | Cadastra uma nova empresa.                    | `201 Created`, `400 Bad Request` (falha de validação do modelo)               | –                                  |
| GET    | `/api/v1/empresas`      | Retorna uma lista paginada de empresas.       | `200 OK`                                                                      | Links `self`, `next-page`, `prev-page` no `PagedResponse`. |
| GET    | `/api/v1/empresas/{id}` | Obtém detalhes de uma empresa pelo ID.        | `200 OK`, `404 Not Found`                                                     | –                                  |
| PUT    | `/api/v1/empresas/{id}` | Atualiza uma empresa existente.               | `200 OK`, `400 Bad Request`, `404 Not Found`                                  | –                                  |
| DELETE | `/api/v1/empresas/{id}` | Remove uma empresa pelo identificador (ID).   | `204 No Content`, `404 Not Found`                                             | –                                  |

---

### 📂 Vagas

| Método | Rota                 | Descrição                                                          | Respostas HTTP                                                                 | HATEOAS                            |
|--------|----------------------|---------------------------------------------------------------------|-------------------------------------------------------------------------------|------------------------------------|
| POST   | `/api/v1/vagas`      | Cadastra uma nova vaga vinculada a uma empresa.                   | `201 Created`, `400 Bad Request`, `404 Not Found` (empresa vinculada não existe) | –                              |
| GET    | `/api/v1/vagas`      | Lista vagas com paginação e filtros opcionais (`modelo`, `status`, `empresaId`). | `200 OK`                                                                      | Links `self`, `next-page`, `prev-page` no `PagedResponse`. |
| GET    | `/api/v1/vagas/{id}` | Obtém os dados de uma vaga pelo ID.                               | `200 OK`, `404 Not Found`                                                     | –                                  |
| PUT    | `/api/v1/vagas/{id}` | Atualiza uma vaga existente.                                      | `200 OK`, `400 Bad Request`, `404 Not Found`                                  | –                                  |
| DELETE | `/api/v1/vagas/{id}` | Remove uma vaga pelo identificador (ID).                          | `204 No Content`, `404 Not Found`                                             | –                                  |

---

### 🩺 Healthchecks

| Método | Rota            | Descrição                                               | Respostas HTTP                   |
|--------|-----------------|---------------------------------------------------------|----------------------------------|
| GET    | `/health/live`  | Verifica se a API está viva (liveness).                | `200 OK`                         |
| GET    | `/health/ready` | Verifica se a API está pronta e com acesso ao banco.   | `200 OK`, `503 Service Unavailable` |

> 🔎 `/health/ready` utiliza `AddDbContextCheck<AppDbContext>()` para validar a conexão com o banco de dados antes de considerar a aplicação “pronta”.

---

### 📌 Observações sobre Status Codes

- **200 OK** – Operação de leitura/consulta concluída com sucesso.  
- **201 Created** – Entidade criada com sucesso (retorna o recurso criado no corpo).  
- **204 No Content** – Operação concluída com sucesso, sem conteúdo no corpo (DELETE e alguns updates).  
- **400 Bad Request** – Dados de entrada inválidos (erros de validação de modelo ou JSON malformado).  
- **404 Not Found** – Recurso não encontrado (empresa/vaga inexistente ou relacionamento inválido).  
- **503 Service Unavailable** – Healthcheck indica falha em dependência crítica (como o banco de dados).

---

### 🔗 Observação sobre o Relacionamento Empresa ↔ Vaga

- Cada **Vaga** possui um campo obrigatório `empresaId`, representando o vínculo com uma **Empresa**.
- O relacionamento é **1:N**:
  - **Uma Empresa** pode possuir **várias Vagas**;
  - **Cada Vaga** está sempre associada a **uma única Empresa**.
- Nos endpoints de Vagas:
  - Se o `empresaId` informado não existir, o POST/PUT retorna **404 Not Found**, evitando registros órfãos.
- No detalhe de empresa (`GET /api/v1/empresas/{id}`), a resposta inclui também as vagas associadas, facilitando a visualização do vínculo lógico entre os dois recursos.
---
## 3. Monitoramento e Observabilidade da API

A **FuturoJobs.Api** implementa um conjunto de mecanismos de observabilidade para garantir visibilidade do estado da aplicação, tanto em execução local quanto no ambiente da Azure.

### 🔍 3.1 Health Checks

A API possui **dois endpoints de health check**, configurados no `Program.cs`:

- **`/health/live`**  
  Verifica apenas se a API está em funcionamento.  
  Não realiza nenhuma dependência externa.  
  Usado para **liveness probe**.

- **`/health/ready`**  
  Verifica a conectividade com o banco de dados SQL Server.  
  Inclui o **DbContextCheck**, garantindo que o serviço está pronto para receber tráfego.  
  Usado para **readiness probe**.

### 📝 3.2 Logging

O sistema utiliza o pipeline de logging padrão do **ASP.NET Core**, com níveis configurados em:

- `appsettings.json`
- `appsettings.Development.json`

O template de logging inclui:
- Log de informação para inicialização e migrações.
- Log de erro caso a aplicação falhe ao aplicar migrations.
- Log estruturado padrão do ASP.NET Core para requisições HTTP.

Além disso, na Azure os logs ficam disponíveis automaticamente via **Log Stream** e **Application Logs**.

### 🔎 3.3 Tracing e Telemetria

A API possui integração nativa com **Application Insights**:

```csharp
builder.Services.AddApplicationInsightsTelemetry();

```
---
## 4. Versionamento da API

A FuturoJobs.Api utiliza o **API Versioning nativo do ASP.NET Core**, configurado no `Program.cs`.  
A versão padrão é **v1**, e quando o cliente não informa a versão, a API automaticamente assume essa versão.
O versionamento é aplicado diretamente na rota, seguindo o padrão:

- api/v1/empresas

- api/v1/vagas
---
## 5. Integração e Persistência

A **FuturoJobs.Api** utiliza o **Entity Framework Core** como camada de persistência, permitindo mapear as entidades `Empresa` e `Vaga` para o banco de dados de forma automática e segura.

---


### 🖥️ 5.1 Ambiente Local — SQL Server LocalDB (SQL Express)

Durante o desenvolvimento, a aplicação utiliza o SQL Server Local via **LocalDB/SQL Express**, configurado no `appsettings.Development.json` através da connection string (você pode alterar):

```json
"DefaultConnection": "Server=localhost\\SQLEXPRESS;Database=FuturoJobsDb;Trusted_Connection=True;TrustServerCertificate=True;"
```
Esse banco é usado automaticamente quando a API roda localmente.
As migrations são aplicadas na inicialização da API por meio de:

```csharp
db.Database.Migrate();
```
---
### ☁️ 5.2 Ambiente de Produção — Azure SQL Database
No ambiente de produção (Azure App Service), a API utiliza um banco Azure SQL.
A connection string é configurada no portal Azure em (pode variar dependendo do serviço):

Serviço (Web App ou outro) -> Configuration → Variables → Cadeia de Conexão

A API detecta o ambiente e aplica as migrations normalmente, garantindo que o schema esteja sincronizado na Azure.

---

### 🛠️ 5.3 Entity Framework Core — Integração
- Contexto: AppDbContext

- Mapeamento automático das entidades via DbSet<Empresa> e DbSet<Vaga>

- Migrations geradas dentro da pasta Migrations/

- Migrations aplicadas automaticamente no startup (exceto no ambiente de Testes)

- Suporte completo a:

  - CRUD

  - Validação

  - Relacionamentos entre Empresa e Vaga (1:N)

  - Essa abordagem unifica o fluxo de persistência entre os ambientes local e de produção, garantindo consistência e portabilidade.
---
## 6. Testes Integrados

A solução **FuturoJobs** inclui um projeto dedicado aos testes integrados:

📁 `FuturoJobs.Api.Tests`  
Este projeto sobe a API de verdade em memória (via `WebApplicationFactory<Program>`) e faz chamadas HTTP reais com `HttpClient`, garantindo que os endpoints funcionem ponta a ponta.

### 🧪 6.1 Ambiente de Teste — EF Core InMemory

Nos testes, o `AppDbContext` é reconfigurado para usar **Entity Framework Core InMemory**, em vez do SQL Server:

- O ambiente é forçado para `"Testing"`;
- As configurações de `DbContextOptions<AppDbContext>` são removidas e recriadas;
- Cada execução usa um banco em memória com nome único;
- O schema é criado automaticamente com `EnsureCreated()`.

Assim, os testes não dependem do SQL Express nem do Azure SQL e podem rodar em qualquer máquina ou pipeline.

### 📂 6.2 Estrutura do Projeto de Testes

Dentro de `FuturoJobs.Api.Tests` temos:

- `IntegrationTestFactory.cs` → fábrica que inicializa a API em modo de teste com banco InMemory;
- `EmpresasTests.cs` → cenários integrados para **Empresas**;
- `VagasTests.cs` → cenários integrados para **Vagas**;
- `HealthCheckTests.cs` → cenários integrados para os **HealthChecks**.

### ✔️ 6.3 Escopo dos 8 Testes Implementados

**Empresas (`EmpresasTests`)**

1. **POST /api/v1/empresas** — deve criar uma empresa com sucesso (`201 Created`);
2. **GET /api/v1/empresas?page=1&pageSize=5** — deve retornar a lista paginada (`200 OK`);
3. **DELETE /api/v1/empresas/99999** — ID inexistente deve retornar `404 Not Found`.

**Vagas (`VagasTests`)**

4. **POST /api/v1/vagas** — cria uma vaga com sucesso, usando uma empresa base criada no próprio teste (`201 Created`);
5. **GET /api/v1/vagas?page=1&pageSize=5** — deve retornar a lista paginada (`200 OK`);
6. **DELETE /api/v1/vagas/99999** — ID inexistente deve retornar `404 Not Found`.

**HealthChecks (`HealthCheckTests`)**

7. **GET /health/ready** — valida que o health de prontidão responde com `200 OK`;
8. **GET /health/live** — valida que a API está viva, retornando `200 OK` e o texto `"API está viva!"`.

Esses testes garantem que os fluxos principais de **Empresas**, **Vagas** e **HealthChecks** estejam funcionando corretamente, sem depender da infraestrutura externa, e são ideais para rodar em pipelines de CI/CD.

---
## 7. Testes Manuais da API

Além dos testes integrados automatizados, a **FuturoJobs.Api** possui um roteiro simples de **testes manuais** para ser executado via Swagger, Postman ou ferramenta similar.  
A sequência abaixo valida o CRUD completo de **Empresas** e **Vagas**, bem como o vínculo entre elas.

> ⚠️ Recomenda-se iniciar com o banco vazio, para garantir que os IDs comecem em `1`.

---

### 📝 Roteiro de Teste (Empresas + Vagas)

Este roteiro garante a validação manual do CRUD de Empresas e Vagas, incluindo o relacionamento e o comportamento esperado dos códigos HTTP.

| # | Ação | Método | Endpoint | Body (Exemplo) | Esperado |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **1.** | **Criar uma Empresa** | **POST** | `/api/v1/empresas` | `{"nome": "OpenAI", "website": "https://openai.com", "setor": "Tecnologia", "pais": "EUA", "cidade": "São Francisco", "descricao": "Empresa de IA"}` | **201 Created** e empresa com **id = 1**. |
| **2.** | **Verificar na lista paginada** | **GET** | `/api/v1/empresas?pageNumber=1&pageSize=10` | N/A | **200 OK** contendo a empresa **“OpenAI”** na lista. |
| **3.** | **Atualizar a empresa** | **PUT** | `/api/v1/empresas/1` | `{"nome": "Google", "website": "https://google.com", "setor": "Tecnologia", "pais": "EUA", "cidade": "São Francisco", "descricao": "Empresa de Tech"}` | **200 OK** ou **204 No Content** e dados atualizados. |
| **4.** | **Verificar detalhes atualizados** | **GET** | `/api/v1/empresas/1` | N/A | **200 OK** com o nome **“Google”** e demais campos atualizados. |
| **5.** | **Criar uma vaga (vinculada a empresa 1)** | **POST** | `/api/v1/vagas` | `{"titulo": "Desenvolvedor C#", "descricao": "API .NET", "modelo": "Remoto", "nivel": "Pleno", "faixaSalarial": "8k - 12k", "status": "Ativa", "empresaId": 1}` | **201 Created** e vaga com **id = 1**. |
| **6.** | **Verificar lista paginada de vagas** | **GET** | `/api/v1/vagas?pageNumber=1&pageSize=10` | N/A | **200 OK** com a vaga **“Desenvolvedor C#”** na lista. |
| **7.** | **Atualizar a vaga** | **PUT** | `/api/v1/vagas/1` | `{"titulo": "Desenvolvedor Java", "descricao": "API SpringBoot", "modelo": "Híbrido", "nivel": "Sênior", "faixaSalarial": "8k - 12k", "status": "Ativa", "empresaId": 1}` | **200 OK** ou **204 No Content** com a vaga atualizada. |
| **8.** | **Verificar detalhes da vaga atualizada** | **GET** | `/api/v1/vagas/1` | N/A | **200 OK** exibindo o novo título **“Desenvolvedor Java”**. |
| **9.** | **Confirmar vínculo (vagas na empresa)** | **GET** | `/api/v1/empresas/1` | N/A | **200 OK** com os dados da empresa e a vaga vinculada (se a resposta incluir a coleção de vagas). |
| **10.** | **Excluir a vaga** | **DELETE** | `/api/vagas/1` | N/A | **204 No Content** ou **200 OK** indicando exclusão bem-sucedida. |
| **11.** | **Excluir a empresa** | **DELETE** | `/api/v1/empresas/1` | N/A | **204 No Content** ou **200 OK**. Após isso, `GET /api/v1/empresas/1` deve retornar **404 Not Found**. |

Esse roteiro garante a validação manual de:

- CRUD completo de Empresas;

- CRUD completo de Vagas;

- Relacionamento Empresa ↔ Vaga;

- Comportamento esperado de códigos HTTP ao longo do ciclo de vida dos registros.

---

## 8. Guia de Execução Local

Este guia descreve como rodar a solução **FuturoJobs** localmente utilizando o .NET SDK 8.0 e o SQL Server Express (configuração padrão do projeto).
> ⚠️ Você pode configurar outro banco de sua preferência em *appsettings.Development.json* -> *ConnectionStrings* -> *DefaultConnection*

```
1 - Clonar a solução
AzureRepos(https ou ssh):
git clone https://RM558117@dev.azure.com/RM558117/Global%20Solution%202%20-%20Devops/_git/Global%20Solution%202%20-%20Devops
ou
git clone git@ssh.dev.azure.com:v3/RM558117/Global%20Solution%202%20-%20Devops/Global%20Solution%202%20-%20Devops
ou
Github:
git clone 

2 - Abrir a solução
cd FuturoJobs

3 - Restaurar as dependências da solução
dotnet restore

4 - Compilar os executáveis da solução
dotnet build 

5 - Testar o projeto de teste
dotnet test
Obs: Os testes utilizam EF Core InMemory e não dependem do SQL Express ou do appsettings.Development.json 

6 - Abrir o projeto da API
cd FuturoJobs.Api

7 - Rodar o projeto da API
dotnet run 
Obs: Alterar a DefaultConnection em appsettings.Development.json caso queira utilizar outro banco para teste local. Atualmente está configurado com a string padrão do SQL Express: "Server=localhost\\SQLEXPRESS;Database=FuturoJobsDb;Trusted_Connection=True;TrustServerCertificate=True;"

8 - Disponível em:
http://localhost:5216

9 - Acessar a Documentação Interativa em: 
http://localhost:5216/swagger/index.html

10 - Parar a aplicação
Cntrl + C no terminal que está rodando

11 - Limpar o banco de dados local 
dotnet ef database drop

```

---
## 9. Guia de Deploy na Azure

Este guia descreve como deployar a solução **FuturoJobs** na plataforma Azure DevOps de forma simplificada.

```
1 - Entrar na conta da Azure
az login

2 - Criar o AzureSQL - Server e Database (PAAS)
az group create --name rg-futurojobs-database --location brazilsouth
az sql server create -l brazilsouth -g rg-futurojobs-database -n sqlserver-futurojobs -u admsql -p devops@FuturoJobs --enable-public-network true
az sql db create -g rg-futurojobs-database -s sqlserver-futurojobs -n futurojobsdb-prod --service-objective Basic --backup-storage-redundancy Local --zone-redundant false
az sql server firewall-rule create -g rg-futurojobs-database -s sqlserver-futurojobs -n AllowAll --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
az sql db show-connection-string -s sqlserver-futurojobs -n futurojobsdb-prod -c ado.net
Obs: é preciso guardar essa conexão para configurar na pipeline.

3 - Criar o Web App
az provider register --namespace Microsoft.Web
az group create --name rg-futurojobs-app --location brazilsouth
az appservice plan create --name plan-futurojobs-app --resource-group rg-futurojobs-app --location brazilsouth --is-linux --sku B1
az --% webapp create --resource-group rg-futurojobs-app --plan plan-futurojobs-app --name app-futurojobs --runtime "DOTNETCORE|8.0"

4 - Configurar a String de Conexão do DB no Web App em variável de ambiente (como Cadeia de Conexão protegida)
Server=tcp:sqlserver-futurojobs.database.windows.net,1433;Initial Catalog=futurojobsdb-prod;Persist Security Info=False;User ID=admsql;Password=devops@FuturoJobs;MultipleActiveResultSets=False;Encrypt=true;TrustServerCertificate=False;Connection Timeout=30;

5 - Criar a Pipeline de CI
Criar pipeline em modo clássico
Selecionar Repositório
Selecionar template .NET Core
Configurar o Agent Job (Azure Pipelines / ubuntu-latest)
Adicionar as 4 tarefas principais: Restore, Build, Test, Publish
Adicionar Publish Build Artifacts
Ativar gatilho de CI para a branch main
Salvar e Executar (Save & Queue)

6 - Criar a Pipeline de Release
Criar pipeline em modo clássico
Selecionar template “Azure App Service Deploy”
Criar um stage
Configurar Agente Job
Selecionar o artefato vindo da pipeline de CI
Ativar Trigger da Release
Salvar a Release Pipeline
Criar nova Release
Executar Deploy

7 - Disponível em: 
app-futurojobs.azurewebsites.net

8 - Acessar a documentação Interativa em:
https://app-futurojobs.azurewebsites.net/swagger/index.html

9 - Excluir Grupos de Recursos
az group delete --name rg-futurojobs-database
az group delete --name rg-futurojobs-app

10 - Excluir Pipelines (se houver, Projeto) pela interface gráfica do Azure DevOps

```

---
## 10. Licença

Projeto desenvolvido exclusivamente para fins acadêmicos na FIAP — Global Solution 2025.  

O uso, cópia ou distribuição não é permitido sem autorização dos autores.


