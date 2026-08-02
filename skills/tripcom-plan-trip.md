---
name: Plan a trip (flights + hotels + things to do)
description: Compose a destination trip plan by chaining Trip.com flight, hotel, and attraction searches.
api: openapi/tripcom-plugin-openapi-original.yml
operations: [search_flight_ticket, search_hotel, search_attraction_ticket_and_activity]
auth: none
---

# Plan a trip on Trip.com

Chain the Trip.com AI travel-assistant plugin operations to assemble a full trip plan. No authentication is required. Prompt the user for only the mandatory parameter of each step, one at a time.

## Steps

1. **Flights** — call `search_flight_ticket` (`POST /searchFlightTicket`) with a `FlightQueryRequest` (`destinationCityCode`, `locale`, `originalInput`, `originalInputInEnglish`; optional `originCityCode`, `departureDate`, `returnDate`, `oneWayOrRoundTrip`). Capture `bookFlightLink` and the chosen dates.
2. **Hotels** — call `search_hotel` (`POST /searchHotel`) with a `HotelQueryRequest` (`cityName`, `locale`, `originalInput`, `originalInputInEnglish`; optional `checkIn`/`checkOut` matching the flight dates, `starList`, `topHotel`). Capture `hotelList[].hotelLink`.
3. **Things to do** — call `search_attraction_ticket_and_activity` (`POST /searchAttractionAndActivity`) with an `AttractionAndActivityQueryRequest` (`locale`, `originalInput`, `originalInputInEnglish`; optional `destination`, `categories`, `beginDate`/`endDate`). Capture `results[].url`.
4. Assemble the plan: outbound/return flight, hotel for the stay, and a shortlist of attractions — each presented with its Trip.com deep booking link. The API returns links only; the user completes booking on Trip.com.

## Conventions & errors

- Reuse `locale` and pass `originalInput` + `originalInputInEnglish` on every call (`conventions/tripcom-conventions.yml`).
- Any step returning `422 Validation Error` (`HTTPValidationError` `{ detail }`) should be corrected on the flagged field and retried before moving on (`errors/tripcom-problem-types.yml`).
