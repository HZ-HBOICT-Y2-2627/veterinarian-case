# Stakeholder Interview Transcripts

**Veterinary Practice Information System — Requirements Analysis Exercise**

*Software Engineering Bachelor Programme*

| **Course** | Requirements Engineering |
| --- | --- |
| **Topic** | Veterinary practice workflow integration & online appointment booking |
| **Interviews** | 5 stakeholders |
| **Version** | 1.0 |

## About this document

This document contains five interview transcripts conducted with stakeholders at a fictional veterinary practice (Dierenkliniek De Vries). The interviews were conducted as part of a requirements elicitation exercise for a new integrated practice management system.

The conversations are realistic but constructed for educational purposes. Stakeholders have different roles, different levels of technical literacy, and sometimes contradictory views. This is intentional.

## How to read these transcripts

Each interview is presented as a dialogue. Throughout the transcripts you will find annotation boxes marked with > 📌 **Annotation:** — these highlight moments where a requirement, constraint, or design concern is embedded in the conversation. Use these as starting points, not as a complete list. Many requirements are implicit and require interpretation.

Tension between stakeholders is deliberate. Where one stakeholder's wish conflicts with another's concern, students should identify this as a conflict and propose a resolution strategy.

## Assignment tasks

1.  Identify all functional requirements across the five interviews. For each requirement, note which stakeholder(s) raised it and whether it was stated explicitly or implicitly.

2.  Identify all non-functional requirements. Formalise at least three of these into a measurable specification (e.g. 'The system shall respond to appointment search queries within 2 seconds for 95% of requests under normal load').

3.  Identify at least three conflicts between stakeholder views. For each conflict, describe the tension and propose how it could be resolved in the design.

4.  Create a MoSCoW prioritisation of the requirements you have identified. Justify your Must-Have selections.

5.  Draw a context diagram showing the system boundaries and all external actors and systems mentioned across the interviews.

## Stakeholder overview

| **Interview 1** | Dr. Erik de Vries — Practice owner, lead vet |
| --- | --- |
| **Interview 2** | Fatima Yilmaz — Senior receptionist |
| **Interview 3** | Lars Hoekstra — Veterinary assistant |
| **Interview 4** | Mrs. Anita Bakker — Pet owner (client) |
| **Interview 5** | Thomas Bergmann — Lab technician (external partner) |

---

# Interview 1 — Dr. Erik de Vries

*Practice Owner & Lead Veterinarian*

| **Role** | Practice owner, lead veterinarian |
| --- | --- |
| **Experience** | 18 years in veterinary practice, owner for 9 years |
| **Tech literacy** | Moderate — uses email, looks up clinical literature, skeptical of hype |
| **Interview context** | Conducted in his office, after morning rounds. Occasionally interrupted. |

## Opening — Context & Background

**Interviewer** _Student_

Dr. de Vries, thank you for making time. Could you start by describing the practice and your role here?

**Dr. de Vries** _Practice Owner_

Sure. We've been running this practice for about nine years now. We have four vets — including myself — three veterinary assistants, and two receptionists. We do mostly companion animals: dogs, cats, the occasional rabbit or guinea pig. We do have one colleague who handles exotic animals two days a week, but that's a separate thing.

**Interviewer** _Student_

And how would you describe the current state of your systems — the software and tools you use to manage the practice?

**Dr. de Vries** _Practice Owner_

Organised chaos, I'd say. We use a practice management system — it's called VetBase — and it handles patient records, invoicing, appointment planning, that sort of thing. But it's... look, we bought it eight years ago and it hasn't really kept up with the times. The interface is stuck somewhere in 2012.

**Interviewer** _Student_

What does that mean concretely for your daily operations?

**Dr. de Vries** _Practice Owner_

It means our receptionists are doing a lot of things manually that really shouldn't be manual anymore. If a client wants to know when their dog's next vaccination is due, Fatima has to open the record, scroll through, find the last vaccination date, calculate the interval herself, and then call or email the client. There's no automated reminder system. We do it with a printed calendar on the wall and sticky notes.

