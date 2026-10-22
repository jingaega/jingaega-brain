---
title: Glossary
type: reference
project: "[[Holding ERP]]"
created: 2026-10-13
tags: [internship, erp, glossary]
---

# Glossary

The words we use for the Holding ERP, one meaning each. Specs, code and chats use these words. Part of [[Holding ERP]]. If a term is not here yet, add it when it gets settled.

## Group and access

**Company**:
One of the 6 PTs in the group. Every record belongs to exactly one company.

**Sibama**:
Short name for PT Siar Bagja Maraloka.
_Avoid_: Maraloka

**Group company**:
A company inside the group when it shows up as another company's customer or vendor, like Anggoan buying from Pandu.
_Avoid_: sister company, affiliate

**Elimination**:
In the holding's combined total, sales, debts and unsold profit between group companies are crossed out, so the group shows only what came from outside. Each company's own books stay the same, and the crossed out amount shows as its own line. Example: Pandu sells to Anggoan for Rp 100 jt and Anggoan sells to customers for Rp 130 jt, so group sales are Rp 130 jt, not Rp 230 jt. Required for group reports (PSAK 65). Built in Phase 9.
_Avoid_: removing sales, deleting

**Super Admin**:
The holding level account that can see and change everything in every company.
_Avoid_: super user (when you mean this one)

**Super User**:
The holding level account that can see everything in every company and change nothing.
_Avoid_: viewer

**User**:
An account that can sign in to the ERP, with its email or NRP, and its access to companies. Managed under Settings, Users.
_Avoid_: people, staff (when you mean an account)

**Employee**:
Someone who works for a group company, kept by the HR module (Phase 7) with personal data, contracts and payroll. Many employees never sign in. Later an employee can be linked to a user.
_Avoid_: user (when you mean the person on the payroll)

**NRP**:
A person's staff number (Nomor Registrasi Pegawai), unique across all 6 companies, and their ID in the ERP. People sign in with it. Existing staff keep the NRP HR already gave them (like `20123456`); new people get one automatically. Stored on the user for now; once HR exists, HR owns it and the user takes it from the linked employee.
_Avoid_: Employee ID, employee number

**NIK**:
The 16 digit number on a person's KTP (national ID card). Kept in their personal details, only for people allowed to see them.
_Avoid_: NIK for the staff number (that is the NRP)

**Level**:
A person's rank inside one company: Director, GM, Manager, SPV, Lead or OPR. Together with their department's function it decides what they can do. Since Foundation slice 11 this replaces the old roles (company admin, director, manager, supervisor, staff, viewer).
_Avoid_: role

**Department**:
A group of people inside one company, like Purchasing or Sales. Each person is in one department. Each department has a function: management, procurement, sales, marketing, warehouse, finance, HR, production, quality, general affairs or other. The function decides which records its people work on.
_Avoid_: division (when you mean this record)

**Company Admin**:
A switch on a person, next to their level, that lets them manage Settings for their company (users, departments, permissions). It is not a level.

**Permission**:
One action on one record type, like "create item" or "view customer".

**Access**:
A user holding a level in a department inside a company. One user can have access to several companies.

**Theme**:
A company's colors and logo, which the whole screen switches to.

## Master data

**Item**:
Something a company sells, buys or makes.
_Avoid_: product (unless it is a finished good)

**UoM**:
Unit of measure, like pcs, kg, liter or box.

**Customer**:
Who a company sells to: an outside business or a group company.

**Vendor**:
Who a company buys from: an outside business or a group company.
_Avoid_: supplier (except in the name of Pandu's file, the supplier list)

**PIC**:
Person in charge: the one contact person at a customer or vendor. Can have several phone numbers and emails.
_Avoid_: contact person

**NPWP**:
Indonesian tax ID number, 15 digits (16 since 2024).

## Selling

**RFQ**:
A customer asking for a price. Handled by hand, outside the ERP.

**Quote**:
Our price offer to a customer, made by hand and recorded in the ERP with its item lines.
_Avoid_: quotation (fine in speech, but write quote), penawaran

**Minimum margin**:
The lowest margin a quote line may have without a manager's approval.
_Avoid_: floor price

**COGM**:
Cost of goods material: all the materials that make a product (chemical, packaging, sticker, cap). In this project it does not mean "cost of goods manufactured". Model in [[Finance Accounting Tax Module#Costing and pricing]].

**MAP**:
Moving average price, the costing method. Each material's price is its moving average; together they build the COGM. Procurement sees it.

**DL**:
Direct labor: what the people making the product cost per month.

**FOH**:
Factory overhead: electricity, water, rent and so on, per month. In Marketing's way DL is grouped into it as one standard rate.

**COGS**:
Cost of goods sold = COGM + DL + FOH. Also called HPP.
_Avoid_: HPP (fine in speech)

**VA**:
Value added = revenue - COGM. Finance's way to reach the GP: VA - DL - FOH = GP.

**GP**:
Gross profit, the margin: (selling price - COGS) / selling price.
_Avoid_: markup (that is on cost, not on the price)

**SPPR**:
Selling price publish rate = COGS + GP. The price Sales sees and quotes from. HET on the whiteboard.
_Avoid_: publish rate (alone), HET

**PO cust**:
The customer's purchase order, their official commitment to buy.
_Avoid_: PO (alone, it also means our own purchase order)

**SO**:
Sales order. Our record of a confirmed order, built from the PO cust. Everything after it runs off it.

**DO / BAST**:
One document: the delivery order plus the handover minutes the customer signs (Berita Acara Serah Terima).

**PI**:
Proforma invoice. A payment request sent before delivery, used in CBD.

**Tempo**:
Payment on credit terms: we deliver, invoice after the BAST is signed, and the customer pays by the due date.

**CBD**:
Cash before delivery: the customer pays the PI, then we deliver.

**QC**:
Quality check on returned or reworked goods.

**Kirim**:
Sending goods back out after rework passes QC, through a new DO / BAST.

## Finance

**Chart of accounts**:
The list of a company's accounts, each with a code like `1-1300` and a type (asset, liability, equity, revenue, expense). The same codes in every company.

**Journal**:
One booking in the ledger: lines on accounts, debit equals credit. Never changed once posted; a correction is a reversal journal.
_Avoid_: entry (alone), posting (when you mean the record)

**Account mapping**:
Which account a business step uses for a job (AR, sales revenue, PPN out...), set per company.

**AR**:
Accounts receivable: what customers owe on posted invoices.

**AP**:
Accounts payable: what we owe vendors on posted vendor invoices.

**Customer deposit**:
Money a customer paid before an invoice exists (a PI payment). It is owed back until an invoice uses it up.
_Avoid_: down payment (that is the PI kind)

**Invoice**:
The customer's bill, posted when a BAST is signed. Not the PI.
_Avoid_: proforma (that is the PI)

**Receipt**:
Money received from a customer on one or more invoices.

**Credit note**:
A document that lowers what a customer owes, from a return or a price correction.

**Faktur Pajak**:
The tax invoice for PPN, numbered through the tax office system (Coretax).

**DPP**:
Dasar Pengenaan Pajak, the base PPN is worked out on. Can be a fraction of the price, like 11/12.

**Period**:
One month of the books. A closed period takes no journal.

**Reversal**:
A journal with debit and credit swapped that cancels an earlier one. Done by a Manager.

## Open terms
- Is the colleague's "Surat Jalan" the same document as our DO / BAST?
