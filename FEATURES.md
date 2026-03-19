# Apocalypse Computer Features

This document lists the intended features of the **Apocalypse Computer**: an offline-first resilience computing platform designed to preserve knowledge, provide local services, and stay useful during infrastructure failure.

## Core Platform Features

### 1. Offline-First Operation
- Operates without internet connectivity
- No cloud dependency for core functions
- Local accounts and local service access
- Designed to remain useful during grid-down, internet-down, or degraded logistics conditions

### 2. Resilient Power Design
- Battery-backed operation
- Designed for LiFePO4-based DC power systems
- Supports multiple charging inputs:
  - AC wall charging
  - solar charging
  - vehicle DC charging
- DC-native rails where possible for better efficiency

### 3. Local Compute Stack
- Low-power Linux-based compute node
- Runs all critical services locally
- Replaceable local storage
- Designed for stable, recoverable operation under stress

### 4. Local Network Services
- Local Wi‑Fi hotspot and/or Ethernet access
- Local DNS and service discovery
- Friendly local service names instead of raw IP dependency
- Browser-first access from phones, tablets, and laptops

### 5. Unified Local Portal
- Single homepage / entry point into the system
- Quick access to maps, documents, logs, files, and local tools
- Optimized for use during emergency or field conditions

## Knowledge and Library Features

### 6. Offline Survival Library
- Local survival manuals
- Local medical and first aid references
- Repair and maintenance guides
- Water, shelter, sanitation, and food preservation references
- Agriculture and gardening references

### 7. Local eBooks and PDF Archive
- Offline collection of survival-oriented books and PDFs
- Searchable and browsable reference library
- Supports rapid access to dense knowledge without internet
- Intended for manuals, field guides, reference books, and structured documents

### 8. Offline Maps
- Local map storage
- Topographic and reference maps
- Area-specific maps and notes
- Useful for navigation, logistics, and local coordination

### 9. Search and Retrieval
- Cross-search across manuals, PDFs, notes, and local archives
- Fast lookup of high-value reference material
- Supports retrieval-oriented workflows in disconnected environments

## AI and Knowledge Features

### 10. Local Ollama Runtime
- Runs Ollama locally
- Supports local LLM inference without cloud dependency
- Enables local question-answering, summarization, and assistance
- Useful when internet models are unavailable or inappropriate

### 11. Local OpenClaw Runtime
- Runs OpenClaw locally
- Provides local agent-driven workflows and automation
- Supports local orchestration, task routing, and service access
- Designed to remain available even in offline conditions

### 12. RAG (Retrieval-Augmented Generation)
- Uses local document collections as grounding data for local models
- Supports question-answering over manuals, PDFs, SOPs, and archived knowledge
- Enables high-value retrieval from survival, repair, and medical documentation
- Reduces reliance on raw model memory alone

### 13. Local Knowledge Graph
- Maintains structured local knowledge about people, places, procedures, systems, and resources
- Supports relationship-aware lookup and context-building
- Useful for local planning, resource tracking, and accumulated operational knowledge
- Complements RAG with structured facts and linked entities

## Communications and Coordination Features

### 14. Meshtastic Support
- Runs or interfaces with Meshtastic locally
- Supports local/off-grid mesh messaging workflows
- Can act as a bridge or coordination node for disconnected communications
- Useful for neighborhood-scale or field-scale resilience messaging

### 15. Local Coordination Tools
- Shared notes and incident logs
- Local SOP updates
- Operational observations and event tracking
- Designed for team or neighborhood coordination

### 16. File Sharing
- Local file upload, download, and sharing
- Useful for moving maps, documents, images, field reports, and backups
- Works without cloud file services

## Resilience and Recovery Features

### 17. Degraded-Mode Operation
- Essential services remain available under low-power or partial-failure conditions
- Supports a stripped-down “survival mode” with only core functions enabled

### 18. Backup and Recovery Support
- Local configuration backups
- Content backups
- Boot/recovery media support
- Printed quick-start and recovery documentation
- Replaceable storage and recoverable system design

### 19. Simple, Repairable Architecture
- Avoids unnecessary complexity in v1
- Prioritizes serviceability and understandable system layout
- Built around practical, replaceable parts and local control

## Optional Expansion Features

### 20. Local Chat
- Local-only chat for disconnected teams
- Supports basic coordination without external messaging platforms

### 21. Task Board
- Shared local task list or work queue
- Useful for response coordination and operational management

### 22. Inventory Tracker
- Tracks parts, supplies, tools, and other resources locally
- Useful for planning and survival logistics

### 23. Medical Reference Index
- Specialized quick-access layer for medical and trauma references
- Designed to reduce time-to-information during emergencies

### 24. Radio / Comms Reference Tools
- Frequency references
- radio documentation
- procedures and quick-reference communications notes

### 25. Small Local AI Assistant
- Browser-accessible assistant powered by local models
- Grounded on local knowledge via RAG
- Can assist with troubleshooting, retrieval, and procedural guidance

## Feature Priorities

The intended priority order is:

1. Offline usability
2. Power efficiency
3. Simplicity
4. Repairability
5. Knowledge quality
6. Local coordination capability
7. AI augmentation
8. Advanced expansion features

## Current Status

At present, these are **architectural and planned features**, not a full implementation.

The repository currently contains:
- `README.md`
- `architecture.md`
- `FEATURES.md`
- starter folders for docs, hardware, software, and assets

Implementation work will define the exact service stack, hardware BOM, and deployment model.
