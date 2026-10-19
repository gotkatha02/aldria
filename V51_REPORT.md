# BUILD A GOLEM — V5.1 “INFINITE DUNGEON” · รายงานการพัฒนา

> ต่อยอดจาก V5.0 (ไม่ได้สร้างใหม่) · Save key เดิม · ทดสอบด้วย Engine จริง (Playwright) ทุกระบบ

## สรุปสิ่งที่ทำ (ทุก Phase)
| Phase | สถานะ | สิ่งที่ทำจริง |
|---|---|---|
| 1 · ไม่มีเวลา + Instant Exit | ✅ | Run Rules แยกชัดเจน (`R.timed=false`) — Normal / Standard / Deep / Endless ไม่นับถอยหลัง ไม่จบเพราะเวลา ไม่มีกติกา 70% · แสดง **⏱ Time Explored** จาก `R.elapsed` (หยุดเมื่อ Pause/เปิดหน้าต่าง) · ปุ่ม **🏠 Return to Workshop** → หน้ายืนยัน (แร่, Coins คาดการณ์, ห้อง, %, วัตถุดิบพิเศษ, สถานะกล่อง, Mastery/Pet EXP) → กลับทันทีจากทุกจุด · กันกดซ้ำ · Mining Boss ยังคงเวลา 180 วิ (กลไกบอส) |
| 2 · ขนาดแผนที่แบบไดนามิก | ✅ | `W/H` เปลี่ยนเป็นค่าของแผนที่ (map.w/h, `setDims`) · Standard **80×48** · Deep **96×64** · Endless 80×48 (B1–40) / 96×64 (B41+) · Mining Boss / Sim เดิมยังเป็น 22×14 · TypedArray ใช้ Int32 (ไม่ Overflow) |
| 3 · ห้อง + กล้อง + Minimap + Fog | ✅ | Generator ใหม่: 10–18 ห้อง (Deep 16–25) เชื่อมด้วย MST + Loop ทางเดินกว้าง 2 ช่อง · 12 ประเภทห้อง · ห้องลับเข้าทางผนังลับ · กล้อง Smooth Follow ไม่หลุดขอบ · Minimap (ห้อง, สถานะ, จุดเหตุการณ์, ห้องสมบัติ, แร่ที่ Pet/Relic ตรวจพบ) · แผนที่ใหญ่ (ปุ่ม 🗺 / M) แตะห้องเพื่อเดินไป · Fog of War 3 สถานะ (ยังไม่สำรวจ / สำรวจแล้ว / มองเห็นอยู่) |
| 4 · Auto Explore + Pathfinding | ✅ | Dijkstra แบบ Binary Heap (≈1 ms บน 96×64) · Auto Explore: ตัดสินใจ Event/Relic → ฝากแร่เมื่อกระเป๋าใกล้เต็ม → ขุด → เข้าห้องที่เห็นแต่ยังไม่เข้า → เดินไป Frontier · 4 โหมด: ใกล้สุด / มูลค่าสูง / สำรวจ / เคลียร์ห้อง · หยุดรอเมื่อมีตัวเลือก Roguelite |
| 5 · Clear Detection + Treasure Chest | ✅ | เคลียร์ = ห้องหลักครบ + แร่ตามเป้า (65% ของแร่ที่โกเลม "ขุดถึงได้จริง") + Event บังคับครบ + ภารกิจหลัก (สายแร่หลัก) · ไม่บังคับห้องลับ/แร่ Power ไม่พอ/เก็บแร่ทุกก้อน · ประกาศ **🏆 DUNGEON CLEARED!** อัตโนมัติ · เปิดกล่องได้ทันทีหรือที่ Treasure Chamber · แอนิเมชันกล่อง + Skip |
| 6 · Random Chest + Anti-Dup | ✅ | 5 ระดับกล่อง · การันตี + Bonus Roll จาก Weighted Loot Table (แสดง Drop Rate จริง) · ไม่มีอุปกรณ์สำเร็จรูป · สถานะ unlocked → opened (บันทึกรางวัลที่สุ่ม) → claimed ผูกกับ Run ID · สุ่มแบบ Seed ตาม Run ID (Reload ไม่สุ่มใหม่) |
| 7 · Relics + Events + Difficulty | ✅ | 24 Relics (เฉพาะรอบ) + เพดาน · 7 Events ที่มีการตัดสินใจและผลจริง · 4 ระดับความยาก (ปลดล็อกทีละขั้นต่อดันเจี้ยน) |
| 8 · Interactive Tutorial | ✅ | **GEARLY** (โดรนตาเรืองแสงฟ้า วาดด้วย Canvas) · 12 ขั้นที่ต้องลงมือทำจริง + ไฮไลต์ปุ่ม · Tutorial Mine (40×26) · เริ่มอัตโนมัติกับ Save ใหม่ · ผู้เล่นเก่าไม่ถูกบังคับ · Skip ได้ · เล่นซ้ำจาก ⚙ Settings › Help (รางวัลครั้งเดียว) · คำแนะนำสั้นเมื่อพบระบบใหม่ครั้งแรก |
| 9 · Deep/Endless + Suspend/Resume | ✅ | Deep: แต่ละชั้นเป็นแผนที่ใหญ่ ต้องเคลียร์ก่อนลง + กล่องต่อชั้น · Endless: ผ่านเป้าหมาย → Descend / สำรวจต่อ / Extract · เคลียร์ชั้น = กล่องเพิ่ม · Checkpoint/Milestone เดิมคงอยู่ · **Pause → Continue / Save & Return / Extract & End** · Resume ด้วย Seed + State Diff (ไม่สร้างของซ้ำ) |
| 10 · Balance + Performance + Migration + Tests | ✅ | ดูหัวข้อด้านล่าง |

