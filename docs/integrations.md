# Integrations

This document defines external and adjacent-system integrations for the **Apocalypse Computer**.

The first integration target is **Pantry Helper**, which provides pantry, food, and household inventory data over an API.

## Design Intent

The Apocalypse Computer should be able to consume pantry state as a local resilience signal.

That means it should be able to answer questions like:

- What food do we currently have?
- What is running low?
- What categories are overstocked or understocked?
- What can we cook from current inventory?
- What supplies are most critical in an outage scenario?
- What should be preserved, rotated, rationed, or replaced soon?

The integration should work **locally first**, degrade gracefully, and avoid unnecessary external dependencies.

---

## Integration Principles

### 1. Local-first access
Prefer local network API access to Pantry Helper whenever available.

Examples:
- `http://pantry-helper.local`
- `http://pantry-helper.lan`
- static local IP or mDNS name

### 2. Graceful degradation
If Pantry Helper is unavailable, the Apocalypse Computer should:
- continue operating normally
- report pantry integration status clearly
- fall back to cached pantry snapshots if available
- avoid blocking unrelated system functions

### 3. Read-mostly by default
The initial integration should be **read-heavy** and conservative.

Preferred early capabilities:
- read pantry inventory
- read categories and quantities
- read expiry / freshness / low-stock state
- read meal or supply suggestions if Pantry Helper provides them

Mutation should come later and be deliberate.

### 4. Explicit trust boundaries
Pantry data is operationally useful but should not be treated as perfect truth.

The Apocalypse Computer should distinguish between:
- live Pantry Helper API data
- cached pantry snapshots
- locally inferred planning recommendations

---

## Primary Use Cases

### Pantry status lookup
- show current inventory
- show low-stock items
- show critical shortages
- show shelf-stable reserve levels

### Resilience planning
- identify weak points in food and supply reserves
- compare inventory against target stock levels
- support outage and bug-in planning

### Meal and ration support
- surface what can be cooked from current inventory
- support meal suggestions under constraint
- support ration-aware planning during disruptions

### Knowledge enrichment
- feed pantry state into RAG and knowledge workflows
- connect pantry items to survival documents, recipes, preservation guides, and local SOPs

### Coordination workflows
- expose pantry summaries in local dashboards
- support local status boards or neighborhood planning if desired

---

## Proposed Architecture

```text
[ Pantry Helper API ]
         |
         v
[ Integration Client / Sync Job ]
         |
         +------------------------+
         |                        |
         v                        v
[ Local Cache / Snapshot ]   [ Normalized Data Model ]
         |                        |
         +-----------+------------+
                     |
                     v
          [ Apocalypse Computer Services ]
                     |
     +---------------+----------------+
     |               |                |
     v               v                v
 [ Portal ]   [ RAG / AI Assistant ] [ Planning / Logs ]
```

---

## Integration Components

### 1. Pantry API Client
A local integration component responsible for:
- calling Pantry Helper endpoints
- handling auth if required
- validating responses
- reporting service health

### 2. Normalizer
Transforms Pantry Helper responses into a stable local shape.

This is important because the Apocalypse Computer should not couple every internal feature directly to Pantry Helper’s raw schema.

### 3. Local Cache / Snapshot Store
Stores the most recent successful pantry state.

Purpose:
- survive temporary Pantry Helper outages
- speed up queries
- provide a recoverable local record
- support degraded mode

### 4. Integration Health Status
Expose whether Pantry Helper is:
- reachable
- authenticated
- stale
- unavailable
- using cached data

This status should be visible in the local portal.

---

## Suggested Data Domains

The normalized pantry model should support at least these domains:

### Inventory Items
- item name
- category
- quantity
- unit
- location
- freshness / expiry
- minimum threshold
- notes

### Categories
- canned goods
- dry goods
- water
- medical supplies
- hygiene supplies
- cooking fuel
- pet supplies
- tools / consumables

### Inventory State Signals
- in stock
- low stock
- out of stock
- expiring soon
- critical item
- shelf-stable
- rotation candidate

### Planning Signals
- days of food remaining (if derivable)
- high-risk gaps
- dependency categories
- recommended restock priorities

---

## Suggested API Capabilities

These are the capabilities the Apocalypse Computer should expect or request from Pantry Helper.

### Read endpoints

#### `GET /api/inventory`
Returns current pantry inventory.

#### `GET /api/inventory/low-stock`
Returns items below configured thresholds.

