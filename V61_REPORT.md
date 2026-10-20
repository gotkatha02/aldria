# Build a Golem V6.1 — Three Combat Paths (Warrior / Mage / Assassin)

ต่อยอดจากไฟล์ V6.0 (Living World & Golem Camp) — ไม่ได้สร้างเกมใหม่ ไม่ได้ลบระบบเดิม เซฟเดิมโหลดได้ (`buildAGolem_save_v1`)

## โครงสร้างโค้ดที่เพิ่ม (5 โมดูล ต่อท้ายระบบเดิม)
| ไฟล์ | หน้าที่ |
|---|---|
| v7data.js | Class Metadata, Registry `CFAM` (แยกจาก Mining `FAMS`), 60 ไอเทมใหม่, 72 ความสามารถ, Set Bonus, วัตถุดิบ 12 ชนิด, สูตรคราฟต์, สูตรแปรรูป, Mastery paths |
| v7state.js | Save defaults + Migration (ทำครั้งเดียว), ยกเลิก Enhancement + ชดเชย, ระบบวัตถุดิบ (haveMat/payMat/matRows), Direct Crafting, แหล่งดรอป, calcCombat post (สกิลซ้าย/ขวา, Spec, Discipline) |
| v7cmb.js | Combat engine: กระสุนจริงมี Collision, กับดัก/วงเวท/ป้อม/Guardian/Satellite/ประตูมิติ/ร่างเงา, Poison/Mark/Elemental Reaction, Stealth/Decoy, Dash Charge, Momentum, Afterimage, ท่าใหม่ใน poseC, HUD |
| v7art.js | กราฟิก 12 ตระกูล × 6 ชิ้น (helm/chest/gaU/gaF/legs/back) บน Rig เดิม (`drawArmored → drawArmorPiece → AR`) + ไอคอน + texture |
| v7ui.js | Blacksmith: แท็บ Warrior · Mage · Assassin · Refinery · Materials · Mastery, Preview Character หมุน/เล่นแอนิเมชันได้, หน้าจอ Enhancement → Mastery |

## สิ่งที่ทำเสร็จและทดสอบจริง (ชุดทดสอบ V6.1: 33/33 ผ่าน)
1. **Warrior 12 ตระกูลเดิม** — ID เดิมทุกตัว + `cls:'warrior'`
2. **Mage 6 เซ็ต / Assassin 6 เซ็ต** = 60 definitions ใหม่ (รวม 120) ไม่ได้ใส่ใน Mining registry
3. **72 ความสามารถ** ใช้ในการต่อสู้จริง — ทดสอบทุกตระกูล: สกิลซ้าย, สกิลขวา, Ultimate, Back, Dodge ถูกใช้และสร้างดาเมจผ่าน `hitEnemy()` จริง (รวมกลไก: กับดักระเบิด, Elemental Reaction, Execution, Backstab, Finisher, Shadow Pierce, Toxic Bloom, Poison DoT, กระสุน Void/Portal)
4. **ถุงมือชิ้นเดียว สกิลต่างตามมือ** (ซ้าย = Active 1, ขวา = Active 2 + Basic Attack)
5. **ผสมข้ามสายได้** — Build A ตามเอกสาร 10.3 ได้ Rune Analysis / Titan Fortress / Rune Trap / Mantis Strike / Twin Mantis Blade / Triple Dash / Crystal Satellites และชนะ Warlord ได้
6. **Set Bonus 2/4/6** นับจากชิ้นที่ใส่จริง (2 Runic + 4 Velocity → runic2, velocity2, velocity4)
7. **กราฟิกใหม่ทุกชิ้น** silhouette ต่างกันชัด ไม่ใช่การเปลี่ยนสี Warrior; ท่า Cast/Channel/Dual-blade/Slash/Stab/Dash/Leap/Blink + Afterimage, ปีกใบมีดกาง/หุบ, วงแหวน Reactor หมุน
8. **วัตถุดิบ 12 ชนิด** เก็บแยกในคลัง + แหล่ง: แร่ตาม Tier (มีระบบ Pity การันตี), บอสทุกครั้งที่ชนะ, หีบ V5.1, Refinery 10 สูตร (ไม่มีวงจรปิด)
9. **T1 ทั้งสองสายคราฟต์ได้จากพื้นที่เริ่มต้น** (Stone/Copper + ดรอปจากแร่ Tier 1–2)
10. **Direct Crafting** ตามตารางเอกสาร × ตัวคูณ (ปัดขึ้น) — ไม่ต้องใช้ชุด Tier ต่ำ ไม่ต้อง Blueprint
11. **ยกเลิก Enhancement +1..+10** — สำรองค่าเดิม (`S.v7.enhBackup`), ชดเชยวัสดุ/เหรียญเต็มจำนวนครั้งเดียว (มีบันทึกตรวจสอบได้), โหลดซ้ำไม่จ่ายซ้ำ, `doEnhance` ไม่เปลี่ยนค่าอีก, ป้าย “+N” ทั้งหมดถูกเอาออก
12. **Mastery ระยะยาว** — Part Specialization Lv.120 (ผูกกับแบบไอเทม), Discipline Mastery Lv.100 ต่อสาย, เลือกเส้นทาง 1 ใน 3 ที่ Lv.40 เปลี่ยนได้ไม่เสีย EXP; Warrior Discipline ตั้งต้นจาก Mastery เดิม
13. **Mining 6.2** — ทุกแร่ทดลองตีได้, ไม่มีรูปกุญแจ/ไม่แสดง Power ที่ต้องการ/ไม่มีคำเตือน, พลังไม่พอ HP แร่ไม่ลดและไม่มีรางวัล, Auto Mining ไม่วนตีแร่ที่ตีไม่ได้
14. **Save** — เซฟ V6.0 เก่าโหลดได้ (UID/ไอเทม/Equip/Mastery/Pet/Camp เหมือนเดิม) + สำรอง `buildAGolem_backup_pre_v61`
15. **UI Desktop + Mobile (390px)** ไม่มี horizontal scroll

