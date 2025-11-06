# Fitness Center Database Design & SQL Analysis

This project proposes a complete relational database design for "Fitness Center ABC.". It goes beyond just schema creation by using **SQL analysis** to answer a critical business question: **"Which classes are most popular among members in the premium tiers?"** 

The goal is to provide data-driven recommendations to help the fitness center boost revenue and member engagemen.

---

## Project Objective

The business wants to increase revenue and member satisfaction but needs to know where to focus its operational efforts. This report addresses that need by:

1.  **Identifying** the most valuable member segment based on revenue.
2.  **Analyzing** the class attendance and preferences of this specific segment.
3.  **Providing** data-driven recommendations to increase engagement and attract similar high-value members.

## Tools Used

* **Database Design:** A relational database schema was designed to manage all operations.
* **SQL :** Used for both Data Definition Language (DDL) to create the database and Data Query Language (DQL) to perform the analysis.
* **Data Visualization:** Bar charts were used to present the findings (created in a BI tool).

---

## Methodology

### 1. Database Schema Design

A relational database was designed with 5 key tables to manage all business operations:
* `MembershipTier`: Stores the different membership tiers and their prices.
* `Members`: Stores all member information and links to their chosen tier.
* `Trainer`: Stores information for all class trainers.
* `Class`: Stores the schedule and details for all classes, linking to a trainer.
* `Reservation`: A junction table that connects `Members` and `Class` to track all bookings (a Many-to-Many relationship).

[cite_start]The full Entity-Relationship Diagram (ERD) and table definitions are available in the attached report .

### 2. SQL Analysis

Two key SQL queries were written to answer the business question.

**Query 1: Identify the Most Valuable Segment**
This query joined the `Members` and `MembershipTier` tables, grouping by `TierName` to calculate the `TotalRevenue` from each segment .

**Query 2: Analyze Premium Member Preferences**
This query joined four tables (`Reservation`, `Class`, `Members`, `MembershipTier`), filtered for `TierName = 'Premium', and then grouped by `ClassName` to count reservations, identifying the most popular classes.

---

## Key Findings

1.  **The "Premium" Tier is the Most Valuable Segment:** The analysis showed that Premium members contribute the most revenue at **468,000 JPY**, significantly more than the Standard and Basic tiers.
2.  **Premium Members Prefer Yoga & Pilates:** The most popular classes for this valuable segment are heavily focused on yoga and pilates. **3 of the top 5 most-attended classes** are 'Morning Yoga', 'Restorative Yoga', and 'Core&More Pilates'.

---

## Business Recommendations

Based on these findings, the following data-driven recommendations were made:

* **Optimize Class Scheduling:** Add more yoga and pilates classes during peak hours (evenings and weekends) to meet the high demand from the most valuable member segment.
* **Launch Targeted Marketing:** Use marketing materials (posters, social media) that highlight the gym's strength in yoga and pilates to attract a similar high-value clientele and increase premium-tier sign-ups.
* **Re-allocate Resources:** Reduce the frequency of less popular classes to allocate trainer resources more efficiently to the high-demand classes.

---

## Appendix: Database & Analysis SQL Code

### SQL: Database Schema (DDL)

```sql
-- MembershipTier Table
CREATE TABLE MembershipTier (
    MembershipTierID SERIAL PRIMARY KEY,
    TierName VARCHAR(50) NOT NULL,
    Price DECIMAL(10, 2) NOT NULL
);

-- Trainer Table
CREATE TABLE Trainer (
    TrainerID SERIAL PRIMARY KEY,
    FirstName VARCHAR(50) NOT NULL,
    LastName VARCHAR(50) NOT NULL,
    PhoneNumber VARCHAR(20),
    Specialization VARCHAR(50)
);

-- Class Table
CREATE TABLE Class (
    ClassID SERIAL PRIMARY KEY,
    ClassName VARCHAR(50) NOT NULL,
    Description VARCHAR(255),
    ClassTime TIMESTAMP NOT NULL,
    TrainerID INTEGER,
    FOREIGN KEY (TrainerID) REFERENCES Trainer (TrainerID)
);

-- Members Table
CREATE TABLE Members (
    MemberID SERIAL PRIMARY KEY,
    FirstName VARCHAR(50) NOT NULL,
    LastName VARCHAR(50) NOT NULL,
    PhoneNumber VARCHAR(20),
    JoinedDate DATE NOT NULL,
    MembershipTierID INTEGER,
    FOREIGN KEY (MembershipTierID) REFERENCES MembershipTier (MembershipTierID)
);

-- Reservation Table (Junction Table)
CREATE TABLE Reservation (
    ReservationID SERIAL PRIMARY KEY,
    MemberID INTEGER NOT NULL,
    ClassID INTEGER NOT NULL,
    ReservationTime TIMESTAMP NOT NULL,
    FOREIGN KEY (MemberID) REFERENCES Members (MemberID),
    FOREIGN KEY (ClassID) REFERENCES Class (ClassID)
);
