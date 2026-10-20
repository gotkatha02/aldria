# BUILD A GOLEM V6.0 — LIVING WORLD & GOLEM CAMP · รายงานผลการพัฒนา

ต่อยอดจากบิลด์ V5.1 โดยตรง (ไม่ได้สร้างใหม่ ไม่ได้ลบระบบเดิม) — หน้า Home เปลี่ยนเป็น **Golem Camp** แบบ Isometric 2.5D ที่เดินได้จริง ทุกอาคารเปิดระบบเดิมของเกม
เมนูแบบเดิมทั้งหมดยังอยู่ (☰ Menu → หน้าเมนู V5 · ปุ่ม 🏕 กลับ Camp)

ไฟล์โค้ดใหม่ 7 โมดูล (~150 KB): `v6a` เรนเดอร์ isometric + วัสดุ/พื้น · `v6b` ธรรมชาติ/พร็อพ · `v6c` อาคาร 9 แห่ง × 3 เลเวล · `v6d` NPC rig / particles / กลางวัน-กลางคืน · `v6e` engine ของค่าย · `v6f` HUD / สถานี / อีเวนต์ / เซฟ · `v6g` Tutorial

---

## 1. สร้างเสร็จแล้ว (ตาม Phase)

| Phase | สถานะ | สิ่งที่ทำจริง |
|---|---|---|
| 1 · Playable Camp | ✅ | แผนที่ 64×48 tiles · Collision grid (อาคาร พร็อพ น้ำ รั้ว ขอบป่า) · A* 8 ทิศ + string-pulling · กล้อง Smooth follow + Dead zone + Zoom (0.55–1.7) + World bounds |
| 2 · ระบบเดิม | ✅ | 8 สถานี + Camp Crystal เชื่อมฟังก์ชันจริงทั้งหมด (ตารางด้านล่าง) · ไม่มี Inventory / Currency ชุดที่สอง |
| 3 · ตัวละคร + Pet | ✅ | โกเลมวาดด้วย part renderer เดิม (ชิ้นที่ใส่จริง) — Idle / Walk / Run / Turn / Interact / Inspect / Crafting / Celebrate / Equip / Pet / Return · ใกล้ Arena เปลี่ยนเป็นชุด Combat Armor อัตโนมัติ · Pet ที่ Equip จริงเดินตาม (Follow / Run / Idle / Sit / Sleep / Look around / Jump / Happy / Hearts / ทักทาย Pet ตัวอื่นใน Sanctuary / Teleport นุ่มนวลเมื่อหลุด) |
| 4 · Premium Environment | ✅ | วัสดุ procedural 9 แบบ (ไม้กระดาน, อิฐหิน, อิฐแดง, กระเบื้องหลังคา, Shingle, ฟาง, โลหะหมุดย้ำ, หิน, กระจก) map บนผนัง/หลังคาด้วย perspective จริง · AO · เงาตามดวงอาทิตย์ · พื้นหญ้า/ทางหิน/Cobblestone/ไม้/น้ำ/ทราย · ต้นไม้ 5 แบบ, พุ่มไม้, หิน, คริสตัล, เห็ด, รั้ว, โคม, ม้านั่ง, ป้ายบอกทาง, บ่อน้ำ, ทั่ง, ชั้นอาวุธ, รถแร่, ลังไม้, รูปปั้นโกเลมหิน |
| 5 · Living NPC | ✅ | 7 NPC (ช่างตีเหล็ก Brom, วิศวกร Pip, ดร.ลูเมน, ทอบพนักงานคลัง, วัลก้าผู้คุมอารีน่า, รุคนักผจญภัย, มิร่าพ่อค้าเร่) · State machine Idle/Walk/Work/Talk/Rest/Wave/Cheer + cooldown + เส้นทางด้วย A* · ทักทาย/หันหาผู้เล่น · บทสนทนาเปลี่ยนตามความก้าวหน้าจริง · เครื่องหมาย ❗ เมื่อมีข่าว (ล้มบอส, ปลดล็อกดันเจี้ยน, Camp Level) |
| 6 · Atmosphere | ✅ | Day/Night 18 นาที (Day / Sunset / Night / Dawn) · โคมไฟติด, หน้าต่างเรืองแสง, หิ่งห้อย, เงายาวตอนเย็น · Particles (ควัน, ไอน้ำ, ประกายไฟ, ถ่าน, หมอก, ใบไม้, ผีเสื้อ, นก, ฝุ่น) แบบมี budget · เสียง SFX เดิม (ค้อน, ก้าวเดิน, ติดตั้ง, เหรียญ ฯลฯ) |
| 7 · Camp Progression | ✅ | 3 เลเวล: Starter Outpost → Mechanical Settlement → Grand Golem Fortress · คำนวณจาก Research, ดันเจี้ยนที่ปลดล็อก, Mining/Combat Boss, การคราฟต์, Mastery, Endless (ไม่มีค่าอัปเกรด) · อาคาร/พื้น/โคม/หอเครื่องจักร/แขนกลเปลี่ยนจริง |
| 8 · Tutorial | ✅ | 15 ขั้น เริ่มในค่าย (GEARLY บินตามโกเลมในฉาก + ลูกศรทอง) · ทุกขั้นรอการกระทำจริง · Skip / เล่นซ้ำ · รางวัลครั้งเดียว · ผู้เล่นเก่าไม่ถูกบังคับ |
| 9 · Responsive | ✅ | PC: WASD/ลูกศร, Shift วิ่ง, คลิกพื้น/อาคาร/NPC, E, M, ESC, ล้อเมาส์ · Mobile: จอยซ้าย, แตะเพื่อเดิน, ปุ่มใช้งานขวา, หนีบนิ้วซูม · HUD เล็ก (Objective, Character, Coins ▾ ทรัพยากรทั้งหมด, 🔔, Minimap, Map, Menu, Options, Pet status, ทางลัดสถานีล่าสุด) |
| 10 · Test & Optimize | ✅ | ดูหัวข้อ 3–4 |