## ผลจำลองบอส (บอท, 3 รอบต่อ build, Mastery 20, Fine)
| Build | Warlord |
|---|---|
| Full Warrior (Thunder) | 3/3 (เร็วสุด ~75 วิ) |
| Full Mage (Elemental) | 3/3 |
| Full Assassin (Phantom) | 2/3 |
| Warrior+Mage | 3/3 |
| Warrior+Assassin | 3/3 |
| Mage+Assassin | 3/3 |
| Warrior+Mage+Assassin (Build A) | 3/3 |

Void Tyrant (Mastery 30, Fine): ทุกสายชนะได้ใน Tier 4–6 (Spectral/Nightreaper 2/3)

## ข้อจำกัดที่ต้องรายงานตามจริง
- **Milestone Lv.40–120 ของ Part Specialization** เป็นการเพิ่มพลัง/ลดคูลดาวน์/เพิ่มระยะเวลาแบบทั่วไป ยังไม่ใช่ท่าใหม่เฉพาะของแต่ละสกิล
- **เส้นทาง Discipline** มีผลระดับสาย (ทำงานเมื่อใส่ชิ้นสายนั้น ≥ 2 ชิ้น) ไม่ใช่ Skill Map แยกต่อชิ้น
- เกมไม่มีมอนสเตอร์ทั่วไปในเหมือง — แหล่ง “มอนสเตอร์พิษ/เวท/เงา” จึงใช้แร่ตาม Tier + บอส + หีบ + Refinery แทน
- Renderer เดิมรองรับมุมหน้า/เฉียงข้าง + หันกลับซ้ายขวา — ไม่มีมุมหลังจริง
- ร่างล่อ (Smoke/Cloak/Afterimage) หลอกได้เฉพาะตัวบอส ไม่รวมลูกน้องบอส
- Mining Part Upgrade Lv.1–10 (ฝั่งขุด) ยังอยู่ — การยกเลิกตีบวกใช้กับ Combat Armor ตามเอกสาร
- Balance ทดสอบด้วยบอทจำลอง ยังไม่ผ่าน playtest จากผู้เล่นจริง; Arcanist (Construct) ยังแรงกว่าเฉลี่ยเล็กน้อย
