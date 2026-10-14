---
date: 2026-10-14
tags: [meeting, internship, whiteboard]
project: "[[Holding ERP]]"
---

# Whiteboard: costing and pricing

Back to [[Meetings]]. Photo shared by Jingga on 2026-10-14. Who wrote it and when is not known.

![[2026-10-14 whiteboard costing and pricing.jpg]]

## What it says (Claude's reading)
- **COGS, COGM, HPP** grouped together: all names for the cost of a product. COGM (cost of goods manufactured) is what making it cost, COGS (cost of goods sold) is the cost of what was sold, HPP (harga pokok penjualan) is the Indonesian name for COGS
- **HPP is made of:** main raw material (bahan baku utama), packaging (kemasan), and looks (estetika: sticker, cap and so on). An arrow points to **"15 to 22"**, meaning unclear (a target GP %?)
- **GP (%) = (selling price - COGS) / selling price x 100 %.** Gross profit on the selling price, not markup on cost
- **Cost methods:** "VA" *(unclear)*, moving average price, standard price
- **BoM** (bill of materials), for production
- **Publish rate = HET** (harga eceran tertinggi, the highest retail price), with branches to **Dis A Surabaya** and **Dis B Bandung** *(reading unsure)*: a published price per distributor or region
- Left side is cut off: "penyelesaian", "design", a "keterangan" box, "diperiksa oleh" (checked by) and "Adit". Looks like a document layout with sign off boxes

## Where it went
- GP formula: minimum margin in [[Sales and Marketing Module]]
- Cost method: open question in [[Sales and Marketing Module]]. Jingga reads "15 to 22" as nothing important
- HET and distributor prices: not in any spec yet. Pricing (price lists) is outside the ERP today, see [[Sales and Marketing Module]]
- BoM: [[Production Module]]

## Second whiteboard (Jingga, 2026-10-14)

![[2026-10-14 whiteboard costing 2.jpg]]

Claude's reading:
- COGM = cost of goods material = C + P + E (chemical, packaging, looks) and so on. COGS = COGM + DL + FOH. SPPR = COGS + margin (GP)
- **Marketing's way:** standard rate FOH as a %: SPPR = COGM + ...% FOH + ...% margin. Example: **COGS = COGM + 15 %**
- **Example on the right:** COGM x 115 % = COGS, GP 40 %, so **SPPR = COGS / 60 %**. Confirms GP on the price
- **Finance's way:** revenue - COGM = VA, VA - DL - FOH = GP. DL and FOH per month (operator Rp 5.000.000, factory SPV Rp 7.000.000, electricity, water, rent): DL and FOH = Rp 100 jt a month, split per PC (100 jt / 5.000) or per L (100 jt / 4.000), or per batch, per month, per machine hour
- So the ERP keeps one company **FOH rate in %** of COGM, worked out by Finance from the monthly split *(Claude's reading, ask supervisor)*
