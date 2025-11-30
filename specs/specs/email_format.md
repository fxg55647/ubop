# UBOP Email Transport Format Specification
Version: 1.0.0  
Status: Draft

## Table of Contents
1. Human-Friendly Disclaimer  
2. Overview  
3. Subject Line Format  
4. Headers  
5. Email Body  
6. Provider Response Rules  
7. Error Handling  
8. Encoding Rules  
9. Security Notes  
10. Lifecycle

## 1. Human-Friendly Disclaimer

Every UBOP email should begin with a brief, human-readable disclaimer.

Its purpose is to reassure human recipients that:
- they may reply in normal language  
- they are not required to write JSON  
- UBOP agents can interpret free-form replies  

Recommended text:

"You may reply in plain language.
UBOP agents can interpret human responses even when not formatted as JSON."

This appears at the very top of the email body, above the JSON payload.  
UBOP parsers ignore this line.

## 2. Overview

UBOP messages can be exchanged via email using a simple structure:

- MIME type must be `application/json`  
- The body contains exactly one UBOP message  
- The subject begins with `[UBOP]`  
- Providers respond in the same email thread  

Email transport is optional and announced in the provider manifest.

## 3. Subject Line Format

A UBOP email must use this subject format:

[UBOP] <OPERATION> <CORRELATION_ID?>

Examples:

[UBOP] car_rental_quote 9f88c231
[UBOP] hotel_booking_modify 423-AF89
[UBOP] generic_booking_cancel

Rules:
- `[UBOP]` prefix is required  
- OPERATION must match the JSON field  
- Correlation ID is optional but recommended  
- Extra text is discouraged  

## 4. Headers

Required email headers:

Content-Type: application/json
X-UBOP-Version: <version>
X-UBOP-Provider-ID: <optional>
X-UBOP-Correlation-ID: <recommended>

Example:

Content-Type: application/json
X-UBOP-Version: 0.1.0-draft
X-UBOP-Provider-ID: hertz.fi
X-UBOP-Correlation-ID: 9f88c231

## 5. Email Body

The email body contains a single UBOP message in JSON.  
It must conform to `ubop.schema.json`.

Example:

{
  "type": "REQUEST",
  "operation": "car_rental_quote",
  "meta": {
    "ubop_version": "0.1.0-draft",
    "message_id": "MSG-29183",
    "timestamp": "2025-12-01T10:15:00Z",
    "correlation_id": "9f88c231",
    "agent_id": "agent.example",
    "strict_mode": true
  },
  "data": {
    "party": { "total_guests": 2 },
    "travel_segments": [
      {
        "segment_id": "pickup",
        "mode": "rental_car",
        "from": { "type": "address", "name": "Helsinki Airport" },
        "to": { "type": "address", "name": "Helsinki Downtown" },
        "departure_time": "2025-12-03T08:00:00Z",
        "arrival_time": "2025-12-10T10:00:00Z"
      }
    ]
  }
}


## 6. Provider Response Rules

Providers replying to a UBOP email must:

- reply to the same email thread  
- include a UBOP message of type `RESPONSE` or `ERROR`  
- use the same `correlation_id`  
- use `Content-Type: application/json`  

Example subject:

Re: [UBOP] car_rental_quote 9f88c231

Example response:

{
  "type": "RESPONSE",
  "operation": "car_rental_quote",
  "meta": {
    "ubop_version": "0.1.0-draft",
    "message_id": "MSG-29184",
    "timestamp": "2025-12-01T10:16:30Z",
    "correlation_id": "9f88c231",
    "provider_id": "hertz.fi"
  },
  "result": {
    "success": true,
    "price_total": 349.00,
    "currency": "EUR",
    "provider_confirmation_code": "HRZ-239912"
  }
}

## 7. Error Handling

If the provider cannot accept the request, it sends an `ERROR` message.

Example:

{
  "type": "ERROR",
  "operation": "car_rental_quote",
  "meta": {
    "ubop_version": "0.1.0-draft",
    "message_id": "MSG-29185",
    "timestamp": "2025-12-01T10:17:00Z",
    "correlation_id": "9f88c231"
  },
  "errors": [
    {
      "code": "INVALID_FIELD",
      "message": "Missing required field: travel_segments[0].from",
      "field": "travel_segments[0].from",
      "retryable": true
    }
  ]
}
Error responses must include:

the same operation

the same correlation_id

at least one error object

## 8. Encoding Rules

- UTF-8 only  
- no HTML  
- no attachments  
- only plain JSON  

Future versions may add optional signing or encryption.

## 9. Security Notes

Email is not secure by default. Providers should:

- treat all content as untrusted  
- validate JSON strictly  
- enforce rate limiting  
- avoid unnecessary personal data  
- optionally whitelist known senders

## 10. Lifecycle

Email transport is intended for:

- small hotels  
- restaurants  
- taxis and ride services  
- rental desks  
- local venues  
- legacy backends  

Providers may disable email support at any time by updating their manifest.

End of document.
