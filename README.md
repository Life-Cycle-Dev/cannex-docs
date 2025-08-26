---
icon: book
layout:
  width: default
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
  metadata:
    visible: true
---

# System Architecture Diagram

## Overview

This system is designed as a multi-tier architecture that separates responsibilities between **frontend**, **backend**, **database**, and **infrastructure services**. It ensures scalability, maintainability, and security for both end-users and administrators.

<figure><img src=".gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

## Components

#### Users

* **End Users** interact with the system via the **Next.js frontend application** hosted on the **Frontend Webserver**.
* **Administrators** access the system through the **Strapi Admin Interface** for content and configuration management.

#### Frontend Layer

* **Technology:** Next.js
* **Hosted on:** Frontend Webserver
* **Responsibility:**
  * Provides the web interface to end users.
  * Sends requests to the backend API server for dynamic data.
  * Handles client-side rendering and static asset delivery.

#### Backend Layer

* **Technology:** Strapi (Headless CMS)
* **Hosted on:** Backend Server
* **Responsibilities:**
  * Exposes RESTful or GraphQL APIs for both frontend and admin operations.
  * Manages content, user input, and system configuration.

#### Admin Interface

* **Technology:** Strapi Admin Panel
* **Accessed by:** Admins only
* **Responsibilities:**
  * Manage content (CRUD operations).
  * Configure application settings.
  * Oversee data stored in databases and object storage.

#### Database Layer

* **Postgres Server**
  * Stores relational data such as user information, content, and configuration.
  * Ensures persistence and transactional integrity.

#### File Storage

* **AWS S3 (Object Storage Server)**
  * Stores uploaded media assets (images, documents, etc.).
  * Provides scalability and redundancy for file storage.

#### Email Services

* **SMTP Server**
  * Handles transactional emails (notifications, confirmations, etc.).
  * Integrated with the backend server for automated messaging.

## Data Flow

1. **User Access**
   * Users send requests via the Next.js frontend.
   * Admins log in via the Strapi Admin Interface.
2. **Request Processing**
   * Frontend communicates with the backend Strapi server.
3. **Data Management**
   * Backend retrieves or updates structured data from the Postgres server.
   * Files are uploaded to and served from AWS S3.
4. **Communication**
   * For outbound emails, the backend communicates with the SMTP server.