> 📌 **Annotation:** Implicit requirement: automated vaccination/appointment reminders linked to patient records.

**Interviewer** _Student_

Is that a problem specific to reminders, or is it broader?

**Dr. de Vries** _Practice Owner_

It's broader. The lab results are a good example. We send samples to an external lab — Laboratorium Centraal — and they email us a PDF with the results. Then one of my assistants manually enters the key values into VetBase. So the data exists in two places, and sometimes there are errors in transcription, and sometimes the PDF sits in the inbox for a day before anyone notices it. Last month we had a situation where a follow-up was delayed because the results were in the email but not in the patient record. That's not acceptable.

> 📌 **Annotation:** Explicit pain point: lab result integration — PDF emails, manual transcription, risk of delay or error.

## Current Workflow & Integration Problems

**Interviewer** _Student_

You mentioned VetBase. Are there other systems you're using alongside it?

**Dr. de Vries** _Practice Owner_

Yes, and that's precisely the problem. VetBase for records and invoicing. A separate system — just a shared Google Calendar — for appointment overview, because frankly the VetBase calendar is unusable. My assistants use a WhatsApp group for internal communication. We have an email account for client contact. And for medication stock we use an Excel spreadsheet that Lars updates manually every week.

> 📌 **Annotation:** Current landscape: VetBase + Google Calendar + WhatsApp + email + Excel — five disconnected systems.

**Interviewer** _Student_

That's quite a few separate tools. Is there any connection between them?

**Dr. de Vries** _Practice Owner_

Essentially none. That's the problem I'd really like to solve. If I add an appointment in Google Calendar, VetBase doesn't know about it. If a client calls and says they booked online — which they can't right now, but in theory — the receptionist would still have to enter it manually somewhere. It's all silos.

**Interviewer** _Student_

You mentioned clients can't book online. Is that something you're considering?

**Dr. de Vries** _Practice Owner_

It's something I want. Clients ask for it constantly. Especially younger clients — they don't want to phone during opening hours. They want to book at ten at night when they remember their dog needs a check-up. I lose appointments over this. People just go to another practice that has an online form or an app.

> 📌 **Annotation:** Strong stakeholder demand: online/app-based appointment booking for clients.

**Interviewer** _Student_

Do you have a sense of what that would involve technically?

**Dr. de Vries** _Practice Owner_

[pauses] Not really, no. That's — I assume it requires some kind of website or app that's connected to whatever we use for planning. My worry is that the two don't sync properly and we get double bookings or gaps. That's a nightmare scenario for us. We're already fully booked most days.

> 📌 **Annotation:** Non-functional concern: scheduling consistency, no double bookings — availability and concurrency requirement.

## Business & Reporting Needs

**Interviewer** _Student_

From a business perspective, what kind of information do you need to manage the practice effectively?

**Dr. de Vries** _Practice Owner_

Revenue per period, which appointments are generating the most income, which vet is handling how many cases, how many clients we're retaining versus losing. Right now I get none of that automatically. I export an Excel file from VetBase every quarter and then try to make sense of it myself, which takes a full afternoon and usually results in me calling my accountant with questions.

> 📌 **Annotation:** Requirement: reporting and analytics dashboard — revenue, appointment types, practitioner load, client retention.

**Interviewer** _Student_

Would that kind of reporting need to be real-time, or is a weekly or monthly overview sufficient?

**Dr. de Vries** _Practice Owner_

Monthly is fine for most things. For stock — medications, consumables — I'd want something closer to real-time. We've had situations where we ran out of a common antibiotic and didn't know until someone went to the cabinet.

> 📌 **Annotation:** Requirement: near-real-time inventory/stock monitoring for medications.

## Privacy, Compliance & Risk

**Interviewer** _Student_

Are there any legal or compliance requirements that affect what kind of system you can use?

