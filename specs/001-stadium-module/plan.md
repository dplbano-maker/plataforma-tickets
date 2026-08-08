# Implementation Plan: Módulo de Estadios

**Branch**: `001-stadium-module` | **Date**: 2026-08-07 | **Spec**: [spec.md](file:///c:/Users/user/plataforma-tickets/specs/001-stadium-module/spec.md)

---

## Summary

Implementar el Módulo de Estadios completo para la plataforma SaaS de Tickets sobre el stack de Open SaaS (Wasp Framework, React 18, Node.js, Prisma ORM, Tailwind CSS y Shadcn UI). La solución abarca catálogo y ficha de estadios, venta interactiva de entradas en mapa de asientos SVG con bloqueo atómico, billetera de boletos digitales con códigos QR dinámicos e inspección offline, módulo de pedidos in-stadium de concesiones (alimentos/recuerdos), control de acceso en puertas y panel administrativo integral de gestión y analítica.

---

## Technical Context

**Language/Version**: TypeScript 5.9+, Node.js 20+

**Primary Dependencies**: Wasp Framework (`@wasp.sh/wasp`), React 18, Tailwind CSS 3, Shadcn UI, Prisma ORM 5+

**Storage**: PostgreSQL (Prisma ORM) con bloqueos `SELECT FOR UPDATE` para disponibilidad de asientos en tiempo real

**Testing**: Playwright E2E testing suite (`e2e-tests/`), Vitest / Unit tests

**Target Platform**: Responsive Web App (Desktop & Mobile Chrome/Safari/Edge), PWA con soporte offline Service Worker

**Project Type**: Full-Stack SaaS Web Application (Wasp framework with full client & server unification)

**Performance Goals**: Carga de pantalla principal <2s, validación QR en puerta <1.5s, soporte para 10,000 usuarios concurrentes en compras de partidos destacados.

**Constraints**: Cero duplicidad de asientos vendida, soporte offline para boletos descargados, cifrado HTTPS/TLS, cumplimiento de reglas ESLint/Prettier sin advertencias.

**Scale/Scope**: Módulo completo con 65 Requerimientos Funcionales, 15 Requerimientos No Funcionales, 11 Modelos Prisma y 4 Historias de Usuario priorizadas (P1–P4).

---

## Constitution Check

*GATE: Passed before Phase 0 research. Re-evaluated post Phase 1 design.*

| Principle | Compliance Status | Validation Rationale |
|-----------|------------------|----------------------|
| **I. Full-Stack End-to-End Type Safety** | ✅ PASSED | Operaciones Wasp (Queries & Actions) con tipos inferidos automáticamente entre backend Node.js y componentes React sin `any`. |
| **II. Component & Modular Architecture** | ✅ PASSED | UI desacoplada con componentes Shadcn UI + Tailwind CSS, separación clara entre lógica de mapa de asientos y servicios de negocio. |
| **III. Automated Testing Discipline** | ✅ PASSED | Pruebas E2E en Playwright para checkout de boletos, generación de QR, compras in-stadium y panel de administración. |
| **IV. Security & Data Integrity** | ✅ PASSED | Validación server-side de sesiones Wasp, control de acceso por roles en panel admin y prevención de fraudes QR con HMAC TOTP. |
| **V. Code Quality & Format Consistency** | ✅ PASSED | Verificación mediante `npm run lint` y `npm run prettier:check` en pipeline CI. |

---

## Project Structure

### Documentation (this feature)

```text
specs/001-stadium-module/
├── spec.md              # Especificación funcional (/speckit-specify output)
├── plan.md              # Este plan de implementación (/speckit-plan output)
├── research.md          # Investigación técnica & decisiones (/speckit-plan output)
├── data-model.md        # Esquema de datos & modelos Prisma (/speckit-plan output)
├── quickstart.md        # Guía de validación runnable (/speckit-plan output)
├── contracts/           # Especificación de operaciones Wasp (/speckit-plan output)
│   └── stadium-operations.md
└── checklists/
    └── requirements.md  # Checklist de calidad
```

### Source Code Layout (Wasp SaaS Application)

```text
template/app/
├── main.wasp.ts         # Configuración central de rutas, operaciones y entidades Wasp
├── schema.prisma        # Modelos Prisma (Stadium, Sector, Seat, Event, Ticket, Order, etc.)
└── src/
    ├── stadium/         # Módulo de Estadios (Frontend & Backend)
    │   ├── pages/
    │   │   ├── StadiumDetailsPage.tsx
    │   │   ├── EventSeatMapPage.tsx
    │   │   ├── TicketWalletPage.tsx
    │   │   ├── InStadiumConcessionsPage.tsx
    │   │   └── DoorScanGatePage.tsx
    │   ├── components/
    │   │   ├── SeatMapSvg.tsx
    │   │   ├── DynamicQRCode.tsx
    │   │   ├── ConcessionCart.tsx
    │   │   └── StadiumAccessMap.tsx
    │   ├── operations.ts   # Queries y Actions Wasp (reservas, pagos, validación QR)
    │   └── services/       # Servicios de cifrado TOTP, lógica de reservas y reportes
    ├── admin/              # Panel de Administración de Estadios y Concesiones
    │   ├── pages/
    │   │   ├── StadiumManagementPage.tsx
    │   │   ├── EventManagementPage.tsx
    │   │   ├── ConcessionInventoryPage.tsx
    │   │   └── AccessAuditLogsPage.tsx
    │   └── operations.ts
    └── client/
        └── service-worker.ts # Caché offline para boletos descargados
```

---

## Complexity Tracking

> **No violations identified.** All principles fully respected.

| Aspect | Choice | Justification |
|--------|--------|---------------|
| Seat Selection | Interactive SVG Grid | Optimal vector performance and light bundle size vs heavy 3D frameworks. |
| QR Security | HMAC TOTP dynamic token | Completely prevents static screenshot ticket duplication. |
| Data Access | Wasp Operations + Prisma | Guarantees end-to-end type safety and clean modular backend structure. |

---

## Verification Plan

### Automated Tests
- `npm run lint` — Verificación de linting TypeScript & React.
- `npm run prettier:check` — Verificación de formato Prettier.
- `npx playwright test` — Ejecución de la suite E2E de Playwright para flujos de mapa de asientos, compra de boletos, escaneo QR y compras in-stadium.

### Manual Verification
- Ejecutar los 5 escenarios detallados en [quickstart.md](file:///c:/Users/user/plataforma-tickets/specs/001-stadium-module/quickstart.md) para validar la selección interactiva de asientos, la resistencia offline de boletos y la consola administrativa de concesiones.
