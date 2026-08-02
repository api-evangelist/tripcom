---
name: Search flights on Trip.com
description: Find flights matching a traveler's route and dates and return a Trip.com booking link.
api: openapi/tripcom-plugin-openapi-original.yml
operations: [search_flight_ticket]
auth: none
---

# Search flights on Trip.com

Use the Trip.com AI travel-assistant plugin API to find flights. No authentication is required; call the public endpoint over HTTPS.

## Steps

1. Collect the mandatory inputs from the user: destination city and the traveler's raw request. Prompt for only one mandatory parameter at a time.
2. Call **`search_flight_ticket`** — `POST /searchFlightTicket` (base `https://www.trip.com/ai-resource`) with a `FlightQueryRequest` body:
   - Required: `destinationCityCode`, `locale`, `originalInput` (the user's exact words), `originalInputInEnglish` (English translation).
   - Optional: `originCityCode`, `departureDate` / `returnDate` (format `yyyy-MM-dd`), `oneWayOrRoundTrip` (`OW` | `RT`). Do not ask for optional parameters; call even if the user omits them.
3. Read the `FlightQueryResponse`: surface `bookFlightLink` and iterate `recommendedListOfOtherFlights[]` (each `Flight` has `airline`, `departureTime`, `arrivalTime`, `pricePerTicket`, `currency`, `numberOfStops`, `flightDuration`, `flightTicketLink`).
4. Present options and hand the user the deep booking link — the API returns links, not a booking action.

## Conventions & errors

- Always send both `originalInput` and `originalInputInEnglish` plus a `locale` (see `conventions/tripcom-conventions.yml`).
- On `422 Validation Error` the response is `HTTPValidationError` (`{ detail: string }`); fix the flagged field and retry (see `errors/tripcom-problem-types.yml`).
