# Build a Golem — V7 Skill Revolution · รายงาน

ไฟล์ส่งมอบ: `Build_a_Golem_V7_Skill_Revolution.html` (เกมเต็มเวอร์ชันเดียว มี V6.2 Underdeep และ V7 Endless Grinding ครบ)
โมดูลใหม่: `v11tree.js` (ข้อมูล canonical) · `v11a.js` (Core engine) · `v11b.js` (Combat Library) · `v11c.js` (Mining Library) · `v11d.js` (Runtime / Core Active / Evolution / Core EXP) · `v11e.js` (UI)

## 1. สิ่งที่ทำเสร็จ

**Passive Tree (canonical)**
- นำเข้า 240 node / 342 link จากไฟล์ `Build_a_Golem_V7_Skill_Tree_Active_Evolution_240.html` ตรงตามต้นฉบับ ไม่ได้แต่ง node ขึ้นเอง
  - แบ่งตามกลุ่ม: W54 · M54 · A54 · N42 · U24 · H12
  - แบ่งตามชนิด: root 1 · small 141 · notable 50 · evolution 24 · active 12 · keystone 12
- ตรวจกราฟทุกครั้งที่โหลด: ทุก node ต้องเชื่อมถึง U0 ได้ และไม่มี link ซ้ำหรือ link เสีย
- Core Lv สูงสุด 70 · แต้มรวม 75 (จากเลเวล 69 + Milestone 6) · Keystone ไม่เกิน 3
- การซื้อแต้มใช้ BFS แบบเดียวกับ prototype และไม่ให้เส้นทางผ่าน Keystone ตัวที่ 4
- Refund ทำไม่ได้ถ้าจะทำให้ node อื่นขาดการเชื่อมต่อ และทำได้เฉพาะใน Golem Camp หรือ Outpost (Respec ทั้งต้นก็เช่นกัน)
- Passive Effect Registry ครอบคลุมครบ 240 id
  - ค่าสถานะอ่านจากคำอธิบายของ node
  - Notable แบบมีเงื่อนไข, Hybrid และ Keystone ใช้ตารางที่ใส่ตัวเลขจากคำอธิบายต้นฉบับ
- Notable แบบ "Reinforcement" ลด drawback ของ Keystone ตระกูลเดียวกันลง 25% ต่อจุด (ลดได้สูงสุด 50%)

**ระบบค่าพลังกลาง** (ลำดับคำนวณ: Base → Equipment+Quality → Set → Core Lv → Passive → Caps → ค่าที่ได้จากสกิล)
- ใช้ร่วมกันทั้ง Combat และ Mining
- เพดาน:
  - Crit 55% · DR 60% · CDR 35%
  - Passive: AS +30 · Move +25 · Mining Speed +25 · Mining Power +12%
  - ดาเมจ Passive ต่อ hit รวมแบบบวกกัน ไม่เกิน +40%

**ลบ Mastery stacking**
- Part Mastery, Specialization, Discipline, Activity Mastery และ GM path ไม่ให้ค่าพลังอีกแล้ว
- ข้อมูลเดิมเก็บถาวรไว้ใน `S.sr.archive`
- ปุ่ม Mastery เดิมพาไปที่ Core Tree หรือหน้า archive แบบอ่านอย่างเดียว

**Core EXP** มาจากกิจกรรมจริง และนับวัตถุแต่ละชิ้นได้ครั้งเดียว:

| กิจกรรม | Core EXP |
|---|---|
| ทำลายแร่ | 2 + 2×tier |
| มอนสเตอร์ | 15×tier (elite ×3, rare ×10) |
| บอส | 400–1000 |
| Raid | 250 |
| สำรวจพื้นที่ | 0.2 ต่อช่อง |
| Landmark | 150 |
| Alcove | 120 |
| Discovery / Puzzle | 80 |
| Machine | 60 |
| คราฟต์ | 20 ต่อชิ้น |
| Refine | 10 ต่อครั้ง |
| Contract | 120 |
| Bounty | 400 |
| Relic | 200 |
| Training dummy | 0 |

**Equipment Skill Library — 324 ตัวเลือก**
- Combat 240 = 24 ตระกูล × 5 ช่อง × 2
  - เป็นสกิลเดิม 108 ตัว (A) และสกิลใหม่ 132 ตัว
  - Warrior แขนขวาใหม่ทั้ง A/B เพราะ Warrior เดิมใช้นิยาม GSK เดียวกันทั้งสองมือ ถ้านับซ้ำจะกลายเป็น alias
