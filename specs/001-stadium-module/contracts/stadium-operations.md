# API Contracts & Operations: Módulo de Estadios

**Feature**: [spec.md](file:///c:/Users/user/plataforma-tickets/specs/001-stadium-module/spec.md) | **Branch**: `001-stadium-module` | **Date**: 2026-08-07

---

## Overview

Este documento define las operaciones Wasp (Queries y Actions) y endpoints expuestos para la interacción del cliente web/móvil y los dispositivos de escaneo en puerta.

---

## Wasp Operations (Queries & Actions)

### 1. Stadiums & Event Queries

#### `getFeaturedEvents` (Query)
- **Input**: `{ limit?: number }`
- **Output**: `Array<Event & { stadium: Stadium, homeTeam: Team, awayTeam: Team, minPrice: number }>`
- **Auth**: Public

#### `getStadiumDetails` (Query)
- **Input**: `{ stadiumId: String }`
- **Output**: `Stadium & { sectors: Array<Sector & { availableCapacity: number }> }`
- **Auth**: Public

#### `getEventSeatMap` (Query)
- **Input**: `{ eventId: String, sectorId?: String }`
- **Output**: 
  ```ts
  {
    eventId: string;
    stadium: { id: string; name: string; svgMapUrl: string };
    sectors: Array<{
      id: string;
      name: string;
      code: string;
      price: number;
      tax: number;
      fee: number;
      availableSeatsCount: number;
      seats: Array<{
        id: string;
        row: string;
        number: number;
        status: 'AVAILABLE' | 'RESERVED' | 'SOLD' | 'BLOCKED';
      }>;
    }>;
  }
  ```
- **Auth**: Public / Authenticated

---

### 2. Booking & Ticket Actions

#### `reserveSeats` (Action)
- **Input**: `{ eventId: String, seatIds: Array<String> }`
- **Output**: 
  ```ts
  {
    reservationId: string;
    expiresAt: string; // ISO Date (5 min TTL)
    seatIds: string[];
    subtotal: number;
    taxes: number;
    fees: number;
    totalAmount: number;
  }
  ```
- **Errors**: `SEAT_ALREADY_TAKEN`, `MAX_SEATS_PER_USER_EXCEEDED`
- **Auth**: Authenticated User

#### `checkoutTicketPurchase` (Action)
- **Input**: `{ reservationId: String, paymentMethodId: String }`
- **Output**: 
  ```ts
  {
    orderId: string;
    tickets: Array<{
      id: string;
      ticketCode: string;
      seatInfo: { sectorName: string; row: string; number: number };
      qrSecret: string;
    }>;
    receiptUrl: string;
  }
  ```
- **Auth**: Authenticated User

#### `getDynamicTicketQR` (Query)
- **Input**: `{ ticketId: String }`
- **Output**: 
  ```ts
  {
    ticketId: string;
    dynamicToken: string; // HMAC SHA-256 rotating token
    expiresInSeconds: number; // 20s
    offlineSignaturePayload: string; // Local fallback verification token
  }
  ```
- **Auth**: Authenticated Ticket Owner

#### `validateDoorAccess` (Action)
- **Input**: `{ ticketCode: String, dynamicToken: String, gateId: String }`
- **Output**: 
  ```ts
  {
    valid: boolean;
    reason?: string;
    attendeeName?: string;
    seatInfo?: string;
    validatedAt: string;
  }
  ```
- **Auth**: Authenticated Gate Staff / Scanner Device

---

### 3. In-Stadium Concession Operations

#### `getConcessionMenu` (Query)
- **Input**: `{ stadiumId: String }`
- **Output**: `Array<ConcessionItem>`
- **Auth**: Public

#### `createInStadiumOrder` (Action)
- **Input**: 
  ```ts
  {
    eventId: string;
    items: Array<{ itemId: string; quantity: number }>;
    deliveryMode: 'SEAT_DELIVERY' | 'PICKUP_POINT';
    seatLocation?: string;
    pickupPoint?: string;
  }
  ```
- **Output**: 
  ```ts
  {
    orderId: string;
    status: 'PENDING' | 'PREPARING' | 'READY';
    estimatedMinutes: number;
  }
  ```
- **Auth**: Authenticated User

---

### 4. Admin Management Operations

#### `createOrUpdateStadium` (Action)
- **Input**: `{ id?: String, name: String, city: String, address: String, capacity: Int, sectors: Array<SectorInput> }`
- **Auth**: Admin Role

#### `updateEventSeatStatus` (Action)
- **Input**: `{ eventId: String, seatIds: Array<String>, isBlocked: boolean }`
- **Auth**: Admin Role

#### `getSalesAndAttendanceAnalytics` (Query)
- **Input**: `{ eventId: String }`
- **Output**: 
  ```ts
  {
    totalCapacity: number;
    ticketsSold: number;
    attendanceCount: number;
    totalRevenue: number;
    concessionRevenue: number;
    sectorBreakdown: Array<{ sectorName: string; sold: number; revenue: number }>;
  }
  ```
- **Auth**: Admin Role
