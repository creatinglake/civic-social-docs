---
status: stable
last-reviewed: 2026-04-19
owners: [adam]
version: 0.1
---

# Civic.Social Infrastructure Program

## Overview

Civic.Social is an initiative to develop shared digital infrastructure for democratic participation. Rather than building a single platform, the project explores how an open ecosystem of civic spaces, tools, and data services can interoperate to support healthier civic life.

Today, civic participation is often fragmented across disconnected platforms, organizations, and tools. Civic.Social proposes a modular approach in which communities operate their own civic spaces while connecting to shared capabilities and information flows.

The Civic.Social Infrastructure Program is a coordinated set of pilot initiatives designed to test and demonstrate this architecture in real-world contexts.

Each pilot focuses on a specific layer of the system while remaining compatible with the broader architecture described in the Civic.Social Architecture Baseline.

---

## Core Concept

Civic.Social separates three elements that are often bundled together in traditional civic platforms:

**Civic Spaces**
Community-operated digital spaces where discussion, collaboration, and civic participation occur. These are implemented as *Civic Hubs*.

**Civic Processes**
Modular capabilities that enable civic activity—such as citizen assemblies, advisory voting, petitions, messaging tools, or civic data services. These capabilities are implemented as *plugins*.

**Citizen Interfaces**
Interfaces that help individuals discover and participate in civic life, including civic dashboards and civic activity feeds.

These components are connected through open standards and shared identity credentials so that civic processes can appear across multiple communities and interfaces without requiring centralized control.

In this model, a capability—such as a legislative testimony tool or participatory budgeting process—can appear inside a civic hub, a personal dashboard, a civic activity feed, or even an external government website while remaining part of the same interoperable ecosystem.

---

## Architecture Strategy

The Civic.Social architecture emphasizes several design principles:


- **Federated civic spaces** operated by communities, organizations, and public institutions

- **Modular civic processes** that can be installed and reused across hubs

- **Event-based civic information flows** that surface participation opportunities

- **Citizen-controlled identity credentials** enabling trusted participation

- **Curated civic interfaces** that help individuals navigate civic life

This architecture allows many independent actors to innovate within a shared civic infrastructure while maintaining interoperability across the ecosystem.

---

## Civic.Social Infrastructure Pilots

To explore and validate this architecture, Civic.Social will implement a coordinated set of pilots. Each pilot focuses on one layer of the civic infrastructure while remaining interoperable with the others.

### 1. Civic Hub Deployment Pilot

Deploy and test real civic hubs with communities, civic organizations, or public institutions.

This pilot will explore:


- hub governance and moderation

- community collaboration tools

- installation and use of civic process plugins

The goal is to demonstrate how communities can operate their own civic spaces within the broader ecosystem.

---

### 2. Civic Process Plugin Framework Pilot

Develop the modular plugin framework that allows civic processes to integrate with civic hubs and other interfaces.

This pilot will:


- establish the plugin architecture

- create a plugin registry or marketplace prototype

- integrate several example civic processes

Example capabilities may include advisory voting, citizen assemblies, petitions, or legislative testimony tools.

This pilot demonstrates how an ecosystem of civic processes can operate across many communities.

---

### 3. Civic Activity Feed & Discovery Pilot

Develop the event aggregation and discovery layer of the system—an "Inbox for Civic Life" paired with the indexing and search infrastructure that makes civic activity findable across the ecosystem.

The civic activity feed aggregates civic events emitted by civic hubs and processes, while the discovery layer indexes hubs and processes so citizens can locate relevant civic opportunities wherever they occur.

Example feed items may include:


- civic process openings

- vote results

- community updates

- participation opportunities

Discovery capabilities include civic hub directories, process descriptors, and cross-hub search so citizens can find participation opportunities beyond the hubs they already follow.

This pilot demonstrates how civic events and civic metadata can flow across the ecosystem to reduce fragmentation in civic engagement.

---

### 4. Citizen Dashboard Pilot

Create the reference citizen interface that helps individuals navigate civic life.

The dashboard allows citizens to:


- subscribe to civic hubs

- view civic activity through the feed

- access civic tools such as ballot information or representative messaging

The dashboard serves as a curated interface for civic participation while remaining compatible with alternative interfaces built by others.

---

### 5. Civic Identity Pilot

Test the decentralized identity infrastructure that underpins trusted participation across the ecosystem.

This pilot explores:


- Decentralized Identifiers (DIDs) and the Citizen Node primitive

- Verifiable Credentials issued by jurisdictions, civic organizations, and institutions

- Personal Data Stores (PDS) for user-controlled social graph and preferences

- Identity modes and credential-based access control across civic processes

The goal is to demonstrate that citizens can authenticate and participate across many hubs and interfaces without relying on centralized identity providers, while preserving privacy and user autonomy.

---

### 6. Civic Credentialing Pilot

Test the issuance, display, and verification of civic credentials—including publicly-displayed credentials known as badges—that communicate civic roles, participation history, and affiliations.

This pilot explores:


- resident credentials

- organization credentials

- verification credentials for official civic hubs

- publicly-displayable civic badges and credential profiles

These credentials allow civic processes to verify participation eligibility and convey trusted civic identity in context, while building on the identity infrastructure developed in the Civic Identity Pilot.

---

## Program Logic

Each pilot produces meaningful value independently.

For example:


- civic hubs enable new forms of community collaboration

- civic processes expand the ecosystem of participation tools

- civic activity feeds and discovery improve awareness of civic activity

- dashboards simplify citizen participation

- decentralized civic identity enables trusted, portable participation

- civic credentials communicate roles, affiliations, and verified civic status

However, the greatest impact emerges when these components operate together as an integrated civic infrastructure.

Together they create a system in which civic participation becomes continuous, visible, and interoperable across communities, organizations, and institutions.

---

## Program Goal

The Civic.Social Infrastructure Program seeks to demonstrate that a modular civic ecosystem—rather than a single centralized platform—can provide a stronger foundation for democratic participation.

Through a coordinated set of six pilots, the program will explore how civic hubs, modular processes, civic activity feeds and discovery, citizen dashboards, decentralized civic identity, and civic credentials can work together to support a healthier civic environment.

These pilots collectively form the foundation for a new generation of interoperable civic infrastructure.

---

*Last updated: April 17, 2026*
*Civic.Social — civic.social | contact@civic.social*