- Mining 72 = 12 ตระกูล × (แขนซ้าย 3 + แขนขวา 3) เลือกแยกกันได้แต่ละแขน
  - เป็นสกิลเดิม 12 ตัว (แขนซ้าย A) และสกิลใหม่ 60 ตัว
- Core Active 12 ตัว
- โหลดเกมแล้วระบบตรวจทันที ผลคือไม่มี behaviour signature (shape + rider) ซ้ำ ไม่มีชื่อซ้ำ และทุกช่องมีจำนวนตัวเลือกครบ
- คำอธิบายสกิลสร้างจากนิยามที่ใช้รันจริง ข้อความจึงตรงกับพฤติกรรม
- ทุก hit วิ่งผ่าน `hitEnemy` หรือ `damageOre` จริง
  - Mining ทุกสกิลยังติดกฎ Mining Power: ถ้าพลังขุดไม่พอ ดาเมจเป็น 0

**ปลดล็อกสกิล** (ตัวเลือก A และ Mining แขนขวา D เปิดตั้งแต่แรก) — แต่ละตัวเลือกปลดได้ 2 ทาง:

| ช่อง | เงื่อนไขหลัก (t = tier ของตระกูล) |
|---|---|
| แขนซ้าย | Mineral Codex |
| แขนขวา Mage / Assassin | ชนะบอส |
| แขนขวา Warrior B | Contract |
| Chest | ล้ม Rare Guardian |
| ขา | ขุดแร่ Mutated |
| หลัง | Relic Fragment |
| Mining แขนซ้าย B / C | Codex / Excavation Project |
| Mining แขนขวา E / F | Contract / Bounty |

- ทางสำรองที่รับประกันว่าปลดได้: Core Lv ≥ 8t+4
- ปลดแล้วเป็นของถาวร ไม่ผูกกับชิ้นอุปกรณ์

**Core Active 12 ตัว**
- ใช้ปุ่ม F หรือปุ่ม ✦ บน HUD ได้ทั้งในโหมด Combat และ Mining
- ติดตั้งได้โหมดละ 1 ตัว
- Core Active ของ class ต้องสวมชิ้นส่วน class นั้นอย่างน้อย 2 ชิ้น
- มี cooldown (ลดได้ด้วย CDR) และใช้ Energy:
  - Combat: 15–25 Energy
  - Mining: ใช้ Mining Energy (0–100, ฟื้นเอง) Overdrive 36, Scanner/Collector 30
- ยกเลิกได้: Seismic Breaker โดนมึนหรือหลบระหว่างง้าง = ยกเลิก · Counter Stance หลบ = ยกเลิก

**Evolution 24 ตัว**
- ใช้ได้โหมดละไม่เกิน 2 ตัว
- ตรวจ class ของชิ้นที่สวมในช่องนั้น และ tag ของสกิลที่เลือก ถ้าไม่ผ่านจะไม่ให้ผลเลย

**UI**
- Passive Tree เต็มจอ: ซูมด้วยล้อเมาส์หรือบีบนิ้ว, ลากเพื่อเลื่อน, hover/tap ดูรายละเอียด, เส้นประทองแสดงเส้นทาง, allocate, refund (คลิกขวาหรือปุ่ม), respec พร้อมยืนยัน, fit
- แผงข้าง: Core Active, Evolution, Milestone, Build Presets 3 ช่อง (สลับได้เฉพาะในพื้นที่ปลอดภัย) และสรุป Passive พร้อมเพดาน
- Skill Library เข้าได้จาก Golem pane และจาก Tree
  - แท็บ: ชิ้นที่สวม / แขนขุด / ดูทุกตระกูล / Core Active
  - แสดง tag, Energy, cooldown, เงื่อนไขปลดล็อก และความเข้ากันได้กับ Evolution