### สถานีในค่าย → ระบบจริง

| สถานี | เปิดอะไร |
|---|---|
| 🧍 Golem Garage | Garage เดิม (Mining/Combat Loadout, Parts, Mastery, Presets) · หลังยืนยัน Equip → โกเลมยืนบนแท่นประกอบ แขนกล 2/4/6 แขน (ตามเลเวลค่าย) ติดตั้งชิ้นส่วนพร้อมประกายไฟ |
| ⚒ Blacksmith Forge | Assembly Forge (Mining Parts) · Armor Blacksmith · Enhancement · Runes · Upgrade · Evolution — หักวัสดุจาก Ore Storage จริง |
| 🐾 Pet Sanctuary | ระบบ Pet เดิม (Pets / Eggs / Feed & Cook / Codex / Evolve / Equip) · Pet ที่ไม่ได้ติดตามเดินเล่นในสวน |
| ⛏ Mine Entrance | Standard / Deep / Endless / Mining Boss / Resume (ไม่มีเวลาจำกัดตาม V5.1) |
| ⚔ Arena Gate | Preview โกเลมในชุด Combat + Combat Pet + บอสถัดไป → Boss Arena / Combat Loadout / Training / Raid / Boss Codex |
| 🏦 Warehouse | รับรางวัล Extract (Sell/Store/Split) · Ore Storage · Mining Parts · Combat Armor |
| 📋 Quest Board | Daily / Weekly (รับรางวัลได้ในหน้าบอร์ด) · Mining / Combat Contracts · Achievements |
| 🔬 Research Lab | Research · Part Mastery · Codex · Boss Codex · Records |
| 💎 Camp Crystal | Camp Level + คะแนนความก้าวหน้า · เมนูแบบเดิม · แจ้งเตือน · แผนที่ · Tutorial |

