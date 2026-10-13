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

**Super Admin**:
The holding level account that can see and change everything in every company.
_Avoid_: super user (when you mean this one)

**Super User**:
The holding level account that can see everything in every company and change nothing.
_Avoid_: viewer (that is a company role)

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

**Role**:
A named set of permissions inside one company. For now one of: company admin, director, manager, supervisor, staff, viewer *(assumed)*.

**Permission**:
One action on one record type, like "create item" or "view customer".

**Access**:
A user holding a role inside a company. One user can have access to several companies.

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
_Avoid_: supplier

## Selling

**RFQ**:
A customer asking for a price. Handled by hand, outside the ERP.

**Quote**:
Our price offer to a customer, made by hand and recorded in the ERP with its item lines.
_Avoid_: quotation (fine in speech, but write quote), penawaran

**Minimum margin**:
The lowest margin a quote line may have without a manager's approval.
_Avoid_: floor price

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

## Open terms
- Is the colleague's "Surat Jalan" the same document as our DO / BAST?