## ระบบใหม่อื่น ๆ
- **Expedition Storage Station** (ห้องพัก): ฝากแร่จาก Bag ได้ (ความจุ 2× Capacity พื้นฐานต่อ Station) — แร่ยังไม่ถาวรจนกว่าจะกลับ Workshop · Capacity สูง = ต้องฝากน้อยกว่า
- เมื่อกระเป๋าเต็มยัง **ขุดต่อได้** (แร่ตกอยู่บนพื้น) จึงไม่ติดค้างในแผนที่
- **ความลึกของห้อง**: ห้องที่ไกลจากจุดเริ่มต้น (นับตามทางเชื่อม) มีแร่ Tier สูงขึ้นและมูลค่า +สูงสุด 45% · **Clear Bonus** มูลค่าแร่ทั้งรอบ +20% → การสำรวจคุ้มกว่าการเข้า-ออกซ้ำ
- Contract “Speed Miner (12 ก้อนใน 90 วิ)” → **Room Explorer (สำรวจ 3 ห้อง)** · Daily ใหม่ 5 แบบ (สำรวจห้อง, เปิดกล่อง, เลือก Relic, ผ่าน Event, เคลียร์ดันเจี้ยน)
- GM Effect “เพิ่มเวลารอบ” (Spare Boiler / Cryo Core / Rift Core) → เปลี่ยนเป็น **เข้าห้องใหม่ → Mining Speed +p นาน 10 วิ** · Activity Mastery “Deep Exploration” → มูลค่าแร่ Endless/Deep +0.5%/Lv
- Blueprint Fragment/Pet Shard ต่อการ Extract ต้องมี Clear Progress ≥40% (กันฟาร์มเข้า-ออก)