ทางลัดไม่ให้การเดินทำให้เล่นช้า: แผนที่ (M) มีปุ่ม **🚶 เดินไป** และ **⚡ เปิดทันที** ทุกสถานี · ทางลัดสถานีล่าสุด 3 อัน · แตะ Objective เพื่อเดินไปเอง · คลิกตัวอาคารเพื่อเดินไปใช้งาน

### Camp Events (สูงสุด 6 ครั้ง/วัน · ไม่บังคับ · หมดเวลาเองใน 4–5 นาที)
🛒 พ่อค้าเร่ (ซื้อขายด้วยทรัพยากรจริง มีจำกัด) · 📦 พัสดุหาย (ได้แร่เข้า Ore Storage) · ⚡ เครื่องปฏิกรณ์เสีย (จ่าย 🪙150 หรือ Stone ×15 → 📄2) · 🥚 แสงในสวน (Pet Shard) · 📡 สัญญาณโบราณ (Research Data) · ⛏ สายแร่ใหม่ (รอบถัดไปในดันเจี้ยนนั้น มูลค่าแร่ ×1.15) · ⚔ ประกาศท้าชิงบอส (ชนะบอส → 🪙 + 📄2) · 🎉 ฉลองเวิร์กช็อป (Camp Level ใหม่ / คราฟต์ครบทุก 10 ชิ้น)

### กลับจากดันเจี้ยน / อารีน่า
รางวัลคำนวณด้วยระบบเดิม → หน้าต่าง Sell/Store/Split → โกเลมเดินออกจากปากเหมือง (หรือประตูอารีน่า) → Pet วิ่งมาดีใจ → NPC ทักทาย → ถ้า Extract/ชนะ เล่นท่าฉลอง · ไม่มีการรับรางวัลซ้ำ

### เซฟ
เพิ่มเฉพาะ `S.camp` (level, pos, unlocked, events, tod, dayLock, labels, decorations, recent, visits, view, merchant, rich, bounty, tutV6 …) · สำรอง `buildAGolem_backup_pre_v6` ก่อนเพิ่มฟิลด์ครั้งแรก · ไม่ reset ข้อมูลเดิมใด ๆ · คีย์เซฟเดิม `buildAGolem_save_v1` · Export/Import/Backup เดิมยังใช้ได้

---

## 2. Performance (วัดจริงใน headless Chromium + software GPU — ไม่ใช่เครื่องจริง)

| ฉาก | frame time (median) |
|---|---|
| Camp Lv.1 / Lv.3 (กลางวัน) | 33 ms |
| Camp Lv.3 กลางคืน | 33 ms (p90 50) |
| Dungeon V5.1 (เทียบ) | 16.7 ms |

เทคนิคที่ใช้: พื้นแคชเป็นแผ่น 512px ทึบไม่ซ้อนกัน (blit ครั้งเดียวต่อพิกเซล) · ป่าที่ผู้เล่นเข้าไม่ถึงถูก bake ลงพื้น/เลเยอร์หน้า · sprite ทุกชิ้นแคชและ trim ส่วนโปร่งใส · glow/เงาเป็น sprite แทน gradient · culling · LRU cache · particle budget ตามคุณภาพ · ความละเอียดจำกัด 1.5× DPR · Adaptive Quality เดิม (ลดคุณภาพอัตโนมัติเมื่อ FPS ต่ำ) · Reduced Motion ปิด sway/ambient
จากเดิมก่อน optimize 63 ms → 27 ms (avg) ในสภาพแวดล้อมเดียวกัน

---

## 3. ผลทดสอบ (รันจริงทั้งหมด)

**ชุด V6 Acceptance — 42/42 ผ่าน (ครอบคลุม Acceptance Tests ข้อ 1–35 + ทดสอบเพิ่ม E1–E5)**