**Save** ใช้ namespace `S.sr` (migrationVersion 80)
- ก่อน migrate มี backup ที่ `buildAGolem_backup_pre_skillrev`
- แปลง Mastery เดิมครั้งเดียว: EXP ที่ได้มาจริงตามสูตรเลเวลเดิม แปลงเป็น Core EXP 1:1 แต่ไม่เกินระดับ Core Lv 35
- ตรวจ id ทุกตัวตอนโหลด: id ที่ไม่รู้จักถูกตัดทิ้ง ถ้าการจัดสรรไม่เชื่อมต่อจะ reset ส่วน skill id ที่ไม่รู้จักจะกลับไปใช้ค่าเริ่มต้น

## 2. จำนวนสกิลที่ทำเสร็จจริง

| หมวด | เป้า | เสร็จจริง | หมายเหตุ |
|---|---|---|---|
| Combat | 240 | 240 | เดิม 108 + ใหม่ 132 · เลือกและร่ายผ่าน input จริงได้ทุกตัว (T11) |
| Mining | 72 | 72 | เดิม 12 + ใหม่ 60 · ร่ายได้จริงทุกตัว และกฎพลังขุดผ่านทั้ง 72 (T14–T15) |
| Core Active | 12 | 12 | T16–T24 |
| Evolution | 24 | 24 | T25–T34 |
| Passive node | 240 | 240 | T1–T10, T35–T39 |

ข้อควรทราบอย่างตรงไปตรงมา: สกิลใหม่ 192 ตัวสร้างจากเอนจินเดียว คือ "รูปแบบการโจมตี (shape) + ผลเสริม (rider)"
- Combat มี shape 28 แบบ, mobility 10 โหมด, utility 15 ชนิด · Mining มี shape 24 แบบ
- ไม่มีสองตัวไหนใช้ shape + rider ชุดเดียวกัน
- แต่ภาพเอฟเฟกต์ใช้ชุด FX ที่มีอยู่แล้วในเกม (slash/beam/nova/quake/rift/ring ฯลฯ) ลงสีตามตระกูล **ไม่ได้วาดแอนิเมชันเฉพาะให้แต่ละสกิล**

## 3. ความคลาดเคลื่อนของข้อมูลต้นฉบับ

1. Attack Speed cap: ข้อมูลระบุ 35 แต่ prompt ระบุ 30 → ใช้ **30**
2. N6 Titan Drill ให้ Mining Power +14% เกินเพดาน Passive 12% → ผลรวมถูก clamp ที่ 12%
3. `adjustedNodeCount` = 235 ไม่ตรงกับ 240 → ยึดจำนวน node จริงคือ 240
4. Notable ที่ซ้ำกัน (W15=W17, W33=W35, W51=W53, M15/17, M33/35, M51/53, A15/17, A33/35, A51/53) → มีครบสองจุด internal cooldown ลด 25% (ไม่ได้ทำให้ผลซ้อน)
5. `balance.status` = "design/prototype only" และทุก node ใช้ 1 แต้ม → ใช้ตามข้อมูล
6. สเตตที่ต้องตีความเป็นกลไกในเกม:
   - Maximum Energy → ลดค่า Energy ของ Ultimate
   - Mining Skill Energy Efficiency → ลด cooldown สกิลขุด และค่า Energy ของ Core Active โหมดขุด
   - Ore Discovery → โอกาสต่อวินาทีที่จะเจอแร่ซ่อนในรัศมี 3
   - Multi-Mine → การขุดปกติมีโอกาสโดนแร่ข้างเคียงด้วย 50% (ตรวจพลังขุดซ้ำ)
   - Hazard Resistance → ลดเวลามึนในการขุด
   - Parry Window → ยืดหน้าต่าง Guard
   - Gravity Effect → ดาเมจต่อเป้าที่ติด Gravity
   - Core Lv → base HP/ATK/DEF +1% ต่อเลเวล (แทน Mastery multiplier)
7. M13 / A13 (ลดพื้นที่ Ultimate): ส่วนดาเมจทำงานจริง แต่**ไม่ได้ลดรัศมีของ Ultimate เดิม** เพราะท่าเดิมฝังรัศมีไว้ในโค้ด
8. N20 "Rare Ore +6%" ใช้ผ่านช่องทาง Ore Discovery (เปิดเผยแร่ซ่อน) ไม่ได้เพิ่มอัตราเกิดแร่หายาก

## 4. Save Migration
- ทดสอบจาก save ของ V7 Grinding จริงที่ตั้ง Mastery Lv22 ไว้ 4 ชิ้น + Discipline 12:
  - ผลแปลง: legacyExp 25,180 → Core Lv 13
  - Coins / Scrap / inventory / Underdeep / v10 stats / ore storage ตรงกับก่อน migrate ทุกค่า
