# Quick Dry Clean — Order Management System

A lightweight laundry order management system built with Node.js + Express.

---

## Setup

```bash
cd backend
npm install
npm start
```

Open `http://localhost:3000` in your browser.

---

## Features

- Create orders with customer name, phone, garments, quantity
- Auto-calculated bill from hardcoded price config
- Unique order IDs (QDC-XXXXXX format)
- Estimated delivery date (3 days from order)
- Status flow: RECEIVED → PROCESSING → READY → DELIVERED
- Forward-only status transitions (can't go backward)
- Filter orders by status, name, phone, garment type
- Dashboard: total orders, revenue, pending revenue, 7-day trend
- Simple frontend UI — no framework, just HTML/CSS/JS

---

## Price Config

| Garment  | Price  |
|----------|--------|
| Shirt    | ₹40    |
| Pants    | ₹50    |
| Saree    | ₹120   |
| Suit     | ₹200   |
| Jacket   | ₹150   |
| Kurta    | ₹60    |
| Bedsheet | ₹100   |
| Blanket  | ₹180   |

---

## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/prices | Get price config |
| POST | /api/orders | Create order |
| GET | /api/orders | List orders (with filters) |
| GET | /api/orders/:id | Get single order |
| PATCH | /api/orders/:id/status | Update status |
| GET | /api/dashboard | Dashboard stats |

### Create Order — Sample Request
```json
POST /api/orders
{
  "customerName": "Rahul Sharma",
  "phone": "9812345678",
  "garments": [
    { "type": "shirt", "quantity": 3 },
    { "type": "saree", "quantity": 1 }
  ]
}
```

---

## AI Usage Report

### Tool Used
Claude (Anthropic) — used as the primary development tool and Chatgpt for undertsnading and structuring.

### What AI Did
- Scaffolded the entire Express server structure from the assignment brief
- Generated all route handlers, the in-memory store pattern, and helper functions
- Built the full single-file frontend including dynamic garment rows and live total calculation
- Wrote the dashboard aggregation logic and 7-day trend grouping

### Sample Prompts I Used
- *"Design a REST API for a dry cleaning OMS. Features: create order with garments and auto-billing, status tracking (RECEIVED → PROCESSING → READY → DELIVERED), order filters, dashboard stats. Node.js + Express, in-memory storage."*
- *"Add query param filtering to GET /api/orders for status, name (partial match), phone, and garment type."*
- *"Build a single-file HTML/CSS/JS frontend. Dark industrial theme. Three sections: Dashboard with stats and bar chart, Create Order with dynamic garment rows and live total, Orders list with filters and inline status update dropdown."*

### What AI Got Wrong / What I Fixed

1. **Wildcard route syntax** — AI used `app.get('*', ...)` which broke in Express 5. Fixed to `app.get('/{*path}', ...)`.
2. **Backward status update not blocked** — AI's status update route had no guard. Added the `currentIdx < newIdx` check to prevent moving backward.
3. **Garment row deletion bug** — AI used class-based selectors to recalculate totals. Broke when rows were deleted. Fixed by using unique per-row IDs (`g1`, `g2`, etc.) for all elements.
4. **Missing await on init** — AI forgot `await` before `loadPrices()` in the init function, causing the garment dropdown to render empty. Fixed.

---

## Tradeoffs

### Skipped
- Database — in-memory only, data resets on server restart
- Authentication — no login system
- Pagination — all orders load at once

### Would Add with More Time (currently doing)
- MongoDB for persistence
- JWT auth for staff vs admin roles
- WhatsApp/SMS notification on status change
- Deploy to Railway or Render
- Printable order receipt                                                                                                                                                                                        <img width="1919" height="963" alt="Screenshot 2026-04-16 023037" src="https://github.com/user-attachments/assets/9d7b69a8-f6a0-43c3-b883-ff0b2c5c706b" />                                                        <img width="1908" height="870" alt="Screenshot 2026-04-16 023049" src="https://github.com/user-attachments/assets/90002f49-cbe9-48e3-a520-46972d7fee48" />                                                           <img width="1909" height="873" alt="Screenshot 2026-04-16 023056" src="https://github.com/user-attachments/assets/87bef541-908b-4b31-8ee0-df2bff51293e" />


