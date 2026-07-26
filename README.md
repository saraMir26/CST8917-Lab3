# CST8917 – Lab 3: FleetBook Vehicle Booking System

**Course:** CST8917 – Serverless Applications  
**Student:** Sara Mirzaei  
**Semester:** Spring/Summer 2026

---

# Overview

FleetBook is a serverless vehicle booking application built using Microsoft Azure services. The solution integrates Azure Service Bus, Azure Logic Apps, Azure Functions, and Outlook to process booking requests through an event-driven workflow.

A customer submits a booking request through the FleetBook web client. The request is placed into an Azure Service Bus queue where a Logic App is triggered. The Logic App calls an Azure Function to evaluate vehicle availability and pricing, then determines whether the booking should be confirmed or rejected. Finally, the workflow sends an email notification and publishes the result to a Service Bus topic for downstream consumers.

---

# Architecture

```
                FleetBook Web Client
                        │
                        
             Service Bus Queue
               (booking-queue)
                        │
                        
                 Azure Logic App
                        │
        ┌───────────────┴───────────────┐
        │                               │
                                       
 Azure Function                 Parse Function Result
(check-booking)                        │
        │                              
        └────────────── Condition ───────────────
                          │                 │
                     Confirmed          Rejected
                          │                 │
              Send Confirmation     Send Rejection
                    Email                Email
                          │                 │
                          └──────┬──────────┘
                                 
                     Service Bus Topic
                     (booking-results)
                         │        │
                                 
                  confirmed-sub rejected-sub
```

---

# Azure Services Used

- Azure Functions
- Azure Logic Apps
- Azure Service Bus
- Outlook Connector
- Azure Storage Account

---

# Resources Created

## Service Bus

- Namespace
- Queue
  - booking-queue
- Topic
  - booking-results
- Subscriptions
  - confirmed-sub
  - rejected-sub

## Azure Function

Function App

```
check-booking
```

Additional endpoint

```
health
```

## Logic App

```
process-booking
```

Workflow includes:

- Queue Trigger
- Decode Message
- Parse Booking Request
- Azure Function Call
- Parse Function Response
- Condition
- Confirmation Email
- Rejection Email
- Publish Confirmed Result
- Publish Rejected Result

---

# Workflow

1. Customer submits a booking request.
2. Request is stored in Service Bus Queue.
3. Logic App is triggered automatically.
4. Queue message is decoded.
5. JSON request is parsed.
6. Azure Function evaluates vehicle availability.
7. Function calculates pricing.
8. Logic App checks booking status.
9. Confirmation or rejection email is sent.
10. Booking result is published to the Service Bus Topic.
11. Topic subscriptions route confirmed and rejected bookings independently.

---

# Test Results

## Confirmed Booking

Input

- Vehicle Type: Sedan
- Pickup Location: Ottawa

Result

- Booking confirmed
- Confirmation email received
- Confirmation message published to Service Bus Topic

---

## Rejected Booking

Input

- Vehicle Type: Sedan
- Pickup Location: Montreal

Result

- Booking rejected
- Rejection email received
- Rejection message published to Service Bus Topic

---

# Project Files

```
function_app.py
requirements.txt
test-function.http
client.html
README.md
local.settings.example.json
```

---

# Demo Video 

YouTube Demonstration

[![Watch the video](https://img.youtube.com/vi/g8Qo4sbYbnM/hqdefault.jpg)](https://www.youtube.com/watch?v=g8Qo4sbYbnM)


The demonstration includes:

- Service Bus namespace
- Queue
- Topic
- Topic subscriptions
- Confirmed booking workflow
- Logic App True branch
- Confirmation email
- Rejected booking workflow
- Logic App False branch
- Rejection email
- Topic subscription verification

---

# Technologies

- Python
- Azure Functions
- Azure Logic Apps
- Azure Service Bus
- Outlook Connector
- HTML
- REST API
- JSON

---

# Security

Sensitive information has **not** been committed to this repository.

Excluded items include:

- local.settings.json
- Connection strings
- SAS Keys
- Azure credentials

A `local.settings.example.json` file is included for configuration reference only.

