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

# Database Diagram

## ER Diagram

This document describes the **Entity Relationship (ER) diagram** for the Strapi-based content management system. The diagram defines the relationships between key entities such as **Newsrooms, Events, Highlights, Contact Forms**, and **SEO Configurations**.

<figure><img src="../.gitbook/assets/Cannex - ER Diagram (1).png" alt=""><figcaption></figcaption></figure>

<details>

<summary>Code</summary>

```dbml
Table "contact_forms" {
  id integer PK
  document_id varchar(255)
  firstname varchar(255)
  lastname varchar(255)
  email varchar(255)
  reason varchar(255)
  message text
  created_at timestamp
  updated_at timestamp
  published_at timestamp
}

Table "events" {
  id integer PK
  document_id varchar(255)
  title varchar(255)
  slug varchar(255)
  description text
  content text
  view integer
  created_at timestamp
  updated_at timestamp
  published_at timestamp
}

Table "newsrooms" {
  id integer PK
  document_id varchar(255)
  title varchar(255)
  slug varchar(255)
  description text
  content text
  view integer
  created_at timestamp
  updated_at timestamp
  published_at timestamp
}

Table "contact_form_configs" {
  id integer PK
  document_id varchar(255)
  enable boolean
  reason text
  reply_subject varchar(255)
  reply_message text
  reply_to varchar(255)
  created_at timestamp
  updated_at timestamp
  published_at timestamp
}

Table "highlights" {
  id integer PK
  document_id varchar(255)
  created_at timestamp
  updated_at timestamp
  published_at timestamp
}

Table "highlights_events_lnk" {
  id SERIAL PK
  highlight_id integer
  event_id integer
}

Table "newsrooms_events_lnk" {
  id SERIAL PK
  highlight_id integer
  event_id integer
}

Table "seos" {
  id integer PK
  created_at timestamp
  updated_at timestamp
  published_at timestamp
}

Table "seos_cmps" {
  id SERIAL PK
  entity_id interger unique
  cmp_id interger unique
  component_type varchar(255) unique
  field varchar(255) unique
}

Table "newsrooms_cmps" {
  id SERIAL PK
  entity_id interger unique
  cmp_id interger unique
  component_type varchar(255) unique
  field varchar(255) unique
}

Table "events_cmps" {
  id SERIAL PK
  entity_id interger unique
  cmp_id interger unique
  component_type varchar(255) unique
  field varchar(255) unique
}

Ref: events.id > seos_cmps.entity_id
Ref: newsrooms.id > seos_cmps.entity_id
Ref: seos_cmps.cmp_id > seos.id

Ref: newsrooms.id > newsrooms_cmps.entity_id
Ref: events.id > events_cmps.entity_id

Ref: highlights_events_lnk.highlight_id > highlights.id
Ref: highlights_events_lnk.event_id > events.id

Ref: newsrooms_events_lnk.highlight_id > highlights.id
Ref: newsrooms_events_lnk.event_id > newsrooms.id

```

</details>

### Entities and Attributes

#### 1. **Highlights**

* **Purpose:** Represents featured items that can be linked to events or newsrooms.
* **Key Attributes:**
  * `id` (PK)
  * `document_id`
  * `created_at`, `updated_at`, `published_at`

#### 2. **Events**

* **Purpose:** Stores information about events.
* **Key Attributes:**
  * `id` (PK)
  * `document_id`
  * `seo_id` (FK → `seos.id`)
  * `title`, `slug`, `description`, `content`
  * `view` (event views)
  * `created_at`, `updated_at`, `published_at`

#### 3. **Newsrooms**

* **Purpose:** Stores newsroom articles or press releases.
* **Key Attributes:**
  * `id` (PK)
  * `document_id`
  * `seo_id` (FK → `seos.id`)
  * `title`, `slug`, `description`, `content`
  * `view` (newsroom views)
  * `created_at`, `updated_at`, `published_at`

#### 4. **SEOs**

* **Purpose:** Stores SEO metadata for both events and newsrooms.
* **Key Attributes:**
  * `id` (PK)
  * `document_id`
  * `created_at`, `updated_at`, `published_at`

#### 5. **Highlights–Events Link (highlights\_events\_lnk)**

* **Purpose:** Join table linking highlights and events (many-to-many).
* **Key Attributes:**
  * `id` (PK)
  * `highlight_id` (FK → `highlights.id`)
  * `event_id` (FK → `events.id`)

#### 6. **Newsrooms–Events Link (newsrooms\_events\_lnk)**

* **Purpose:** Join table linking highlights and newsrooms (many-to-many).
* **Key Attributes:**
  * `id` (PK)
  * `highlight_id` (FK → `highlights.id`)
  * `event_id` (FK → `newsrooms.id`)

#### 7. **Contact Forms**

* **Purpose:** Stores user-submitted contact form entries.
* **Key Attributes:**
  * `id` (PK)
  * `document_id`
  * `firstname`, `lastname`, `email`, `reason`, `message`
  * `created_at`, `updated_at`, `published_at`

#### 8. **Contact Form Configs**

* **Purpose:** Stores configuration for contact forms, including automatic reply settings.
* **Key Attributes:**
  * `id` (PK)
  * `document_id`
  * `enable` (boolean)
  * `reason`
  * `reply_subject`, `reply_message`, `reply_to`
  * `created_at`, `updated_at`, `published_at`