**Dr. de Vries** _Practice Owner_

Yes, and this is something I feel strongly about. Patient data — animal records — is linked to owner data, which is personal data under the AVG. We're a Dutch practice, so GDPR applies. Any system has to handle that correctly. We can't just store client data on some American server with unclear data processing agreements.

> 📌 **Annotation:** Explicit compliance requirement: GDPR / AVG compliance, data processing agreements, data residency.

**Interviewer** _Student_

Has that ever caused issues with your current setup?

**Dr. de Vries** _Practice Owner_

Honestly, I'm not sure our current setup is fully compliant. VetBase says it is, but I've never had it properly audited. It's one of those things that worries me but hasn't caused a problem yet. If we're building something new, I want that to be watertight from the beginning.

**Interviewer** _Student_

One last question — what's your biggest fear about introducing a new, more integrated system?

**Dr. de Vries** _Practice Owner_

Disruption. We can't go offline. I mean that literally — if the system is down during opening hours, we cannot function. We have no paper backup, or not a usable one. So whatever we build needs to be reliable. Not 'pretty reliable' — actually reliable. And the migration from VetBase... that's a project in itself. Eight years of patient data. That has to move over perfectly.

> 📌 **Annotation:** Critical non-functional requirements: high availability, zero data loss during migration, offline resilience.

> ⚠️ **Teaching note:** Key tensions for students to identify: desire for integration vs fear of disruption; cost control vs full-featured tooling; data ownership vs modern cloud infrastructure.

---

# Interview 2 — Fatima Yilmaz

*Receptionist (Senior)*

| **Role** | Senior receptionist, front-desk operations, client communication |
| --- | --- |
| **Experience** | 6 years at this practice, previously retail and administration |
| **Tech literacy** | Low-moderate — comfortable with phone, email, basic software; resistant to change |
| **Interview context** | Conducted at the reception desk between appointments. Frequently interrupted by the phone. |

## Opening — Daily Workflow

**Interviewer** _Student_

Fatima, can you walk me through what a typical Monday morning looks like for you?

**Fatima** _Receptionist_

Right, so I get in at half past eight, the practice opens at nine. First thing I do is check the email — usually there are a few messages from the night before, people asking about appointments or with questions about their animals. I write those down in a notepad so I don't forget them. Then I check the calendar — our Google Calendar — to see who's coming in and when, and I print it out because the computer at reception is a bit slow and I find it easier to work from paper.

> 📌 **Annotation:** Pain point: slow system at reception forces paper workarounds — performance non-functional requirement.

**Interviewer** _Student_

You print the calendar every day?

**Fatima** _Receptionist_

Every morning, yes. Sometimes twice if there are last-minute changes. [laughs] It sounds old-fashioned, I know. But I know exactly where I am with a piece of paper. If the computer freezes — and it does freeze, quite often — I still know who's coming in.

**Interviewer** _Student_

What happens when clients call to make an appointment?

**Fatima** _Receptionist_

I check the calendar for a free slot, ask them which vet they prefer, what it's about, note the animal's name and their name. Then I add it to the Google Calendar. Then I also have to go into VetBase and add a note there, or look up if they're already a client. It's two steps every time. And sometimes I forget the second one, especially when it's busy.

> 📌 **Annotation:** Process gap: appointments entered in two systems separately — integration point, risk of inconsistency.

## The Online Booking Question

**Interviewer** _Student_

Dr. de Vries mentioned he'd like to introduce online appointment booking. What's your reaction to that?

**Fatima** _Receptionist_

[sighs] Look, I understand why he wants it. But I have concerns. When I take a booking by phone, I can ask the right questions. Is it urgent? Is the animal showing symptoms? What's the reason for the visit? That helps me decide how long the slot should be. A quick vaccination is fifteen minutes. A first consultation for a new puppy is forty-five. If clients just click 'I want an appointment' online, how does the system know which one to give them?

