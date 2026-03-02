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
4. **Tool and notation independence:** You can draw C4 diagrams using whiteboards, simple boxes and lines, or diagrams-as-code tools.

Think of it as **Google Maps for your code**. It allows you to zoom in and out of an architecture from a high-level overview down to the specific implementation details.

---

## 🧱 Core Abstractions

To create maps of your code, we first need a common set of abstractions:
* **🧍 Person:** The end-users or actors interacting with the system.
* **🏢 Software System:** The highest level of abstraction. A system is made up of multiple containers.
* **📦 Container:** A separately deployable or runnable unit (e.g., a web application, database, microservice, mobile app).
* **⚙️ Component:** A grouping of related functionality encapsulated behind a well-defined interface, running inside a container.

---

## 🔍 The 4 Levels of Diagrams

Here is a breakdown of the four core diagram types, illustrated using Mermaid's C4 diagram support for a theoretical **E-Commerce Platform**.

### Level 1: System Context Diagram
The System Context diagram provides a starting point, showing how the software system in scope fits into the world around it. 

**Target Audience:** Non-technical stakeholders, business analysts, and developers.

```mermaid
C4Context
title System Context Diagram - E-Commerce Platform
Person(customer, "Customer", "A customer who browses and buys products online.")
System(ecommerce, "E-Commerce System", "Allows customers to view products and checkout.")
System_Ext(payment_gateway, "Payment Gateway", "Processes credit card payments.")
System_Ext(email_system, "Email System", "Sends order confirmations.")
Rel(customer, ecommerce, "Browses products using")
Rel(ecommerce, payment_gateway, "Processes payments via")
Rel(ecommerce, email_system, "Sends emails using")
```

### Level 2: Container Diagram
Zooming into the System Context, the Container diagram shows the high-level shape of the software architecture and how responsibilities are distributed.

**Target Audience:** Software developers, support staff, and IT operations.

```mermaid
C4Container
title Container Diagram - E-Commerce Platform
Person(customer, "Customer", "A customer who browses and buys products online.")
System_Ext(payment_gateway, "Payment Gateway", "Processes credit card payments.")
System_Boundary(ecommerce_boundary, "E-Commerce System") {
  Container(web_app, "Web Application", "React", "Delivers the single page application.")
  Container(api, "Backend API", "Node.js", "Provides e-commerce functionality.")
  ContainerDb(db, "Database", "PostgreSQL", "Stores user profiles and products.")
}
Rel(customer, web_app, "Visits", "HTTPS")
Rel(web_app, api, "Makes API calls to", "JSON")
Rel(api, db, "Reads and writes to", "SQL")
Rel(api, payment_gateway, "Makes API calls to", "HTTPS")
```
### Level 3: Component Diagram
Zooming into an individual Container reveals the Component diagram. It shows the internal components of a container and how they interact.

**Target Audience:** Software developers and software architects.

```mermaid
C4Component
title Component Diagram - Backend API Container
Container(web_app, "Web Application", "React", "Delivers the SPA.")
ContainerDb(db, "Database", "PostgreSQL", "Stores application data.")
System_Ext(payment_gateway, "Payment Gateway", "Stripe")
Container_Boundary(api_boundary, "Backend API") {
  Component(order_controller, "Order Controller", "Express Router", "Handles HTTP requests.")
  Component(security, "Security Component", "JWT Middleware", "Validates tokens.")
  Component(payment_service, "Payment Service", "Node Module", "Interfaces with gateways.")
  Component(db_repo, "Database Repository", "Prisma ORM", "Handles DB operations.")
}
Rel(web_app, order_controller, "Requests", "HTTPS")
Rel(order_controller, security, "Uses")
Rel(security, payment_service, "Delegates to")
Rel(security, db_repo, "Delegates to")
Rel(payment_service, payment_gateway, "Calls", "HTTPS")
Rel(db_repo, db, "Queries", "SQL")
```

### Level 4: Code Diagram
The deepest zoom level. It shows how an individual component is implemented under the hood (e.g., UML Class Diagrams).

> ⚠️ Note from the creator: This level is considered optional and is usually ignored because modern codebases change too fast. Code-level documentation is best generated automatically directly from the source code via IDE tooling.

---

## 🛠️ Additional Supporting Diagrams
The official C4 model also specifies three supplementary diagrams for when the static structural models aren't enough:
1. System Landscape Diagram: Shows the "big picture" of the IT landscape across multiple systems within an enterprise.
2. Dynamic Diagram: Used to show how elements interact at runtime to implement a specific use case or user story (similar to a UML Sequence Diagram).
3. Deployment Diagram: Shows how containers in the static model are mapped to the physical infrastructure (e.g., AWS, Azure, on-premise servers).

---

## 🔗 References & Resources
- [The Official C4 Model Website](https://c4model.com/)
- [Mermaid C4 Syntax Documentation](https://mermaid.js.org/syntax/c4.html)
- [Structurizr](https://structurizr.com/) (Diagramming tool built specifically for the C4 model by Simon Brown)
