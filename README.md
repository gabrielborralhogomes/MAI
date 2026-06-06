# 🧠 MAI — Matching & AI Intelligence

<p align="center">
  <img src="https://img.shields.io/badge/React-19-61DAFB?style=for-the-badge&logo=react" />
  <img src="https://img.shields.io/badge/TypeScript-6-3178C6?style=for-the-badge&logo=typescript" />
  <img src="https://img.shields.io/badge/Node.js-Express-339933?style=for-the-badge&logo=node.js" />
  <img src="https://img.shields.io/badge/Gemini-Vertex%20AI-4285F4?style=for-the-badge&logo=google" />
  <img src="https://img.shields.io/badge/SQL%20Server-PostgreSQL-CC2927?style=for-the-badge&logo=microsoftsqlserver" />
  <img src="https://img.shields.io/badge/Status-Private%20Project-red?style=for-the-badge" />
</p>

> O código-fonte é proprietário e confidencial. Este repositório serve como referência de portfólio.
> The source code is proprietary. This repository serves as a portfolio reference only.

---

Plataforma SaaS B2B para **comparação e matching inteligente de itens** usando IA. O caso de uso principal é reconciliação de catálogos farmacêuticos — cruzar produtos de diferentes fontes e identificar correspondências com validação de dosagem, forma farmacêutica e composição — mas a plataforma é agnóstica a domínio.

O usuário carrega um arquivo de referência e um arquivo alvo; a IA analisa, pontua candidatos e retorna os matches com nível de confiança, pronto para exportação.

### Funcionalidades

- **Pipeline de comparação com IA:** processamento em etapas — parse de arquivos (PDF, XLSX, CSV, DOCX), normalização de texto, geração de candidatos, scoring via Gemini e validação final
- **Memória de comparações:** cache de decisões anteriores para acelerar matches futuros sem custo de IA
- **Fontes de dados flexíveis:** catálogos carregados via XLSX, bancos de dados (SQL Server, PostgreSQL, MySQL) ou APIs REST
- **Exportação multicanal:** resultados exportados para XLSX, banco de dados (insert/append/replace) ou APIs externas
- **Multi-tenant com planos e cotas:** sistema de assinaturas com limites mensais por usuário, feature flags por plano e sub-cotas para times
- **Autenticação completa:** registro com verificação por e-mail, login com bcrypt, sessões httpOnly, convites por link e reset de senha

---

## 🏗️ Architecture

```mermaid
graph TB
    subgraph Frontend["Frontend — React + TypeScript"]
        UI["Pages & Components\nShadcn UI · Tailwind CSS"]
        STORE["State\nZustand"]
        FORMS["Forms\nReact Hook Form · Zod"]
    end

    subgraph Backend["Backend — Node.js + Express"]
        API["REST API\nExpress 5"]
        COMP["Comparison Service\nPipeline orchestration"]
        REPO["State Repository\nDB persistence layer"]
        CRYPTO["Crypto Utils\nCredential encryption"]
    end

    subgraph AI["AI — Google Gemini"]
        VERTEX["Vertex AI\nGemini 2.5 Flash"]
    end

    subgraph DB["Database"]
        MSSQL["SQL Server\n(primary)"]
        PG["PostgreSQL"]
        MYSQL["MySQL"]
    end

    subgraph Sources["Reference Sources"]
        XLSX_S["XLSX Upload"]
        DB_S["Database Query"]
        API_S["REST API"]
    end

    subgraph Targets["Export Targets"]
        XLSX_T["XLSX Download"]
        DB_T["Database Insert"]
        API_T["REST API Push"]
    end

    UI --> API
    API --> COMP
    API --> REPO
    COMP --> VERTEX
    COMP --> Sources
    COMP --> Targets
    REPO --> DB
```

---

## 🔄 Comparison Pipeline

```mermaid
flowchart TD
    A(["User uploads target file\n+ selects reference source"]) --> B

    subgraph B["Stage 1 — Parse"]
        P["XLSX · CSV · DOCX · PDF\n(vision OCR via Gemini)"]
    end

    B --> C

    subgraph C["Stage 2 — Load Reference"]
        R["XLSX · Database · REST API"]
    end

    C --> D

    subgraph D["Stage 3 — Candidate Generation"]
        N["Text normalization\nCandidate scoring heuristics"]
    end

    D --> E

    subgraph E["Stage 4 — AI Scoring"]
        AI["Gemini 2.5 Flash\nDosage · Form · Ingredient validation\n(parallel requests, configurable concurrency)"]
    end

    E --> F

    subgraph F["Stage 5 — Memory & Finalization"]
        MEM["Check memory cache\nApply similarity threshold"]
    end

    F --> G(["Results with confidence score\nReady for export or manual review"])
```

---

## 🛠️ Technology Stack

| Category | Technologies |
|---|---|
| **Frontend** | React 19 · TypeScript 6 · Vite · Tailwind CSS · Shadcn UI · Radix UI |
| **State Management** | Zustand · React Hook Form · Zod |
| **Charts** | Recharts |
| **Backend** | Node.js · Express 5 |
| **AI** | Google Gemini 2.5 Flash · Vertex AI · Gemini API |
| **Databases** | SQL Server · PostgreSQL · MySQL |
| **File Processing** | xlsx · pdf-parse · pdfjs-dist · mammoth (DOCX) |
| **Auth & Security** | bcryptjs · express-session · AES-256 encryption · rate-limiter-flexible |
| **Email** | nodemailer |
| **HTTP / APIs** | fetch · CORS · Multer (file upload) |
| **Architecture** | Multi-tenant SaaS · Subscription plans · Pipeline pattern · Memory cache · Multi-DB abstraction |

---

## 💳 Subscription Plans

A plataforma implementa um modelo de preços dinâmico baseado em uso real:

| Plano | Usuários | Comparações/mês | IA | Fontes DB/API |
|---|---|---|---|---|
| Gratuito | 1 | Limitado | — | — |
| Essencial | 1 | ✅ | ✅ | — |
| Profissional | 1 | ✅ | ✅ | ✅ |
| Empresarial | Time | ✅ | ✅ | ✅ |
| Corporativo | Ilimitado | ✅ | ✅ | ✅ |

---

## 👤 Author

**Gabriel Paulino**
- GitHub: [@gabrielborralhogomes](https://github.com/gabrielborralhogomes)
- LinkedIn: [gabrielborralho](https://www.linkedin.com/in/gabrielborralho)
- Email: gabrielborralho98@gmail.com

---

> *Este projeto é proprietário. Este repositório serve apenas como referência de portfólio.*
> *This project is proprietary. This repository serves as a portfolio reference only.*
