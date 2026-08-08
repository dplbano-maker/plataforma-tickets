<!--
Sync Impact Report
------------------
Version change: Initial Template [0.0.0] -> 1.0.0
Bump Rationale: Initial ratification replacing generic placeholder tokens with concrete principles and governance rules tailored to Plataforma Tickets.
Modified principles:
  - Filled [PRINCIPLE_1_NAME] -> I. Full-Stack End-to-End Type Safety
  - Filled [PRINCIPLE_2_NAME] -> II. Component & Modular Architecture
  - Filled [PRINCIPLE_3_NAME] -> III. Automated Testing Discipline
  - Filled [PRINCIPLE_4_NAME] -> IV. Security & Data Integrity
  - Filled [PRINCIPLE_5_NAME] -> V. Code Quality & Format Consistency
Added sections:
  - Technology Stack & Standards
  - Development & Review Workflow
Removed sections: None
Templates requiring updates:
  - ✅ .specify/templates/plan-template.md (Validated - consistent)
  - ✅ .specify/templates/spec-template.md (Validated - consistent)
  - ✅ .specify/templates/tasks-template.md (Validated - consistent)
Follow-up TODOs: None
-->

# Plataforma Tickets Constitution

## Core Principles

### I. Full-Stack End-to-End Type Safety
Every data contract, operation, query, and action MUST enforce strict end-to-end TypeScript types across client and server boundaries (leveraging Wasp operations, Prisma schemas, and shared types). Untyped escape hatches (`any` or unvalidated `unknown`) are strictly prohibited without explicit code review justification.

### II. Component & Modular Architecture
UI components MUST be built using Shadcn UI patterns and Tailwind CSS utilities, strictly decoupling presentational UI elements from backend business operations. Integrations (payments, authentication, email notification, analytics) MUST be encapsulated into modular services or adapters to maintain separation of concerns.

### III. Automated Testing Discipline
Features MUST be testable in isolation. End-to-end user journeys (authentication, ticket creation, payments) and core business logic MUST be covered by Playwright tests or automated unit/contract tests. New features or critical bug fixes MUST NOT be merged without passing automated test suites in CI.

### IV. Security & Data Integrity
All authentication and authorization checks MUST be executed and validated server-side. User permissions and database access controls MUST be explicitly enforced through Wasp authentication and Prisma queries. Sensitive API keys, secrets, and credentials MUST NEVER be embedded in client code or committed to version control.

### V. Code Quality & Format Consistency
Code styling MUST strictly comply with project ESLint (`npm run lint`) and Prettier (`npm run prettier:check`) rules. All contributions MUST pass linting and formatting validation without errors or warnings prior to approval.

## Technology Stack & Standards

- **Full-Stack Framework**: Wasp (React, Node.js, Prisma)
- **Styling & UI**: Tailwind CSS and Shadcn UI components
- **Database & Data Access**: PostgreSQL managed via Prisma ORM
- **Testing Infrastructure**: Playwright E2E and unit test harnesses
- **Quality Tools**: ESLint and Prettier configured via repository standards

## Development & Review Workflow

- All pull requests and code modifications MUST pass local formatting (`npm run prettier:check`) and linting (`npm run lint`) before review.
- Database schema changes MUST be accompanied by corresponding Prisma migration files and updated TypeScript interfaces.
- Feature branches MUST follow standard branch conventions (`[###-feature-name]`) and link to design specs in `.specify/` or `/specs/`.

## Governance

- This Constitution supersedes all informal team guidelines and development practices.
- Amendments to principles or governance require documentation updates, compliance verification, and formal version increments.
- Versioning rules:
  - **MAJOR**: Incompatible governance policy changes or removal/redefinition of core principles.
  - **MINOR**: Addition of new principles, sections, or expanded standards.
  - **PATCH**: Non-semantic clarifications, formatting, or wording adjustments.

**Version**: 1.0.0 | **Ratified**: 2026-08-07 | **Last Amended**: 2026-08-07
