# Database Schema & API Contract

## Database Schema (D1)

### Users
- id (UUID, PK)
- email (unique)
- password_hash (nullable, for password login)
- passkey_credential (nullable, for passkey login)
- created_at
- updated_at

### UserEventRoles
- id (UUID, PK)
- user_id (FK: Users.id)
- event_id (FK: Events.id)
- role (enum: super_admin, event_admin, judge, timekeeper, finance_officer)

### Invitations
- id (UUID, PK)
- email
- event_id (FK: Events.id)
- role (enum)
- token
- expires_at
- accepted (bool)

### Locations
- id (UUID, PK)
- name
- address
- country
- notes

### Events
- id (UUID, PK)
- name
- location_id (FK: Locations.id)
- start_date
- end_date
- description
- is_active (bool)

### Distances
- id (UUID, PK)
- event_id (FK: Events.id)
- name (e.g., "2000m")
- length_meters
- is_active (bool)

### Splits
- id (UUID, PK)
- distance_id (FK: Distances.id)
- name (e.g., "500m split")
- meters
- order

### Crews
- id (UUID, PK)
- event_id (FK: Events.id)
- name
- club
- members (JSON or separate table for crew_members)

### CrewMembers
- id (UUID, PK)
- crew_id (FK: Crews.id)
- name
- birthdate
- role (e.g., cox, rower)

### StartingOrders
- id (UUID, PK)
- event_id (FK: Events.id)
- distance_id (FK: Distances.id)
- crew_id (FK: Crews.id)
- order

### Timings
- id (UUID, PK)
- crew_id (FK: Crews.id)
- distance_id (FK: Distances.id)
- split_id (nullable, FK: Splits.id)
- time (timestamp)
- type (enum: start, finish, split)
- recorded_by (FK: Users.id)

### Penalties
- id (UUID, PK)
- crew_id (FK: Crews.id)
- distance_id (FK: Distances.id)
- type (enum: time, disqualification, exclusion, warning, custom)
- value (nullable, e.g., seconds for time penalty)
- description
- imposed_by (FK: Users.id)
- created_at

### Payments
- id (UUID, PK)
- event_id (FK: Events.id)
- crew_id (nullable, FK: Crews.id)
- club (nullable)
- amount
- status (enum: pending, paid, overdue)
- marked_by (FK: Users.id)
- marked_at

---

## API Contract (by Worker)

### Auth/User Worker
- POST /auth/invite (invite user)
- POST /auth/register (accept invite, set password/passkey)
- POST /auth/login (password or passkey)
- POST /auth/logout
- POST /auth/forgot-password
- POST /auth/reset-password
- GET /auth/me
- GET /users/:id
- GET /users (admin only)

### Event Worker
- GET /events
- POST /events (admin)
- GET /events/:id
- PUT /events/:id (admin)
- DELETE /events/:id (admin)

### Location Worker
- GET /locations
- POST /locations (admin)
- GET /locations/:id
- PUT /locations/:id (admin)
- DELETE /locations/:id (admin)

### Crew Worker
- GET /events/:eventId/crews
- POST /events/:eventId/crews
- GET /crews/:id
- PUT /crews/:id
- DELETE /crews/:id
- GET /crews/:id/members
- POST /crews/:id/members

### Distance Worker
- GET /events/:eventId/distances
- POST /events/:eventId/distances
- PUT /distances/:id
- DELETE /distances/:id
- GET /distances/:id/splits
- POST /distances/:id/splits

### Order Worker
- GET /events/:eventId/orders
- POST /events/:eventId/orders
- PUT /orders/:id

### Timing Worker
- POST /timings (record start/finish/split)
- GET /crews/:crewId/timings

### Penalty Worker
- POST /penalties
- GET /crews/:crewId/penalties
- GET /events/:eventId/penalties

### Finance Worker
- GET /events/:eventId/payments
- POST /events/:eventId/payments/mark
- GET /crews/:crewId/invoice (PDF)
- GET /clubs/:club/invoice (PDF)

### Results Worker
- GET /events/:eventId/results
- GET /events/:eventId/draw
- GET /events/:eventId/entries

### Static Renderer Worker
- POST /static/rebuild (trigger static page/API regeneration)

---

This document is a living reference and will be updated as the project evolves. 