> 📌 **Annotation:** Functional requirement: appointment booking must include reason/type selection to determine slot duration.

**Interviewer** _Student_

That's a fair point. What else worries you about it?

**Fatima** _Receptionist_

What worries me is that people will book online and then not show up, and we won't have called to confirm. Right now I call every client the day before as a reminder. It takes maybe an hour in the afternoon but almost nobody cancels last-minute if I've spoken to them. If it's all automated — just an email reminder — I don't think people take it as seriously.

> 📌 **Annotation:** Requirement insight: automated reminders must be robust; no-show risk is real — possible need for confirmation/cancellation workflow.

**Interviewer** _Student_

Do you think your role would change if online booking was introduced?

**Fatima** _Receptionist_

[pause] I worry a bit, yes. If the phone stops ringing because everyone books online, what does that mean for me? I'm not just making appointments. I'm answering questions, handling complaints, comforting people whose pets are sick. A system can't do that. But I'm not sure everyone sees that.

> 📌 **Annotation:** Stakeholder concern: fear of job displacement — organizational change management consideration (out of scope technically, but relevant to adoption).

## Client Communication & Follow-up

**Interviewer** _Student_

Beyond appointments, how do you communicate with clients?

**Fatima** _Receptionist_

Phone, mostly. Email for some things — like sending invoices, or if a vet wants to explain something in writing after a consultation. We don't really do text messages, though some clients have asked for it. And we have a Facebook page but I'm the only one who posts on it sometimes, and that's mostly just holiday hours or something.

**Interviewer** _Student_

What about vaccination reminders or follow-up visits?

**Fatima** _Receptionist_

That's all me, manually. Every week I go through the records and check who's due. It's actually in VetBase — you can see the vaccination dates — but there's no alert. So I just have to remember to check. And sometimes I forget. Last month I found twelve clients who were overdue for reminders.

> 📌 **Annotation:** Explicit gap: no automated follow-up alerts in VetBase — requirement for automated reminders with configurable intervals.

**Interviewer** _Student_

What would make your job easier?

**Fatima** _Receptionist_

Honestly? One system where everything is in one place. Not five different things. If I look up a client, I want to see their appointments, their animal's records, whether they have an outstanding invoice, all of it on one screen. And it needs to be fast. The thing I dread most is a client standing at the desk and me clicking around for two minutes because the system is loading.

> 📌 **Annotation:** Key usability requirements: single unified view per client, fast response time — UX and performance requirements.

> ⚠️ **Teaching note:** Tension to flag: Fatima's resistance to online booking stems from legitimate workflow concerns (slot sizing, no-shows), not just technophobia. Students should distinguish valid functional concerns from change resistance.

---

# Interview 3 — Lars Hoekstra

*Veterinary Assistant*

| **Role** | Veterinary assistant — patient intake, medication administration, lab sample handling |
| --- | --- |
| **Experience** | 3 years at this practice, studied veterinary nursing |
| **Tech literacy** | High — enthusiastic adopter, uses apps for everything, has opinions about software |
| **Interview context** | Conducted in the prep room during a quieter period. Lars was eager to talk. |

## Opening — Enthusiasm & Current Pain Points

**Interviewer** _Student_

Lars, what does your day-to-day look like in terms of the systems you use?

**Lars** _Veterinary Assistant_

Honestly it's a bit of a mess. I use VetBase for patient records, but half the time the information I need isn't in there, or it's in there but not up to date. I use WhatsApp to communicate with the vets when they're in consultation — which I know isn't ideal from a data perspective, but it's what actually works. I use a paper form for lab sample requests. An actual paper form. In 2025. And I enter the results into VetBase manually when they come back as a PDF by email.

> 📌 **Annotation:** Confirms Dr. de Vries's account: lab result pipeline is fully manual — PDF → email → manual entry.

**Lars** _Veterinary Assistant_

I've actually looked into whether Laboratorium Centraal has an API or any kind of integration capability. They do — they have an HL7 interface that some larger clinics use. We just never set it up. It would solve the lab thing almost entirely.

