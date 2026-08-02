---
name: Search hotels on Trip.com
description: Find hotels in a city matching a traveler's dates, star rating, and facilities, and return Trip.com booking links.
api: openapi/tripcom-plugin-openapi-original.yml
operations: [search_hotel]
auth: none
---

# Search hotels on Trip.com

Use the Trip.com AI travel-assistant plugin API to find hotels. No authentication is required.

## Steps

1. Collect the mandatory inputs: the destination `cityName` and the user's raw request. Ask for one mandatory parameter at a time.
2. Call **`search_hotel`** — `POST /searchHotel` (base `https://www.trip.com/ai-resource`) with a `HotelQueryRequest` body:
   - Required: `cityName`, `locale`, `originalInput`, `originalInputInEnglish`.
   - Optional: `checkIn` / `checkOut` (`yyyy-MM-dd`), `topHotel` (max results), `starList[]` (1–5), `facilityList[]`, `themeList[]`, `typeList[]`. Do not prompt for optional parameters.
3. Read the `HotelQueryResponse.hotelList[]`: each `Hotel` has `hotelName`, `hotelPrice` + `hotelCurrency`, `star`, `score`, `numberOfReviews`, `hotelAddress`, and `hotelLink`.
4. Present ranked options and hand the user the `hotelLink` deep booking link.

## Conventions & errors

- Send `originalInput`, `originalInputInEnglish`, and `locale` on every call (`conventions/tripcom-conventions.yml`).
- `422 Validation Error` → `HTTPValidationError` (`{ detail }`); correct the field and retry (`errors/tripcom-problem-types.yml`).