### Relics (24)
| Relic | ผล |
|---|---|
| ⚡ Rapid Drill | Mining Speed +12 |
| 🔨 Heavy Strike | ดาเมจต่อแร่แข็ง (Tier 4+) +15% |
| 🧲 Crystal Magnet | ระยะเก็บแร่ +1.5 ช่อง |
| 🌀 Overclock | ทุบแร่แตก 3 ก้อนภายใน 8 วิ → Mining Speed +25 นาน 6 วิ |
| 💥 Geode Burst | แร่แตก: 25% โอกาสระเบิดใส่แร่รอบข้าง 40% Power |
| 🗝 Treasure Sense | เปิดเผยห้องพิเศษทั้งหมดบนแผนที่ + ทางเข้าห้องลับ |
| 🛡 Stone Shield | กันหินถล่มครั้งถัดไป — ชาร์จใหม่ทุกครั้งที่เข้าห้องใหม่ |
| 🦿 Swift Legs | Movement +15% |
| 🔁 Energy Loop | สกิลทำให้แร่แตก → คูลดาวน์สกิลนั้น −2 วิ |
| 🎯 Mineral Focus | แร่ Tier สูงสุดที่ขุดได้: ดาเมจ +20% · มูลค่า +10% |
| ⚙️ Ancient Engine | พลังสกิลขุด +15% |
| 🎒 Packing Expert | Capacity +20% (รอบนี้) |
| 🍀 Lucky Vein | 8% โอกาสได้แร่เพิ่ม 1 ก้อน |
| 🔍 Appraiser's Lens | มูลค่าแร่ +10% |
| 🐾 Pet Bond Charm | คูลดาวน์สกิล Pet −25% |
| 👁 Hidden Eye | ตรวจจับแร่ซ่อน +2 · มองเห็นในหมอก +1.5 |
| 🥾 Lava Boots | ผลกระทบลาวา/พื้นหน่วง −50% |
| ✦ Critical Pick | 10% โอกาสขุดคริติคอล ×2 |
| 🏃 Momentum | เดินครบ 6 ช่อง → การขุดครั้งถัดไป +50% |
| ↻ Echo Hammer | 12% โอกาสตีซ้ำทันที |
| 📦 Storage Link | Expedition Storage Station รับแร่ได้ +50% |
| 🔺 Glass Cannon | ดาเมจขุด +30% แต่ Movement −15% |
| 🛰 Scout Drone | มองเห็นในหมอก +3 ช่อง · Minimap แสดงแร่ Tier สูงสุด |
| 💨 Second Wind | เข้าห้องใหม่ → คูลดาวน์สกิลรีเซ็ต |

เพดานรวม (Relic + Buff เหตุการณ์): Mining Speed +40 · Movement ±30% · มูลค่าแร่ +40% · Capacity +40% · ดาเมจ +50% · Area 60%

### Random Events (7)
| Event | ตัวเลือก |
|---|---|
| 💠 Crystal Overload | ⛏ ขุดต่อ (เสี่ยง) — คริสตัลมูลค่า ×1.5 เกิดเพิ่ม 6 ก้อน แต่พื้นรอบ ๆ ร้อนเป็นลาวา<br>🔌 ปิดเครื่อง — เลือก Relic 1 จาก 3<br>➡ ข้าม — ไม่มีผล |
| ⚒ Abandoned Forge | 🔥 หลอมแร่ 6 ก้อน — เสียแร่ 6 ก้อน (ราคาต่ำสุดก่อน) → เลือก Relic<br>🔹 หลอมแร่ 3 ก้อน — เสียแร่ 3 ก้อน → Basic Stone ×2<br>➡ ข้าม — ไม่มีผล |
| 🧰 Lost Miner Cache | 🪙 เอาเงิน — Coins (ตามระดับดันเจี้ยน)<br>🔹 เอาวัสดุ — Basic Stone ×2 + Ore Treat ×1<br>🐾 ให้ Pet ค้น — Pet EXP +80 · 30% โอกาส Pet Essence ×2 (ต้องมี Pet) |
| 🖥 Ancient Terminal | 🔓 ปลดล็อกห้องลับ — เปิดทางเข้าห้องลับทั้งหมดในชั้นนี้<br>⚠ Overclock Sector — หินถล่มถี่ขึ้น 60% ตลอดรอบ แต่กล่องสมบัติ +1 Bonus Roll<br>➡ ข้าม — ไม่มีผล |
| 🔮 Mysterious Crystal | ⚡ Power Surge — ดาเมจขุด +20% · Movement −20% (รอบนี้)<br>🪶 Featherweight — Movement +25% · Capacity −15% (รอบนี้)<br>➡ ปฏิเสธ — ไม่มีผล |
| 🐾 Pet Discovery | 🐾 ตามไป — เปิดแร่ซ่อน 3 จุด + มาร์กแร่ดีที่สุด · Pet EXP +40 · 🥚 Pet Shard +1<br>➡ ไม่ตาม — ไม่มีผล |
| ☠ Cursed Room | ☠ รับคำสาป — แร่ต้องคำสาป 5 ก้อน (HP ×2, มูลค่า ×2) · ทุบครบ = 📄2 + กล่อง +1 Bonus Roll<br>➡ หลีกเลี่ยง — ไม่มีผล |

