# Apocalypse Computer Architecture

This document defines the high-level architecture for the **Apocalypse Computer**: a portable, offline-first resilience computing platform designed to remain useful when internet access, grid power, cloud services, or normal logistics are unavailable.

It is intentionally practical.

## Design Goals

- **Offline first** — core functions must work with zero internet.
- **Power resilient** — runs from battery and DC-native power sources where possible.
- **Useful under stress** — fast access to the information and tools that matter.
- **Repairable and understandable** — simple systems beat fragile magic.
- **Portable or deployable** — usable as a field node, cache, or local base-station.
- **Self-hosted** — no dependency on cloud auth, app stores, or SaaS for essential functions.

## System Purpose

The Apocalypse Computer exists to provide:

- **knowledge access**
- **local communications**
- **mapping/navigation**
- **file sharing**
- **logging/coordination**
- **power-resilient compute**

It should remain functional when:

- the internet is down
- grid power is unreliable or unavailable
- cloud services are unreachable
- normal coordination and logistics are degraded

## Top-Level Architecture

The system is organized into seven layers:

1. **Power Layer**
2. **Compute Layer**
3. **Network Layer**
4. **Application Layer**
5. **Data Layer**
6. **Interface Layer**
7. **Resilience / Recovery Layer**

---

## 1) Power Layer

The power layer provides stable energy to the rest of the system.

### Inputs

- AC wall charging
- solar input
- vehicle DC input
- optional external battery packs

### Core components

- LiFePO4 battery
- charge controller / battery management
- fused DC distribution
- DC-DC converters
- voltage/current monitoring

### Outputs

- 12V rail
- 5V rail
- USB-C PD rail if required

### Design principles

- battery-buffered operation
- DC-native wherever possible
- avoid dependence on always-on AC inversion unless necessary
- safe, fused power distribution with visible state

---

## 2) Compute Layer

The compute layer runs the actual services.

### Core node

- low-power mini PC or Raspberry Pi-class system

### Responsibilities

- host local applications
- serve docs, maps, and files
- maintain local database and search index
- manage local users and authentication
- log activity and system health

### Storage model

- internal OS storage
- main SSD for content and operational data
- removable or external backup/export media

### Design principles

- low idle draw
- replaceable storage
- straightforward recovery path
- boring, stable Linux over cleverness

---

## 3) Network Layer

The network layer creates the local digital environment.

### Access modes

- local Wi‑Fi hotspot
- wired Ethernet
- optional point-to-point radio bridge or mesh link

### Core functions

- DHCP / DNS
- local service discovery
- local web portal
- optional segmentation for:
  - admin
  - user/client
  - radio/sensor devices

### Design principles

- all core services reachable locally with zero internet
- friendly local names instead of raw IPs
- simple onboarding for stressed users

---

## 4) Application Layer

This is what users actually interact with.

### Core services

#### Portal / Homepage
A single entry point to the whole system.

#### Knowledge base
Offline wiki, manuals, PDFs, procedures, and survival references.

#### Maps
Local maps, topo references, navigation layers, and area notes.

#### File server
Upload, download, sync, and sharing of critical files.

#### Search
Cross-search docs, notes, manuals, and local archives.

#### Notes / Logbook
Incident logs, observations, updates, and local SOP edits.

### Optional services

- local chat
- task board
- inventory tracker
- medical reference index
- radio frequency/reference tools
- small local AI assistant

### Design principles

- browser-first UX
- usable from phones, tablets, and laptops
- no weird client dependency for core functions

---

## 5) Data Layer

The data layer is the actual survival payload.

### Content domains

- medical / first aid / trauma
- repair / electrical / mechanical
- agriculture / gardening / food preservation
- water purification / sanitation
- shelter / heating / fuel
- radio / communications
- local maps / routes / infrastructure
- SOPs / contacts / local plans

### Data classes

- static archives
- editable operational notes
- logs and events
- config and backups

### Design principles

- keep reference library separate from live operational data
- prioritize dense, high-value knowledge over bulk clutter
- make export and backup easy

---

## 6) Interface Layer

This is how humans use the system.

### Primary clients

- phones
- tablets
- laptops
- optional directly attached display and keyboard

### Interaction model

1. connect to local Wi‑Fi or Ethernet
2. open the portal in a browser
3. access services from the homepage