- โหลดซ้ำแล้ว EXP, เลเวล และแต้มไม่เพิ่ม
- save ที่เสียหาย (id ไม่รู้จัก หรือการจัดสรรไม่เชื่อมต่อ) ถูก reset อย่างปลอดภัย

## 5. การทดสอบที่ทำจริง (Playwright + swiftshader)

**ชุดใหม่ T1–T57 ผ่าน 57/57**

| ไฟล์ | ผล | สิ่งที่ทดสอบ |
|---|---|---|
| t11a | 10/10 | กราฟ / allocation / refund / Keystone / 75 แต้ม / respec / caps |
| t11b | 3/3 | Combat 240 ตัวเลือก: ทุกตัวใช้ id ที่เลือก, cooldown/Energy ถูกต้อง, เกิด hit หรือผลจริง · สกิลดาเมจใหม่ 84 ตัวสร้างดาเมจจริงทั้งหมด · mobility ใหม่เคลื่อนที่จริง |
| t11c | 2/2 | Mining 72 ตัวเลือก + กฎพลังขุด (ดาเมจต่อแร่ที่พลังไม่พอ = 0 ทั้ง 72) |
| t11d | 9/9 | Core Active 12 ตัว รวมกฎ class, Energy, ปุ่ม F และการยกเลิก |
| t11e | 10/10 | Evolution 24 ตัว ทั้งกรณีเข้ากันได้และเข้ากันไม่ได้ · จำกัด 2 ตัวต่อโหมด |
| t11f | 6/6 | caps ภายใต้ input สุดโต่ง · Keystone 12 ตัว · notable แบบมีเงื่อนไข · hybrid · ไม่มี proc chain ไม่รู้จบ (ต่อสู้บอส 90 วินาที × 3 build) |
| t11g | 7/7 | migration / idempotent / save เสีย / Core EXP กันนับซ้ำ / milestone / unlock / preset |
| t11h | 3/3 | **build 7 แบบ** (W, M, A, W+M, W+A, M+A, ทั้งสาม) ต่อสู้ Warlord ×3 + Underdeep, ใช้ Core Active จริง · Underdeep integration · บั๊ก crash เมื่อบอสตายจาก reflect |
| t11i | 13/13 | UI desktop 1366×800 และมือถือ 390×844: tree, คลิก/tap, hover, wheel, drag, pinch, allocate, Library, ปุ่ม HUD |

**Regression** (ชุดทดสอบของเวอร์ชันก่อน รันกับ build นี้)

| ชุด | baseline ก่อนอัปเดต | ผลตอนนี้ | ข้อที่ไม่ผ่านเพิ่มจาก baseline |
|---|---|---|---|
| t62 (V6.2 Underdeep) | 49/49 | 48/49 | X2 |
| t7g (V7 Grinding) | 25/25 | 24/25 | 11 |
| t6b (V6 camp) | 41/42 | 41/42 | ไม่มี — ตัวที่ไม่ผ่านเป็นตัวเดิม (23b tutorial) |
| t51b (V5.1) | 46/46 | 45/46 | 27 "Permanent Mastery grows" |
| t5b (V5) | 57/60 | 51/60 | 11, 14, 14b, 15, 16, 18 — ทุกข้อทดสอบ Mastery/GM |
| t7 (V6.1 classes) | 33/34 | 30/34 | M4, 16, 17 — Discipline / Part Specialization |
| t4 (V4) | 65/72 | 65/72 | ไม่มี — ข้อที่ไม่ผ่าน 7 ข้อเป็นชุดเดิมทั้งหมด |

- ข้อที่ไม่ผ่านเพิ่มทุกข้อเป็นการทดสอบระบบ Mastery ที่ prompt สั่งให้ถอดออก
- t5b และ t7 ฉบับเดิมหยุดกลางทาง (crash) เพราะไปอ่านข้อมูล Mastery ที่ไม่ถูกสร้างแล้ว
  - จึงทำสำเนา `t5b_sr.py` และ `t7_sr.py` ที่แก้แค่การอ่านค่า null เพื่อให้ชุดทดสอบที่เหลือรันจนจบ