- ✅ **20** Old V5.1 save loads: coins / parts / armor / pets / mastery / blueprints / runes / boss clears / ores / dungeon progress untouched + backup + camp fields
- ✅ **1** Game opens straight into the playable Golem Camp (canvas rendered, rich colour)
- ✅ **2** Golem walks with WASD (real key events)
- ✅ **3** Camera follows the golem (within dead-zone)
- ✅ **4** Camera stays inside world bounds (view centre clamped on all 4 corners)
- ✅ **5** Building / prop collision (cannot walk into Garage; 2500-step random walk never inside a blocked tile)
- ✅ **6** Click on the ground → pathfinding walk to that spot
- ✅ **7** Equipped pet follows the golem (real equipped pet, catches up)
- ✅ **8** NPCs really move / work (state machines with routes)
- ✅ **9** Ambient animation (particles: smoke / steam / sparks / leaves …, butterflies)
- ✅ **10** Garage opens through the building (auto-walk → interact)
- ✅ **11** Blacksmith Forge → real crafting (materials deducted from Ore Storage, part added to inventory)
- ✅ **12** Pet Sanctuary opens the real Pet system
- ✅ **13** Warehouse → Ore Storage uses real resources (sell → coins up, ores down)
- ✅ **14** Mine Entrance → Standard Expedition starts (no time limit, camp hidden)
- ✅ **18** After Return the golem is back in Camp beside the Mine Entrance (return animation)
- ✅ **19** Reward claimed once (second claim of the same extraction does nothing; pending not lost)
- ✅ **15** Arena Gate → combat loadout preview → real Boss Arena fight starts
- ✅ **30** Combat system still works from the camp (fight ran to the end) and returns to the Arena Gate
- ✅ **16** Quest Board → Daily / Weekly / Contracts work (reward once)
- ✅ **17** Research Lab → real Research upgrade
- ✅ **21** Camp position is saved and restored after reload
- ✅ **22** Camp level follows real progression and changes the graphics (3 different building sprites)
- ✅ **31** Graphics quality setting changes camp resolution / particle budget
- ✅ **32** Frame cost with NPCs + pet + level-3 camp (headless software renderer)
- ✅ **33** Golem is drawn by the modular part renderer (not a static picture)
- ✅ **34** Golem in camp shows the parts actually equipped (new head → big change; reverting → back to the same look)
- ✅ **35** Every main station is reachable on foot (A* from the plaza to each interaction spot)
- ✅ **27** NPCs never stand in walls and keep cycling their routes (200 s simulated, day + night)
- ✅ **24** PC controls: E interacts, M map, ESC pause menu, mouse wheel zoom
- ✅ **26** Camp HUD keeps the scene visible (desktop HUD area)
- ✅ **E1** Camp events: merchant arrives + real purchase (coins deducted once, limit respected), lost delivery found → real ores, cannot be claimed twice
- ✅ **E2** Day / sunset / night cycle really changes the lighting (stations stay usable)
- ✅ **E3** NPC dialogue reacts to real progress (boss clear → new line + "!" marker)
- ✅ **E4** Equipping in the Garage → golem stands on the assembly pad and the robotic arms install the part (equipment data stays correct)
- ✅ **E5** UI overlays block game input (keys / clicks under a window do not move the golem)
- ✅ **28** No JavaScript errors (desktop session)
- ✅ **25** Mobile controls: virtual joystick moves, tap-to-move, interact button opens the station
- ✅ **26b** Camp HUD on a phone keeps most of the scene visible
- ✅ **23** Interactive camp tutorial completes by real play (walk → garage → part → forge → craft → equip → mine → dig → skill → return → store → warehouse → pets → quest board → free play) + reward once
- ✅ **23b** Tutorial can be replayed and skipped (reward not given twice)
- ✅ **28b** No JavaScript errors (mobile + tutorial sessions)