> 📌 **Annotation:** Key integration opportunity identified by stakeholder: lab API (HL7) already exists but is not implemented.

**Interviewer** _Student_

Did you raise that with Dr. de Vries?

**Lars** _Veterinary Assistant_

I mentioned it once. He said 'that sounds expensive' and the conversation kind of ended there. [laughs] I think the issue is no one has actually sat down and scoped what it would take. I suspect it's not as complicated as he imagines.

## Mobile Access & Field Use

**Interviewer** _Student_

Do you need access to systems outside of the practice building?

**Lars** _Veterinary Assistant_

Yes! This is actually a big one. When Dr. de Vries or Dr. Smits does a house call — which happens maybe twice a week — they take a printed summary of the patient's history. A printed piece of paper. If they need to check something or update the record while they're there, they can't. They have to remember or write notes and enter it when they're back.

> 📌 **Annotation:** Requirement: mobile access to patient records for field/house call use cases.

**Interviewer** _Student_

Would a mobile app help there?

**Lars** _Veterinary Assistant_

Absolutely. Even just a read-only view of patient records on a phone would be better than nothing. Ideally you'd be able to update as well — add a note, log medication administered. But even read-only would be a massive improvement. I'd love a proper app honestly. I'm on my phone all day anyway.

**Interviewer** _Student_

What about the appointment booking side — clients booking online or through an app?

**Lars** _Veterinary Assistant_

I'm all for it. I think Fatima is a bit scared of it but I get her concerns — she raised the point about slot types, which is valid. But you can solve that with a dropdown when they book. Choose: vaccination, general check-up, skin issue, follow-up, whatever. Each type gets a predefined duration. It's not rocket science, other practices do it already.

> 📌 **Annotation:** Proposed solution: appointment type selection at booking determines slot duration — functional design suggestion from stakeholder.

## Medication & Stock

**Interviewer** _Student_

You mentioned medication earlier. How does stock management work currently?

**Lars** _Veterinary Assistant_

I update a spreadsheet every Friday. I count what's in the cabinet, compare it to last week, and if anything is getting low I flag it to Dr. de Vries or the office manager. It's not... systematic. Last month we ran out of ketamine — not a good situation — because I was sick the Friday before and no one else checked.

> 📌 **Annotation:** Stock management gap: no automated low-stock alerts, manual weekly process, single point of failure.

**Interviewer** _Student_

How would you want that to work ideally?

**Lars** _Veterinary Assistant_

Every time we administer a medication — which we log in VetBase anyway — that should automatically decrement the stock count. Then when something hits a threshold, an alert goes out. That's basic inventory management. Any decent system should do that. It would also help with the FIDIN registration — we're required to register controlled substance use, and right now that's also done manually at the end of each month.

> 📌 **Annotation:** Linked requirements: medication administration logging → automatic stock decrement → low-stock alerts. Plus: FIDIN compliance for controlled substances.

## Integration & Technical Wishlist

**Lars** _Veterinary Assistant_

Can I also just say — and I know this might be out of scope — but I think the whole thing should ideally be one platform. Not VetBase plus six other things. One login, one place, everything connected. Records, appointments, stock, invoicing, lab results, communication with clients. I've seen demos of systems that do all of this. PetDesk, Vetstoria, eVetPractice — there are options. The question is whether we migrate entirely or try to keep VetBase and bolt things onto it.

> 📌 **Annotation:** Architecture question raised by stakeholder: full replacement vs integration layer on top of VetBase. This is a key scoping question for students to identify.

**Interviewer** _Student_

Do you have a preference?

**Lars** _Veterinary Assistant_

Full replacement, honestly. VetBase is the root cause of half the problems. But I know that's a bigger project and Dr. de Vries will worry about the data migration. Fatima will worry about having to learn a new system. So politically... it's complicated.

