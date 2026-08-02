---
name: Search attractions and activities on Trip.com
description: Find attraction tickets, tours, experiences, and travel services for a destination and return Trip.com links.
api: openapi/tripcom-plugin-openapi-original.yml
operations: [search_attraction_ticket_and_activity]
auth: none
---

# Search attractions and activities on Trip.com

Use the Trip.com AI travel-assistant plugin API to find things to do. No authentication is required.

## Steps

1. Collect the mandatory inputs: the user's raw request and `locale`. Optionally capture a `destination` (country/city/province) or a specific `pointOfInterest`.
2. Call **`search_attraction_ticket_and_activity`** — `POST /searchAttractionAndActivity` (base `https://www.trip.com/ai-resource`) with an `AttractionAndActivityQueryRequest` body:
   - Required: `locale`, `originalInput`, `originalInputInEnglish`.
   - Optional: `destination`, `pointOfInterest`, `beginDate` / `endDate` (`yyyy-MM-dd`), `categories[]` (`All` | `Attractions` | `TravelServices` | `Experiences` | `Tours` | `WiFi&PhoneCards`), `sort` (`Recommended` | `TravelerRating` | `SalesVolume`), `limit`.
3. Read the `AttractionAndActivityQueryResponse.results[]`: each `AttractionAndActivity` has `title`, `summary`, `startingPrice` + `currency`, `reviewRating`, `openTime`, `distanceFromDowntown`, and a product `url`.
4. Present ranked options with their `url` deep links.

## Conventions & errors

- Send `originalInput`, `originalInputInEnglish`, and `locale` on every call (`conventions/tripcom-conventions.yml`).
- `422 Validation Error` → `HTTPValidationError` (`{ detail }`); correct the field and retry (`errors/tripcom-problem-types.yml`).