- X2 (t62) และ 11 (t7g) เป็นการต่อสู้ที่ตั้ง Mastery Lv20–30 ไว้
  - เมื่อแทนด้วย Core Level ที่เทียบเท่า (Mastery 30 → Core Lv 70, Mastery 20 → Lv 47 พร้อมจัดสรรแต้ม) ชนะครบทุกไฟต์ (6 ไฟต์ × 3 รอบ = 18/18)
  - เวลาใกล้เคียง build เก่า เช่น Horizon Eater 143–150 วิ เทียบกับเดิม 149–154 วิ

**Build 7 แบบ (T48)** — Core Lv 35, ชุด T4 (Magnetite / Elemental / Phantom), ต่อสู้ Warlord ×3 + Underdeep 3 ตัว (T3)

| Build | CP | แต้มที่ใช้ | Core Active | Warlord | ใช้ Core Active | Underdeep |
|---|---|---|---|---|---|---|
| Warrior | 606 | 18 | W6 | 3/3 (273 วิ) | 24 | ชนะ 111 วิ, HP 100% |
| Mage | 601 | 15 | M6 | 3/3 (104 วิ) | 12 | ชนะ 27 วิ, HP 91% |
| Assassin | 606 | 16 | A24 | 2/3 (123 วิ) | 12 | ชนะ 58 วิ, HP 40% |
| Warrior+Mage | 611 | 18 | W6 | 3/3 (197 วิ) | 17 | ชนะ 60 วิ, HP 100% |
| Warrior+Assassin | 640 | 24 | A6 | 3/3 (144 วิ) | 17 | ชนะ 71 วิ, HP 100% |
| Mage+Assassin | 646 | 22 | M24 | 3/3 (95 วิ) | 9 | ชนะ 52 วิ, HP 30% |
| ทั้งสาม class | 652 | 23 | W6 | 3/3 (128 วิ) | 12 | ชนะ 60 วิ, HP 45% |

**ผลกระทบที่ต้องรู้จากการลบ Mastery**
- t5b ข้อ 15/16/18 เคยยืนยันว่าชิ้นส่วน Common ที่ถึง GM สู้ Legendary ได้ ตอนนี้เงื่อนไขนั้นไม่มีแล้ว
- ชิ้น rarity ต่ำในโหมดขุดจึงอ่อนกว่าเดิมเมื่อเทียบกับผู้เล่นที่เคยเลี้ยง GM
- Passive Tree ช่วยชดเชยได้เพียงบางส่วน (Mining Power สูงสุด +12% และ Core Lv ให้ +1% ต่อเลเวล)

**สมดุลหลังลบ Mastery** (บอท 4 ไฟต์ต่อชุด, ผลแพ้ชนะต่อบอส)
- สรุป: New Lv1 ≈ Old Mastery 0 · New Lv35 ≈ Old Mastery 15 · New Lv70 ≈ Old Mastery 30
- ข้อยกเว้น: ชุด tier ต่ำ (เช่น Crystal T2, Void ต่อ Tyrant) อ่อนกว่าเดิมเล็กน้อยที่ระดับสูงสุด เพราะ Mastery เดิมคูณค่าชิ้นได้ถึง ×2.0

## 6. บั๊กที่ทราบ / งานที่ยังไม่ครบ
- สกิลใหม่ใช้ชุด FX ที่มีอยู่ ไม่มีแอนิเมชันเฉพาะรายสกิล
- M13 / A13 ไม่ได้ลดรัศมีของ Ultimate เดิม (ข้อ 3.7)
- Smoke Break / Shadow Pivot / Mirror Decoy ใช้ระบบล่อเป้าของ V6.1:
  - บอสถูกล่อทุกกรณี
  - ศัตรูทั่วไปถูกล่อเฉพาะตอนใช้ Smoke Break
- Build Preset ยังไม่ได้บันทึกชุดอุปกรณ์ (เป็นข้อเลือกได้ใน prompt)
- Tyrant ยังเอาชนะยากมากสำหรับ build ส่วนใหญ่ ซึ่งเป็นสมดุลเดิมที่มีอยู่ก่อน ไม่ได้เปลี่ยน
- ยังไม่ได้ทดสอบบนอุปกรณ์มือถือจริง (ทดสอบผ่าน emulation ใน Playwright เท่านั้น)
