# Database Design and Use (HS2021) — Individual Assignment

Relational database design for **SmartPark**, a smart parking management company operating across multiple Australian cities. This assignment covers a full database design — from requirements analysis through to an Entity-Relationship Diagram, a logical schema, and a detailed data dictionary — plus two reflection essays on database design principles.

> Coursework completed for HS2021 (Database Design and Use), Holmes Institute, Trimester 1, 2026.

## Overview

SmartPark needs a relational database to manage its day-to-day operations: parking facilities, zones, spaces, user memberships, vehicle registrations, bookings, payments, staff, and parking violations. The goal of this design is to capture accurate, consistent data that supports smooth operations, reliable reporting, and future growth.

## What's included

### Part A — Database Design
- **Case study summary** — an overview of SmartPark's data requirements and how the entities relate to one another.
- **Entity-Relationship Diagram (ERD)** — entities, attributes, primary keys, foreign keys, relationships, and cardinalities using Crow's Foot notation. A many-to-many relationship between employees and facilities is resolved with an associative entity.
- **Data dictionary** — a table-by-table reference covering field names, data types and sizes, primary/foreign key indicators, descriptions, and constraints (NOT NULL, UNIQUE, CHECK).

### Part B — Reflection Questions
- **Question 1** — ensuring data integrity and enforcing constraints in a relational database (entity, referential, and domain integrity).
- **Question 2** — factors to consider when selecting data types and designing attributes.

## Core entities

| Entity | Purpose |
| --- | --- |
| City | Cities and their regions |
| Facility | Parking facilities within a city |
| Parking Zone | Zones within a facility (General, VIP, Disabled, EV) |
| Parking Space | Individual spaces and their status |
| User | Customers and their membership details |
| Vehicle | Vehicles registered to users |
| Booking | Parking sessions linking a user, vehicle, and space |
| Payment | Payments linked to bookings |
| Employee | Staff and their roles |
| Violation | Parking violations issued against vehicles |

## Files

| File | Description |
| --- | --- |
| `Database_Design_T1_3.docx` | Full assignment submission (Parts A and B) |

## Tools & concepts

- Relational database design and modelling
- Entity-Relationship Diagrams (Crow's Foot notation)
- Logical schema design with primary and foreign keys
- Data integrity constraints (NOT NULL, UNIQUE, CHECK)
- Data type selection and data dictionary documentation

## Author

**Sayed M Jardhe**
[GitHub Profile](https://github.com/sayedmjardhe-sudo)
Update README
