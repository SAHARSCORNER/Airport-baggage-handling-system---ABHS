# Implementation Documentation

## 1. Project Overview

The **Airport Baggage Handling Monitoring System (ABHMS)** is a web-based operational monitoring platform designed to simulate the software layer of an airport baggage handling environment.

The system provides centralized visibility into:

* Baggage movement
* Flights
* Conveyor systems
* Scanners
* Operational faults
* Maintenance activities
* Equipment status
* Operational performance

The project uses simulated operational data to demonstrate how airport baggage handling processes can be monitored through a centralized enterprise application.

---

# 2. Implementation Objectives

The primary objectives of the implementation were:

1. Create a centralized airport baggage monitoring platform.
2. Track baggage through different operational stages.
3. Monitor conveyor and scanner status.
4. Provide fault detection and incident management.
5. Support preventive and corrective maintenance.
6. Provide operational dashboards and KPIs.
7. Implement role-based access control.
8. Maintain relationships between flights, baggage, equipment, and maintenance activities.
9. Design the system so that real-time equipment integrations could be added in the future.

---

# 3. Technology Architecture

## Backend

* Python
* Django
* Django REST Framework

## Database

* PostgreSQL

## Frontend

* HTML5
* CSS3
* JavaScript
* Bootstrap

## Data Visualization

* Chart.js

## Development & Version Control

* Git
* GitHub

---

# 4. System Architecture

```text
                     ┌───────────────────────┐
                     │        Users          │
                     │                       │
                     │ Admin / Operator      │
                     │ Supervisor / Engineer │
                     └───────────┬───────────┘
                                 │
                                 ▼
                     ┌───────────────────────┐
                     │    Web Interface      │
                     │ HTML / CSS / JS       │
                     └───────────┬───────────┘
                                 │
                                 ▼
                     ┌───────────────────────┐
                     │    Django Backend     │
                     │                       │
                     │ Business Logic        │
                     │ Authentication        │
                     │ REST APIs              │
                     └───────────┬───────────┘
                                 │
                                 ▼
                     ┌───────────────────────┐
                     │      PostgreSQL       │
                     │       Database        │
                     └───────────────────────┘
```

---

# 5. Functional Architecture

The application was divided into multiple functional areas.

```text
ABHMS
│
├── Authentication
│
├── Dashboard
│
├── Flight Management
│
├── Baggage Tracking
│
├── Conveyor Monitoring
│
├── Scanner Monitoring
│
├── Fault Management
│
├── Maintenance
│
├── Employee Management
│
└── Reports & Analytics
```

This modular structure makes it possible to extend individual components without redesigning the complete system.

---

# 6. Database Design

The database was designed around the relationships between airport operational entities.

### Core Entities

```text
User
Employee
Airport
Terminal
Flight
Baggage
Conveyor
Scanner
Fault
Maintenance
Notification
ActivityLog
```

### Simplified Relationship

```text
Airport
   │
   └── Terminal
          │
          ├── Conveyor
          │      └── Scanner
          │
          └── Flight
                 │
                 └── Baggage
                        │
                        └── Movement History

Conveyor
   │
   ├── Fault
   │
   └── Maintenance
```

---

# 7. Authentication & Authorization

The application implements role-based access.

### Administrator

Can:

* Manage users
* Manage airport configuration
* Manage assets
* View reports
* Manage system settings

### Operations Supervisor

Can:

* Monitor operations
* View baggage statistics
* Monitor conveyors
* Review faults
* View reports

### Operator

Can:

* Track baggage
* Monitor conveyors
* View operational information
* Report faults

### Maintenance Engineer

Can:

* View assigned faults
* Update maintenance tasks
* Record repair information
* Update equipment status

Role-based permissions ensure that users only access functionality relevant to their responsibilities.

---

# 8. Baggage Tracking Implementation

Each baggage item receives a unique identifier.

Example:

```text
BAG-2026-001245
```

A baggage record is associated with:

* Flight
* Passenger/demo identifier
* Origin
* Destination
* Current location
* Current conveyor
* Current status
* Timestamp

### Baggage Lifecycle

```text
CHECK-IN
   ↓
SECURITY
   ↓
SCANNED
   ↓
SORTING
   ↓
CONVEYOR
   ↓
MAKE-UP
   ↓
LOADING
   ↓
COMPLETED
```

The system maintains the current state of baggage and can display its operational journey.

---

# 9. Conveyor Monitoring

Conveyors are treated as operational equipment.

Each conveyor contains information such as:

* Conveyor ID
* Terminal
* Location
* Status
* Speed
* Capacity
* Current load
* Health percentage
* Maintenance status

### Status Model

```text
RUNNING
BUSY
IDLE
FAULT
OFFLINE
MAINTENANCE
```

The dashboard aggregates conveyor states to provide an overall operational view.

