# Architecture and Design Decisions

## Components

### Static frontend

The public experience is implemented with semantic HTML, custom CSS, and vanilla JavaScript. It contains the service information, interactive availability calendar, and request-preparation form.

### Availability API

The frontend retrieves month-specific availability from a separate HTTPS endpoint hosted on Cloudflare Workers. Responses are validated before they are used, including the requested month, time zone, timestamp freshness, and busy-period structure.

### Local request preparation

Form data remains in the browser while the customer reviews the request. The site then opens a prefilled email draft or copies the message to the clipboard. This avoids claiming a booking has been submitted or confirmed when the client still needs to review it.

## Reliability Behavior

- availability requests use a timeout rather than waiting indefinitely
- cached month data expires and is refreshed
- the active month refreshes when the page becomes visible again
- selected dates are checked again before a request is prepared
- unavailable calendar data produces an explicit confirmation-needed state
- input changes during an availability check require the customer to prepare the request again

## Date Handling

- calendar calculations use explicit date keys rather than locale-formatted strings
- the configured business time zone is validated before use
- requests are limited to valid dates within a rolling 12-month window
- overnight departure dates are treated as checkout dates
- drop-in visits allow a one-day range
- unusually long ranges and malformed API periods are rejected

## Accessibility

- controls use native buttons, inputs, selects, and text areas
- dynamic updates are announced through appropriate live regions
- calendar buttons expose descriptive labels and selection state
- keyboard focus remains visible
- reduced-motion preferences are respected
- the layout and controls adapt for small screens

## Security and Privacy

The deployment uses defensive browser headers including:

- Content Security Policy restricting content and network connections
- denial of framing
- MIME-sniffing protection
- disabled camera, microphone, and geolocation permissions
- restrictive form actions

The interface also tells customers not to enter door codes or similar sensitive information and makes clear that nothing is sent automatically.

## Scope

This repository is a public case study. It describes the solution and verified behavior without publishing client data, availability records, credentials, or private operational details.