> ⚠️ **Teaching note:** Tech-savvy stakeholder introducing scope and architecture risk. Students should identify: (1) HL7 lab integration as a near-term quick win, (2) full replacement vs. integration layer as a key architectural decision, (3) Lars's enthusiasm may lead to gold-plating — distinguish must-have from nice-to-have.

---

# Interview 4 — Mrs. Anita Bakker

*Pet Owner (Client)*

| **Role** | Client — owner of two dogs (Benji and Roos), client for 4 years |
| --- | --- |
| **Experience** | Retired teacher, limited smartphone use, prefers phone calls |
| **Tech literacy** | Low — has a smartphone but uses it mainly for calling and WhatsApp with family |
| **Interview context** | Interviewed by phone after being referred by the practice. Friendly, talkative. |

## Opening — Client Experience

**Interviewer** _Student_

Mrs. Bakker, thank you for agreeing to speak with us. Can you tell me how you currently interact with the veterinary practice?

**Mrs. Bakker** _Pet Owner / Client_

Well, I just call them when I need something. Benji is getting older so he needs his checks more often now, and Roos had a skin condition last spring. I always speak to Fatima — she's lovely. I call, she tells me when there's a slot, and I write it in my diary. That's how it works.

**Interviewer** _Student_

Do you ever use email or the internet to contact the practice?

**Mrs. Bakker** _Pet Owner / Client_

I have their email somewhere but I never use it. I find email a bit fussy — you write something, then you wait, and you're not sure if they've seen it. The phone is just clearer. You talk to a person, you get an answer.

**Interviewer** _Student_

If the practice introduced an app or website where you could book appointments yourself, would that be something you'd use?

**Mrs. Bakker** _Pet Owner / Client_

[pause] An app? On my phone? My granddaughter keeps trying to get me to use apps but I find them complicated. Everything is so small on the screen and you have to register and remember passwords... I don't know. Maybe if it was very simple. But I'd probably still just call Fatima.

> 📌 **Annotation:** Usability insight: a significant client segment (older users) will not use online booking without extremely simple UX and may continue to prefer phone — multi-channel support remains necessary.

## Communication & Reminders

**Interviewer** _Student_

Do you receive reminders from the practice about upcoming appointments or vaccinations?

**Mrs. Bakker** _Pet Owner / Client_

Fatima calls me the day before an appointment, yes. That's very helpful because I sometimes forget. [laughs] But for vaccinations — I have to be honest, I'm not always sure when they're due. I rely on Fatima to tell me. Last year Roos was apparently overdue by six weeks before anyone noticed. I felt terrible about that.

> 📌 **Annotation:** Confirms gap: proactive vaccination reminders are missing. Client cannot track this themselves — confirms need for automated reminders from practice side.

**Interviewer** _Student_

Would you like to receive text messages or emails to remind you?

**Mrs. Bakker** _Pet Owner / Client_

Text messages, yes — I can manage a text message. An email I might miss. But a text on my phone, that pops up and I see it. As long as it's in clear language, not a lot of technical things or links I have to click.

> 📌 **Annotation:** Preferred channel for reminders: SMS. Accessibility requirement: plain language, no complex links or login required to read the reminder.

**Interviewer** _Student_

What about being able to access Benji and Roos's medical records — vaccination history, treatments — online?

**Mrs. Bakker** _Pet Owner / Client_

That would be nice, actually. When Roos had her skin thing and I took her to the specialist clinic in Utrecht, they asked me what medication she'd been given here. I didn't know. I had to call the practice and they had to look it up and call me back. If I had that somewhere I could just look at, that would have saved time.

> 📌 **Annotation:** Requirement: client-accessible view of animal records (vaccination history, treatments, medications) — a patient portal or summary view.

**Interviewer** _Student_

Would you feel comfortable using a website for that — not an app, just a website on your computer?

**Mrs. Bakker** _Pet Owner / Client_

