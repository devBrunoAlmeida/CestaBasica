# 🧺 Sistema Gerenciador de Cesta Básica

Plataforma corporativa completa para gestão de programas sociais e distribuição de cestas básicas, integrando controle de lotes em estoque, baixa ágil por leitura óptica de código de barras e comunicação automatizada via WhatsApp.
Desenvolvida em **.NET 8** com arquitetura desacoplada entre **ASP.NET Core Web API**, interface interativa em **Blazor Server** estilizada com **Tailwind CSS v4** e persistência em **PostgreSQL 15**.

![.NET 8](https://img.shields.io/badge/.NET%208-512BD4?style=for-the-badge&logo=dotnet&logoColor=white)
![Blazor Server](https://img.shields.io/badge/Blazor%20Server-512BD4?style=for-the-badge&logo=blazor&logoColor=white)
![ASP.NET Core Web API](https://img.shields.io/badge/ASP.NET%20Core%20API-512BD4?style=for-the-badge&logo=dotnet&logoColor=white)
![PostgreSQL 15](https://img.shields.io/badge/PostgreSQL%2015-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Tailwind CSS v4](https://img.shields.io/badge/Tailwind%20CSS%20v4-06B6D4?style=for-the-badge&logo=tailwindcss&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Evolution API v2](https://img.shields.io/badge/Evolution%20API%20v2-25D366?style=for-the-badge&logo=whatsapp&logoColor=white)

---

## 📋 Índice

- [Visão Geral](#-visão-geral)
- [Interface & Experiência do Usuário (UI/UX)](#-interface--experiência-do-usuário-uiux)
- [Capturas de Tela](#-capturas-de-tela)
- [Principais Funcionalidades](#-principais-funcionalidades)
- [Arquitetura e Estrutura do Projeto](#-arquitetura-e-estrutura-do-projeto)
- [Tecnologias Utilizadas](#-tecnologias-utilizadas)
- [Pré-requisitos](#-pré-requisitos)
- [Como Executar o Projeto](#-como-executar-o-projeto)
- [Carga de Dados de Teste (Seed)](#-carga-de-dados-de-teste-seed)
- [Integração com WhatsApp (Evolution API)](#-integração-com-whatsapp-evolution-api)
- [Importação e Exportação de Funcionários](#-importação-e-exportação-de-funcionários)
- [Principais Endpoints da API](#-principais-endpoints-da-api)

---

## 🎯 Visão Geral

O **Sistema Gerenciador de Cesta Básica** automatiza e audita todo o ciclo operacional de entrega de benefícios a servidores e funcionários:

1. **Gestão de Lotes e Estoque**: Controle de lotes mensais de cestas básicas com acompanhamento de saldo disponível em tempo real.
2. **Identificação Instantânea**: Localização de beneficiários por **leitor de código de barras USB**, matrícula ou nome, eliminando filas e impedindo retiradas duplicadas na mesma competência.
3. **Comunicação Direta via WhatsApp**: Disparo de avisos e convocações integrado à **Evolution API v2**, com pareamento via QR Code ou código numérico diretamente pelo painel web.
4. **Visão Gerencial e Auditoria**: Dashboard analítico com indicadores em tempo real, evolução mensal de entregas, distribuição por secretaria/setor e linha do tempo por funcionário.

---

## 🎨 Interface & Experiência do Usuário (UI/UX)

O frontend (`CestaBasica.Web`) foi projetado com foco em usabilidade corporativa, acessibilidade e performance operacional:

- **Design System Moderno & Limpo**: Construído com **Tailwind CSS v4**, tipografia **Plus Jakarta Sans**, paleta institucional em tons `Slate` e `Blue`, badges semânticos em formato *pill* e iconografia 100% em **SVG vetorial**.
- **Responsividade Mobile-First**: Layout adaptativo desde smartphones (`360px`) com Top Bar e navegação em **Drawer sobreposto** até monitores widescreen (`2xl`) com Sidebar fixa e tabelas com rolagem horizontal suave.
- **Autenticação Split-Screen**: Tela de login dividida entre painel institucional e formulário com seletor rápido de perfil (*Segmented Control*: `Operador` / `Administrador`).
- **Operação Otimizada para Atendimento**: Tela de registro de retiradas com foco imediato para leitores ópticos USB (EAN-13) e confirmação em um clique.
- **Central WhatsApp Interativa**: Pré-visualização em tempo real da mensagem em formato de balão do WhatsApp e monitoramento automático de conexão do aparelho.

---

## 📸 Capturas de Tela

> Salve as imagens na pasta `docs/images/` com os nomes abaixo para exibi-las automaticamente no repositório:

| Tela de Login (Split-Screen) | Dashboard Gerencial |
| :---: | :---: |
| ![Tela de Login](docs/images/login.png) | ![Dashboard Gerencial](docs/images/dashboard.png) |

| Registro de Retiradas (Leitor Código de Barras) | Central de Notificações & WhatsApp |
| :---: | :---: |
| ![Registro de Retiradas](docs/images/retiradas.png) | ![Central de Notificações](docs/images/notificacoes.png) |

---

## ✨ Principais Funcionalidades

- **📊 Dashboard Gerencial (`/dashboard`)**:
  - 4 KPIs em tempo real: Total de Funcionários, Total de Retiradas, Cestas Disponíveis e Notificações Enviadas.
  - Filtro dinâmico por período (Data Início e Data Fim).
  - Gráfico de evolução de retiradas nos últimos 6 meses e barras de progresso com distribuição percentual por secretaria/setor.
  - Resumo operacional das últimas retiradas e comunicações recentes.

- **👥 Gestão de Funcionários (`/funcionarios`, `/novo`, `/importacao`, `/funcionarios/{id}/historico`)**:
  - Cadastro, edição inline na tabela, exclusão e filtros combinados (busca textual, status de retirada, secretaria/setor e intervalo de datas).
  - **Histórico Individual Completo**: Dossiê do funcionário com perfil, histórico de retiradas, histórico de notificações e linha do tempo cronológica de eventos.
  - **Importação em Lote**: Upload de planilhas Excel (`.xlsx`, `.xls`) ou arquivos `.csv` com pré-visualização, validação linha a linha e histórico de importações.
  - **Exportação**: Download imediato da base cadastral em formato Excel (`.xlsx`).

- **🧺 Controle de Estoque de Cestas (`/cestas`)**:
  - Card de destaque para o **Lote Ativo Vigente** com barra de progresso do saldo disponível.
  - Cadastro de novos lotes mensais, edição inline de informações/quantidades e ativação/desativação de lotes.

- **📦 Registro de Retiradas (`/retiradas`)**:
  - Identificação rápida via leitor de código de barras ou busca manual com autocomplete por nome e matrícula.
  - Card de conferência dos dados do funcionário, atualização automática de status para `Retirado` e dedução do estoque vigente.
  - Tabela inferior com o histórico detalhado das retiradas mais recentes.

- **🔔 Central de Notificações e WhatsApp (`/notificacoes`)**:
  - Painel de conexão com a **Evolution API v2**: configuração de servidor, geração de **QR Code** com renovação automática ou **Código de Pareamento**, exibição do perfil/número conectado e envio de mensagem de teste.
  - Seleção em lote de destinatários com filtro rápido, editor de mensagem com contador de caracteres e pré-visualização estilo WhatsApp.

- **👤 Administração de Usuários e Configurações (`/usuarios`, `/configuracoes`)**:
  - Controle de acesso baseado em perfis (`Admin` e `Usuario`) com senhas criptografadas via **BCrypt**.
  - Modal de criação/edição de usuários com definição de perfil, status (`Ativo`/`Inativo`) e tags de permissões.
  - Painel de configurações gerais de canais de notificação, tempo de sessão, rotinas de backup, políticas de segurança e logs administrativos.

---

## 🏗️ Arquitetura e Estrutura do Projeto

A solução (`CestaBasica.sln`) segue uma arquitetura em camadas separando responsabilidades entre API REST, Frontend Blazor Server e Contratos Compartilhados:

```text
CestaBasica/
├── CestaBasica.Api/                  # Backend REST API (ASP.NET Core 8)
│   ├── Applications/                 # Orquestração de casos de uso da aplicação
│   ├── Controllers/                  # Endpoints REST (Dashboard, Funcionarios, Cestas, Retiradas, etc.)
│   ├── Data/                         # Contexto do EF Core (AppDbContext e Factory)
│   ├── Migrations/                   # Migrações automatizadas do PostgreSQL
│   ├── Models/                       # Entidades de domínio mapeadas no banco
│   ├── Repositories/                 # Camada de persistência e consultas EF Core
│   ├── Services/                     # Regras de negócio e integrações (EvolutionApiService, ImportacaoService)
│   └── Program.cs                    # Configuração de DI, Swagger, EF Core e auto-migration
│
├── CestaBasica.Web/                  # Frontend Web (Blazor Server - .NET 8)
│   ├── Components/
│   │   ├── Layout/                   # MainLayout (Sidebar + Drawer Mobile) e EmptyLayout (Login)
│   │   ├── App.razor                 # Configuração global, Tailwind CSS v4 e Plus Jakarta Sans
│   │   └── Routes.razor              # Roteamento principal Blazor
│   ├── Models/                       # Modelos de Request/Response de autenticação
│   ├── Pages/
│   │   ├── Login/                    # Tela de autenticação Split-Screen (Login.razor)
│   │   └── Dashboard/                # Módulos do sistema:
│   │       ├── Dashboard.razor       # Painel de indicadores e gráficos
│   │       ├── Funcionarios/         # Listagem, Cadastro (/novo), Importação (/importacao) e Histórico
│   │       ├── EstoqueCesta/         # Gestão de lotes mensais (/cestas)
│   │       ├── RetiradaCesta/        # Baixa por código de barras ou busca manual (/retiradas)
│   │       ├── DispararNotificacao/  # Central WhatsApp / Evolution API (/notificacoes)
│   │       ├── Usuarios/             # Gestão de operadores e administradores (/usuarios)
│   │       └── Configuracoes/        # Parâmetros gerais do sistema (/configuracoes)
│   ├── Services/                     # SessaoUsuarioService e UsuarioApiService
│   └── wwwroot/                      # Ativos estáticos e script JS de exportação (download.js)
│
├── CestaBasica.Shared/               # Biblioteca de contratos compartilhados (API <-> Web)
│   ├── DTOs/                         # Data Transfer Objects de todos os módulos
│   └── Enums/                        # Enumeradores (PerfilUsuario: Admin, Usuario)
│
├── database/                         # Scripts de banco de dados e arquivos de exemplo
│   ├── seed_teste_postgres.sql       # Carga completa (1.060 funcionários, 6 lotes, retiradas e usuários)
│   └── exemplo_importacao_novos.csv  # Planilha CSV modelo para teste de importação
│
├── docker-compose.yml                # Serviço PostgreSQL 15 containerizado
└── start.sh                          # Script de inicialização rápida (Banco + API + Web)
```

---

## 🚀 Tecnologias Utilizadas

### Frontend & Design (`CestaBasica.Web`)
- **Blazor Web App (.NET 8)** com renderização interativa via SignalR (`InteractiveServerRenderMode`)
- **Tailwind CSS v4** para estilização utilitária, layout responsivo (*Mobile-First*) e componentes modernos
- **Plus Jakarta Sans** (Google Fonts) como tipografia principal e ícones vetoriais em **SVG**
- **JavaScript Interop** para download direto de relatórios e planilhas Excel no navegador

### Backend & Persistência (`CestaBasica.Api` & `CestaBasica.Shared`)
- **ASP.NET Core 8 Web API** (C# 12)
- **Entity Framework Core 8** + driver **Npgsql** (`Npgsql.EntityFrameworkCore.PostgreSQL`)
- **PostgreSQL 15** como banco de dados relacional
- **BCrypt.Net-Next** para hashing seguro de senhas
- **EPPlus** e **ClosedXML** para processamento, validação e geração de arquivos `.xlsx` e `.csv`
- **Swashbuckle / Swagger UI** para documentação e teste interativo dos endpoints REST

### Infraestrutura & Integrações
- **Evolution API v2** (integração `WHATSAPP-BAILEYS` via `HttpClient`) para pareamento e envio de mensagens WhatsApp
- **Docker & Docker Compose** para provisionamento do banco de dados

---

## ⚙️ Pré-requisitos

Certifique-se de ter instalado no ambiente de desenvolvimento:
- [.NET 8.0 SDK](https://dotnet.microsoft.com/download/dotnet/8.0)
- [Docker](https://www.docker.com/) e **Docker Compose**
- *(Opcional)* Instância da **Evolution API v2** na porta `8080` para disparos reais via WhatsApp

---

## ▶️ Como Executar o Projeto

### Opção 1: Execução Automatizada via `start.sh` (Linux / macOS)

O script [start.sh] sobe o container do PostgreSQL, verifica containers locais da Evolution API (se existirem), aguarda o banco aceitar conexões e inicia a API e o Frontend Web simultaneamente:

```bash
chmod +x start.sh
./start.sh
```

> As migrações do Entity Framework Core são aplicadas **automaticamente** na inicialização da API (`db.Database.Migrate()`).

### Opção 2: Execução Manual Passo a Passo

1. **Inicie o container do PostgreSQL 15**:
   ```bash
   docker compose up -d
   ```

2. **Execute o Backend (`CestaBasica.Api`)**:
   ```bash
   dotnet run --project CestaBasica.Api/CestaBasica.Api.csproj --launch-profile http
   ```

3. **Em um segundo terminal, execute o Frontend (`CestaBasica.Web`)**:
   ```bash
   dotnet run --project CestaBasica.Web/CestaBasica.Web.csproj --launch-profile http
   ```

### 🌐 Endereços Locais

| Serviço | URL / Porta | Descrição |
| :--- | :--- | :--- |
| **Aplicação Web (Blazor)** | `http://localhost:5093/login` | Interface principal do sistema |
| **API REST & Swagger UI** | `http://localhost:5237/swagger` | Documentação interativa dos endpoints |
| **PostgreSQL 15 (Docker)** | `localhost:5432` | Banco: `cestabasica_teste` (`postgres` / `postgres`) |
| **Evolution API (Opcional)** | `http://localhost:8080` | Servidor padrão para integração WhatsApp |

---

## 🗄️ Carga de Dados de Teste (Seed)

O arquivo [database/seed_teste_postgres.sql](database/seed_teste_postgres.sql) popula o banco `cestabasica_teste` com um cenário realista pronto para demonstração:
- **4 usuários** com senhas criptografadas em BCrypt;
- **6 lotes mensais** de cestas básicas (5 competências encerradas e 1 lote ativo vigente com **640 unidades disponíveis**);
- **1.060 funcionários** distribuídos entre secretarias municipais, com matrículas, códigos de barras EAN-13 e telefones;
- Histórico completo de retiradas e notificações enviadas para alimentar os gráficos do Dashboard.

Para aplicar a carga de teste com o container do banco em execução:

```bash
docker exec -i postgres-cestabasica psql -U postgres -d cestabasica_teste < database/seed_teste_postgres.sql
```

### 🔐 Credenciais de Acesso (Seed)

| Perfil | E-mail | Senha | Status |
| :--- | :--- | :--- | :--- |
| **Administrador** | `admin@cestabasica.com` | `Admin@123` | Ativo (Acesso total) |
| **Operador (Atendimento)** | `operador@cestabasica.com` | `Operador@123` | Ativo |
| **Operador (Almoxarifado)** | `triagem@cestabasica.com` | `Operador@123` | Ativo |
| **Usuário Inativo** | `inativo@cestabasica.com` | `Operador@123` | Inativo (Bloqueado no login) |

---

## 💬 Integração com WhatsApp (Evolution API)

A central de notificações (`/notificacoes`) permite gerenciar a conexão com o WhatsApp sem sair do sistema:

1. No banner **Telefone Remetente (WhatsApp)**, clique em **Configurar Evolution API** (ou **Servidor**).
2. Informe a **URL da Evolution API** (padrão: `http://localhost:8080`), a **API Key** e o **Nome da Instância** (padrão: `cesta-basica`).
3. Clique em **Salvar e Conectar Telefone** para que o sistema crie a instância automaticamente e exiba o **QR Code** na tela (ou marque a opção **Código de Pareamento** para conectar digitando o número do celular).
4. Após a leitura, o status mudará automaticamente para **Conectado** e você poderá testar o disparo no botão **Testar envio**.

---

## 📄 Importação e Exportação de Funcionários

No módulo **Funcionários** (`/funcionarios`), é possível exportar toda a base para `.xlsx` ou acessar **Importar funcionários** (`/importacao`) para carregar arquivos `.csv` (delimitados por `;` ou `,`) e planilhas `.xlsx`.

### Formato Esperado da Planilha

A primeira linha é interpretada como cabeçalho e as colunas devem seguir a ordem abaixo (exemplo disponível em [database/exemplo_importacao_novos.csv](database/exemplo_importacao_novos.csv)):

| Coluna 1 | Coluna 2 | Coluna 3 | Coluna 4 | Coluna 5 |
| :--- | :--- | :--- | :--- | :--- |
| `Nome` *(Obrigatório)* | `Matricula` *(Obrigatório)* | `CodigoBarras` *(Obrigatório / Único)* | `Telefone` | `Setor` |

```csv
Nome;Matricula;CodigoBarras;Telefone;Setor
CARLA FERNANDA ROCHA;12001;7891000120019;(15) 99744-1001;Secretaria de Educação
MARCELO HENRIQUE PRADO;12002;7891000120026;(15) 99744-1002;Secretaria de Saúde
```

---

## 🔌 Principais Endpoints da API

| Módulo | Método | Rota | Descrição |
| :--- | :---: | :--- | :--- |
| **Dashboard** | `GET` | `/api/Dashboard` | Retorna KPIs, evolução mensal, distribuição por setor e listas recentes |
| **Funcionários** | `GET` / `POST` | `/api/Funcionarios` | Lista todos os funcionários ou cadastra um novo registro |
| **Funcionários** | `GET` / `PUT` / `DELETE` | `/api/Funcionarios/{id}` | Busca por ID, atualiza dados cadastrais ou remove um funcionário |
| **Funcionários** | `GET` | `/api/Funcionarios/{id}/historico` | Retorna o histórico consolidado e timeline de eventos do funcionário |
| **Funcionários** | `GET` | `/api/Funcionarios/buscar?termo=` | Busca manual por nome ou matrícula |
| **Funcionários** | `GET` | `/api/Funcionarios/codigo/{codigoBarras}` | Localiza funcionário pela leitura do código de barras |
| **Funcionários** | `GET` | `/api/Funcionarios/setores` | Lista todas as secretarias/setores cadastrados |
| **Funcionários** | `GET` | `/api/Funcionarios/exportar-excel` | Gera e baixa a planilha `funcionarios.xlsx` |
| **Importação** | `POST` | `/api/Importacao/funcionarios` | Processa e valida upload de planilha `.csv` ou `.xlsx` |
| **Importação** | `GET` | `/api/Importacao/historico` | Lista o histórico de arquivos importados |
| **Cestas** | `GET` / `POST` | `/api/Cestas` | Lista ou cadastra lotes de cestas básicas |
| **Cestas** | `PUT` | `/api/Cestas/{id}` | Atualiza nome, descrição ou saldo de um lote |
| **Cestas** | `PUT` | `/api/Cestas/{id}/ativar` \| `desativar` | Ativa ou desativa um lote de cestas básicas |
| **Retiradas** | `POST` | `/api/Retiradas/confirmar/{funcionarioId}` | Confirma a entrega da cesta para o funcionário e baixa o estoque ativo |
| **Retiradas** | `POST` | `/api/Retiradas/registrar` | Registra retirada via DTO (`FuncionarioId` / `CodigoBarras`) |
| **Retiradas** | `GET` | `/api/Retiradas/recentes-detalhado` | Lista o histórico detalhado das retiradas mais recentes |
| **Notificações** | `GET` / `POST` | `/api/Notificacoes` \| `/api/Notificacoes/enviar` | Lista histórico ou dispara notificação para funcionário |
| **WhatsApp** | `GET` / `PUT` | `/api/Notificacoes/whatsapp/status` \| `configurar` | Consulta status da instância ou salva credenciais da Evolution API |
| **WhatsApp** | `POST` | `/api/Notificacoes/whatsapp/conectar` \| `desconectar` | Gera QR Code / Pairing Code ou encerra sessão do aparelho |
| **WhatsApp** | `POST` | `/api/Notificacoes/whatsapp/teste` | Envia mensagem de teste para validar a instância conectada |
| **Usuários** | `POST` | `/api/Usuarios/login` | Autentica usuário por e-mail, senha e perfil |
| **Usuários** | `GET` / `POST` | `/api/Usuarios/listar` \| `/api/Usuarios/criar` | Lista ou cadastra usuários do sistema |
| **Usuários** | `PUT` / `DELETE` | `/api/Usuarios/{id}` | Atualiza dados/senha/status ou exclui um usuário |
| **Configurações** | `GET` / `PUT` | `/api/Configuracoes` | Obtém ou atualiza as configurações gerais da plataforma |
