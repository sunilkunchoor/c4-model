# 🗺️ Mastering the C4 Model for Software Architecture

Welcome to my repository! This project serves as a consolidation of my understanding and learning regarding the **C4 model**, an "easy to learn, developer-friendly approach to software architecture diagramming" created by Simon Brown.

Information and concepts here are adapted from the official [C4 Model website](https://c4model.com/).

---

## 📖 What is the C4 Model?

The C4 model is a standardized, hierarchical framework for visualizing software architecture. It solves the common problem of confusing, inconsistent, and overly complex architecture diagrams. 

At its core, the C4 model provides:
1. **A set of hierarchical abstractions:** Software systems, containers, components, and code.
2. **A set of hierarchical diagrams:** System context, containers, components, and code.
3. **An additional set of supporting diagrams:** System landscape, dynamic, and deployment.
4. **Tool and notation independence:** You can draw C4 diagrams using whiteboards, simple boxes and lines, or diagrams-as-code tools (like Mermaid or PlantUML).

Think of it as **Google Maps for your code**. It allows you to zoom in and out of an architecture from a high-level overview down to the specific implementation details.

---

## 🧱 Core Abstractions

To create maps of your code, we first need a common set of abstractions:
* **🧍 Person:** The end-users or actors (human or external) interacting with the system.
* **🏢 Software System:** The highest level of abstraction. A system is made up of multiple containers.
* **📦 Container:** Not to be confused with Docker. A container is a separately deployable/runnable unit (e.g., a web application, database, microservice, mobile app).
* **⚙️ Component:** A grouping of related functionality encapsulated behind a well-defined interface, running inside a container (e.g., an Order Controller, a Security Service).

---

## 🔍 The 4 Levels of Diagrams (with Examples)

Here is a breakdown of the four core diagram types, illustrated using Mermaid's C4 diagram support. Let's use a theoretical **E-Commerce Platform** as an example.

### Level 1: System Context Diagram
The System Context diagram provides a starting point, showing how the software system in scope fits into the world around it. It focuses on people (users) and external systems.

**Target Audience:** Non-technical stakeholders, business analysts, and developers.

```mermaid
C4Context
  title System Context Diagram: E-Commerce Platform

  Person(customer, "Customer", "A customer who browses and buys products online.")
  
  System(ecommerce, "E-Commerce System", "Allows customers to view products, manage their cart, and checkout.")
  
  System_Ext(payment_gateway, "Payment Gateway", "Processes credit card payments (e.g., Stripe/PayPal).")
  System_Ext(email_system, "Email System", "Sends order confirmations and shipping updates.")

  Rel(customer, ecommerce, "Browses products and makes purchases using")
  Rel(ecommerce, payment_gateway, "Processes payments via")
  Rel(ecommerce, email_system, "Sends e-mails using")

C4Container
  title Container Diagram: E-Commerce Platform

  Person(customer, "Customer", "A customer who browses and buys products online.")
  System_Ext(payment_gateway, "Payment Gateway", "Processes credit card payments.")

  System_Boundary(ecommerce_boundary, "E-Commerce System") {
    Container(web_app, "Web Application", "React", "Delivers the static content and single page application.")
    Container(api, "Backend API", "Node.js / Express", "Provides e-commerce functionality via a JSON/HTTPS API.")
    ContainerDb(db, "Database", "PostgreSQL", "Stores user profiles, product catalogs, and order history.")
  }

  Rel(customer, web_app, "Visits", "HTTPS")
  Rel(web_app, api, "Makes API calls to", "JSON/HTTPS")
  Rel(api, db, "Reads from and writes to", "SQL/TCP")
  Rel(api, payment_gateway, "Makes API calls to", "JSON/HTTPS")

C4Component
  title Component Diagram: Backend API Container

  Container(web_app, "Web Application", "React", "Delivers the single page application.")
  ContainerDb(db, "Database", "PostgreSQL", "Stores user, product, and order data.")
  System_Ext(payment_gateway, "Payment Gateway", "Stripe")

  Container_Boundary(api_boundary, "Backend API") {
    Component(order_controller, "Order Controller", "Express Router", "Handles incoming HTTP requests for orders.")
    Component(security, "Security Component", "JWT Middleware", "Validates authorization tokens.")
    Component(payment_service, "Payment Service", "Node Module", "Interfaces with external payment gateways.")
    Component(db_repo, "Database Repository", "Prisma ORM", "Handles database read/writes.")
  }

  Rel(web_app, order_controller, "Makes requests to", "JSON/HTTPS")
  Rel(order_controller, security, "Uses")
  Rel(security, payment_service, "Delegates to")
  Rel(security, db_repo, "Delegates to")
  
  Rel(payment_service, payment_gateway, "Calls", "HTTPS")
  Rel(db_repo, db, "Queries", "SQL/TCP")
```
