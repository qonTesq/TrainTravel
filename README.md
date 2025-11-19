> ⚠️ Archived: This repository is now a public archive. The source code is preserved for educational/reference purposes and may no longer function without updates (library deprecations, dependency vulnerabilities, etc.). Use or deploy at your own discretion.

# 🚆 TrainTravel (Railway Reservation System)

<img width="878" height="323" alt="Welcome Screen" src="https://github.com/user-attachments/assets/dfa38e7b-58da-4b5d-b561-8cdd0dca14ff" />

A lightweight Windows Forms (VB.NET) desktop application that demonstrates core concepts of a railway ticket reservation workflow: searching trains between cities, validating seat availability over partial segments, and booking tickets into a simple Microsoft Access database.

> Educational / demo project – intentionally scoped and simplified.

## Overview

TrainTravel simulates a minimal reservation layer over a static railway network:

- Limited predefined cities: Jaipur, Rajkot, Ahmedabad, Vadodara, Mumbai.
- Each train has a small fixed capacity (5 seats) to make overbooking and seat overlap logic easy to observe.
- Bookings consider only the segment traveled (origin → destination) and do not "consume" seats for segments the passenger does not ride.

## Feature Highlights

- Welcome screen with entry navigation
- Train search by:
  - Origin & destination
  - Travel class
  - Date of journey
  - Passenger count
- Dynamic seat availability per partial segment
- Booking workflow with persistence
- Microsoft Access (OleDb) backend
- Input validation + defensive error handling
- Modular helper functions for data access
- Easily adjustable capacity logic

## Tech Stack

| Layer         | Technology                                                    |
| ------------- | ------------------------------------------------------------- |
| Language      | VB.NET                                                        |
| UI Framework  | Windows Forms (.NET Framework)                                |
| Database      | Microsoft Access (`RailwayReservation.accdb`)                 |
| Data Access   | OleDb                                                         |
| IDE           | Visual Studio                                                 |
| UI Components | Standard WinForms + (optional) `Guna.UI2.dll` styling library |

## Architecture & Flow

1. User launches application → `WelcomeForm`
2. User proceeds to booking → `MainForm`
3. App loads reference data (stations, trains, stops)
4. User enters search criteria → queries candidate trains
5. For each train:
   - Compute segment overlap
   - Determine available seats
6. If sufficient seats → allow booking
7. Insert booking row(s) into `Bookings` table
8. Refresh availability

## Database Schema

Minimal conceptual schema (field names illustrative – adjust to actual implementation):

### Stations

| Field           | Type       | Notes                 |
| --------------- | ---------- | --------------------- |
| StationId (PK)  | AutoNumber |                       |
| Name            | Text       | Unique name (logical) |
| Code (optional) | Text       | Short identifier      |

### Trains

| Field         | Type       | Notes          |
| ------------- | ---------- | -------------- |
| TrainId (PK)  | AutoNumber |                |
| Name          | Text       |                |
| Number (opt.) | Text       | Display number |

### TrainStops

| Field            | Type       | Notes                     |
| ---------------- | ---------- | ------------------------- |
| TrainStopId (PK) | AutoNumber |                           |
| TrainId (FK)     | Number     |                           |
| StationId (FK)   | Number     |                           |
| SequenceNumber   | Number     | Defines order along route |

### Bookings

| Field                | Type       | Notes                                    |
| -------------------- | ---------- | ---------------------------------------- |
| BookingId (PK)       | AutoNumber |                                          |
| TrainId (FK)         | Number     |                                          |
| OriginStationId      | Number     |                                          |
| DestinationStationId | Number     |                                          |
| TravelDate           | Date/Time  |                                          |
| Class                | Text       | E.g., "Sleeper", "AC", etc. (simplified) |
| PassengerCount       | Number     |                                          |
| CreatedAt            | Date/Time  | Audit timestamp                          |

> If multiple passengers are treated individually, you could later expand to a `BookingPassengers` table.

## Project Structure

| File / Folder                    | Purpose                              |
| -------------------------------- | ------------------------------------ |
| `Railway Reservation System.sln` | Visual Studio solution               |
| `Railway Reservation System/`    | Main VB.NET project (forms, modules) |
| `WelcomeForm.vb`                 | Entry form with navigation           |
| `MainForm.vb`                    | Core logic: search + booking         |
| `Guna.UI2.dll`                   | Optional UI styling library          |
| `data/RailwayReservation.accdb`  | Access database                      |
| `LICENSE.txt`                    | License                              |
| `README.md`                      | This documentation                   |

## Setup & Installation

### 1. Prerequisites

- Windows with .NET Framework 4.x
- Visual Studio (Community or higher)
- Microsoft Access Database Engine (if not already installed)

### 2. Clone Repository

```bash
git clone https://github.com/qonTesq/Railway-Reservation-System.git
```

### 3. Database Placement

Ensure `RailwayReservation.accdb` resides at:

```
[data]/RailwayReservation.accdb
```

(Or update the connection string accordingly.)

### 4. Connection String

Typically defined similar to:

```
Provider=Microsoft.ACE.OLEDB.12.0;Data Source={calculatedPath}\RailwayReservation.accdb;
```

If you get a provider error, install:  
Microsoft Access Database Engine 2010 or 2016 Redistributable.

## Running the App

1. Open the solution in Visual Studio.
2. Restore any missing references (e.g., add `Guna.UI2.dll` if used).
3. Build Solution (`Ctrl+Shift+B`).
4. Run (`F5`).  
   You should see the Welcome screen.

## Usage Walkthrough

1. Launch app → Welcome screen.
2. Click "Book".
3. Select:
   - Origin
   - Destination (must differ)
   - Class
   - Travel Date
   - Passenger Count
4. Press "Search".
5. Review available trains + seats.
6. Select train (UI dependent – e.g., row selection).
7. Confirm booking → success message if seats available.

## Error Handling & Validation

| Scenario                      | Handling                                   |
| ----------------------------- | ------------------------------------------ |
| Same origin & destination     | User prompt / validation message           |
| Missing field                 | Prevents search                            |
| Database unavailable          | Catches exception and displays message     |
| Zero availability             | Disables booking action                    |
| Overlapping bookings conflict | Recomputes availability just before commit |

Graceful degradation ensures the app does not crash on common user mistakes.

## 📸 Screenshots

<img width="548" height="452" alt="Booking Form" src="https://github.com/user-attachments/assets/f1d3b5d6-d84f-4d07-b439-30766f1477f8" />
<img width="662" height="387" alt="Result / Status" src="https://github.com/user-attachments/assets/f3d80a24-f529-457d-8732-bd3f2fd1c5cb" />