### Difficulty
| ระดับ | ห้อง | Hazard Rooms | Events | Relic Altars | Modifiers | Ore HP | Chest | Bonus Rolls |
|---|---|---|---|---|---|---|---|---|
| 🧭 Explorer | 10–12 (+ห้องลับ) | 0 | 1 | 2 | 0 | ×1 | +0 ระดับ | +0 |
| ⚔ Adventurer | 12–15 (+ห้องลับ) | 1 | 2 | 2 | 1 | ×1.08 | +0 ระดับ | +1 |
| 🔥 Veteran | 14–17 (+ห้องลับ) | 2 | 2 | 3 | 2 | ×1.18 | +1 ระดับ | +2 |
| 💀 Master | 16–18 (+ห้องลับ) | 3 | 3 | 3 | 3 | ×1.3 | +1 ระดับ | +3 |

### Room Types
| ห้อง | แร่ (ความหนาแน่น) | หมายเหตุ |
|---|---|---|
| 🏁 Starting Room | 7% | จุดเริ่มต้น (ปลอดภัย) |
| ⛏ Ore Room | 26% | จุดเริ่มต้น (ปลอดภัย) |
| 💰 Rich Ore Room | 42% | แร่หนาแน่น |
| 🔶 Rare Ore Chamber | 28% | แร่ Tier สูงเกิดบ่อย |
| 💎 Crystal Cavern | 34% | คริสตัลมูลค่าสูง |
| ⚙️ Ancient Machine Room | 18% | ซากเครื่องจักร (Modifier Machine ให้ Scrap) |
| ⚠️ Hazard Room | 24% | ระวังลาวา/พื้นหน่วง |
| 🏕 Rest Area | 0% | มี 📦 Expedition Storage Station ฝากแร่ได้ |
| 🔮 Relic Room | 8% | แท่น Relic — เลือกพลังเสริม 1 จาก 3 |
| ❓ Event Room | 12% | เหตุการณ์พิเศษ — ต้องตัดสินใจ |
| ✨ Secret Room | 55% | ห้องลับ (Optional) |
| 🏆 Treasure Chamber | 0% | ห้องสมบัติ — กล่องเปิดได้เมื่อเคลียร์ดันเจี้ยน |

### Treasure Chests
| กล่อง | การันตี | Bonus Rolls |
|---|---|---|
| Wooden Chest | 🪙400–700 · แร่ 10–16 ก้อน · 📄1 | 1 (+ความยาก +เหตุการณ์) |
| Iron Chest | 🪙900–1400 · แร่ 14–22 ก้อน · 📄2 | 2 (+ความยาก +เหตุการณ์) |
| Crystal Chest | 🪙2000–3000 · แร่ 16–24 ก้อน · 📄3 | 2 (+ความยาก +เหตุการณ์) |
| Ancient Chest | 🪙4000–6000 · แร่ 18–26 ก้อน · 📄4 · ✴️1 | 3 (+ความยาก +เหตุการณ์) |
| Void Chest | 🪙8000–12000 · แร่ 20–30 ก้อน · 📄5 · ✴️1 | 3 (+ความยาก +เหตุการณ์) |

