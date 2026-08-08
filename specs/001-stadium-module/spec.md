# Feature Specification: Módulo de Estadios

**Feature Branch**: `001-stadium-module`

**Created**: 2026-08-07

**Status**: Draft

**Input**: User description: "Desarrollar el Módulo de Estadios para la Plataforma SaaS de Tickets, basado en el stack base de Open SaaS (Wasp, React, Node.js, Prisma y Tailwind CSS)..."

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Consulta de Estadios, Sectores y Compra de Entradas (Priority: P1)

Un hincha o espectador ingresa a la plataforma, explora los estadios y eventos deportivos disponibles, consulta la capacidad y aforo por localidad, selecciona sus asientos mediante un mapa interactivo visual y realiza la compra segura de sus entradas recibiendo su comprobante electrónico.

**Why this priority**: Es el flujo central de negocio (MVP) indispensable para la comercialización de entradas a eventos deportivos y generación de ingresos.

**Independent Test**: Puede probarse de manera independiente registrando un estadio con localidades, creando un evento deportivo, seleccionando asientos desde el mapa interactivo y completando el checkout de compra de boletos.

**Acceptance Scenarios**:

1. **Given** un espectador autenticado o visitante en la plataforma, **When** navega al catálogo de eventos o busca por su equipo favorito, **Then** el sistema muestra la información detallada del estadio asignado, dirección, capacidad total y aforo disponible por sección.
2. **Given** un espectador en la pantalla de compra de un evento deportivo, **When** selecciona una localidad y abre el mapa interactivo del estadio, **Then** el sistema despliega el mapa visual con disponibilidad de asientos en tiempo real y precios con impuestos y comisiones desglosados antes de pagar.
3. **Given** un espectador confirmando su compra de entradas, **When** la transacción es procesada con éxito, **Then** el sistema reserva los asientos de forma definitiva y emite el comprobante electrónico correspondiente.

---

### User Story 2 - Gestión de Boletos Digitales, Transferencias y Control de Acceso (Priority: P2)

Un usuario que compró entradas accede a su billetera de boletos digitales con códigos QR dinámicos, puede descargar sus boletos para usarlos sin conexión, compartirlos o distribuirlos individualmente entre miembros de su grupo, o ponerlos en reventa oficial dentro de la plataforma. El personal del estadio valida los accesos en puerta mediante escaneo QR o documento de identidad.

**Why this priority**: Garantiza la operatividad el día del evento, permitiendo la validación ágil y segura en puerta, el uso offline y opciones de flexibilidad para los hinchas.

**Independent Test**: Puede probarse generando boletos digitales para una compra completada, simulando el funcionamiento sin conexión, ejecutando la transferencia individual de boletos a otro usuario y validando el ingreso desde el módulo de control de accesos.

**Acceptance Scenarios**:

1. **Given** un hincha con entradas adquiridas, **When** ingresa a su perfil o billetera de eventos, **Then** puede visualizar sus boletos con código QR dinámico y descargarlos para uso sin conexión a internet.
2. **Given** un usuario que compró un paquete grupal de entradas, **When** selecciona la opción de redistribución o transferencia individual, **Then** el sistema permite transferir boletos específicos a otros usuarios registrados o vía invitación.
3. **Given** el personal de control de accesos en las puertas del recinto, **When** escanea el código QR dinámico o consulta el documento de identidad del asistente, **Then** el sistema valida inmediatamente el derecho de acceso, registra el ingreso y previene duplicados o intentos de fraude.

---

### User Story 3 - Compras In-Stadium y Servicios al Hincha (Priority: P3)

Durante el partido o evento deportivo, el asistente consulta el menú de alimentos, bebidas y recuerdos del estadio desde su teléfono móvil, realiza su pedido con carrito de compras integrado, selecciona si desea entrega en su asiento o retiro en punto autorizado, y monitorea el estado del pedido en tiempo real.

