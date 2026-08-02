---
name: Search rental cars on Trip.com
description: Find rental cars for a destination city and return Trip.com booking links.
api: openapi/tripcom-plugin-openapi-original.yml
operations: [search_cars]
auth: none
---

# Search rental cars on Trip.com

Use the Trip.com AI travel-assistant plugin API to find rental cars. No authentication is required.

## Steps

1. Collect the mandatory inputs: `originCountryCode` (which country the user is from — ask if not stated), `destinationCityName`, and the user's raw request.
2. Call **`search_cars`** — `POST /searchCars` (base `https://www.trip.com/ai-resource`) with a `CarRentalRequest` body:
   - Required: `originCountryCode`, `destinationCityName`, `locale`, `originalInput`, `originalInputInEnglish`.
3. Read the `CarRentalResponse`: surface `carRentalLink` and iterate `carList[]` (each `Car` has `productName`, `groupName`, `transmissionType` (1 Automatic / 2 Manual), `seatCount`, `doorCount`, `luggageCount`, `price`, `airportCode`, `bookingLink`).
4. Present options and hand the user the `bookingLink`.

## Conventions & errors

- Always include `originalInput`, `originalInputInEnglish`, and `locale` (`conventions/tripcom-conventions.yml`).
- `422 Validation Error` → `HTTPValidationError` (`{ detail }`); fix and retry (`errors/tripcom-problem-types.yml`).
