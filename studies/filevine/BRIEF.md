================ BRIEF: Filevine functional study ================

- Application: Filevine, Mohamad's trial tenant, open in his browser (the tab titled "Filevine"). The trial project seen there: "Filevine Free Trial Walk Through", client "Trial Client", with Time & Billing (Summary, Billing Entries, Project Funds, Settings).
- Repository to create: Qadi-AI/filevine-functional-study (private).
- Public documentation to read first: Filevine Help Center (support.filevine.com) and the Filevine API reference (developer.filevine.io). Verify both URLs.

AREAS, one findings file each. Top navigation seen: Tasks, Feed, Project Hub, Documents, project search. Project sections seen: Activity, Docs, Time & Billing, Incident, Conflict Check, Project Calendar, Deadlines, Contacts, Team, Related Projects; a Vitals panel and a project status.
1. projects.md: Filevine's project is its matter. Project types and templates, phases, sections and custom sections (Manage Sections), vitals, status, numbering, client linkage, related projects, permissions. Create a project from a template and record every request.
2. contacts.md: contact object, person versus organisation, roles on a project, conflict check, custom fields.
3. tasks.md: fields, assignees, due dates, task templates and automatic task creation from phases, the global Tasks view, notifications.
4. calendar-deadlines.md: Project Calendar and Deadlines: deadline chains and rules, court-rule integration if present, reminders, link to tasks.
5. time-billing.md: Billing Entries: time and expense entries, rates, flat fee, billable flags, timers, approval. Summary numbers (Total Invoices, Total Transactions, Current Balance, Unbilled Time Entries, Expenses, Flat Fee) and how each is computed. Create entries and record the wire.
6. invoices-payments.md: invoices and transactions: creation from unbilled entries, invoice states, payments, adjustments, Project Funds (trust or retainer handling), Billing Settings per project.
7. documents.md: Docs section and the global Documents area: folders, upload, versions, templates and document generation, e-signature, integrations, OCR or search.
8. feed-communications.md: Feed and Activity: notes, emails, texts, mentions, how communications attach to projects and become billable.
9. reports.md: reports and dashboards available in the trial, filters, exports, the numbers computed.
10. settings.md: org and project settings that change the above: billing settings, rates, custom fields, permissions, templates.

WIRE WORK: Filevine is a single-page app; its UI calls an internal API. For each created or changed record, capture the request and response shape with REA and write the endpoint into findings/api-inventory.md with placeholders for ids. Compare with the public API reference and note every difference.

EXTRAS: findings/data-model.md, a Mermaid entity diagram covering project, project type, section, contact, task, deadline, billing entry, invoice, transaction, project fund, document, note, user. findings/clio-comparison-hooks.md, a short table of the terms and structures that differ from Clio (project versus matter, sections versus fixed tabs, funds versus trust accounts), to be merged with the Clio study later.