**Why this priority**: Mejora drásticamente la experiencia del espectador dentro del recinto, reduce filas en concesiones y abre una línea adicional de ingresos para el estadio.

**Independent Test**: Puede probarse realizando un pedido de comida/recuerdos desde el carrito de compras dentro de un evento activo, seleccionando punto de retiro o entrega a asiento, y actualizando el estado del pedido desde el panel de concesiones.

**Acceptance Scenarios**:

1. **Given** un espectador presente en el estadio con un evento en curso, **When** accede a la sección de compras dentro del recinto, **Then** puede explorar el menú de productos, agregar elementos al carrito, ajustar cantidades y pagar su pedido.
2. **Given** un usuario finalizando una compra in-stadium, **When** selecciona el método de entrega (entrega directa al asiento o retiro express en punto autorizado), **Then** el sistema genera un número de pedido y muestra su estado de preparación en tiempo real.
3. **Given** un usuario que sigue el partido, **When** ingresa a la vista del evento, **Then** puede consultar las estadísticas del partido en tiempo real y repeticiones destacadas disponibles.

---

### User Story 4 - Panel de Administración del Estadio, Eventos y Operativa (Priority: P4)

Un administrador del módulo gestiona la infraestructura de estadios (localidades, capacidad, mapa de accesos), configura y programa eventos deportivos, administra la disponibilidad de asientos y precios, supervisa ventas e ingresos en tiempo real, monitorea accesos en puerta, y gestiona inventarios y solicitudes de soporte/reembolsos.

**Why this priority**: Proporciona a los operadores del estadio las herramientas necesarias para la configuración previa, control de aforos, supervisión en tiempo real y reportería analítica post-evento.

**Independent Test**: Puede probarse creando un estadio completo con sectores y precios desde el panel administrativo, ejecutando el bloqueo/desbloqueo de asientos, emitiendo promociones y generando reportes analíticos de ventas y asistencia.

**Acceptance Scenarios**:

1. **Given** un usuario con rol de Administrador, **When** accede al panel de gestión del módulo, **Then** puede registrar o modificar estadios, definir sectores, configurar capacidades y ajustar precios por localidad.
2. **Given** un administrador supervisando un evento activo o en venta, **When** consulta el panel de control, **Then** visualiza métricas en tiempo real de ventas, aforo ocupado, registros de ingreso en puertas y rendimiento de ventas in-stadium.
3. **Given** un administrador gestionando solicitudes de usuarios, **When** recibe solicitudes de cambios, devoluciones o soporte, **Then** puede revisar los antecedentes de la compra, aprobar o rechazar reembolsos y actualizar el estado de las solicitudes.

---

### Edge Cases

- **Pérdida de conectividad en el estadio**: ¿Cómo se comporta la app el día del evento si la red móvil del estadio se satura? La aplicación permite mostrar los boletos previamente descargados offline con cifrado local y firma digital verificable localmente por los escáneres de puerta.
- **Intento de reingreso o duplicado de QR**: Si un usuario intenta ingresar dos veces con el mismo código QR o con una captura de pantalla estática, el código QR dinámico (que rota tokens periódicamente) y el registro en tiempo real de accesos bloquean el segundo intento indicando "Boleto ya utilizado".
- **Cancelación o reprogramación repentina de un partido**: Si un evento se suspende o reprograma por mal clima o fuerza mayor, el sistema notifica automáticamente a los compradores, congela los boletos o habilita el flujo de reembolso según las políticas configuradas.
- **Agotamiento simultáneo de asientos en mapa interactivo**: Si dos usuarios seleccionan exactamente el mismo asiento al mismo segundo, la verificación de disponibilidad previa al pago coloca una reserva temporal (lock por 5 minutos); el segundo usuario recibe un aviso inmediato de "Asiento en proceso de selección por otro usuario".

## Requirements *(mandatory)*

### Functional Requirements

