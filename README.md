# Radio-Status V2.4

> Lightweight, static and near-real-time radio amateur station status system.

[![Version](https://img.shields.io/badge/version-2.4-blue)](#)
[![Status](https://img.shields.io/badge/status-stable-green)](#)
[![Platform](https://img.shields.io/badge/platform-Web-orange)](#)
[![Architecture](https://img.shields.io/badge/architecture-static-lightgrey)](#)

**Radio-Status** is a lightweight web application designed for radio amateurs to publish the current operating status of a radio station or operator.

The system is fully client-side and does not require a traditional application backend, database or server-side runtime.

**Live instance:**  
https://sp7kjr-kacper.github.io/radio-status/

---

## Table of Contents

- [About the Project](#about-the-project)
- [Project Background](#project-background)
- [Key Features](#key-features)
- [Architecture](#architecture)
- [Data Source](#data-source)
- [Technologies](#technologies)
- [Application Structure](#application-structure)
- [Status System](#status-system)
- [Dynamic Themes](#dynamic-themes)
- [Reliability](#reliability)
- [Security](#security)
- [Performance](#performance)
- [Responsive Design](#responsive-design)
- [Accessibility](#accessibility)
- [Integrations](#integrations)
- [Installation and Deployment](#installation-and-deployment)
- [Configuration](#configuration)
- [Limitations](#limitations)
- [Example Use Cases](#example-use-cases)
- [Future Development](#future-development)
- [Version 2.4](#version-24)
- [Authors](#authors)
- [Summary](#summary)

---

# About the Project

**Radio-Status V2.4** is a web-based status publishing system created for radio amateurs.

It allows an operator to publish information such as:

- current operating status,
- date and time of the last update,
- frequency or repeater,
- radio network,
- QTH,
- additional comments or notices.

The displayed information is retrieved automatically from a public Google Sheets data source and periodically refreshed by the client.

The system was designed around several principles:

- minimal infrastructure,
- low resource usage,
- simple deployment,
- high availability,
- operation without an application backend,
- mobile-friendly status management,
- easy integration with an amateur radio website or QRZ profile.

The application can be hosted on a static hosting platform such as GitHub Pages.

---

# Project Background

Radio amateurs frequently need a simple way to communicate whether a station is currently active, what it is doing and where it is operating.

Typical information includes:

- whether the operator is available,
- whether a QSO is currently in progress,
- which frequency or repeater is being monitored,
- which network is being used,
- whether the operator is working mobile or portable,
- whether a special notice or alert should be displayed.

Radio-Status was created as a lightweight solution to this problem.

The project intentionally avoids unnecessary infrastructure. Instead of introducing a dedicated backend and database, the application uses Google Sheets as its operational data source and processes the information directly in the user's browser.

---

# Key Features

## Near-real-time status updates

The application periodically retrieves the current status from Google Sheets.

The default refresh interval is:

```text
60 seconds
```

This provides near-real-time updates without requiring WebSockets or a permanent server connection.

---

## Public status panel

The public interface can display:

- status,
- last update time,
- frequency or repeater,
- network,
- QTH,
- comment / notice.

The information is presented in a single status-oriented dashboard.

---

## Dynamic status themes

The visual appearance of the interface changes according to the current operating status.

Depending on the status, the application can modify:

- background gradients,
- panel colors,
- accent colors,
- animations,
- activity indicators,
- visual effects.

This allows the current state of the station to be recognized immediately without reading every field.

---

## Local data cache

The application stores the most recently retrieved data in the browser's `LocalStorage`.

If the remote data source temporarily becomes unavailable, the application can use the previously stored data instead of displaying an empty interface.

---

## Failure handling

The application includes client-side mechanisms for handling temporary communication problems.

These include:

- request timeout,
- retry attempts,
- network failure handling,
- locally cached data,
- fallback behaviour.

---

# Architecture

Radio-Status uses a client-side architecture.

```text
┌─────────────────┐
│   Google Sheets │
└────────┬────────┘
         │
         ▼
┌────────────────────────────┐
│ Google Visualization /    │
│ gviz data endpoint         │
└────────┬───────────────────┘
         │
         ▼
┌─────────────────┐
│ Fetch API       │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Data Parser     │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Status Logic    │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ HTML / CSS UI   │
└─────────────────┘
```

The application does not require a dedicated server-side application runtime.

The current architecture does not depend on:

- Node.js,
- PHP,
- Python,
- MySQL,
- PostgreSQL,
- Docker,
- a custom backend API.

The main application logic executes in the user's browser.

---

# Data Source

Google Sheets is used as the operational data source.

A simplified data structure may look like:

```text
status / notice    OFFLINE
updated            23.08.2026, 16:12
frequency          SR7KU
network            NO NETWORK
qth                Lodz
comment
```

The client retrieves the published spreadsheet data and maps individual fields to the corresponding elements of the interface.

The parser is responsible for interpreting the received data and providing it to the status and rendering logic.

---

# Technologies

## Frontend

- HTML5
- CSS3
- Vanilla JavaScript
- JavaScript Fetch API
- LocalStorage

## Data

- Google Sheets
- Google Visualization / `gviz` endpoint

## Hosting

The application is compatible with static hosting.

The reference deployment uses:

- GitHub Pages

No server-side build process is required for the basic application.

---

# Application Structure

The application is implemented primarily as a single HTML document containing the required frontend structure, styling and JavaScript logic.

Conceptually, the interface consists of:

```text
Header
│
├── Status Panel
│
├── Dashboard
│
├── Comments / Notices
│
├── Contact Section
│
├── Abbreviations
│
├── QRZ Logbook
│
└── Footer
```

The single-file approach reduces deployment complexity and eliminates the need for a frontend build system.

---

# Status System

Radio-Status uses predefined status categories to communicate the current operating condition of the station.

## OFFLINE

The operator or station is currently unavailable.

```text
OFFLINE
```

---

## QRL / DND

The operator is currently busy or should not be disturbed.

```text
QRL
DND
```

---

## SCAN / QSX

The station is actively monitoring or working radio traffic.

```text
SCAN
QSX
```

These statuses represent active radio operation without necessarily indicating a specific QSO.

---

## MOBILE / PORTABLE

The operator is working away from the normal station location.

```text
MOBILE
PORTABLE
```

This can be used for mobile, portable or field operation.

---

## QAZ / ALERT

An operational alert or warning condition.

```text
QAZ
ALERT
```

The status is visually distinguished from normal operating states.

> The exact semantics of individual statuses should follow the project's current implementation and status specification.

---

# Dynamic Themes

The status system is integrated with the visual theme engine.

The active status can determine:

- background appearance,
- interface accents,
- panel styling,
- button styling,
- animation state,
- activity indicators.

Certain active states can additionally enable visual animation such as pulsing effects.

The purpose is functional rather than purely decorative: the current state should remain recognizable at a glance.

---

# Reliability

## Request timeout

Remote requests use a timeout mechanism to prevent the application from waiting indefinitely for an unavailable source.

Current configured timeout:

```text
12 seconds
```

---

## Retry mechanism

Failed requests may be retried automatically.

Current configuration:

```javascript
MAX_RETRIES = 2
```

The exact behaviour should be considered together with the current implementation in `index.html`.

---

## Local cache

The latest successful data set is stored locally.

Current storage key:

```text
sp7kjr-radio-status-v2
```

When the remote source cannot currently be reached, previously cached data may be used as a fallback.

---

# Security

The application uses a **Content Security Policy (CSP)** to restrict the sources from which content and network requests may be loaded.

The policy is intended to reduce the attack surface associated with:

- unauthorized script execution,
- unexpected external resources,
- unwanted network connections.

The effective policy should always be verified against the current CSP declaration in the source code.

Security controls in a client-side application do not replace security controls on the data source itself.

---

# Performance

The project follows a minimal-dependency architecture.

Key characteristics include:

- single HTML application,
- no JavaScript framework,
- no external JavaScript libraries required for the core interface,
- no application backend,
- no database,
- static hosting compatibility,
- relatively small client-side workload.

The architecture is particularly suitable for a status page where the amount of transferred operational data is small.

---

# Responsive Design

The interface is designed to adapt to different screen sizes.

Supported device categories include:

- smartphones,
- tablets,
- laptops,
- desktop computers.

The current mobile breakpoint is approximately:

```css
560px
```

Responsive behaviour is implemented through CSS media queries and adaptive layout rules.

---

# Accessibility

The interface includes accessibility-oriented HTML attributes and semantic roles.

Examples include:

```html
aria-label
aria-live
role="status"
role="alert"
```

These mechanisms can improve communication of dynamic status changes to assistive technologies such as screen readers.

Accessibility should be considered an ongoing part of the project rather than a one-time feature.

---

# Integrations

## QRZ

The project can be associated with the operator's QRZ profile.

Reference station profile:

```text
https://www.qrz.com/db/SP7KJR
```

---

## QRZ Logbook

The application can include an embedded QRZ Logbook view.

Reference:

```text
https://logbook.qrz.com/lbstat/SP7KJR/
```

The integration allows the status page to combine current operating information with a view of logged radio contacts.

---

## Google Sheets

Google Sheets acts as the operational data source:

```text
Google Sheets
      │
      ▼
Visualization / gviz
      │
      ▼
Radio-Status
```

This allows the operator to update the status without modifying the application source code.

---

# Installation and Deployment

Radio-Status is designed for static deployment.

## Basic requirements

- a web browser with JavaScript support,
- a public Google Sheets data source,
- static web hosting,
- the project source files.

A server-side application runtime is not required.

## Example deployment

The reference deployment uses GitHub Pages.

Typical deployment flow:

```text
Clone repository
      │
      ▼
Configure data source
      │
      ▼
Commit changes
      │
      ▼
GitHub repository
      │
      ▼
GitHub Pages
      │
      ▼
Public Radio-Status page
```

---

# Configuration

The application's configuration depends on the current source implementation.

Before deploying another instance, the following project-specific values may need to be changed:

- Google Sheets identifier / endpoint,
- station callsign,
- public links,
- QRZ Logbook identifier,
- displayed station information,
- status definitions,
- branding,
- contact information.

Configuration should be performed directly against the current source code rather than copying values specific to the reference station.

---

# Limitations

The static architecture provides simplicity, but it also introduces limitations.

The current design does not inherently provide:

- server-side authentication,
- a dedicated administration panel,
- server-side history storage,
- guaranteed instant delivery of updates,
- centralized multi-station management,
- a dedicated REST API,
- server-side validation of published status data.

The system is also dependent on the availability and behaviour of its external data source.

The project therefore prioritizes:

> **simplicity, low infrastructure requirements and reliability over large-scale backend functionality.**

---

# Example Use Cases

## Individual radio amateur

A personal station can publish its current operating status on a website or profile.

## Radio club

A club can use the system to communicate the current activity of its station.

## Special event station

The status panel can provide current operating information to visitors and other operators.

## Portable operation

The system can indicate that an operator is working portable or mobile.

## SOTA / POTA / WWFF

The status page can complement an activation by publishing the current station state and operating information.

## Operational communication

The status mechanism can be used for simple availability or warning information where appropriate.

---

# Future Development

Possible future development directions include:

- multi-station support,
- status history,
- activity statistics,
- APRS integration,
- DMR integration,
- BrandMeister integration,
- automatic activity detection,
- REST API,
- location map,
- push notifications,
- data export,
- additional radio-network integrations.

These features are not necessarily part of the current V2.4 implementation.

---

# Version 2.4

Version 2.4 represents a significant stage in the development of Radio-Status.

The release includes improvements to the application's:

- interface,
- status logic,
- visual system,
- reliability mechanisms,
- integrations,
- information architecture.

The exact differences between V2.3 and V2.4 should be documented in the project's changelog or release notes.

---

# Authors

## Creator

**Kacper Juszko**

Project concept, architecture, implementation, interface design and development of Radio-Status.

## AI-assisted development

The project was developed with the assistance of modern AI-based development tools, including:

- ChatGPT / OpenAI Codex
- GitHub Copilot
- Gemini

AI tools were used as development assistance in areas such as:

- architecture discussion,
- implementation,
- debugging,
- interface development,
- optimization,
- documentation.

The project design, decisions, testing and final integration remain part of the project development process.

---

# Summary

Radio-Status V2.4 is a lightweight, client-side system for publishing amateur radio station status information.

Its architecture deliberately avoids a traditional backend and database. Instead, the application retrieves operational data from Google Sheets, processes it in the browser and presents the current information through a responsive web interface.

The combination of static hosting, client-side processing, local caching, retry handling and dynamic status themes makes the system suitable for individual amateur radio operators, clubs, special event stations and portable operation.

The main architectural principle is simple:

> **Provide useful real-time-oriented station information with as little infrastructure as possible.**

Radio-Status is therefore not intended to replace a full backend platform. It is a deliberately lightweight tool designed around the practical needs of amateur radio communication.
