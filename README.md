# Restaurant Management System

A backend system built with Node.js, Express.js, and MongoDB to streamline day-to-day restaurant operations — from taking orders and managing tables to tracking inventory and generating sales reports.

## Overview

Running a restaurant means juggling dozens of moving parts at once: customers walking in, tables filling up, orders flowing to the kitchen, and ingredients running low without warning. Manual systems (paper tickets, spreadsheets) break down quickly and cause mistakes that cost money.

This Restaurant Management System (RMS) solves that by providing a single backend that handles the entire lifecycle of restaurant operations through clean, secure REST APIs. It is designed to be the brain behind any restaurant POS, admin dashboard, or customer-facing app.

## What It Does

- Order Management — Customers or staff place orders containing multiple menu items. The system validates availability, calculates totals, and tracks each order through its lifecycle (pending, preparing, ready, served, completed).
- Table Management — Assign tables to dine-in orders, track real-time status (available, occupied, reserved, cleaning), and prevent double-booking.
- Reservation System — Book tables in advance with automatic time-overlap conflict detection so two parties cannot be booked on the same table at the same time.
- Inventory Auto-Update — Every menu item stores a recipe (list of ingredients it consumes). When an order is placed, the system automatically deducts raw ingredients from inventory. Cancellations restore stock.
- Low-Stock Alerts — Every inventory item has a threshold. When quantity falls below it, the system raises an alert so staff can reorder before running out.
- Reporting — Daily sales summaries, top-selling items, order status breakdowns, and stock alerts.
- Authentication and Roles — JWT-based login with two roles: admin (full access) and staff (operational access). Admin-only routes protect sensitive endpoints.

## Key Design Highlights

| Feature | How It Works |
|---------|--------------|
| Inventory auto-deduction | Each MenuItem stores a recipe array linking to Inventory documents. Placing an order consumes stock atomically. |
| Double-booking prevention | Reservation checks use a time-window overlap query, not just status === 'available'. |
| Reversible operations | Order cancellation restores inventory and frees the table. |
| Role-based security | Protect middleware verifies JWTs; adminOnly guards admin routes. |
| Clean architecture | Models, routes, middleware, and helpers are separated for maintainability. |

## Tech Stack

| Layer | Technology |
|-------|------------|
| Runtime | Node.js |
| Framework | Express.js |
| Database | MongoDB + Mongoose |
| Auth | JWT + bcrypt |
| Dev Tools | Nodemon, Morgan, CORS, Dotenv |

erDiagram
    USER {
        ObjectId _id PK
        string email UK
        string password
        string role
    }

    MENU_ITEM {
        ObjectId _id PK
        string name UK
        number price
        string category
        boolean available
        array recipe
    }

    INVENTORY {
        ObjectId _id PK
        string name UK
        number quantity
        number threshold
        string unit
    }

    TABLE {
        ObjectId _id PK
        number number UK
        number capacity
        string status
        string location
    }

    RESERVATION {
        ObjectId _id PK
        ObjectId table FK
        string customerName
        datetime reservationTime
        string status
    }

    ORDER {
        ObjectId _id PK
        string orderNumber UK
        ObjectId table FK
        array items
        number total
        string status
    }

    MENU_ITEM ||--o{ INVENTORY : "recipe consumes"
    TABLE ||--o{ RESERVATION : "is booked by"
    TABLE ||--o{ ORDER : "serves"
    ORDER }o--|| MENU_ITEM : "contains"
    USER ||--o{ ORDER : "processes"

## Why This Project

This project demonstrates real-world backend engineering: schema design for related entities, business logic layered on top of CRUD, authentication, transactional thinking (inventory consistency), and reporting — all the pieces that interviewers and production systems care about.

## Use Cases

- Restaurant POS backends — plug in a frontend for order-taking
- Table booking apps — for fine-dining establishments
- Learning reference — a clean example of REST + MongoDB + JWT architecture
- Interview projects — demonstrates full-stack backend competence

## Status

Core features complete — orders, tables, reservations, inventory, reports, and auth all working.
Future: WebSocket live order updates, payment gateway integration, multi-branch support.
- **Interview projects** — demonstrates full-stack backend competence

##  Status

Core features complete — orders, tables, reservations, inventory, reports, and auth all working.
Future: WebSocket live order updates, payment gateway integration, multi-branch support.