#### Autenticación y Registro
- **FR-001**: El sistema MUST permitir el registro de nuevos usuarios en la plataforma de estadios.
- **FR-002**: El sistema MUST permitir el inicio de sesión reutilizando credenciales centralizadas de la plataforma Tickets.
- **FR-003**: El sistema MUST proveer funcionalidad de recuperación de contraseña para las cuentas de usuario.

#### Información del Estadio y Ubicación
- **FR-004**: El sistema MUST permitir consultar el estadio asignado para cualquier evento deportivo seleccionado.
- **FR-005**: El sistema MUST mostrar la ubicación geolocalizada, dirección exacta y mapa visual de accesos del recinto.
- **FR-006**: El sistema MUST ofrecer rutas e indicaciones interactiva para llegar al estadio.

#### Capacidad y Aforo
- **FR-007**: El sistema MUST permitir la consulta de la capacidad total de espectadores autorizada por recinto.
- **FR-008**: El sistema MUST permitir consultar la capacidad total y el aforo disponible desglosado por localidad, zona y sección.

#### Gestión de Entradas
- **FR-009**: El sistema MUST permitir la compra de entradas para eventos deportivos.
- **FR-10**: El sistema MUST desplegar un mapa interactivo del estadio con la ubicación exacta de cada asiento.
- **FR-011**: El sistema MUST verificar la disponibilidad del asiento de manera atómica antes de procesar el pago.
- **FR-012**: El sistema MUST generar un boleto digital con código QR dinámico para prevenir duplicaciones.
- **FR-013**: El sistema MUST permitir descargar el boleto digital para su visualización y validación sin conexión a internet (offline).
- **FR-014**: El sistema MUST permitir compartir entradas compradas con otros usuarios de la plataforma.
- **FR-015**: El sistema MUST permitir distribuir individualmente cada entrada perteneciente a una compra grupal.
- **FR-016**: El sistema MUST proveer una funcionalidad de reventa oficial regulada dentro de la plataforma.
- **FR-017**: El sistema MUST validar el ingreso al estadio mediante el escaneo del código QR dinámico.
- **FR-018**: El sistema MUST permitir la validación alternativa de ingreso mediante documento de identidad registrado.

#### Pantalla de Inicio y Personalización
- **FR-019**: El sistema MUST mostrar los próximos partidos y eventos destacados en la pantalla principal.
- **FR-020**: El sistema MUST permitir al usuario seleccionar y visualizar sus equipos deportivos favoritos.
- **FR-021**: El sistema MUST publicar noticias y novedades relevantes asociadas a los equipos favoritos del usuario.

#### Calendario y Recordatorios
- **FR-022**: El sistema MUST ofrecer un calendario personalizable con los eventos adquiridos por el usuario.
- **FR-023**: El sistema MUST mostrar en el calendario los próximos encuentros programados de los equipos favoritos.
- **FR-024**: El sistema MUST permitir la configuración de recordatorios para los partidos.

#### Compra de Entradas y Localidades
- **FR-025**: El sistema MUST mostrar el listado y distribución de localidades disponibles en el recinto.
- **FR-026**: El sistema MUST publicar de forma clara el precio por localidad y tipo de asiento.
- **FR-027**: El sistema MUST calcular y mostrar el precio final detallado (incluyendo impuestos y comisiones) antes de confirmar el pago.
- **FR-028**: El sistema MUST emitir un comprobante electrónico oficial de compra una vez procesado el pago.

#### Compras Dentro del Estadio (In-Stadium)
- **FR-029**: El sistema MUST ofrecer la consulta del menú de alimentos, bebidas y productos/recuerdos oficiales.
- **FR-030**: El sistema MUST permitir agregar productos del menú de concesiones al carrito de compras.
- **FR-031**: El sistema MUST permitir el pago electrónico de pedidos realizados dentro del estadio.
- **FR-032**: El sistema MUST permitir seleccionar la modalidad de entrega: despacho directo al asiento o retiro en punto autorizado del recinto.
- **FR-033**: El sistema MUST mostrar la actualización en tiempo real del estado de preparación y despacho del pedido.

