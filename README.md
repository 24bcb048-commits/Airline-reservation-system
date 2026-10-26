# SkyBook Airlines - Airline Reservation System ✈️

A clean, modern, beginner-friendly, and fully functional **Airline Reservation System** website designed for a college **Software Engineering (SE) Lab Mini-Project**.

---

## 📌 1. Project Overview

**SkyBook Airlines** is a complete full-stack web application that allows passengers to search for domestic Indian flights, select seats interactively from a visual cabin layout, generate instant booking references (PNR), view and print electronic boarding passes, and cancel bookings. It also includes an administrative dashboard for airline operators to manage flight schedules, view system statistics, and monitor bookings.

### Key Highlights
- **Architecture**: Model-Template-View / MVC pattern using Python Flask and SQLite3.
- **Visual Cabin Seat Selection**: 60-seat interactive aircraft grid (A320 layout) where booked seats are locked to prevent duplicate bookings.
- **Security**: Password hashing with Werkzeug, parameterized SQL queries preventing SQL injection, and session-based role authorization (`user` vs `admin`).
- **Zero Complex Tooling**: Pure HTML, CSS, JavaScript, Bootstrap 5, Python Flask, and SQLite. No React, Node.js, or complex setups required.

---

## 🛠️ 2. Technology Stack

| Layer | Technology | Purpose |
|---|---|---|
| **Frontend** | HTML5, CSS3, JavaScript (ES6) | Responsive UI, client-side validation, seat selection |
| **Styling** | Bootstrap 5.3 + Custom CSS | Clean blue and white airline aesthetic, mobile responsiveness |
| **Icons** | Bootstrap Icons | Airplane, user, and dashboard icons |
| **Backend** | Python Flask (v3.x) | Routing, session control, business logic, templating |
| **Database** | SQLite3 | Local relational storage with foreign key constraints |
| **Security** | Werkzeug Security | PBKDF2/SHA-256 password hashing & validation |

---

## 📁 3. Project Structure

```text
airline-reservation-system/
│
├── app.py                  # Main Flask application, routes, DB initialization, auth
├── requirements.txt        # Python package dependencies (Flask)
├── database.db             # SQLite database (auto-generated on first run)
├── README.md               # Complete project documentation & viva guide
│
├── templates/              # Jinja2 HTML Templates
│   ├── base.html           # Common layout: Navbar, footer, flash alerts
│   ├── index.html          # Homepage: Banner, search widget, popular routes
│   ├── login.html          # User & Admin login with 1-click demo autofill
│   ├── register.html       # User registration form with validation
│   ├── flights.html        # Flight search results & filters
│   ├── booking.html        # Passenger details & interactive seat map
│   ├── my_bookings.html    # User bookings list & cancellation modal
│   ├── ticket.html         # Printable Boarding Pass / E-Ticket with barcode
│   └── admin.html          # Admin dashboard: statistics, flights & user tables
│
└── static/                 # Static Assets
    ├── css/
    │   └── style.css       # Blue & white theme, seat map & boarding pass styling
    └── js/
        └── script.js       # Seat selection logic, validation & alert auto-dismiss
```

---

## 🗄️ 4. Database Schema

The system uses three relational SQLite tables:

### 1. `users` Table
Stores customer and administrator credentials.
- `id` (INTEGER, Primary Key, Auto Increment)
- `name` (TEXT, Not Null)
- `email` (TEXT, Unique, Not Null)
- `password_hash` (TEXT, Not Null)
- `phone` (TEXT)
- `role` (TEXT, Default `'user'`) — either `'user'` or `'admin'`
- `created_at` (TIMESTAMP)

### 2. `flights` Table
Stores scheduled domestic flight information.
- `id` (INTEGER, Primary Key, Auto Increment)
- `flight_number` (TEXT, Unique, Not Null) — e.g. `SB-101`
- `airline` (TEXT, Not Null) — e.g. `SkyBook Express`, `Air India`, `IndiGo`
- `source` (TEXT, Not Null) — e.g. `Ahmedabad`, `Mumbai`, `Delhi`
- `destination` (TEXT, Not Null)
- `departure_time` (TEXT, Not Null) — e.g. `06:30 AM`
- `arrival_time` (TEXT, Not Null) — e.g. `07:45 AM`
- `flight_date` (TEXT, Not Null) — `YYYY-MM-DD`
- `duration` (TEXT, Not Null) — e.g. `1h 15m`
- `total_seats` (INTEGER, Default `60`)
- `available_seats` (INTEGER, Default `60`)
- `price` (REAL, Not Null)

### 3. `bookings` Table
Tracks ticket purchases and passenger details.
- `id` (INTEGER, Primary Key, Auto Increment)
- `booking_reference` (TEXT, Unique, Not Null) — Unique 6-character PNR code
- `user_id` (INTEGER, Foreign Key &rarr; `users.id`)
- `flight_id` (INTEGER, Foreign Key &rarr; `flights.id`)
- `passenger_name` (TEXT, Not Null)
- `passenger_age` (INTEGER, Not Null)
- `passenger_gender` (TEXT, Not Null)
- `contact_number` (TEXT, Not Null)
- `seat_number` (TEXT, Not Null) — e.g. `3A`, `4C`
- `booking_date` (TIMESTAMP)
- `status` (TEXT, Default `'Confirmed'`) — `'Confirmed'` or `'Cancelled'`
- `total_fare` (REAL, Not Null)

