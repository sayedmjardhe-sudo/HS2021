# SmartPark — Database Design (Part A)

A relational database design for **SmartPark**, a smart parking management
company that operates across multiple cities and offers automated parking
services through mobile apps.

This repository contains the complete database design: the case study, the
entity-relationship model, and the full data dictionary for all tables.

---

## Table of Contents

1. [Case Study Summary](#case-study-summary)
2. [Entity Relationship Diagram (ERD)](#entity-relationship-diagram-erd)
3. [Entities Overview](#entities-overview)
4. [Data Dictionary](#data-dictionary)
   - [City](#1-city)
   - [Facility](#2-facility)
   - [Parking Zone](#3-parking-zone)
   - [Parking Space](#4-parking-space)
   - [User](#5-user)
   - [Vehicle](#6-vehicle)
   - [Booking](#7-booking)
   - [Payment](#8-payment)
   - [Employee](#9-employee)
   - [Facility_Employee](#10-facility_employee-associative-entity)
   - [Violation](#11-violation)
5. [Key Relationships](#key-relationships)

---

## Case Study Summary

SmartPark is an emerging smart parking management company that operates in
several cities and offers smart parking services with automated systems and
mobile apps. The company handles business operations such as parking
facilities, parking zones, parking spaces, users, vehicles, bookings,
payments, staff, and offences.

The company needs a relational database to manage data related to its
operations:

- A **city** has multiple **parking facilities**.
- Each **facility** has multiple **zones** — General, VIP, Disabled, and
  Electric Vehicle zones.
- Each **zone** has multiple **parking spaces**, each with a status
  (Available, Occupied, Reserved, or Out of Service).
- **Users** can register and own multiple **vehicles**.
- Users **book** parking spaces; each booking records entry/exit times and
  cost.
- Bookings are **paid** for using various payment methods, and payments are
  tracked.
- **Facilities** are managed and monitored by **employees**. An employee can
  manage more than one facility (a many-to-many relationship), resolved with
  an **associative entity** (`Facility_Employee`).
- The system records **violations** — overstay, no payment, or improper
  parking — associated with vehicles and issued by employees.

The design maintains correct relationships among entities and supports data
storage, retrieval, and reporting, helping SmartPark improve efficiency,
maintain data integrity, and plan for growth.

---

## Entity Relationship Diagram (ERD)

> **Figure 1: ERD Diagram of SmartPark**
>
> Add your ERD image to an `images/` folder in this repo and it will display
> here. (Replace the path below with your file name.)

![SmartPark ERD](images/erd.png)

---

## Entities Overview

| # | Entity | Purpose |
|---|--------|---------|
| 1 | City | Cities where SmartPark operates |
| 2 | Facility | Parking facilities within a city |
| 3 | Parking Zone | Zones inside a facility (General, VIP, Disabled, EV) |
| 4 | Parking Space | Individual parking spaces within a zone |
| 5 | User | Registered customers |
| 6 | Vehicle | Vehicles owned by users |
| 7 | Booking | Parking reservations made by users |
| 8 | Payment | Payments made against bookings |
| 9 | Employee | Staff who manage facilities |
| 10 | Facility_Employee | Associative entity (employee ⇄ facility) |
| 11 | Violation | Parking offences issued by employees |

---

## Data Dictionary

### 1. City

| Field Name | Data Type | PK/FK | Description | Constraints |
|------------|-----------|-------|-------------|-------------|
| cityID | INT | PK | Unique city ID | NOT NULL |
| cityName | VARCHAR(100) | | Name of city | NOT NULL |
| region | VARCHAR(50) | | State | NOT NULL |

### 2. Facility

| Field Name | Data Type | PK/FK | Description | Constraints |
|------------|-----------|-------|-------------|-------------|
| facilityID | INT | PK | Unique facility ID | NOT NULL |
| facilityName | VARCHAR(100) | | Facility name | NOT NULL |
| streetAddress | VARCHAR(150) | | Facility address | NOT NULL |
| capacity | INT | | Total parking spaces | CHECK (capacity > 0) |
| OccupiedSpaces | INT | | Occupied spaces | CHECK (OccupiedSpaces >= 0) |
| cityID | INT | FK | Linked city | REFERENCES City |

### 3. Parking Zone

| Field Name | Data Type | PK/FK | Description | Constraints |
|------------|-----------|-------|-------------|-------------|
| zone_id | INT | PK | Unique zone ID | NOT NULL |
| zone_type | VARCHAR(50) | | Zone category | NOT NULL |
| hourly_rate | DECIMAL(10,2) | | Parking rate | CHECK (hourly_rate > 0) |
| max_duration | INT | | Max hours allowed | CHECK (max_duration > 0) |
| facility_id | INT | FK | Linked facility | REFERENCES Facility |

### 4. Parking Space

| Field Name | Data Type | PK/FK | Description | Constraints |
|------------|-----------|-------|-------------|-------------|
| space_id | INT | PK | Unique space ID | NOT NULL |
| space_status | VARCHAR(30) | | Status of space | CHECK (space_status IN ('Available','Occupied','Reserved','Out of Service')) |
| zone_id | INT | FK | Linked zone | REFERENCES Parking_Zone |
| facility_id | INT | FK | Linked facility | REFERENCES Facility |

### 5. User

| Field Name | Data Type | PK/FK | Description | Constraints |
|------------|-----------|-------|-------------|-------------|
| user_id | INT | PK | Unique user ID | NOT NULL |
| user_name | VARCHAR(100) | | Name of user | NOT NULL |
| address | VARCHAR(150) | | Address | |
| phone | VARCHAR(20) | | Phone number | |
| email | VARCHAR(100) | | Email address | UNIQUE |
| membership_type | VARCHAR(20) | | Type of membership | CHECK (membership_type IN ('Casual','Monthly','Premium')) |
| membership_start_date | DATE | | Start date | |
| membership_expiry_date | DATE | | Expiry date | |

### 6. Vehicle

| Field Name | Data Type | PK/FK | Description | Constraints |
|------------|-----------|-------|-------------|-------------|
| vehicle_id | INT | PK | Unique vehicle ID | NOT NULL |
| registration_number | VARCHAR(20) | | Vehicle number | UNIQUE |
| vehicle_type | VARCHAR(30) | | Type of vehicle | |
| user_id | INT | FK | Owner user | REFERENCES User |

### 7. Booking

| Field Name | Data Type | PK/FK | Description | Constraints |
|------------|-----------|-------|-------------|-------------|
| booking_id | INT | PK | Unique booking ID | NOT NULL |
| start_datetime | TIMESTAMP | | Start time | NOT NULL |
| end_datetime | TIMESTAMP | | End time | |
| entry_time | TIMESTAMP | | Entry time | |
| exit_time | TIMESTAMP | | Exit time | |
| total_cost | DECIMAL(10,2) | | Parking cost | CHECK (total_cost >= 0) |
| user_id | INT | FK | Linked user | REFERENCES User |
| vehicle_id | INT | FK | Linked vehicle | REFERENCES Vehicle |
| space_id | INT | FK | Linked space | REFERENCES Parking_Space |

### 8. Payment

| Field Name | Data Type | PK/FK | Description | Constraints |
|------------|-----------|-------|-------------|-------------|
| payment_id | INT | PK | Unique payment ID | NOT NULL |
| payment_date | DATE | | Date of payment | NOT NULL |
| amount | DECIMAL(10,2) | | Payment amount | CHECK (amount > 0) |
| payment_method | VARCHAR(30) | | Payment type | |
| booking_id | INT | FK | Linked booking | REFERENCES Booking |

### 9. Employee

| Field Name | Data Type | PK/FK | Description | Constraints |
|------------|-----------|-------|-------------|-------------|
| employee_id | INT | PK | Unique employee ID | NOT NULL |
| employee_name | VARCHAR(100) | | Name | NOT NULL |
| phone | VARCHAR(20) | | Contact | |
| email | VARCHAR(100) | | Email | |
| role | VARCHAR(50) | | Job role | |
| city_id | INT | FK | Assigned city | REFERENCES City |

### 10. Facility_Employee (Associative Entity)

Resolves the many-to-many relationship between **Employee** and **Facility**.

| Field Name | Data Type | PK/FK | Description | Constraints |
|------------|-----------|-------|-------------|-------------|
| facility_id | INT | PK, FK | Facility ID | REFERENCES Facility |
| employee_id | INT | PK, FK | Employee ID | REFERENCES Employee |
| assigned_date | DATE | | Assignment date | |

### 11. Violation

| Field Name | Data Type | PK/FK | Description | Constraints |
|------------|-----------|-------|-------------|-------------|
| violation_id | INT | PK | Unique violation ID | NOT NULL |
| violation_type | VARCHAR(50) | | Type of violation | |
| issue_date | DATE | | Date issued | NOT NULL |
| fine_amount | DECIMAL(10,2) | | Fine amount | CHECK (fine_amount > 0) |
| vehicle_id | INT | FK | Related vehicle | REFERENCES Vehicle |
| employee_id | INT | FK | Issued by | REFERENCES Employee |
| booking_id | INT | FK | Related booking | NULL allowed |

---

## Key Relationships

| Relationship | Type | Notes |
|--------------|------|-------|
| City → Facility | One-to-Many | A city has many facilities |
| Facility → Parking Zone | One-to-Many | A facility has many zones |
| Parking Zone → Parking Space | One-to-Many | A zone has many spaces |
| User → Vehicle | One-to-Many | A user owns many vehicles |
| User → Booking | One-to-Many | A user makes many bookings |
| Vehicle → Booking | One-to-Many | A vehicle appears in many bookings |
| Parking Space → Booking | One-to-Many | A space can be booked many times |
| Booking → Payment | One-to-Many | A booking can have payments |
| Employee ⇄ Facility | Many-to-Many | Resolved via `Facility_Employee` |
| Vehicle → Violation | One-to-Many | A vehicle can have many violations |
| Employee → Violation | One-to-Many | An employee issues many violations |

---

*Database Design — Part A. SmartPark Smart Parking Management System.*
