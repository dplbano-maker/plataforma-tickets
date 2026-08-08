# Research & Technical Decisions: Módulo de Estadios

**Feature**: [spec.md](file:///c:/Users/user/plataforma-tickets/specs/001-stadium-module/spec.md) | **Branch**: `001-stadium-module` | **Date**: 2026-08-07

---

## Technical Context & Decisions

### 1. Full-Stack Framework & Architecture
- **Decision**: Wasp Framework (React 18 + Node.js + Prisma ORM + Express).
- **Rationale**: Reuses the core Open SaaS foundation of the workspace (`plataforma-tickets`). Enables automatic end-to-end type safety between Wasp backend Operations (Queries & Actions) and React frontend hooks without manual API client generation.
- **Alternatives Considered**: Next.js App Router (requires separate backend API setup and manual ORM integration; breaks compatibility with existing Wasp setup in repository).

### 2. Interactive Stadium Seat Map (Mapa Interactivo de Asientos)
- **Decision**: SVG-based Interactive Seat Grid with Canvas/React-Zoom-Pan-Pinch fallback for high-density stadiums (up to 80,000 seats).
- **Rationale**: SVG renders crisp vector shapes per sector/row/seat, supports direct DOM events (hover, click, select), and integrates seamlessly with Tailwind CSS styling and Shadcn UI tooltips/popovers.
- **Alternatives Considered**: Three.js 3D Stadium Visualizer (overly complex for core ticket selection, heavy bundle size > 5MB, poor performance on low-end mobile devices).

### 3. Dynamic QR Code & Anti-Fraud Security
- **Decision**: Time-based One-Time Token (TOTP) / HMAC-SHA256 Signed Dynamic QR payload combined with offline JWT signatures.
- **Rationale**: Prevents screenshot sharing and ticket duplication. The QR code on client screen rotates every 15-30 seconds using an offline-verifiable HMAC token tied to ticket ID + user secret + timestamp.
- **Alternatives Considered**: Static QR code containing plain Ticket ID (highly vulnerable to screenshot transfer and illegal resale fraud).

### 4. Offline Ticket Storage & Validation
- **Decision**: Web Storage / IndexedDB Cache storing encrypted ticket payload & signature + Service Worker.
- **Rationale**: Guarantees that stadium attendees can open and display valid QR tickets even under zero mobile network connectivity inside saturated stadium venues. Scanner devices can verify offline signatures using public key verification.
- **Alternatives Considered**: Direct network query on door scan (fails under stadium network saturation).

### 5. Atomic Seat Booking & Concurrency Locking
- **Decision**: PostgreSQL Row-Level Locking (`SELECT FOR UPDATE`) with Redis/Prisma temporary reservation lease (5-minute TTL lock).
- **Rationale**: Guarantees zero double-booking when thousands of users attempt to purchase seats simultaneously for high-demand matches.
- **Alternatives Considered**: Optimistic locking without reservation lock (high failure rate at checkout when multiple users pick the same seat).

### 6. In-Stadium Concession Orders & Real-Time Status
- **Decision**: Wasp Background Jobs + Server-Sent Events (SSE) / WebSocket status polling for Order Status (Received -> Preparing -> Ready for Pickup / Out for Delivery).
- **Rationale**: Provides real-time order tracking without overloading backend database.
- **Alternatives Considered**: High-frequency short HTTP polling (causes backend request spikes during match events).

---

## Best Practices & Integration Patterns

- **UI & Components**: Tailwind CSS + Shadcn UI (Dialog, Sheet, Popover, Card, Toast, Badge, Command).
- **Database Schema**: Prisma ORM with strict indexes on `(eventId, seatId)`, `(userId, ticketId)`, and `(eventId, status)`.
- **Testing Discipline**: Playwright E2E suites verifying:
  1. Seat Map Selection & Checkout.
  2. Ticket Wallet & Dynamic QR display.
  3. Door Access Scanning & Validation.
  4. In-Stadium Concession Ordering.
  5. Admin Event & Stadium Management.
