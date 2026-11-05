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
## 🏗️ System Architecture

### High-Level Architecture

```mermaid
flowchart TB
    subgraph Client["🖥️ Client Layer"]
        UI[Web / Mobile UI]
        PM[Postman / API Clients]
    end

    subgraph API["⚙️ Express.js API Server"]
        MW[Middleware<br/>CORS · JSON · JWT Auth · Error Handler]
        
        subgraph Routes["API Routes"]
            R1[/api/auth]
            R2[/api/menu]
            R3[/api/tables]
            R4[/api/reservations]
            R5[/api/orders]
            R6[/api/inventory]
            R7[/api/reports]
        end
        
        subgraph Logic["Business Logic"]
            L1[Order Processing]
            L2[Table Availability Check]
            L3[Inventory Auto-Deduct]
            L4[JWT Authentication]
        end
    end

    subgraph DB["🗄️ MongoDB Database"]
        M1[(users)]
        M2[(menu_items)]
        M3[(tables)]
        M4[(reservations)]
        M5[(orders)]
        M6[(inventory)]
    end

    UI --> MW
    PM --> MW
    MW --> Routes
    Routes --> Logic
    Logic --> DB
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