---

## 🚀 5. Installation and Setup Instructions

### Prerequisites
- Python 3.8 or higher installed on your computer.

### Step 1: Open the Project in VS Code or Terminal
Open VS Code, select **File &rarr; Open Folder**, and open `airline-reservation-system`.

Or navigate to the folder using your terminal:
```bash
cd airline-reservation-system
```

### Step 2: Install Flask
Install the required packages using pip:
```bash
pip install -r requirements.txt
```
*(Or simply: `pip install flask`)*

### Step 3: Run the Application
Start the Flask development server:
```bash
python app.py
```

### Step 4: Access in Your Browser
Open your browser and navigate to:
```text
http://127.0.0.1:5000
```

> **Note:** The database (`database.db`) is automatically created and populated with sample Indian flights, demo users, and bookings upon the first launch!

---

## 🔑 6. Demo Login Credentials

For quick demonstrations during college presentations and lab evaluations, use these pre-seeded accounts:

| Role | Email Address | Password | Privileges |
|---|---|---|---|
| **Administrator** | `admin@skybook.com` | `admin123` | Full access to Admin Dashboard, add/edit/delete flights, cancel any booking, view statistics |
| **Demo Customer** | `rahul@gmail.com` | `user123` | Search flights, pick seats, book tickets, view e-tickets, cancel personal bookings |

*(On the Login page, you can also click the **"User Demo"** or **"Admin Demo"** quick-fill buttons to automatically insert the credentials!)*

---

## 🎯 7. Step-by-Step Testing & Demonstration Workflow

When demonstrating this project for your Software Engineering mini-project viva, follow these steps:

1. **Homepage (`/`)**:
   - Show the airplane banner and search form.
   - Select Origin: **Ahmedabad** and Destination: **Mumbai**.
   - Click **Find Flights**.

2. **Flight Search (`/flights`)**:
   - Point out the flight cards displaying airline, timing, route line, available seat counter, and price.
   - Show the sorting options (Price Low-to-High / High-to-Low).
   - Click **Select Seat & Book**.

3. **User Authentication (`/login` or `/register`)**:
   - If not signed in, you are redirected to login.
   - Log in using `rahul@gmail.com` / `user123` (or register a brand new account).

4. **Interactive Seat Selection & Booking (`/book/<id>`)**:
   - Demonstrate the Airbus A320 cabin visualizer.
   - Click on an available seat (e.g. `2B`).
   - Notice how already occupied seats are disabled and strike-through.
   - Enter passenger details (Name, Age, Gender, Mobile).
   - Click **Confirm & Book Ticket**.

5. **My Bookings (`/my-bookings`)**:
   - Show the newly generated PNR reference code (e.g., `SB-83921`).
   - Click **E-Ticket** to open the realistic Boarding Pass.
   - Click **Print / Save as PDF** to show print-ready layout.

6. **Booking Cancellation (`/cancel-booking/<id>`)**:
   - Click **Cancel** on a confirmed ticket.
   - Show that the status updates to `Cancelled`.
   - Verify that the seat is returned to the available seat pool.

7. **Admin Dashboard (`/admin`)**:
   - Log in as `admin@skybook.com` / `admin123`.
   - Show the high-level statistics cards (Total Flights, Confirmed Bookings, Users, Revenue).
   - Add a new flight using the **Schedule New Flight** modal.
   - Edit flight fare or timings.
   - Attempt to delete a flight: note that flights with active confirmed bookings cannot be deleted (referential integrity rule).

---

## 🎓 8. Software Engineering Viva Questions & Answers

**Q1: What software development model does this project follow?**  
> *Answer:* It follows an Agile iterative approach and MVC (Model-Template-View in Flask) architectural design, cleanly separating data (SQLite), presentation (Jinja2 HTML templates), and business logic (Flask routes).

**Q2: How does the system prevent double-booking of seats?**  
> *Answer:* When a user selects a seat and submits the booking form, the backend executes an atomic SQL query:
> `SELECT id FROM bookings WHERE flight_id = ? AND seat_number = ? AND status = 'Confirmed'`
> If a record exists, the reservation is rejected with an error message. Only if no confirmed booking is found is the insert completed and the available seats count decremented.

**Q3: How are passwords stored securely?**  
> *Answer:* Passwords are never stored in plain text. They are hashed using Werkzeug's `generate_password_hash()` which implements PBKDF2 with SHA-256 and cryptographic salts. Verification is done via `check_password_hash()`.

**Q4: How is SQL Injection prevented?**  
> *Answer:* All database queries use parameterized placeholders (`?`), ensuring user inputs are treated as literal values rather than executable SQL code.

**Q5: What happens when a user cancels a booking?**  
> *Answer:* The booking's status is updated to `'Cancelled'` to maintain audit history, and the flight's `available_seats` counter is incremented by 1, making the seat immediately available for other travelers.
#   A i r l i n e - r e s e r v a t i o n - s y s t e m  
 