# Tasks: Módulo de Estadios

**Input**: Design documents from `specs/001-stadium-module/` (`spec.md`, `plan.md`, `data-model.md`, `contracts/stadium-operations.md`, `research.md`, `quickstart.md`)

**Prerequisites**: plan.md (completed), spec.md (completed), data-model.md (completed), contracts/ (completed)

---

## Phase 1: Setup (Shared Infrastructure)

**Purpose**: Project initialization and core directory setup for the stadium module within the Wasp SaaS template.

- [ ] T001 Create directory structure `template/app/src/stadium/` and `template/app/src/admin/` per implementation plan
- [ ] T002 Configure Prisma schema extension with stadium models in `template/app/schema.prisma`
- [ ] T003 [P] Add initial stadium module Wasp routes, queries, and actions declarations in `template/app/main.wasp.ts`
- [ ] T004 [P] Configure utility helpers for TOTP HMAC QR generation in `template/app/src/stadium/services/qrService.ts`

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: Core database schema, seed data, base models, and shared Wasp operations.

**⚠️ CRITICAL**: No user story implementation can begin until this phase is complete.

- [ ] T005 Execute initial database migration `npx prisma migrate dev --name init_stadium_module`
- [ ] T006 [P] Create database seed script for stadiums, sectors, seats, teams, and concession items in `template/app/src/stadium/seed.ts`
- [ ] T007 Implement server-side authorization helpers for admin and gate staff roles in `template/app/src/stadium/services/authHelpers.ts`
- [ ] T008 [P] Configure Service Worker caching for offline ticket display in `template/app/src/client/service-worker.ts`

**Checkpoint**: Foundation ready — user story implementation can now begin.

---

## Phase 3: User Story 1 - Consulta de Estadios, Sectores y Compra de Entradas (Priority: P1) 🎯 MVP

**Goal**: Permitir a los usuarios consultar estadios, explorar partidos destacados, visualizar aforos por localidad, seleccionar asientos en un mapa interactivo SVG y realizar el pago seguro de sus entradas.

**Independent Test**: Registrar un estadio y partido, ingresar a `/events/:eventId`, interactuar con el mapa de asientos SVG, reservar asientos y completar la compra generando el comprobante electrónico.

### Implementation for User Story 1

- [ ] T009 [P] [US1] Implement `getFeaturedEvents` and `getStadiumDetails` queries in `template/app/src/stadium/operations.ts`
- [ ] T010 [P] [US1] Implement `getEventSeatMap` query with seat availability status in `template/app/src/stadium/operations.ts`
- [ ] T011 [US1] Implement `reserveSeats` action with 5-minute row-level database lock in `template/app/src/stadium/operations.ts`
- [ ] T012 [US1] Implement `checkoutTicketPurchase` action for ticket issuance in `template/app/src/stadium/operations.ts`
- [ ] T013 [P] [US1] Create interactive SVG seat map component `SeatMapSvg.tsx` in `template/app/src/stadium/components/SeatMapSvg.tsx`
- [ ] T014 [P] [US1] Create stadium access map viewer `StadiumAccessMap.tsx` in `template/app/src/stadium/components/StadiumAccessMap.tsx`
- [ ] T015 [US1] Build event landing and seat selection page `EventSeatMapPage.tsx` in `template/app/src/stadium/pages/EventSeatMapPage.tsx`
- [ ] T016 [US1] Build stadium details and capacity page `StadiumDetailsPage.tsx` in `template/app/src/stadium/pages/StadiumDetailsPage.tsx`
- [ ] T017 [US1] Create purchase checkout modal & electronic receipt component in `template/app/src/stadium/components/CheckoutReceiptModal.tsx`

**Checkpoint**: User Story 1 (MVP) is fully functional and testable independently.

---

## Phase 4: User Story 2 - Gestión de Boletos Digitales, Transferencias y Control de Acceso (Priority: P2)

**Goal**: Proveer la billetera de boletos digitales con códigos QR dinámicos, soporte de visualización sin conexión (offline), transferencia/redistribución individual de boletos y consola de validación en puertas.

**Independent Test**: Abrir la billetera de boletos, simular desconexión a internet y verificar la generación del QR dinámico. Transferir un boleto a otro usuario y validar el ingreso en la consola de puertas.

### Implementation for User Story 2

- [ ] T018 [P] [US2] Implement `getDynamicTicketQR` query in `template/app/src/stadium/operations.ts`
- [ ] T019 [P] [US2] Implement `transferTicket` and `acceptTicketTransfer` actions in `template/app/src/stadium/operations.ts`
- [ ] T020 [US2] Implement `validateDoorAccess` action for gate scanners in `template/app/src/stadium/operations.ts`
- [ ] T021 [P] [US2] Create dynamic rotating QR code component `DynamicQRCode.tsx` in `template/app/src/stadium/components/DynamicQRCode.tsx`
- [ ] T022 [US2] Build user ticket wallet and offline display page `TicketWalletPage.tsx` in `template/app/src/stadium/pages/TicketWalletPage.tsx`
- [ ] T023 [US2] Build ticket transfer & group distribution modal in `template/app/src/stadium/components/TicketTransferModal.tsx`
- [ ] T024 [US2] Build door access validation page for gate staff `DoorScanGatePage.tsx` in `template/app/src/stadium/pages/DoorScanGatePage.tsx`

**Checkpoint**: User Stories 1 AND 2 work independently and seamlessly together.

---

## Phase 5: User Story 3 - Compras In-Stadium y Servicios al Hincha (Priority: P3)

**Goal**: Permitir la compra de alimentos, bebidas y recuerdos dentro del recinto durante el evento, selección de entrega a asiento o punto de retiro, seguimiento en tiempo real y consulta de estadísticas/repeticiones.