Drop Pool ต่อ 1 Bonus Roll (คำนวณจาก weight จริง):
| รางวัล | จำนวน | Wooden | Iron | Crystal | Ancient | Void |
|---|---|---|---|---|---|---|
| 🔹 Basic Enhancement Stone | 2–4 | 27.0% | 23.3% | 20.7% | 19.2% | 19.2% |
| 🔷 Advanced Enhancement Stone | 1–2 | — | 14.0% | 12.4% | 11.5% | 11.5% |
| 💠 Master Enhancement Stone | 1 | — | — | — | 3.8% | 3.8% |
| 🔮 Rune (สุ่มชนิด, Tier ตามกล่อง) | 1 | 10.8% | 9.3% | 8.3% | 7.7% | 7.7% |
| 📜 Rune Recipe Fragment → Rune Crafting EXP | 60–120 | — | — | 5.5% | 5.1% | 5.1% |
| ✨ Pet Essence | 2–4 | 13.5% | 11.6% | 10.3% | 9.6% | 9.6% |
| 🍪 Ore Treat | 2–3 | 9.0% | 7.8% | 6.9% | 6.4% | 6.4% |
| 🥚 Pet Egg Shard (เป้าหมาย) | 2–3 | 9.0% | 7.8% | 6.9% | 6.4% | 6.4% |
| 💠 Evolution Shards | 2–5 | 10.8% | 9.3% | 8.3% | 7.7% | 7.7% |
| 📄 Blueprint Fragment (Rare) | 2–3 | 10.8% | 9.3% | 8.3% | 7.7% | 7.7% |
| ✴️ Masterwork Essence | 1 | — | — | 5.5% | 5.1% | 5.1% |
| 🎨 Cosmetic Fragment | 1–2 | — | — | — | 3.2% | 3.2% |
| ◆ Rare Ore Bundle (Tier สูงสุดของดันเจี้ยน) | 3–6 | 9.0% | 7.8% | 6.9% | 6.4% | 6.4% |

ระดับกล่อง: Crystal Quarry = Wooden · Iron Depths = Iron · Ancient Rift = Crystal · Veteran/Master +1 ระดับ · Endless: B1–20 Wooden · B21–40 Iron · B41–60 Crystal · B61–100 Ancient · B101+ Void · Deep ชั้น 3+ +1 ระดับ · Cosmetic Fragment (10 = ปลดล็อก Cosmetic) มีเฉพาะ Ancient/Void หรือ Master

## Procedural Generation — ตรวจสอบ 1,000 Seeds (อัตโนมัติ)
| ชุด | Seeds | ผ่าน Validation | ห้อง (ต่ำ–สูง, เฉลี่ย) | แร่เฉลี่ย | เวลาสร้าง |
|---|---|---|---|---|---|
| Standard · Quarry · Explorer | 170 | 170/170 (ครั้งแรก) | 11–14 · 12.6 | 98 | 0.6 ms |
| Standard · Depths · Adventurer | 170 | 170/170 | 13–17 · 14.7 | 112 | 0.6 ms |
| Standard · Rift · Veteran | 170 | 170/170 | 13–19 · 16.1 | 116 | 0.7 ms |
| Standard · Rift · Master | 170 | 170/170 | 13–19 · 16.4 | 109 | 1.1 ms |
| Deep · Depths | 170 | 170/170 | 17–25 · 20.8 | 183 | 1.2 ms |
| Endless (ชั้นสุ่ม) | 150 | 150/150 | 13–17 · 14.7 | 112 | 0.6 ms |

Validation: ทุกห้องหลัก/Interactable/ประตูห้องลับเข้าถึงได้ **โดยไม่ต้องขุดแร่เลย** (นับแร่เป็นกำแพง) · ไม่มีแร่ที่ถูกล้อมจนขุดไม่ได้ · ไม่มีแร่บนจุดเกิด · แผนที่ต่างกันทุก Seed (50/50 และ 200/200) · Seed เดิมได้แผนที่เดิม (ใช้กับ Suspend/Resume)

**เล่นจริงจนเคลียร์ (บอท Auto Explore, 3 ระดับ Build × 3 ดันเจี้ยน × 4 ความยาก): 120/120 Seeds เคลียร์ได้** · เวลาเคลียร์เฉลี่ย (เวลาในเกม): Quarry ~5.3 นาที · Depths ~8.3 นาที · Rift ~7.8 นาที · Deep 1 ชั้น ~11.6 นาที

