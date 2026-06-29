# Requirements Specification

**Veterinary Practice Information System — Dierenkliniek De Vries**

| **Document type** | Requirements specification |
| --- | --- |
| **Source** | Stakeholder interview transcripts v1.0 |
| **Stakeholders** | Dr. Erik de Vries · Fatima Yilmaz · Lars Hoekstra · Mrs. Anita Bakker · Thomas Bergmann |
| **Notation** | MoSCoW (Must / Should / Could / Won't) |

---

## Stakeholder key

| Code | Name | Role |
| --- | --- | --- |
| **DV** | Dr. Erik de Vries | Practice owner, lead veterinarian |
| **FY** | Fatima Yilmaz | Senior receptionist |
| **LH** | Lars Hoekstra | Veterinary assistant |
| **AB** | Mrs. Anita Bakker | Pet owner (client) |
| **TB** | Thomas Bergmann | Lab technician, Laboratorium Centraal |

---

## Functional Requirements

| ID | Description | Source | Stated | MoSCoW |
| --- | --- | --- | --- | --- |
| FR-01 | The system shall allow clients to book appointments online via a web interface, selecting date, preferred vet, and appointment type. | DV, LH, AB | Explicit | **Must** |
| FR-02 | The system shall continue to support phone-based appointment booking alongside online booking channels. | FY, AB | Implicit | **Must** |
| FR-03 | The system shall require clients to select an appointment type or reason (e.g. vaccination, general check-up, skin issue, follow-up) during booking, and automatically assign the predefined slot duration for that type. | FY, LH | Implicit | **Must** |
| FR-04 | The system shall enforce scheduling consistency across all booking channels and prevent double bookings. | DV | Explicit | **Must** |
| FR-05 | The system shall send automated reminders to pet owners before vaccination due dates and upcoming appointments, at configurable intervals. | DV, FY, AB | Explicit | **Must** |
| FR-06 | The system shall deliver appointment and vaccination reminders via SMS; email shall be a configurable secondary channel. | AB | Explicit | **Must** |
| FR-07 | The system shall display a unified client view combining the client's contact details, animals, appointment history, patient records, and outstanding invoices on a single screen. | FY | Explicit | **Must** |
| FR-08 | The system shall register controlled substance administration in compliance with FIDIN reporting requirements. | LH | Explicit | **Must** |
| FR-09 | The system shall migrate all existing patient, owner, appointment, and financial data from VetBase with no data loss, verified by a pre/post migration reconciliation report. | DV | Explicit | **Must** |
| FR-10 | The system shall maintain a persistent patient identifier mapping linking the microchip number, internal record ID, and external lab accession number for each animal. | TB | Explicit | **Must** |
| FR-11 | The system shall receive lab results from Laboratorium Centraal electronically (HL7 v2.5 ORU messages, or REST API as fallback) and automatically link each result to the corresponding patient record. | DV, LH, TB | Explicit | **Should** |
| FR-12 | The system shall display LOINC-coded lab results including numeric value, unit, reference range, and an abnormal-value flag. | TB | Explicit | **Should** |
| FR-13 | The system shall automatically send an electronic lab order (HL7 ORM message) to Laboratorium Centraal when a sample is submitted, replacing the current paper form. | TB | Explicit | **Should** |
| FR-14 | The system shall automatically decrement medication stock quantities when a medication administration is logged in a patient record. | LH | Explicit | **Should** |
| FR-15 | The system shall generate a configurable low-stock alert when a medication's quantity falls below a defined threshold, and deliver it to designated staff. | DV, LH | Explicit | **Should** |
| FR-16 | The system shall send automated appointment confirmation messages to clients after booking, and support client-initiated cancellation or rescheduling. | FY | Implicit | **Should** |
| FR-17 | The system shall provide mobile access to patient records (read and write) for use during house calls, without requiring a desktop connection. | LH | Explicit | **Should** |
| FR-18 | The system shall generate monthly management reports covering revenue per period, appointment type breakdown, practitioner caseload, and client retention rate. | DV | Explicit | **Should** |
| FR-19 | The system shall provide clients with a read-only portal to view their animals' vaccination history, administered treatments, and current medications. | AB | Explicit | **Could** |
| FR-20 | The system shall provide a near-real-time inventory overview of medications and consumables, updated whenever a medication is administered or restocked. | DV | Explicit | **Could** |
| FR-21 | The system shall replace the current WhatsApp-based internal staff communication with an in-system messaging or task notification channel. | LH | Implicit | **Won't** |

---

## Non-Functional Requirements

| ID | Description | Source | Stated | MoSCoW |
| --- | --- | --- | --- | --- |
| NFR-01 | The system shall comply with GDPR / AVG, including a signed Data Processing Agreement (DPA), EU data residency, documented data access controls, and a consent mechanism for client personal data. | DV, AB | Explicit | **Must** |
| NFR-02 | The system shall achieve a minimum availability of 99.9 % during practice opening hours (Monday–Saturday 08:00–18:00 local time), measured per calendar month. | DV | Explicit | **Must** |
| NFR-03 | The system shall respond to appointment search queries and patient record lookups within 2 seconds for 95 % of requests under normal operating load (up to 10 concurrent users). | FY | Implicit | **Must** |
| NFR-04 | All data exchanged with Laboratorium Centraal shall be encrypted using TLS 1.2 or higher, with mutual certificate authentication on both sides of the connection. | TB | Explicit | **Must** |
| NFR-05 | Network connections from the system to Laboratorium Centraal shall originate from a registered static IP address or through a dedicated VPN gateway. | TB | Explicit | **Must** |
| NFR-06 | The system shall enforce role-based access control so that client and patient records are accessible only to authenticated staff with the appropriate role (vet, assistant, receptionist, admin). | DV, AB | Implicit | **Must** |
| NFR-07 | The client-facing booking interface and patient portal shall conform to WCAG 2.1 Level AA, including a minimum body font size of 16 px, high-contrast colour scheme, and keyboard-navigable forms. | AB | Implicit | **Should** |
| NFR-08 | SMS and email reminders shall consist of plain-language text containing the essential information (animal name, date, time, location) without requiring the recipient to click a link or log in. | AB | Explicit | **Should** |
| NFR-09 | The online booking interface shall be fully functional on smartphones with screen widths from 375 px upward without horizontal scrolling. | LH, AB | Implicit | **Should** |
| NFR-10 | The system shall retain a locally cached view of the current day's appointment schedule, accessible at reception without an active internet connection, updated at least every 15 minutes when online. | DV, FY | Implicit | **Could** |
| NFR-11 | The HL7 integration layer shall parse and validate incoming ORU messages within 30 seconds of receipt and flag any message that cannot be automatically linked to a patient record for manual review. | TB | Implicit | **Could** |

---

## MoSCoW summary

| Priority | Count | Justification |
| --- | --- | --- |
| **Must Have** | FR-01–10, NFR-01–06 | Legal obligations (GDPR, FIDIN), core clinical workflow (records, scheduling, lab results), data integrity (migration, identifier mapping), and the non-functional baseline without which the system cannot operate safely. |
| **Should Have** | FR-11–18, NFR-07–09 | Significant pain points and quality-of-life improvements strongly requested by multiple stakeholders; the system works without them but is materially worse. |
| **Could Have** | FR-19–20, NFR-10–11 | Valuable features with a single primary source or limited reach; defer to a second release if scope or budget is tight. |
| **Won't Have** | FR-21 | Internal messaging is out of scope for an initial release; existing WhatsApp channel is functional, and designing a replacement carries unnecessary risk during an already large migration. |

---

## Key requirement conflicts to resolve

| Conflict | Stakeholders | Tension | Proposed resolution |
| --- | --- | --- | --- |
| Online booking vs. operational control | DV (wants online booking) vs. FY (wants to control slot sizing and no-show risk) | Fatima's concerns are functionally valid, not just change resistance. | Implement appointment-type selection (FR-03) so the system enforces duration automatically, and add automated confirmation/cancellation (FR-16) to reduce no-shows. Fatima's phone channel is preserved (FR-02). |
| Full platform replacement vs. incremental integration | LH (replace VetBase entirely) vs. DV (fears disruption and migration cost) | Lars may be gold-plating; Dr. de Vries may be underestimating the cost of ongoing integration debt. | Adopt a phased approach: keep VetBase for now, build an integration layer for lab results (FR-11) and booking, and migrate fully in a later phase once the data model (FR-10) is proven. |
| Automated reminders vs. personal client contact | FY (values personal calls) vs. DV (wants automation) | Personal calls genuinely reduce no-shows; automated reminders may not match that effectiveness for all client segments. | Use SMS/email automation (FR-05, FR-06) as the default while leaving room for Fatima to continue calls for high-risk slots or elderly clients (AB segment) who prefer it. |