#### Carrito de Compras
- **FR-034**: El sistema MUST mostrar el resumen claro de productos, cantidades y subtotal/total en el carrito.
- **FR-035**: El sistema MUST permitir modificar las cantidades o eliminar ítems individuales del carrito.
- **FR-036**: El sistema MUST permitir vaciar completamente el carrito de compras.

#### Perfil de Usuario e Historial
- **FR-037**: El sistema MUST desplegar la información de perfil e historial del usuario.
- **FR-038**: El sistema MUST permitir modificar en cualquier momento la lista de equipos favoritos.
- **FR-039**: El sistema MUST mantener un historial completo de las entradas adquiridas por el usuario.
- **FR-040**: El sistema MUST permitir consultar y redescargar los comprobantes electrónicos de compras pasadas.

#### Información del Evento en Vivo
- **FR-041**: El sistema MUST desplegar un mapa interactivo del recinto resaltando sectores y puertas de acceso asignadas.
- **FR-042**: El sistema MUST publicar estadísticas e información del partido en tiempo real.
- **FR-043**: El sistema MUST permitir la reproducción de repeticiones de jugadas destacadas durante el evento.

#### Soporte y Atención al Cliente
- **FR-044**: El sistema MUST integrar un canal de chat de soporte en vivo para asistencia al usuario.
- **FR-045**: El sistema MUST permitir enviar solicitudes formales de cambios, devoluciones o reasignación de entradas.

#### Seguridad Operacional
- **FR-046**: El sistema MUST cifrar toda la información sensible de boletos y transacciones de pago vía HTTPS/TLS.
- **FR-047**: El sistema MUST detectar y bloquear automáticamente intentos de fraude, clonación o duplicado de boletos.

#### Panel de Administración del Módulo
- **FR-048**: El sistema MUST permitir la creación, modificación, suspensión o cancelación de eventos deportivos.
- **FR-049**: El sistema MUST permitir registrar y actualizar la ficha técnica de estadios (nombre, ubicación, mapa, capacidad).
- **FR-050**: El sistema MUST permitir configurar localidades, aforo máximo y precios por recinto y evento.
- **FR-051**: El sistema MUST permitir administrar la disponibilidad y el bloqueo manual de asientos por razones operativas o de seguridad.
- **FR-052**: El sistema MUST permitir definir y actualizar las tarifas y listas de precios de entradas.
- **FR-053**: El sistema MUST ofrecer un tablero de control para la supervisión de ventas en tiempo real.
- **FR-054**: El sistema MUST registrar y permitir la consulta en tiempo real de los accesos validados en puerta.
- **FR-055**: El sistema MUST permitir la gestión del catálogo de productos, comida, bebidas y recuerdos.
- **FR-056**: El sistema MUST permitir actualizar y gestionar el flujo de estados de los pedidos de alimentos y merchandising.
- **FR-057**: El sistema MUST controlar el inventario de productos de concesiones en tiempo real.
- **FR-058**: El sistema MUST permitir la creación y gestión de cupones de descuento y promociones.
- **FR-059**: El sistema MUST permitir administrar los contenidos e imágenes destacadas de la pantalla de inicio.
- **FR-060**: El sistema MUST permitir la publicación de noticias, anuncios y comunicados oficiales del módulo.
- **FR-061**: El sistema MUST proveer una consola para gestionar y responder tickets de soporte al cliente.
- **FR-062**: El sistema MUST permitir evaluar, aprobar o rechazar solicitudes de reembolsos y cambios de boletos.
- **FR-063**: El sistema MUST generar reportes analíticos consolidados de ventas, ingresos, ocupación y aforo asistido.
- **FR-064**: El sistema MUST administrar las cuentas de usuario y la asignación de roles con permisos administrativos.
- **FR-065**: El sistema MUST registrar y mantener auditoría completa de los logs de actividad del sistema.