## Balance — เศรษฐกิจต่อชั่วโมง (Simulation จริง 1 ชม./กลยุทธ์, รวม 10 วิ/รอบสำหรับเมนู)
| ดันเจี้ยน (Build) | กลยุทธ์ | รอบ/ชม. | แร่/ชม. | 🪙 จากแร่/ชม. | มูลค่ากล่อง/ชม. | 🪙 รวม/ชม. | 📄/ชม. | Mastery EXP/ชิ้น/ชม. |
|---|---|---|---|---|---|---|---|---|
| Crystal Quarry (Scrap Lv3) | เคลียร์ทั้งแผนที่ | 6.4 | 575 | 17,733 | 4,959 | 21,175 | 26.6 | 2,306 |
| Crystal Quarry (Scrap Lv3) | เข้า-ออก 2 นาที | 27.7 | 726 | 16,539 | 0 | 17,627 | 17.8 | 2,662 |
| Iron Depths (Stoneguard Lv6 Awk Fine M12) | เคลียร์ทั้งแผนที่ | 6.4 | 451 | 45,585 | 19,551 | 53,579 | 30.2 | 1,814 |
| Iron Depths (Stoneguard Lv6 Awk Fine M12) | เข้า-ออก 2 นาที | 27.7 | 568 | 57,591 | 0 | 59,717 | 22.7 | 2,216 |
| Ancient Rift (Stoneguard Lv10 Asc Perfect M25) | เคลียร์ทั้งแผนที่ | 7.3 | 374 | 198,131 | 104,915 | 217,623 | 43.9 | 1,622 |
| Ancient Rift (Stoneguard Lv10 Asc Perfect M25) | เข้า-ออก 2 นาที | 27.7 | 509 | 199,163 | 0 | 200,943 | 23.7 | 2,104 |

สรุป: การเคลียร์ทั้งแผนที่ได้ 🪙 มากกว่าใน Quarry/Rift และได้ 📄 + วัสดุกล่อง (หิน, Rune, Pet Essence, Evo Shards) มากกว่าในทุกดันเจี้ยน · ใน Depths การเข้า-ออกสั้นได้ 🪙 มากกว่า ~11% แต่ไม่ได้กล่อง · ปริมาณแร่/ชม. ใกล้ V5.0 (ตาราง Crafting เดิมยังสมดุล: Mythic ≈ 2–3 ชม. ฟาร์มแร่)

## Performance
- Render เฉพาะ Tile ในมุมกล้อง · พื้นหลังแคชเป็น Chunk 8×8 แบบสร้างทีละ 2 Chunk/เฟรม · ข้าม Entity นอกจอ/ในหมอก · Minimap อัปเดตทุก 0.25 วิด้วย ImageData · ไม่มี Full-Map Loop ทุกเฟรม (Fog อัปเดตเฉพาะตอนเปลี่ยน Tile)
- วัดใน Headless Chromium (Software Canvas): เฟรมปกติบนแผนที่ 96×64 ≈ **2–5 ms** (เท่ากับแผนที่เดิม 22×14) · มือถือ 390×844 median 3.3 ms · Dijkstra ≈1 ms · ChooseTarget ≈0.5 ms · Generator ≈1 ms
- Save ของ Run ที่พักไว้ ≈ 1–3 KB (Seed + Diff + Fog แบบ RLE)

## Save Migration
- V5.0 → V5.1: `migrationVersion` 51 · เพิ่ม `v51` (chests, diffMax/Sel, tutorial, tips, stats, cosmetic fragments) และ `suspended` · ไม่แตะชิ้นส่วน/Mastery/Pet/Ore/Checkpoint (ทดสอบเทียบ JSON)
- ผู้เล่นเก่าที่มี Progress → Tutorial ถูกตั้งเป็น “ข้ามแล้ว” (ไม่บังคับ) · Save ใหม่ → Tutorial เริ่มอัตโนมัติ
- Run ที่พักไว้: Snapshot อัตโนมัติทุก 15 วิ + เมื่อสลับแท็บ/ปิดหน้า · ถ้าข้อมูลเสีย → กู้คืนเป็น Pending Claim เดียวของแร่ที่มี (ไม่สร้างรางวัลใหม่)

