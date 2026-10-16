# Modelo de domínio

## Entidades

**User**
- Id
- Name
- Email

**Event**
- Id
- Name
- Description
- Date
- Type
- Artist
- Status
- Venue_id

**Venue**
- Id
- Name
- Description
- Type
- Address
- Capacity
- SeatMap[]

**Ticket**
- Id
- Event_id
- Seat
- Price
- Type
- Status: `available` | `unavailable` | `booked`

**Booking**
- Id
- Ticket_id
- User_id

## Relacionamentos

- Um `Venue` hospeda muitos `Event`.
- Um `Event` tem muitos `Ticket`.
- Um `Booking` referencia um `Ticket` e um `User`.
