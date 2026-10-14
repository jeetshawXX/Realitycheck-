# RealityCheck AI

**An AI-powered idea evaluation platform that helps founders pressure-test ideas before investing time and resources.**

RealityCheck AI analyzes a proposed idea through multiple specialist AI agents, combines their findings, and produces a structured assessment of risks, opportunities, and possible next steps.

> **Important:** RealityCheck AI provides AI-generated decision support, not financial, legal, investment, or business guarantees. Verify important claims independently before making consequential decisions.

## Overview

Evaluating an idea involves more than asking whether it sounds promising. Market conditions, execution difficulty, human behavior, timing, and historical patterns can all affect its chances.

RealityCheck AI brings these perspectives together in a single workflow. Submit an idea, run a multi-agent analysis, and review a synthesized assessment designed to help you decide whether to proceed, validate assumptions, pivot, or reconsider.

## Key Features

- **Multi-agent analysis:** Five specialist agents examine an idea from different perspectives.
- **Market reality assessment:** Explore market-related risks and key findings.
- **Execution complexity assessment:** Identify implementation challenges and operational risks.
- **Human and behavioral risk analysis:** Consider user behavior and adoption-related concerns.
- **Timing and external risk analysis:** Examine timing and external factors that may affect the idea.
- **Historical pattern analysis:** Consider relevant patterns and potential warning signs.
- **Synthesized verdict:** Combine specialist findings into a constructive, structured recommendation.
- **Risk scoring:** Aggregate weighted agent scores into an indicative success-probability estimate.
- **Analysis history and dashboard:** Review saved analyses and navigate the application.
- **PDF reports:** Generate downloadable reports of analysis results.
- **Authentication:** Support account access through Better Auth, including Google sign-in when configured.

*Actual results depend on the availability and quality of model responses, configuration, and input details.*

## How It Works

1. **Submit an idea** — Describe the product, business, project, or proposal you want to evaluate.
2. **Run specialist analyses** — Five agents assess market reality, execution complexity, human behavior, timing and external risks, and historical patterns.
3. **Calculate an indicative score** — The application combines available risk scores using configured weights and applies additional penalties for very high individual risks.
4. **Synthesize the findings** — A synthesis agent reviews the specialist reports and produces a consolidated verdict and recommendations.
5. **Review and export** — Explore the results, revisit saved analyses, and generate a PDF report.

## Technology Stack

| Layer | Technologies |
| --- | --- |
| Frontend and application framework | Next.js 15, React 19, TypeScript |
| Styling and UI | Tailwind CSS, Lucide React, Framer Motion |
| AI orchestration | Agno agents, Groq SDK |
| Authentication | Better Auth, Google OAuth |
| Database | PostgreSQL / Neon, Drizzle ORM |
| Validation | Zod |
| Reports | React PDF Renderer |
| Development | Node.js, npm, Git |

## Architecture

```text
User
  |
  v
Next.js application
  |
  +--> Authentication (Better Auth)
  |
  +--> Idea analysis workflow
  |      |
  |      +--> Market Reality Agent
  |      +--> Execution Complexity Agent
  |      +--> Human & Behavioral Risk Agent
  |      +--> Timing & External Risk Agent
  |      +--> Historical Pattern Agent
  |                    |
  |                    v
  |             Risk scoring
  |                    |
  |                    v
  |             Synthesis Agent
  |
  +--> PostgreSQL database (Drizzle ORM)
  |
  +--> Analysis history and PDF reporting
```

The analysis workflow is organized in `lib/reality-check/`. Application routes, UI components, authentication, database access, and reporting are maintained in their respective project directories.

## Getting Started

### Prerequisites

- Node.js compatible with the installed Next.js version
- npm
- A PostgreSQL database (local PostgreSQL or Neon)
- A Groq API key
- Google OAuth credentials if you want to enable Google sign-in

### 1. Clone the repository

```bash
git clone https://github.com/jeetshawXX/Realitycheck-.git
cd Realitycheck-
```

### 2. Install dependencies

```bash
npm ci
```

If `npm ci` reports that the lockfile is missing or out of sync, use `npm install` and review the resulting changes.

### 3. Configure environment variables

Copy the example environment file:

```bash
cp .env.example .env
```

Fill in the values in `.env`:

| Variable | Purpose |
| --- | --- |
| `DATABASE_URL` | PostgreSQL connection string |
| `BETTER_AUTH_SECRET` | Secret used by Better Auth; generate a strong random value |
| `BETTER_AUTH_URL` | Base URL for the application |
| `NEXT_PUBLIC_BETTER_AUTH_URL` | Public application URL used by the authentication client |
| `GOOGLE_CLIENT_ID` | Google OAuth client ID |
| `GOOGLE_CLIENT_SECRET` | Google OAuth client secret |
| `GROQ_API_KEY` | API key used for AI-powered analysis |

For local development, the base URL is normally `http://localhost:3000`. If Next.js starts on a different port, keep the application URLs and OAuth configuration consistent with the port actually in use.

Generate a Better Auth secret with:

```bash
openssl rand -base64 32
```

For Google sign-in, configure the authorized redirect URI to match the callback URL used by the application. For the default local setup, this is typically:

```text
http://localhost:3000/api/auth/callback/google
```

Never commit `.env` or expose API keys, OAuth secrets, authentication secrets, or database credentials. The repository's `.env.example` is intended to contain placeholders only.

### 4. Set up the database

Apply the Drizzle schema to the configured database:

```bash
npx drizzle-kit push
```

Use this command only against the database you intend to initialize or update, and review schema changes before applying them to an existing production database.

### 5. Start the development server

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000).

If the terminal reports that port 3000 is already in use, Next.js may select another port. Open the URL printed in the terminal and update your local OAuth redirect configuration if necessary.

## Available Scripts

| Command | Description |
| --- | --- |
| `npm run dev` | Start the development server with Turbopack |
| `npm run build` | Build the application for production |
| `npm run start` | Start the production server after building |
| `npm run lint` | Run the configured lint script |

## Project Structure

```text
.
├── app/                    # Next.js routes, pages, and API endpoints
├── components/             # Reusable UI components
├── lib/
│   └── reality-check/      # Analysis agents, orchestration, scoring, synthesis
├── drizzle.config.ts       # Drizzle ORM configuration
├── .env.example            # Environment-variable template
├── package.json            # Dependencies and scripts
└── README.md
```

## Configuration and Troubleshooting

- **AI analysis fails:** Verify that `GROQ_API_KEY` is set correctly and that your Groq account can access the configured model.
- **Database connection fails:** Check `DATABASE_URL`, network access, and database availability.
- **Google sign-in returns `redirect_uri_mismatch`:** Ensure the authorized redirect URI in Google Cloud Console exactly matches the callback URL in the error message, including protocol, port, and path.
- **Authentication requests fail:** Check `BETTER_AUTH_URL` and `NEXT_PUBLIC_BETTER_AUTH_URL`, then restart the development server after editing `.env`.
- **A port is already in use:** Use the local URL printed by Next.js and keep OAuth settings aligned with it.

## Security

- Keep all secrets in local environment variables or your deployment provider's secret manager.
- Do not commit `.env` files containing real credentials.
- Use HTTPS and production-appropriate authentication and database settings when deploying.
- Avoid sending sensitive or confidential information to third-party AI services.

## Contributing

Contributions, bug reports, and suggestions are welcome. For significant changes, open an issue first to discuss the proposed improvement. Keep changes focused, document configuration updates, and avoid committing secrets or generated files.

## License

No license has been specified yet. Unless a license is added to this repository, reuse and redistribution are not automatically permitted.