### Relationships

1. **Events → SEOs**
   * One-to-One
   * Each event can have a dedicated SEO record.
2. **Newsrooms → SEOs**
   * One-to-One
   * Each newsroom can have a dedicated SEO record.
3. **Highlights ↔ Events (via highlights\_events\_lnk)**
   * Many-to-Many
   * A highlight can reference multiple events, and an event can belong to multiple highlights.
4. **Highlights ↔ Newsrooms (via newsrooms\_events\_lnk)**
   * Many-to-Many
   * A highlight can reference multiple newsrooms, and a newsroom can belong to multiple highlights.
5. **Contact Forms ↔ Contact Form Configs**
   * Implicit (not shown directly in the diagram, but logical in system design).
   * Each submitted contact form follows one configuration (reply settings, enabled/disabled).



## Dataflow Diagram

### Event/Newsroom Process

**Create/Edit/Published newsroom dataflow diagram**

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

**Create/Edit/Published event dataflow diagram**

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

#### Actors

* **Admin**: The system administrator who has the authority to create, edit, and publish newsroom content.
* **User**: A general user who can only view published newsroom content.

#### Processes

* **Process Newsroom/Event**: The main process responsible for handling newsroom/event content, from draft creation to publication.

#### Data Stores

* **Newsroom/Event Data Store**: The central storage for newsroom/event articles and information, with two content states:
  * **Draft State**: Content created or edited by the admin but not yet visible to users.
  * **Publish State**: Content that has been approved and published, accessible by users.

#### Data Flows

1. **Add/Edit Newsrooms/Event (Admin → Process Newsroom)**
   * The admin creates or edits newsroom/event content.
   * The content is stored in the system as a **Draft State** entry.
2. **Publish Newsrooms/Event (Admin → Process Newsroom)**
   * When the admin publishes, the draft content is updated to **Publish State**.
   * The data is saved into the Newsroom/Event Data Store with published status.
3. **Retrieve Published Newsrooms/Event (User → Process Newsroom)**
   * A user navigates to the newsroom page.
   * The system retrieves only the content stored with **Publish State**.
4. **Display Newsrooms/Event (Process Newsroom/Event → User)**
   * The system processes and sends the published newsroom/event content to be displayed on the newsroom/event page for the user.

#### Workflow Summary

1. Admin adds or edits newsroom content → stored as **Draft**.
2. Admin publishes newsroom content → stored as **Published**.
3. User accesses the newsroom page → only **Published** content is retrieved.
4. Published content is displayed to the user.



### Contact Form Process

**Create contact form** **dataflow diagram**

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

#### Actors

* **User**: A visitor who fills out and submits the contact form on the website.
* **Admin**: A system administrator who can access the submitted contact form data via the admin panel.

#### Processes

* **Process Contact Form**: The main process responsible for handling form submission, validation, storing the information, and replying to the user.

#### Data Stores

* **Contact Form Data Store**:
  * Stores all submitted form information (e.g., name, email, reason, message).
  * Makes data accessible to admins for management and review.
* **Submit Form (User → Process Contact Form)**
  * The user fills out and submits the contact form.
  * The submitted data (name, email, message, etc.) is sent to the system.
* **Store Submitted Form (Process Contact Form → Contact Form Data Store)**
  * The system validates and stores the submitted form information in the Contact Form Data Store.
* **Reply Submitted Email (Process Contact Form → User)**
  * After successful submission, the system sends an automated reply/confirmation email back to the user.
* **Provide Contact Form Data (Contact Form Data Store → Admin)**
  * Admin can access the stored data through the contact form management interface.
* **Enter Contact Form Page (Admin → Contact Form)**
  * Admin navigates to the contact form page in the system to view or manage submissions.

#### Workflow Summary

1. The **User** submits the contact form.
2. The system **processes** the submission and stores the information in the **Contact Form Data Store**.
3. The system sends an **automated reply email** to the user.
4. The **Admin** enters the contact form management page to **review the submissions**.



### Contact Process

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

#### Actors

* **Admin**: The administrator who can configure and update contact settings.
* **User**: A general website visitor who enters the contact page and sees the contact configuration (e.g., contact details, email address).

#### Processes

* **Process Contact**:\
  The system process that manages the configuration of the contact form, including saving updated settings and providing the latest configuration to users.

#### Data Store

* **Contact Config Data Store**:\
  Stores the contact form configuration, such as recipient email address

#### Data Flows

1. **Edit Contact Config (Admin → Process Contact Config)**
   * The Admin updates or edits the contact configuration (e.g., change reply-to email, update message templates).
   * The configuration data is sent to the system.
2. **Store Contact Config (Process Contact Config → Contact Config Data Store)**
   * The system validates and stores the updated configuration in the Contact Config Data Store.
3. **Provide Contact Config (Contact Config Data Store → User)**
   * When a User visits the contact page, the system retrieves the stored configuration.
   * The user receives the current configuration (e.g., the correct contact details or automated reply template).
4. **Enter Contact Page (User → Contact Config)**
   * The User navigates to the contact page.
   * The system fetches the configuration and displays it to the user.

#### Workflow Summary

1. The **Admin** edits the contact configuration.
2. The system **processes** and stores the updated config into the **Contact Config Data Store**.
3. When a **User** enters the contact page, the system retrieves and provides the latest contact configuration.