**Regression ระบบเดิม**
- V5.1 suite: 46/46 (ปรับ 2 ข้อให้ตรงกับดีไซน์ใหม่: Tutorial ย้ายมาเริ่มในค่าย, migrationVersion ≥ 51)
- V5 suite: 60/60 ในรอบหนึ่ง และ 59/60 ในอีกรอบ — ข้อ 13 (Mastery EXP ≥ 40 จากบอตเล่น 60 วิ) แกว่งตามแผนที่สุ่ม เหมือนที่เป็นมาตั้งแต่ V5.1 · ข้อ responsive 38–40 ปรับให้ทดสอบผ่าน ☰ Menu (หน้าเมนูเดิม)
- V4 suite: 66/72 — 5 ข้อเป็นความตั้งใจเดิมตั้งแต่ V5 (บิลด์ V5.1 ได้ 5 ข้อเดียวกัน) · ข้อ "Endless still playable" ตกเป็นครั้งคราว: วัดซ้ำแล้วพบว่าบิลด์ V5.1 ก็ได้ 0 ในบางรอบเพราะบอตหยุดที่หน้าต่าง Event ที่สคริปต์ทดสอบเก่าไม่ได้กดปิด — ไม่ใช่ผลจาก V6

---

## 4. ยังเป็นต้นแบบ / ข้อจำกัดที่ต้องบอกตรง ๆ

- **ยังไม่ได้ทดสอบบนมือถือ/แท็บเล็ตจริง** — ทดสอบด้วยการจำลอง touch ใน Chromium เท่านั้น · FPS บนอุปกรณ์จริง (GPU จริง) ยังไม่ได้วัด
- ในสภาพแวดล้อมทดสอบ (software GPU) ค่ายเร็วประมาณครึ่งหนึ่งของดันเจี้ยน (30 vs 60 FPS) — ระบบปรับคุณภาพอัตโนมัติช่วยได้ แต่บนเครื่องสเปกต่ำอาจยังรู้สึกหน่วงกว่าหน้าเดิม
- กราฟิกทั้งหมดเป็น Procedural Canvas (ไม่มีภาพวาดมือ) — ทุกอาคารมีหลายชั้น วัสดุ แสง เงาจริง แต่ยังไม่ถึงระดับงานอาร์ตวาดมือของเกม Indie ที่ขัดเกลาแล้ว
- โกเลมมีมุมมองหน้า/เฉียง 3/4 เท่านั้น (ใช้ renderer เดิม) — เดินขึ้นด้านบนจอจะไม่เห็นด้านหลัง · NPC พลิกซ้าย/ขวาได้อย่างเดียว
- เข้าไปเดินในอาคารไม่ได้ — สถานีเปิดหน้าต่างระบบเดิม (ไม่ใช่ฉากภายใน)
- ต้นไม้ในป่าลึก (ที่เดินไปไม่ถึง) ถูก bake เป็นภาพนิ่งเพื่อประสิทธิภาพ จึงไม่ไหวตามลม (ต้นไม้ใกล้ทางเดินยังไหว)
- `campDecorations` มีแค่ฟิลด์ในเซฟ ยังไม่มีระบบให้ผู้เล่นวางของตกแต่งเอง
- เสียงใช้ SFX สังเคราะห์และ ambient เดิม ยังไม่มีเพลง/เสียงบรรยากาศค่ายแยกใหม่
- อีเวนต์ที่ทดสอบอัตโนมัติจริง: พ่อค้า + พัสดุหาย (รวมกันซื้อ/รับซ้ำไม่ได้) · อีเวนต์อื่นผ่านการเรียกโค้ดตอนพัฒนาแต่ยังไม่มีชุดทดสอบครบทุกตัว
- Tutorial ทดสอบด้วยบอต ยังไม่ได้ทดสอบกับผู้เล่นจริง
- Pet ติดตามด้วย steering + teleport นุ่มนวล (ไม่ใช่ pathfinding เต็มรูปแบบ) — อาจเดินอ้อมรั้วแปลก ๆ แต่ไม่ติดค้าง
- การเดินระยะไกลในค่ายใช้เวลา ~2–4 วินาที — มี ⚡ เปิดทันทีในแผนที่ แต่ยังไม่มีปุ่ม Quick-open บน HUD หลักโดยตรง