---

# 10. Scanner Monitoring

Scanners are associated with baggage handling equipment.

The system monitors:

* Scanner status
* Number of scans
* Failed scans
* Last activity
* Associated conveyor
* Fault state

Scanner events can be represented using simulated operational data.

---

# 11. Fault Management

Faults are linked to operational equipment.

A fault contains:

* Fault ID
* Equipment
* Fault type
* Priority
* Description
* Reported time
* Assigned engineer
* Status
* Resolution notes

### Fault Lifecycle

```text
REPORTED
    ↓
ASSIGNED
    ↓
IN PROGRESS
    ↓
RESOLVED
    ↓
VERIFIED
    ↓
CLOSED
```

This provides traceability from fault detection to resolution.

---

# 12. Maintenance Management

Maintenance activities are associated with specific equipment.

The system supports:

### Preventive Maintenance

Scheduled activities intended to reduce equipment failure.

### Corrective Maintenance

Maintenance performed after an equipment fault.

### Maintenance Workflow

```text
Maintenance Due / Fault
          ↓
     Work Order
          ↓
 Engineer Assignment
          ↓
      Inspection
          ↓
 Repair / Service
          ↓
     Verification
          ↓
      Completion
```

Maintenance history is retained for operational analysis.

---

# 13. Dashboard Implementation

The dashboard aggregates operational data from different modules.

### Primary KPIs

* Total baggage processed
* Active conveyors
* Faulty conveyors
* Active maintenance tasks
* Delayed baggage
* Scanner failures
* Equipment availability

Charts provide visual representations of operational trends.

Example:

```text
Baggage Throughput
│
│             █
│       █     █
│   █   █  █  █
│___█___█__█__█________
   08  10 12 14 16
        Time
```

---

# 14. REST API Layer

Django REST Framework can be used to expose operational data through APIs.

Example endpoints:

```text
/api/flights/
/api/baggage/
/api/conveyors/
/api/scanners/
/api/faults/
/api/maintenance/
/api/dashboard/
```

The API architecture allows future external systems to communicate with the application.

---

# 15. Operational Simulation

Since the project does not connect to actual airport PLCs or baggage handling equipment, operational events are simulated.

Examples:

```text
Conveyor C-101 → RUNNING
Conveyor C-102 → BUSY
Conveyor C-103 → FAULT

Scanner S-12 → ONLINE
Scanner S-13 → OFFLINE

BAG-10231 → SORTING
BAG-10232 → LOADING
```

This allows the project to demonstrate operational workflows without requiring physical airport infrastructure.

---

# 16. Testing Approach

Testing focused on the major operational workflows.

### Functional Testing

* User login
* Role permissions
* Baggage creation
* Baggage search
* Conveyor creation
* Conveyor status updates
* Fault reporting
* Engineer assignment
* Maintenance completion

### Validation Testing

The system validates:

* Required fields
* Unique identifiers
* Valid relationships
* Status transitions
* User permissions

### Integration Testing

Relationships between:

```text
Flight → Baggage
Baggage → Conveyor
Conveyor → Fault
Fault → Maintenance
Maintenance → Employee
```

were tested to ensure consistent data flow.

---

# 17. Security Considerations

Security considerations include:

* Authentication
* Role-based authorization
* Password protection
* CSRF protection
* Input validation
* Environment variables for sensitive configuration
* Database access controls
* Restricted administrative functionality

Production deployment would require additional security hardening.

---

# 18. Deployment Architecture

A production deployment can use:

```text
                 Internet
                    │
                    ▼
                 Nginx
                    │
                    ▼
                Gunicorn
                    │
                    ▼
                 Django
                    │
                    ▼
               PostgreSQL
```

Docker can be used to package the application and its dependencies.

---

# 19. Development Approach

The system was developed incrementally.

### Phase 1

Project architecture and authentication.

### Phase 2

Database models and airport entities.

### Phase 3

Flight and baggage tracking.

### Phase 4

Conveyor and scanner monitoring.

### Phase 5

Fault and maintenance management.

### Phase 6

Dashboard and analytics.

### Phase 7

Testing, UI improvements, documentation, and deployment preparation.

---

# 20. Current Project Scope

The current implementation focuses on the **software monitoring and management layer**.

It does not directly control physical airport equipment.

The project uses simulated operational data to demonstrate:

* Monitoring
* Tracking
* Fault management
* Maintenance
* Analytics
* Operational workflows

This separation allows the system to be safely demonstrated without requiring access to real airport infrastructure.

---

# 21. Engineering Outcome

The implementation demonstrates how a modular enterprise application can connect multiple operational processes into a centralized monitoring platform.

The architecture was intentionally designed so that future integrations such as PLC systems, IoT devices, event streams, and airport operational databases could be introduced without replacing the core application.
