Manifest Registry and Lighthouse Concept
Version: Draft

This document describes how UBOP providers are discovered using manifests and Lighthouses. It is intentionally simple so that the same pattern can be implemented by anyone, with or without a central authority.

## 1: Provider Manifest Recap

Each provider exposes a small JSON document known as a manifest.

The manifest describes provider capabilities (for example flights, rooms, tables, taxis, rentals, resources), lists supported UBOP channels (email, HTTPS, FTP, queues), includes location (city, region, coordinates), defines validation and rate limits, and provides contact details and human-fallback information.

Agents read the manifest to understand how to communicate with the provider.

## 2: Lighthouse Purpose

A Lighthouse is a registry of manifests for a specific area or scope. It answers the question:

Which providers are available in this city or region?

Key properties:

Anyone may run a Lighthouse.
There is no central authority.
Lighthouses may overlap and compete.
Manifests are referenced by URL, not copied or redefined.

Agents may choose any Lighthouse they trust.

## 3: Lighthouse Data Format (Example)

A Lighthouse can use any format, but a minimal JSON structure might look like this:

{
  "lighthouse_id": "helsinki.example",
  "region": {
    "country": "FI",
    "city": "Helsinki"
  },
  "manifests": [
    {
      "provider_id": "hertz.fi",
      "manifest_url": "https://hertz.fi/.well-known/ubop-manifest.json"
    },
    {
      "provider_id": "example-restaurant.fi",
      "manifest_url": "https://example-restaurant.fi/.well-known/ubop-manifest.json"
    }
  ],
  "last_updated": "2025-12-01T12:00:00Z"
}

This file may be published at a predictable URL, for example:

https://lighthouse-helsinki.example/providers.json
https://example.com/ubop/lighthouse/helsinki.json

Agents only need the Lighthouse URL to discover local providers.

## 4: Discovery Flow

A typical discovery flow is:

1) The agent selects a Lighthouse (for example based on city, region or user preference).
2) The agent downloads the Lighthouse registry JSON.
3) For each entry, the agent retrieves the provider’s manifest URL.
4) The agent filters providers by category (hotel, restaurant, taxi, rental, event, generic), location proximity, supported channels (email, HTTPS), and optional fields such as human_fallback.

The result is a list of candidate providers to contact using UBOP.

## 5: Multiple Lighthouses

There may be multiple Lighthouses covering the same region.

Agents may use a default Lighthouse, allow users to configure preferred Lighthouses, merge provider lists from several sources, and cache manifest URLs locally.

Because there is no single global registry, the system remains decentralized and avoids lock-in.

## 6: Offline and Cached Lighthouses

Lighthouses can also be used offline. They may be cached locally, stored as static files, distributed via QR codes, or shipped as seed data inside an application.

This is useful for airports, conference venues, offline-first applications and local community networks, especially in regions with limited connectivity.

## 7: Security Considerations

Lighthouses are not trusted authorities by default.

Agents should treat Lighthouse content as untrusted, validate manifest URLs, verify provider domains using HTTPS or DNS, and apply rate limits to traffic generated from discovered providers. All UBOP messages must still be validated against the schema and any provider-specific rules.

More detailed security and validation guidance is provided in:

docs/security.md
docs/validation_rules.md

End of document.