### Design principles

- no special software required for normal use
- understandable under stress
- quick path to the most important information

---

## 7) Resilience / Recovery Layer

This layer keeps the system useful after mistakes, corruption, or physical disruption.

### Includes

- configuration backups
- content backups
- boot/recovery media
- printed quick-start and recovery guide
- spare cables, adapters, and storage media
- health/status dashboard
- graceful degraded modes

### Design principles

- fail soft, not hard
- document recovery before it is needed
- assume the operator may be tired, stressed, or offline

---

## Logical Architecture Diagram

```text
[ Power Inputs ]
  AC / Solar / Vehicle
        |
        v
[ Battery + Charge Control + DC Distribution ]
        |
        +-------------------+
        |                   |
        v                   v
 [ Compute Node ]      [ Network Node ]
        |                   |
        +---------+---------+
                  |
                  v
          [ Local Service Mesh ]
                  |
    +------+------+------+------+------+
    |             |             |      |
    v             v             v      v
 [Portal]      [Knowledge]    [Maps] [Files/Logs]
                  |
                  v
           [ Search + Index ]
                  |
                  v
         [ Phones / Tablets / Laptops ]
```

## Operational Modes

### 1) Library Mode
Focus:
- offline docs
- manuals
- maps
- search

Use case:
- research, reference, training, troubleshooting

### 2) Coordination Mode
Focus:
- shared notes
- logs
- files
- local comms
- situation tracking

Use case:
- neighborhood or team response

### 3) Field Utility Mode
Focus:
- battery efficiency
- maps
- medical and repair quick access
- upload of observations, photos, and logs

Use case:
- outage support, field operations, scouting

### 4) Degraded Survival Mode
Only essential services stay up:
- portal
- docs
- maps
- basic file access

Use case:
- low battery, partial hardware failure, reduced staffing

## Security Model

This is resilience security, not enterprise theater.

### Principles

- local accounts
- no cloud auth dependency
- encrypted storage where practical
- role separation:
  - admin
  - operator
  - guest / read-only
- service isolation where useful
- offline backups

### Threats considered

- device theft
- accidental misconfiguration
- data corruption
- low-power events
- user confusion under stress

## Physical Architecture Variants

### Option A — Backpack Node
- smallest
- lowest power
- one compute node + battery + router

Best for:
- portability

### Option B — Case-Based Node
- Pelican/ammo-case style enclosure
- more rugged
- easier cable discipline and field deployment

Best for:
- durable field use

### Option C — Base Station Node
- larger battery
- larger storage
- external antenna options
- optional solar companion kit

Best for:
- neighborhood resilience hub

### Recommended starting point

**Case-Based Node first.**

It is the best balance of portability, ruggedness, serviceability, and sane cable management.

## Recommended v1 Architecture

Keep v1 boring and effective.

### Power
- 12V LiFePO4 battery
- DC fuse block
- 5V / USB-C converters

### Compute
- one low-power Linux node
- one SSD

### Network
- one local AP/router
- local DNS + portal

### Applications
- portal
- offline docs / Kiwix
- offline maps
- file share
- notes / logbook
- search

### Operations
- backup/export process
- printed recovery instructions
- spare cables and boot media

This is enough to produce a real apocalypse computer instead of a complicated prop.

## Design Priorities

The priorities, in order:

1. **Offline usability**
2. **Power efficiency**
3. **Simplicity**
4. **Repairability**
5. **Content quality**
6. **Ruggedness**
7. **Expandability**
8. **Advanced features**

If this order gets reversed, the system becomes impressive and fragile — the worst possible combo.

## Non-Goals

To keep the project sane, this architecture intentionally avoids:

- dependence on cloud services for core functions
- heavy compute for its own sake
- complicated distributed systems in v1
- fancy features that reduce reliability
- brittle hardware stacks that require special parts or tribal knowledge

## Future-State Definition

The Apocalypse Computer is "ready" when:

- critical knowledge is available offline
- local users can connect and use it from a browser
- it runs from resilient battery-backed power
- recovery steps are documented and testable
- the system remains useful in degraded conditions
- storage, services, and physical layout are understandable enough to repair under stress

---

If reality changes, this file should change with it. Architecture that only exists in someone’s head is just future confusion with better branding.