---

### Key Entities

- **Stadium (Estadio)**: Representa el recinto deportivo (nombre, ubicación geolocalizada, dirección, capacidad total, mapa de accesos).
- **Sector (Localidad/Zona)**: Representa las subdivisiones del estadio (Tribuna Norte, Platea Preferencial, Palcos), su aforo asignado y distribución visual.
- **Seat (Asiento)**: Representa la ubicación física exacta (sector, fila, número) y su estado (disponible, reservado, bloqueado, vendido).
- **Event (Evento Deportivo)**: Representa un partido o evento programado en un estadio (equipos, fecha, hora, estado del evento, reglas de venta).
- **Team (Equipo)**: Representa un club o equipo deportivo que participa en eventos (nombre, escudo, estadio local, noticias).
- **Ticket (Boleto/Entrada)**: Representa la entrada comprada por un espectador (evento, asiento, precio pagado, QR dinámico, token offline, estado de validación en puerta).
- **TicketTransfer (Transferencia)**: Registra la reasignación, distribución grupal o transferencia individual de un boleto entre usuarios.
- **ResaleListing (Reventa)**: Registra la publicación y transacción de boletos dentro del mercado secundario oficial.
- **ConcessionItem (Producto de Concesión)**: Representa un ítem del menú de alimentos, bebidas o souvenirs (nombre, categoría, precio, stock en tiempo real).
- **Order (Pedido In-Stadium)**: Representa una compra dentro del estadio (usuario, lista de ítems, monto total, modalidad de entrega: asiento o punto de retiro, estado de preparación).
- **OrderItem (Detalle del Pedido)**: Representa el desglose de productos y cantidades de un pedido in-stadium.
- **SupportTicket (Solicitud de Soporte)**: Registra las consultas, peticiones de devolución o solicitudes de cambio enviadas por los usuarios.
- **AuditLog (Log de Auditoría)**: Registra las acciones administrativas, intentos de acceso y eventos de seguridad del sistema.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: Los usuarios completan el proceso de selección de localidad, elección de asiento en mapa interactivo y compra de entradas en menos de 2 minutos.
- **SC-002**: El sistema valida el código QR dinámico de acceso en puerta en menos de 1.5 segundos por asistente en los puntos de control.
- **SC-003**: El 100% de los boletos descargados en la aplicación móvil o web pueden ser visualizados e inspeccionados sin conexión activa a internet durante la jornada del evento.
- **SC-004**: Los pedidos de alimentos y merchandising realizados dentro del estadio son procesados y notificados a la cocina/punto de despacho en menos de 30 segundos tras la confirmación de pago.
- **SC-005**: El sistema soporta picos de demanda de hasta 10,000 usuarios concurrentes en la venta de partidos de alta convocatoria sin degradación en la reserva de asientos ni ventas duplicadas.
- **SC-006**: La satisfacción del espectador con la usabilidad de la compra de entradas y pedidos in-stadium supera el 90% en encuestas post-evento.

## Assumptions

- **Integración con Plataforma SaaS Base**: El Módulo de Estadios se integra directamente con el esquema unificado de autenticación, usuarios y pasarelas de pago existentes en Open SaaS / Plataforma Tickets.
- **Dispositivos de Lectura en Puertas**: El personal de control de accesos cuenta con dispositivos móviles o escáneres capaces de leer códigos QR dinámicos y consultar la API de validación o su caché local.
- **Moneda e Impuestos**: Los precios de las entradas y productos in-stadium incluyen o desglosan impuestos locales aplicables según las reglamentaciones vigentes en el país del evento.
- **Conectividad de Concesiones**: Los puntos de venta o cocinas de los estadios cuentan con impresoras de pedidos o pantallas de cocina (KDS) conectadas a la red del recinto.