On my computer, yes. I can do that. I use my computer for banking and that kind of thing. As long as I don't have to figure out too much. No pop-ups. Clear buttons. Big enough text.

> 📌 **Annotation:** Accessibility requirements: clear navigation, legible font sizes, minimal distractions — WCAG accessibility considerations.

## Trust & Data

**Interviewer** _Student_

One more question — if the practice stored your personal information and your pets' records in a digital system, would you have any concerns about that?

**Mrs. Bakker** _Pet Owner / Client_

I trust the practice, so I assume they handle it properly. But I wouldn't want strangers seeing it. And I had something once with a webshop where my email ended up on spam lists — that wasn't nice. So as long as they're careful with it. I suppose I'd want to know who can see my information.

> 📌 **Annotation:** Client privacy expectation: data access transparency, no third-party sharing. Supports GDPR consent and transparency requirements.

> ⚠️ **Teaching note:** This interview represents a key non-technical user segment. Students should identify: (1) not all users will adopt online booking — phone must remain supported, (2) SMS is the preferred digital channel for this segment, (3) any client-facing interface must meet basic accessibility standards, (4) a record-sharing scenario with specialists reveals an implicit interoperability requirement.

---

# Interview 5 — Thomas Bergmann

*Lab Technician — Laboratorium Centraal (External Partner)*

| **Role** | IT integration specialist at external diagnostic laboratory |
| --- | --- |
| **Experience** | 7 years at Laboratorium Centraal, manages veterinary clinic integrations |
| **Tech literacy** | Very high — works with APIs, HL7, data standards daily |
| **Interview context** | Video call. Thomas joins from his office. Direct and technically precise. |

## Opening — Current Integration Method

**Interviewer** _Student_

Thomas, thanks for joining us. Can you describe how the current process works between your lab and the veterinary practice?

**Thomas** _Lab Technician / Integration Specialist_

Sure. Currently it's entirely manual from their end. We receive samples, process them, and send a results PDF by email to their shared inbox. That's it. No handshake, no confirmation of receipt, no structured data. It's essentially the same process as sending a fax, just digitally.

> 📌 **Annotation:** Current state: email + PDF = no structured data exchange, no delivery confirmation, no integration.

**Interviewer** _Student_

From your side, is there a better way to do this?

**Thomas** _Lab Technician / Integration Specialist_

Absolutely. We have an HL7 v2.5 interface that we use with about forty other clinics. It handles results delivery in a structured message format — LOINC-coded results, patient identifiers, requesting vet, all of it. When a result is ready, we push it directly to the clinic's system. No email, no PDF, no manual entry. The clinic's system receives the message, parses it, and links it to the patient record automatically.

> 📌 **Annotation:** Solution identified: HL7 v2.5 integration already exists on lab side, used by 40 other clinics. Requires compatible receiver on clinic side.

**Interviewer** _Student_

Does VetBase — the system the practice uses — support HL7?

**Thomas** _Lab Technician / Integration Specialist_

That's the question, isn't it. I've looked at VetBase's documentation. They claim HL7 support in their enterprise tier, but the practice is on a basic license. Whether it's a configuration issue or a licensing one, I can't say — that's something they'd need to discuss with VetBase directly. What I can say is that if they move to a modern system, or even if they build a simple middleware layer, we can connect to it very quickly. We do it regularly.

> 📌 **Annotation:** Key dependency: HL7 integration feasibility depends on VetBase license tier OR migration to a compatible system. Middleware as alternative.

## Data Standards & Technical Detail

**Interviewer** _Student_

You mentioned LOINC codes. Can you explain what those are in this context?

**Thomas** _Lab Technician / Integration Specialist_

LOINC is a standard coding system for laboratory observations. Instead of us sending a result that says 'glucose: 5.2' in free text, we send a structured message that says 'LOINC code 2345-7, value 5.2, unit mmol/L, reference range 3.9-6.1, flag normal.' The receiving system can then display it in a consistent way, trend it over time, flag abnormal values automatically. It's the difference between structured data and a text blob.