## Testing Report
- **V5.1 Acceptance: 46/46 ผ่าน** (ครอบคลุม Part 25 ข้อ 1–42: ไม่มีเวลา, Elapsed, ออกทันที 100%, ไม่ได้กล่องถ้าออกก่อน, ขนาดแผนที่, Reachability, Fog, Minimap, Auto Explore, Pathfinding, กล่อง + Loot Table (ตัวอย่าง 400 กล่อง เบี่ยงจากอัตราประกาศ ≤1.7%), รับครั้งเดียว, Reload ไม่สุ่มซ้ำ, Relic มีผล/หายเมื่อจบ, Mastery/Pet EXP, Ore Storage, Expedition Storage, Deep/Endless/Checkpoint, Tutorial อัตโนมัติ/ลงมือจริงครบ 12 ขั้น/Skip/เล่นซ้ำ, Save V5.0, Suspend → Reload → Resume, Mobile, ไม่มี JS Error, ไม่มี Softlock)
- **V5.0 Regression: 60/60 ผ่าน** (ปรับเงื่อนไขที่เปลี่ยนตามสเปก: ไม่มีเวลา, Deep ต้องเคลียร์ชั้น, Loot เหตุการณ์จ่ายตอน Extract)
- **V4 Regression: 67/72** — 5 ข้อที่ไม่ผ่านเป็นการเปลี่ยนตามสเปก: Save version, Combat Ticket → Fragment (V5), ดันเจี้ยนไม่มี Timeout/70% แล้ว, มูลค่าขายรวมโบนัสความลึกห้อง
- Generator 1,000 Seeds + 120 Full-Clear Runs + Economy Sim (ด้านบน)
- ระหว่างทดสอบพบและแก้: Snapshot ทำแร่บนพื้นซ้ำ (Exploit) · Station ที่เหลือที่น้อยกว่าน้ำหนักแร่ทำให้บอทวน · โกเลมยืนเยื้องกลางช่องแล้ววนขุดแร่แนวทแยง · ห้องที่เห็นแต่ไม่เข้า · ภารกิจที่มีแร่ถูกล้อมด้วยแร่ที่ขุดไม่ได้ · การเข้า-ออกสั้นคุ้มกว่า

## รายการที่ยังไม่ได้ทำ / ทำได้บางส่วน (ตามจริง)
1. **FPS บนมือถือจริงยังไม่ได้วัด** — วัดได้เฉพาะ Headless Chromium (CPU) ซึ่งเฟรมปกติ 2–5 ms; ในการวัดแบบนี้มี Spike ~120 ms เป็นระยะซึ่งเกิดกับ V4/V5 เดิมด้วย (เป็นผลของ Headless Compositor)
2. **กราฟิกห้อง** ใช้ Theme ของ Biome เดิม + สีห้อง + Prop เฉพาะห้อง (เครื่องจักร, คริสตัล, รูปปั้น, แท่น Relic, Station, กล่อง) — ยังไม่มีชุด Tile ใหม่ทั้งหมดต่อ Biome · **ประตูเครื่องจักรที่เปิด-ปิด** ยังไม่มี (ทางเชื่อมเปิดตลอด)
3. ความยากต่างกันที่จำนวนห้อง/Hazard Room/Event/Relic/Modifier/HP แร่ — ยังไม่มี “Hazard Pattern” ใหม่ (ใช้ลาวา, พื้นหน่วง, หินถล่ม) · **Challenge Cosmetics** ของ Master = Cosmetic Fragment → ปลดล็อก Cosmetic ที่มีอยู่แล้ว ไม่มีภาพใหม่
4. Mastery EXP/ชม. ของการเคลียร์ทั้งแผนที่ต่ำกว่าการเข้า-ออกสั้นประมาณ 10–20% (เพราะเวลาเดิน) · ใน Depths เข้า-ออกสั้นได้ Coins มากกว่า ~11%
5. Snapshot อัตโนมัติทุก 15 วิ — ถ้าเบราว์เซอร์ Crash อาจย้อนหลังได้สูงสุด ~15 วิ (ไม่มีของซ้ำ)
6. แตะห้องบนแผนที่ใหญ่ = เดินไปห้องนั้น (ไม่มีเส้นแสดงเส้นทาง) · Optional Speed Challenges ยังไม่มี (เก็บเวลาไว้ใน Records แล้ว)
7. Tutorial ทดสอบแบบอัตโนมัติครบทุกขั้น แต่ยังไม่ได้ทดสอบกับผู้เล่นจริง
