# Data Model Specification: Módulo de Estadios

**Feature**: [spec.md](file:///c:/Users/user/plataforma-tickets/specs/001-stadium-module/spec.md) | **Branch**: `001-stadium-module` | **Date**: 2026-08-07

---

## Prisma Schema Models & Relations

### 1. Stadium (Estadio)
Representa la infraestructura física del estadio.

```prisma
model Stadium {
  id          String   @id @default(uuid())
  name        String
  city        String
  address     String
  latitude    Float
  longitude   Float
  capacity    Int
  accessMapUrl String?
  imageUrl    String?
  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt

  sectors     Sector[]
  events      Event[]
}
```

### 2. Sector (Localidad / Zona)
Representa las áreas o tribunas del estadio (e.g. Tribuna Norte, Platea Alta, Palco VIP).

```prisma
model Sector {
  id          String   @id @default(uuid())
  stadiumId   String
  name        String   // e.g. "Tribuna Occidente Alta"
  code        String   // e.g. "OCC-ALT"
  capacity    Int
  gateAccess  String   // e.g. "Puerta 4 y 5"
  svgPathData String?  // Coordenadas o id para renderizado en mapa SVG

  stadium     Stadium  @relation(fields: [stadiumId], references: [id], onDelete: Cascade)
  seats       Seat[]
  eventPrices EventSectorPrice[]

  @@index([stadiumId])
}
```

### 3. Seat (Asiento / Ubicación)
Representa cada silla física individual dentro de una localidad.

```prisma
model Seat {
  id        String   @id @default(uuid())
  sectorId  String
  row       String   // e.g. "Fila 12"
  number    Int      // e.g. 45
  isBlocked Boolean  @default(false)
  notes     String?  // e.g. "Visión reducida"

  sector    Sector   @relation(fields: [sectorId], references: [id], onDelete: Cascade)
  tickets   Ticket[]
  reservations SeatReservation[]

  @@unique([sectorId, row, number])
}
```

### 4. Team (Equipo Deportivo)
Representa a los clubes o selecciones que disputan los encuentros.

```prisma
model Team {
  id        String   @id @default(uuid())
  name      String
  shortName String
  logoUrl   String?
  city      String?

  homeEvents Event[] @relation("HomeTeam")
  awayEvents Event[] @relation("AwayTeam")
  userFavorites UserFavoriteTeam[]
}
```

### 5. Event (Partido / Evento Deportivo)
Representa el encuentro programado en un estadio.

```prisma
model Event {
  id          String      @id @default(uuid())
  stadiumId   String
  homeTeamId  String
  awayTeamId  String
  title       String      // e.g. "Liga Premier: Alianza vs Universitario"
  startAt     DateTime
  doorsOpenAt DateTime
  status      EventStatus @default(SCHEDULED) // SCHEDULED, LIVE, COMPLETED, CANCELLED, POSTPONED
  featured    Boolean     @default(false)
  createdAt   DateTime    @default(now())

  stadium     Stadium     @relation(fields: [stadiumId], references: [id])
  homeTeam    Team        @relation("HomeTeam", fields: [homeTeamId], references: [id])
  awayTeam    Team        @relation("AwayTeam", fields: [awayTeamId], references: [id])
  
  sectorPrices EventSectorPrice[]
  tickets     Ticket[]
  reservations SeatReservation[]
  orders      Order[]

  @@index([startAt])
  @@index([stadiumId])
}

enum EventStatus {
  SCHEDULED
  LIVE
  COMPLETED
  CANCELLED
  POSTPONED
}
```

### 6. EventSectorPrice (Precios por Sector/Evento)
```prisma
model EventSectorPrice {
  id        String   @id @default(uuid())
  eventId   String
  sectorId  String
  price     Decimal  @db.Decimal(10, 2)
  tax       Decimal  @db.Decimal(10, 2)
  fee       Decimal  @db.Decimal(10, 2)

  event     Event    @relation(fields: [eventId], references: [id], onDelete: Cascade)
  sector    Sector   @relation(fields: [sectorId], references: [id], onDelete: Cascade)

  @@unique([eventId, sectorId])
}
```

### 7. SeatReservation (Bloqueo / Reserva Temporal)
```prisma
model SeatReservation {
  id        String   @id @default(uuid())
  eventId   String
  seatId    String
  userId    String
  expiresAt DateTime

  event     Event    @relation(fields: [eventId], references: [id], onDelete: Cascade)
  seat      Seat     @relation(fields: [seatId], references: [id], onDelete: Cascade)
  user      User     @relation(fields: [userId], references: [id], onDelete: Cascade)

  @@unique([eventId, seatId])
  @@index([expiresAt])
}
```

### 8. Ticket (Boleto Digital)
```prisma
model Ticket {
  id            String       @id @default(uuid())
  eventId       String
  seatId        String
  userId        String
  orderId       String?
  ticketCode    String       @unique // Hash público o UUID impreso
  qrSecret      String       // Clave para generación de HMAC TOTP dinámico
  status        TicketStatus @default(ISSUED) // ISSUED, VALIDATED, TRANSFERRED, RESALE, CANCELLED
  validatedAt   DateTime?
  validatedBy   String?      // ID del personal de puerta
  resalePrice   Decimal?     @db.Decimal(10, 2)

  event         Event        @relation(fields: [eventId], references: [id])
  seat          Seat         @relation(fields: [seatId], references: [id])
  user          User         @relation(fields: [userId], references: [id])
  transfers     TicketTransfer[]

  @@unique([eventId, seatId])
  @@index([userId])
}

enum TicketStatus {
  ISSUED
  VALIDATED
  TRANSFERRED
  RESALE
  CANCELLED
}
```

### 9. TicketTransfer (Transferencias e Invitaciones)
```prisma
model TicketTransfer {
  id          String         @id @default(uuid())
  ticketId    String
  fromUserId  String
  toUserId    String?
  toEmail     String
  status      TransferStatus @default(PENDING) // PENDING, ACCEPTED, REJECTED, EXPIRED
  createdAt   DateTime       @default(now())

  ticket      Ticket         @relation(fields: [ticketId], references: [id])
}

enum TransferStatus {
  PENDING
  ACCEPTED
  REJECTED
  EXPIRED
}
```

### 10. ConcessionItem & Order (Compras In-Stadium)
```prisma
model ConcessionItem {
  id          String   @id @default(uuid())
  stadiumId   String
  name        String
  category    String   // FOOD, BEVERAGE, MERCHANDISE
  price       Decimal  @db.Decimal(10, 2)
  stock       Int
  imageUrl    String?

  orderItems  OrderItem[]
}

model Order {
  id           String      @id @default(uuid())
  eventId      String
  userId       String
  deliveryMode DeliveryMode // SEAT_DELIVERY, PICKUP_POINT
  seatLocation String?     // e.g. "Tribuna Occidente Fila 4 Asiento 12"
  pickupPoint  String?     // e.g. "Módulo Comida N° 3 - Puerta 4"
  totalAmount  Decimal     @db.Decimal(10, 2)
  status       OrderStatus @default(PENDING) // PENDING, PREPARING, READY, DELIVERED, CANCELLED
  createdAt    DateTime    @default(now())

  event        Event       @relation(fields: [eventId], references: [id])
  user         User        @relation(fields: [userId], references: [id])
  items        OrderItem[]
}

model OrderItem {
  id        String         @id @default(uuid())
  orderId   String
  itemId    String
  quantity  Int
  unitPrice Decimal        @db.Decimal(10, 2)

  order     Order          @relation(fields: [orderId], references: [id], onDelete: Cascade)
  item      ConcessionItem @relation(fields: [itemId], references: [id])
}

enum DeliveryMode {
  SEAT_DELIVERY
  PICKUP_POINT
}

enum OrderStatus {
  PENDING
  PREPARING
  READY
  DELIVERED
  CANCELLED
}
```

### 11. UserFavoriteTeam & SupportTicket
```prisma
model UserFavoriteTeam {
  userId    String
  teamId    String

  user      User     @relation(fields: [userId], references: [id], onDelete: Cascade)
  team      Team     @relation(fields: [teamId], references: [id], onDelete: Cascade)

  @@id([userId, teamId])
}

model SupportTicket {
  id          String        @id @default(uuid())
  userId      String
  ticketId    String?
  subject     String
  description String
  status      SupportStatus @default(OPEN) // OPEN, IN_PROGRESS, RESOLVED, CLOSED
  createdAt   DateTime      @default(now())

  user        User          @relation(fields: [userId], references: [id])
}

enum SupportStatus {
  OPEN
  IN_PROGRESS
  RESOLVED
  CLOSED
}
```

---

## State Transitions Matrix

### Ticket Life Cycle
```
[UNRESERVED SEAT] 
     │ (Select seat & checkout)
     ▼
[SEAT RESERVED] (5 min TTL)
     │ (Payment completed)
     ▼
[ISSUED TICKET]
     ├───────────────────────┬────────────────────────┐
     │ (Scan at gate)        │ (Transfer to friend)   │ (Post for resale)
     ▼                       ▼                        ▼
[VALIDATED TICKET]     [TRANSFERRED]            [RESALE LISTING]
```

### In-Stadium Order Life Cycle
```
[PENDING] ──(Accepted by kitchen)──> [PREPARING] ──(Ready)──> [READY] ──(Delivered)──> [DELIVERED]
```