> 📌 **Annotation:** Technical requirement if HL7 is implemented: system must parse and display LOINC-coded results, including reference ranges and abnormal flags.

**Interviewer** _Student_

What about the request side — when the vet wants to order a test?

**Thomas** _Lab Technician / Integration Specialist_

Right now we get a paper form with the sample. A physical piece of paper. Or sometimes a handwritten note. We have to enter the order details manually on our end, which is also slow and error-prone. Ideally, the practice's system sends us an electronic order — also HL7, an ORM message — when they decide to send a sample. That way by the time the sample arrives, we already have the order in our system. We can start processing immediately. End-to-end it probably saves twenty minutes per sample.

> 📌 **Annotation:** Bidirectional integration opportunity: HL7 ORM (order) messages from clinic to lab + HL7 ORU (result) messages from lab to clinic. Currently only paper-based ordering exists.

## Practical Barriers & Workarounds

**Interviewer** _Student_

What's stopping this from happening? You clearly have the technical capability on your side.

**Thomas** _Lab Technician / Integration Specialist_

Honestly? Small practices don't have IT people. This practice has no one who could configure an HL7 endpoint, set up an MLLP listener, or map the patient identifiers between systems. That's not a criticism — it's just the reality. When we integrate with a large hospital, there's an IT department. When we integrate with a four-vet practice, there's... Fatima. Who, from what I understand, still prints the calendar.

> 📌 **Annotation:** Implementation barrier: small practice lacks IT capacity for integration setup — implementation complexity is a real constraint.

**Interviewer** _Student_

Is there a simpler path to integration that doesn't require HL7?

**Thomas** _Lab Technician / Integration Specialist_

We do have a REST API as well. It's newer, less feature-complete, but it's easier to work with. If someone builds a connector — a small service that polls our API for results and pushes them into their practice management system — that could work without needing full HL7 infrastructure. It's a bit more custom engineering but a lower barrier to entry. I'd say: HL7 if you're going full integration, REST API if you want a quick win.

> 📌 **Annotation:** Alternative integration path: REST API available — lower barrier, less complete. Trade-off between effort and feature completeness. Good candidate for phased approach.

## Data Security & Access

**Interviewer** _Student_

Are there any data security considerations on your side that the practice needs to be aware of?

**Thomas** _Lab Technician / Integration Specialist_

Patient confidentiality applies on our end too. Any integration needs to use TLS, and ideally mutual authentication — we verify the clinic's system and they verify ours. We've had cases where smaller clinics try to connect over plain HTTP because it's easier to set up. We refuse those. Also, we only accept connections from registered IP ranges, so the practice would need to have a static IP or use a VPN gateway if they're connecting from dynamic addresses.

> 📌 **Annotation:** Security requirements: TLS, mutual authentication, static IP or VPN for network-level access control.

**Interviewer** _Student_

Last question — from your experience integrating with many clinics, what's the most common mistake they make?

**Thomas** _Lab Technician / Integration Specialist_

Underestimating the patient identifier problem. Every system has its own internal patient ID. Our system has a lab accession number. The clinic has a VetBase record ID. The animal has a chip number. These are all different identifiers for the same animal. If you don't design a proper mapping from the start — and most small clinics don't — you end up with results that can't be linked to the right patient automatically. Then someone manually matches them anyway, and you haven't actually saved any work. Get the identifier strategy right first.

> 📌 **Annotation:** Critical design concern: patient identifier mapping across systems. Chip number as potential universal identifier. Must be addressed in data model before integration.

> ⚠️ **Teaching note:** Thomas is the most technically precise stakeholder. Students may struggle with HL7 jargon — the goal is to identify the integration requirement, not fully understand HL7. Key takeaways: (1) structured bidirectional integration is possible and lab-side ready, (2) REST API as simpler alternative, (3) identifier mapping is a critical design concern, (4) security requirements are non-negotiable.