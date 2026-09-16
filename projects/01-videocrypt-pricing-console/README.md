# VideoCrypt Pricing Console

### Making subscription pricing easier to manage

## Project Overview

VideoCrypt's pricing was connected directly to the website code. This meant that whenever the business team wanted to change a plan's price, they had to depend on the engineering team to make the change.

I explored how this process could be made simpler by creating a **Pricing Management Console**.

The idea was to give authorized business users a simple place where they could manage pricing without needing to change the website code.

### What I worked on

* Defined the problem and the users involved
* Identified the main product requirements
* Designed the pricing management workflow
* Built a working frontend prototype
* Added monthly and annual pricing management
* Added the ability to highlight a recommended plan
* Thought through important cases such as approvals, scheduled pricing changes, and existing subscribers
* Designed a possible production architecture for connecting the console to a backend and database

---

## Working Prototype

I built a functional frontend prototype to demonstrate how the Pricing Management Console could work.

**Prototype:** [Open the VideoCrypt Pricing Console](https://stackblitz.com/edit/vitejs-vite-ze2txe4m?file=index.html)

### How to open and test the prototype

1. Open the prototype using the link above.
2. Open the **Admin** section from the navigation bar.
3. Find one of the paid plans.
4. Change its monthly or annual price.
5. The change is saved in the browser.
6. Go back to the **Pricing** page.
7. You will see the updated price on the customer-facing pricing page.
8. You can also switch between **Monthly** and **Annually** to see the different prices.
9. Try changing the **Recommended** option to see how the pricing page changes.
10. Use **Reset to defaults** in the Admin section to restore the original values.

### Important note about the prototype

This is a **frontend prototype**, not a production system.

The prototype uses React and browser storage (`localStorage`) to demonstrate the main experience. It does not currently have a real backend, database, login system, or user permissions.

For a real production version, I would connect the console to a backend API and database and add appropriate access controls, approval workflows, and audit history.

---

## Problem

VideoCrypt's pricing was connected directly to the website code. So, whenever the business team wanted to change the price of a plan, they had to depend on the engineering team to make the change and publish the website again.

This could make simple pricing changes slower than necessary.

For example, if the marketing team wanted to run a limited-time discount or change the price of a subscription plan, they could not simply update the price themselves. They had to request the change from engineering.

The goal of this project was to create a **Pricing Management Console** where authorized business users could easily update plan prices and other pricing information without changing the website's code.

This would make pricing management faster, easier, and less dependent on engineering for routine changes.

---

## Users & Stakeholders

The product has two main sides:

### Business users

These are the people who may need to manage or review pricing.

Examples include:

* Marketing
* Sales
* Finance
* Product or pricing teams
* Administrators

### Customers

Customers do not use the Admin Console. They use the normal VideoCrypt pricing page to view plans and prices.

The system therefore needs to make sure that when an approved pricing change is made, customers see the correct information.

### User roles

Not everyone should have the ability to change pricing.

A possible role structure is:

| Role        | What they can do                               |
| ----------- | ---------------------------------------------- |
| Viewer      | View pricing and changes                       |
| Editor      | Create and edit pricing changes                |
| Approver    | Review and approve changes                     |
| Super Admin | Manage users, permissions, and system settings |

This creates a basic separation between **making a change** and **approving a change**.

---

## Product Goal

The main goal was:

> **Allow authorized business users to manage pricing without depending on engineering for every routine pricing change.**

The broader idea was to separate pricing information from the website code.

Instead of:

**Pricing → Website Code → Deployment**

the desired approach is:

**Pricing → Pricing Management Console → Central Pricing Data → Website**

This would make the pricing system easier to manage and give the business team more control.

---

## Requirements

### Must-have requirements

The Pricing Console should allow an authorized user to:

* View all available plans
* Change the price of a paid plan
* Manage monthly and annual pricing
* Mark a plan as recommended
* See when a change has been saved
* Reset pricing when needed
* See the updated pricing on the customer-facing page

### Future requirements for a production system

A real production version would also need:

* Login and user authentication
* Different user permissions
* Approval before publishing important pricing changes
* A record of who changed a price and when
* Scheduled pricing changes
* Ability to undo or roll back a change
* Protection against two people changing the same price at the same time
* Clear rules for how pricing changes affect existing subscribers

---

## User Flow

The basic flow is:

**Business user**

Admin Console
↓
Select a plan
↓
Change pricing
↓
Validate the change
↓
Save / Submit the change
↓
Pricing data is updated
↓
Customer pricing page shows the updated information

**Customer**

Open Pricing Page
↓
Pricing information is loaded
↓
Customer sees the current plan prices

---

## Solution

The solution is a **Pricing Management Console** that acts as a central place for managing subscription pricing.

Instead of changing the price inside the website code, a business user can update the price through the console.

The customer-facing pricing page then uses the latest pricing information.

The prototype demonstrates this basic experience using shared application state and browser storage.

---

## Technical Approach

The prototype was built using:

* React
* TypeScript
* Vite
* React Router
* React Context
* Browser `localStorage`

The main idea is simple:

```text
Default Pricing Data
        ↓
Pricing Context
        ↓
 ┌───────────────┐
 │               │
 ▼               ▼
Admin Page    Pricing Page
 │               │
 │ Update        │ Read
 │               │
 └───────┬───────┘
         ↓
    Browser Storage
```

When an admin changes a price, the shared pricing data is updated and stored in the browser.

The customer-facing pricing page reads the same data, so the change can be seen immediately in the prototype.

---

## Production Architecture

The prototype is only the frontend demonstration.

For a real production system, I would move the pricing data from browser storage to a central backend system.

The high-level architecture would be:

```text
Business User
      ↓
Pricing Admin Console
      ↓
Backend API
      ↓
Pricing Database
      ↓
Backend API
      ↓
Customer Pricing Website
```

This would allow pricing to be centrally managed instead of being stored inside an individual user's browser.

A production system could also add authentication, user permissions, approvals, audit history, scheduled changes, and rollback capabilities.

---

## Edge Cases

While designing the solution, I also considered situations that could cause problems if pricing changes were not controlled properly.

### 1. Someone enters an invalid price

The system should not allow negative prices or invalid values.

### 2. Two people change the same price

If two users edit the same plan at the same time, the system should prevent one person's update from accidentally overwriting the other person's change.

### 3. A price needs to change later

The system could allow a business user to schedule a pricing change for a future date.

### 4. Existing customers

A pricing change may need different rules for existing subscribers and new customers. This should be decided with the business, finance, and legal teams before building the production workflow.

### 5. Important pricing changes

For sensitive changes, one person could make the change while another person reviews and approves it before it becomes live.

---

## Success Measures

For a production version, I would measure whether the new system actually improves the pricing management process.

Possible metrics include:

* Time required to make a pricing change
* Number of pricing changes that require engineering support
* Number of pricing-related errors
* Time taken from creating a change to publishing it
* Number of failed or rolled-back pricing changes
* Percentage of pricing changes completed through the console

The main outcome I would look for is a reduction in unnecessary engineering dependency while maintaining proper control over pricing changes.

---

## What I Learned

This project helped me understand that a product problem is not always about creating a new customer-facing feature.

Sometimes the opportunity is to improve an internal process.

In this case, the important problem was the dependency between the business team and engineering for a relatively simple task: changing pricing.

The project also helped me think about the difference between a **working prototype** and a **production-ready product**.

The prototype demonstrates the user experience, while a production system would need additional pieces such as APIs, databases, authentication, permissions, approvals, security, and audit history.