#### `GET /api/inventory/expiring`
Returns items nearing expiry.

#### `GET /api/categories`
Returns available pantry categories.

#### `GET /api/summary`
Returns high-level pantry summary.

Example summary fields:
- total items
- low stock count
- expiring soon count
- critical shortages
- last updated timestamp

#### `GET /api/meals/suggestions`
Optional: returns meal suggestions based on available stock.

#### `GET /api/resilience/summary`
Optional: returns resilience-oriented stock analysis if Pantry Helper implements it.

### Future write endpoints
These should be phased in later.

#### `POST /api/inventory/snapshot`
Push or store an inventory snapshot.

#### `POST /api/consumption`
Record consumption events.

#### `POST /api/restock-plan`
Create or update restock plans.

The initial Apocalypse Computer integration does **not** need to mutate pantry state unless there is a clear operational reason.

---

## Authentication Model

Preferred order:

1. local-network trust with firewall isolation
2. API token
3. mTLS or stronger service auth if the environment warrants it

Initial recommendation:
- use a scoped API token for Apocalypse Computer → Pantry Helper calls
- store credentials locally in the approved secret/config path
- avoid hardcoding secrets in docs or repository files

---

## Caching and Offline Behavior

### Cache strategy
The Apocalypse Computer should cache:
- last successful pantry summary
- last successful full inventory snapshot
- timestamp of last sync
- integration health state

### Offline behavior
If Pantry Helper becomes unavailable:
- mark integration as degraded
- continue serving cached data with clear staleness metadata
- allow RAG and portal features to keep using the last known good snapshot
- do not fail core system startup

### Staleness policy
Recommended display states:
- **fresh**: synced recently
- **stale**: cache available but sync overdue
- **offline**: Pantry Helper unreachable, cache in use
- **unknown**: no successful sync yet

---

## RAG / Knowledge Graph Integration

Pantry data should not just sit in a JSON blob like a forgotten can of beans.

### RAG usage
Pantry state can ground answers such as:
- what can we cook now?
- what critical shortages do we have?
- what food is expiring soon?
- what should we prioritize in an outage?

### Knowledge graph usage
Pantry entities can link to:
- recipes
- preservation guides
- nutrition references
- storage procedures
- local family or team preferences
- replenishment priorities
- related tools or fuel dependencies

This lets the Apocalypse Computer reason over inventory as part of a broader resilience model.

---

## Portal Integration

The local portal should expose pantry information in a dedicated section.

Suggested widgets:
- pantry summary
- low-stock list
- expiring items
- reserve status
- last sync timestamp
- Pantry Helper integration health

Potential pages:
- `/pantry`
- `/pantry/summary`
- `/pantry/critical`
- `/pantry/meals`

---

## Failure Modes

The design should explicitly handle:

### 1. Pantry Helper offline
Use cached data and flag stale status.

### 2. Auth failure
Flag integration unhealthy, preserve cached data, require operator action.

### 3. Schema drift
Normalizer should reject invalid/unexpected payloads safely and log the mismatch.

### 4. Partial data
Portal and assistant should display partial results clearly instead of pretending completeness.

### 5. Corrupt cache
Fall back to previous valid snapshot if possible.

---

## Recommended v1 Scope

For v1, keep this integration lean:

### Must have
- pantry summary fetch
- inventory fetch
- low-stock fetch
- local cache
- health/status reporting
- portal visibility

### Nice to have
- expiring items
- meal suggestions
- RAG grounding from pantry inventory

### Later
- write-back to Pantry Helper
- automated replenishment workflows
- advanced planning and forecasting
- neighborhood/shared inventory federation

---

## Open Questions

These need answers before implementation:

1. What API shape does Pantry Helper already expose?
2. Does Pantry Helper already support auth tokens?
3. What inventory schema is canonical?
4. How often should Apocalypse Computer sync pantry state?
5. Should pantry state be query-only, or can Apocalypse Computer mutate it?
6. Should pantry data participate directly in the knowledge graph, or only through snapshots?
7. What counts as a critical pantry shortage for alerting purposes?

---

## Recommendation

Treat Pantry Helper as a **first-class resilience subsystem**, not just an external app.

Food, water, fuel, and household stock are operational data. The Apocalypse Computer should understand that state well enough to help with planning, prioritization, and decision support — especially when the internet is gone and everyone suddenly discovers logistics is not a theoretical concept.
