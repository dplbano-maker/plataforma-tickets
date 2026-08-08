# Quickstart & Validation Guide: Módulo de Estadios

**Feature**: [spec.md](file:///c:/Users/user/plataforma-tickets/specs/001-stadium-module/spec.md) | **Branch**: `001-stadium-module` | **Date**: 2026-08-07

---

## Runnable Validation Scenarios

### Scenario 1: Setup Infrastructure & Seed Stadium Data

1. **Start Development Environment**:
   ```bash
   npm run dev
   # or wasp start
   ```

2. **Apply Database Migrations & Seed Data**:
   ```bash
   npx prisma migrate dev --name init_stadium_module
   npx prisma db seed
   ```

3. **Verify Data Seeding**:
   - Stadium "Estadio Nacional" registered with 4 Sectors (Occidente, Oriente, Norte, Sur).
   - Seats populated for testing.
   - Sample Event "Clásico Deportivo" created and active.

---

### Scenario 2: End-to-End Ticket Selection & Interactive Map Purchase

1. **Navigate to Stadium Event**:
   - Open browser at `http://localhost:3000/events/sample-event-id`.
2. **Interactive Map Selection**:
   - Click on "Tribuna Occidente".
   - Select seats `Row 5, Seat 12` and `Row 5, Seat 13`.
   - Verify subtotal, tax breakdown, and 5-minute reservation timer.
3. **Checkout Execution**:
   - Click "Proceed to Checkout".
   - Complete test payment.
4. **Verification**:
   - Ticket generated in user wallet (`/wallet`).
   - Seats `Row 5, Seat 12` & `Seat 13` marked `SOLD` in database and map.

---

### Scenario 3: Offline Ticket Dynamic QR & Gate Access Validation

1. **Open Wallet & Simular Modo Offline**:
   - Open `/wallet/ticket-id` in browser or mobile device.
   - Disable internet connection (Airplane Mode / Offline DevTools).
   - Confirm dynamic TOTP QR renders cleanly without network errors.
2. **Gate Staff Scan Verification**:
   - Trigger `validateDoorAccess` Action with ticket token.
   - Confirm response returns `{ valid: true, attendeeName: '...', seatInfo: 'Occidente Fila 5-12' }`.
   - Re-scan identical token -> Confirm system returns `{ valid: false, reason: 'ALREADY_VALIDATED' }`.

---

### Scenario 4: In-Stadium Food & Souvenir Order Flow

1. **Browse Concession Menu**:
   - Open `/stadium/orders` for active event.
   - Select "Combo HotDog + Gaseosa" and "Camiseta Oficial".
2. **Submit Order & Track Real-time Status**:
   - Choose "Delivery to Seat (Occidente Row 5 Seat 12)".
   - Submit order.
3. **Admin Kitchen Console**:
   - Open `/admin/concessions/orders`.
   - Change order status `PENDING` -> `PREPARING` -> `READY`.
   - Verify client screen updates status in real-time.

---

### Scenario 5: Quality & Linting Compliance

Run verification commands:
```bash
npm run prettier:check
npm run lint
```
Confirm zero errors or formatting warnings.