**Independent Test**: Realizar un pedido de comida desde el carrito in-stadium, seleccionar modalidad de entrega, monitorear el cambio de estado en tiempo real y consultar la vista del partido en vivo.

### Implementation for User Story 3

- [ ] T025 [P] [US3] Implement `getConcessionMenu` query in `template/app/src/stadium/operations.ts`
- [ ] T026 [P] [US3] Implement `createInStadiumOrder` action in `template/app/src/stadium/operations.ts`
- [ ] T027 [US3] Implement `getOrderStatusStream` subscription / polling query in `template/app/src/stadium/operations.ts`
- [ ] T028 [P] [US3] Create in-stadium shopping cart component `ConcessionCart.tsx` in `template/app/src/stadium/components/ConcessionCart.tsx`
- [ ] T029 [US3] Build in-stadium menu and ordering page `InStadiumConcessionsPage.tsx` in `template/app/src/stadium/pages/InStadiumConcessionsPage.tsx`
- [ ] T030 [US3] Build live match statistics and highlight replay widget in `template/app/src/stadium/components/MatchLiveStats.tsx`

**Checkpoint**: User Stories 1, 2, and 3 are all operational.

---

## Phase 6: User Story 4 - Panel de Administración del Estadio, Eventos y Operativa (Priority: P4)

**Goal**: Ofrecer la consola de administración integral para registrar estadios, configurar localidades y precios, bloquear asientos, gestionar inventario de concesiones, responder a soporte y visualizar reportes analíticos.

**Independent Test**: Ingresar como administrador, registrar un nuevo recinto con localidades, ajustar precios de evento, cambiar estados de pedidos de concesiones y exportar el reporte analítico de ventas.

### Implementation for User Story 4

- [ ] T031 [P] [US4] Implement `createOrUpdateStadium` and `configureEventPrices` actions in `template/app/src/admin/operations.ts`
- [ ] T032 [P] [US4] Implement `updateEventSeatStatus` (seat locking/unlocking) action in `template/app/src/admin/operations.ts`
- [ ] T033 [P] [US4] Implement `updateConcessionOrderStatus` and `manageConcessionItem` actions in `template/app/src/admin/operations.ts`
- [ ] T034 [P] [US4] Implement `getSalesAndAttendanceAnalytics` query in `template/app/src/admin/operations.ts`
- [ ] T035 [US4] Build stadium infrastructure & sector management page `StadiumManagementPage.tsx` in `template/app/src/admin/pages/StadiumManagementPage.tsx`
- [ ] T036 [US4] Build event scheduling & price configuration page `EventManagementPage.tsx` in `template/app/src/admin/pages/EventManagementPage.tsx`
- [ ] T037 [US4] Build concession inventory & kitchen order dashboard `ConcessionInventoryPage.tsx` in `template/app/src/admin/pages/ConcessionInventoryPage.tsx`
- [ ] T038 [US4] Build real-time attendance audit & sales analytics page `AccessAuditLogsPage.tsx` in `template/app/src/admin/pages/AccessAuditLogsPage.tsx`

**Checkpoint**: All user stories fully integrated and operational.

---

## Phase 7: Polish & Cross-Cutting Concerns

**Purpose**: Quality assurance, end-to-end testing, responsive design polish, and documentation.

- [ ] T039 [P] Create Playwright E2E test suite for seat selection and checkout in `template/e2e-tests/stadium-booking.spec.ts`
- [ ] T040 [P] Create Playwright E2E test suite for door access validation in `template/e2e-tests/stadium-access.spec.ts`
- [ ] T041 Run `npm run prettier:format` and `npm run lint:fix` across all modified files
- [ ] T042 Verify end-to-end quickstart scenarios from `specs/001-stadium-module/quickstart.md`

---

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)**: No dependencies — start immediately.
- **Foundational (Phase 2)**: Depends on Setup completion — BLOCKS all user stories.
- **User Stories (Phases 3–6)**: Depend on Foundational phase completion.
  - Phase 3 (US1 - Purchasing) → Phase 4 (US2 - Tickets & Access) → Phase 5 (US3 - In-Stadium) → Phase 6 (US4 - Admin)
- **Polish (Phase 7)**: Depends on all user story phases being complete.

---

## Parallel Execution Opportunities

### User Story 1 Parallel Tasks
```bash
# Models/Services:
Task: "Implement getFeaturedEvents and getStadiumDetails queries in template/app/src/stadium/operations.ts"
Task: "Implement getEventSeatMap query with seat availability status in template/app/src/stadium/operations.ts"

# UI Components:
Task: "Create interactive SVG seat map component SeatMapSvg.tsx in template/app/src/stadium/components/SeatMapSvg.tsx"
Task: "Create stadium access map viewer StadiumAccessMap.tsx in template/app/src/stadium/components/StadiumAccessMap.tsx"
```

### User Story 2 Parallel Tasks
```bash
Task: "Implement getDynamicTicketQR query in template/app/src/stadium/operations.ts"
Task: "Implement transferTicket and acceptTicketTransfer actions in template/app/src/stadium/operations.ts"
Task: "Create dynamic rotating QR code component DynamicQRCode.tsx in template/app/src/stadium/components/DynamicQRCode.tsx"
```

---

## Implementation Strategy

### MVP First (User Story 1 Only)
1. Complete Phase 1 (Setup) and Phase 2 (Foundational).
2. Complete Phase 3 (User Story 1).
3. Validate independent ticket selection and purchase flow.
4. Demo/Deploy MVP.

### Incremental Delivery
1. Add User Story 2 (Digital Wallet & Gate Scan).
2. Add User Story 3 (In-Stadium Food & Souvenirs).
3. Add User Story 4 (Admin Console & Analytics).
4. Run Phase 7 (Polish & E2E Validation).
