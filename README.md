# UBOP — Universal Booking Protocol

Version: Archived  
Status: No longer actively developed (superseded by OpenAI's Agentic Commerce Protocol)

This repository documents the UBOP protocol as it existed in 2025. UBOP was an attempt to define a simple, universal, human-friendly and agent-friendly way for businesses and autonomous agents to exchange booking and ordering information without scraping websites.

The project is archived, but the ideas, schema, and documentation remain available as a reference for anyone interested in protocol design, agent interoperability, or decentralized commercial infrastructure.

---

## Table of Contents

1. Introduction  
2. Background and Origin  
3. What UBOP Tried To Solve  
4. Provider Manifest  
5. The Lighthouse Concept  
6. Benefits for Both Agents and Providers  
7. Transport Independence  
8. Human Fallback Mode  
9. Adoption Path (Redirect to Full Payments)  
10. Repository Structure  
11. Why Development Stopped  
12. License and Contact

---

## 1. Introduction

UBOP (Universal Booking and Order Protocol) defines a simple JSON-based message format that allows agents and businesses to exchange structured booking requests, availability information, confirmations, and modifications.

The core idea was that any business, regardless of size or technical capability, could speak UBOP using plain text transport such as email, API endpoints, FTP drops, message queues, or even fax if necessary. Agents should not be required to scrape websites or reverse-engineer HTML workflows.

---

## 2. Background and Origin

The work on UBOP began in the summer of 2025. At that time it had become obvious that autonomous agents were forced to operate through human-oriented web interfaces. Even simple tasks required:

- navigating buttons  
- parsing HTML layouts  
- simulating form interactions  
- retrying flows when the UI changed  

This was wasteful, fragile, and inefficient.

Despite clear need, no universal booking protocol existed. No standard body, platform, or company had proposed anything that would allow machine-to-business bookings through a stable, open, text-based interface.

UBOP was an attempt to fill that gap. This repository represents the state of the project by the end of summer 2025.

---

## 3. What UBOP Tried To Solve

UBOP was designed to solve several problems:

- Agents should not scrape websites to obtain data that businesses already have structured internally.  
- Providers should not suffer unnecessary load from automated crawling.  
- Interoperability should not depend on proprietary APIs or vendor-specific SDKs.  
- Small businesses should be able to participate using nothing more than an email client.  
- A single schema should unify all booking types: flights, hotels, restaurants, taxis, events, rentals, and generic resources.

UBOP defined a minimal schema, a manifest system, and several transport-agnostic rules that were intended to work in any environment.

---

## 4. Provider Manifest

Each service provider exposes a simple JSON file called a manifest. This file describes:

- the provider's name and identity  
- physical location (city, region, coordinates)  
- capabilities (for example flights, hotel rooms, tables, taxis, rentals, equipment)  
- supported communication channels (email, HTTPS, FTP, queues)  
- rate limits and validation rules  
- contact details  
- optional human fallback behavior  

The manifest is intentionally simple so that small businesses can create and edit it manually. Agents read the manifest to understand how to communicate with the provider without scraping websites or guessing endpoints.

---

## 5. The Lighthouse Concept

To make discovery possible, UBOP introduced Lighthouses.

A Lighthouse is a directory of provider manifests for a specific region. Anyone may run a Lighthouse. There is no central authority. Examples:

- a Lighthouse listing all hotels and restaurants in Helsinki  
- a Lighthouse containing all taxi fleets near an airport  
- a country-level Lighthouse listing chains and major providers  

Lighthouses allow agents to perform city-level or region-level discovery:

"Find all providers in this location."

This eliminates scraping as a discovery mechanism and decentralizes the ecosystem.

---

## 6. Benefits for Both Agents and Providers

Scraping imposes costs on both sides:

Agents waste compute, GPU time, and bandwidth.  
Providers suffer artificial load, unpredictable traffic, and brittle automation scripts.

Even when scraping works, it is slow, environmentally costly, legally unclear, and operationally fragile.

UBOP replaces adversarial scraping with cooperative communication.

Benefits to agents:

- predictable responses  
- reduced latency  
- no need for HTML parsing  
- consistent schema across all providers  

Benefits to providers:

- reduced site load  
- easier integration  
- no requirement to build or maintain custom APIs  
- messages that a human can also read and reply to  

---

## 7. Transport Independence

UBOP does not require any specific transport. It can operate through:

- email  
- HTTP or HTTPS endpoints  
- FTP or SFTP  
- message queues  
- shared directories  
- offline media such as QR codes or printed text  
- fax (the message body is plain text JSON)  

Any channel that can deliver UTF-8 text is theoretically suitable.

---

## 8. Human Fallback Mode

UBOP was designed to support both automated systems and human operators.

Every UBOP email begins with a plain-language disclaimer:

"You may reply in plain language. UBOP agents can interpret human responses even when not formatted as JSON."

This allows:

- hotel receptionists  
- rental car desks  
- restaurant staff  
- taxi dispatchers  

to simply answer in natural language. The agent will parse the response. This drastically lowers the barrier to adoption for small businesses.

---

## 9. Adoption Path (Redirect to Full Payments)

UBOP was designed for gradual adoption.

Phase 1: Discovery and Redirect  
Providers could reply with a list of suitable options (for example available flights or rooms) and simply include a checkout URL. No payment integration required.

Phase 2: Server-Side Confirmation  
Providers could send UBOP CONFIRMATION messages for holds, reservations, deposits, or bookings that do not require online payment.

Phase 3: Integrated Payments  
More advanced implementations could later add:

- card payments  
- bank transfers  
- vouchers  
- online wallets  
- crypto payments  
- signed receipts or on-chain confirmation  

UBOP did not prescribe a single payment method and allowed the ecosystem to evolve organically.

---

## 10. Repository Structure

The repository contains:

- UBOP JSON schema  
- Provider manifest format  
- Email transport specification  
- Lighthouse concept documentation  
- Validation rules  
- Example messages  

---

## 11. Why Development Stopped

In late 2025, OpenAI introduced the Agentic Commerce Protocol (ACP). ACP solved the same problems UBOP was designed to solve but with significantly broader ecosystem momentum and integration into the agent platform.

Continuing UBOP in parallel would have fragmented the landscape. For this reason, the project was archived.

---

## 12. License and Contact

License: Apache-2.0 (see LICENSE file)

For questions, historical interest, or discussion about agent-to-business protocols, use GitHub Issues or Discussions.

End of README.
