# BUILD A GOLEM — V5.0 “THE MASTER CRAFTSMAN” · รายงานการพัฒนา

> สร้างต่อจาก V4.0 Armor Wars (ไม่ได้สร้างใหม่) · Save key เดิม `buildAGolem_save_v1` · ทดสอบด้วย Engine จริงผ่าน Playwright

## 1. Playable HTML
- เกม: `index.html` (ไฟล์เดียว ~836 KB) — GitHub Pages: https://gotkatha02.github.io/aldria/
- โค้ด V5 แยกเป็นโมดูลก่อนประกอบ: `v5data` · `v5state` · `v5pet` · `v5mine` · `v5combat` · `v5ui` · `v5ui2` · `v5bal`

## 2. สรุประบบ (Game Systems Summary)
| ระบบ | สถานะ | รายละเอียด |
|---|---|---|
| ยกเลิก Gacha | ✅ | Mining Summon → **Mining Assembly Forge**, Armor Summon → **Combat Armor Blacksmith** (ปุ่มเดิมพาไปหน้าใหม่) · Ticket เดิมแปลงครั้งเดียว 🎫1 → 📄2 + 🪙50, 🎟1 → 📄2 + 🔹1 · รางวัลที่เคยให้ Ticket เปลี่ยนเป็น 📄 Blueprint Fragment |
| Crafting ทั้ง 10 ช่อง | ✅ | Mining 60 แบบ + Armor 60 แบบ คราฟต์ได้แน่นอน · โหมด Standard / Fine (Lv.5) / Perfect (Masterwork Lv.8) → Quality แน่นอน · คราฟต์ทีละ 1–5 ชิ้น |
| Rune / Module | ✅ | 11 ชนิด × 3 Tier แทน Trait สุ่ม · 1 ช่องต่อชิ้น · ถอดคืน 100% · ค่าคงที่ไม่ถูกคูณ (Trait เก่ายังอยู่ แต่ Reforge ถูกปิด) |
| Blueprint | ✅ | Mining Epic+ ต่อแบบ (📄6/10/15) · Armor Legendary/Mythic ต่อแบบ · แหล่งได้แน่นอน (ตาราง §7) · แลก ⚙️40 → 📄1 |
| Individual Part Mastery | ✅ | 120 แบบ (ผูกกับ Design ID) · Lv.1–30 · Lv.5/10/15/20/25/30 · Lv.20 เลือก Path 2 สาย (Respec มีค่าใช้จ่าย) · Lv.25 สกิล +10% · Lv.30 Grandmaster Effect · แขนซ้าย/ขวาแบบเดียวกันได้ EXP ครั้งเดียว |
| Equal Potential | ✅ (มีข้อจำกัด §12) | งบ Stat ต่อประเภท × Rarity step 1.00–1.30 · GM ชดเชย Rarity ต่ำ (Common ×2.00 … Mythic ×1.60) · Overflow ของ Speed/Move แปลงเป็น Power เล็กน้อย |
| Pets 14 ตัว | ✅ | Mining 6 · Combat 6 · Hybrid 2 · Lv.1–20 · 3 Stage (Lv10/20) · Bond 1–5 · Passive + Active อัตโนมัติ (Cooldown จริง) + Bond Ability · เดินตามโกเลมทั้งเหมืองและสนามรบ · ไข่จากแหล่งที่กำหนด (เลือกเป้าหมาย ไม่สุ่ม) · Synergy กับตระกูลชิ้นส่วน |
| Farming Modes | ✅ | Quick Mining 180 วิ · Deep Expedition 4 ชั้น (~12 นาที, รวมเป็นรางวัลเดียว) · Endless ชั้นละ 90 วิ (รอบ 20–30 นาที) |
| Endless ใหม่ | ✅ | 6 Biome · Checkpoint ทุก 5 ชั้น (เริ่มใหม่จาก Checkpoint ได้) · Milestone ทุก 10 ชั้นถึง B300 (รับครั้งเดียว) · Modifier ใหม่ 7 แบบ |
| Boss | ✅ | Boss Core Fragment การันตี (2/3/5 ตามระดับ, 5 = 1 Core) · ระดับ Normal / Hard / Nightmare |
| Daily / Weekly | ✅ | Daily 3 งาน/วัน จาก 9 แบบ · Weekly Theme 5 แบบ (Modifier ใน Deep Expedition) · ไม่มี Stamina · พลาดได้ไม่เสียอะไร |
| Activity Mastery | ✅ | 14 สาย (Mining/Combat/Crafting) Lv.1–20 โบนัสเล็กและมีเพดาน |
| Wishlist | ✅ | ปักหมุด 3 อย่าง · แสดงที่ต้องใช้/มี/ขาด · ที่ฟาร์ม · จำนวนรอบโดยประมาณ · Blueprint · แจ้งเตือนเมื่อพร้อม |
| UI ใหม่ | ✅ | 5 แท็บ HOME/GOLEM/ADVENTURE/WORKSHOP/PET + เมนูรอง · Desktop 3 คอลัมน์ / Tablet 2 / Mobile ซ้อน + Bottom Nav + Bottom Sheet |
| Save | ✅ | Export/Import (ข้อความ/ไฟล์) · สำรอง Save ก่อน V5 อัตโนมัติ (`buildAGolem_backup_pre_v5`) |

## 3. Pet Database (14)
| # | Pet | ประเภท | Rarity | Stage 1 → 2 → 3 | Passive | Active (Auto · CD) | Bond Lv.3 | ไข่ (ชิ้น · แหล่ง) | Synergy |
|---|---|---|---|---|---|---|---|---|---|
| 1 | **Rockmole** | Mining | Common | Pebblemole → Rockmole → Crystal Rockmole | Ore Sniff: ตรวจจับแร่ซ่อนได้ไกลขึ้น +1 ช่อง (+1 ที่ Stage 3) | Burrow Ping (22s): มุดดินส่งคลื่น เปิดแร่ซ่อน/ห้องลับในระยะ 4 ช่อง | Deep Dig Friend: แร่ซ่อนที่เจอมูลค่า +15% | 4 · ฟรีเมื่อเปิด Pet Sanctuary ครั้งแรก | Drilltech |
| 2 | **Magnetot** | Mining | Uncommon | Mini Magnetot → Magnetot → Magnafang | Magnetic Pickup: ระยะเก็บแร่ +1.2 | Magnet Burst (18s): ดูดแร่ที่ตกอยู่ในรัศมี 6 ช่องเข้าหาโกเลม | Packed Haul: Capacity +8 | 6 · Mining Extract สำเร็จ (เลือกเป็นเป้าหมาย) | Magnetite |
| 3 | **Prismowl** | Mining | Rare | Glintowl → Prismowl → Aurora Prismowl | Hidden Vein Sight: ตรวจพบห้องลับ/แร่ซ่อน +2 ช่อง | Prism Scan (25s): ทำเครื่องหมายแร่มูลค่าสูงสุด 3 ก้อน (+20% มูลค่า) | Treasure Instinct: มูลค่าแร่ Tier 5+ +8% | 9 · Extract สำเร็จที่ Iron Depths ขึ้นไป · Endless Milestone | Prism |
| 4 | **Gearbeetle** | Mining | Rare | Cogbug → Gearbeetle → Drillbeetle Prime | Assist Chip: Mining Speed +6 | Drill Tap (10s): บินไปเจาะแร่เป้าหมาย 150% Power | Efficient Mining: เวลาตั้งท่าก่อนขุด −30% | 9 · Mining Contracts · Extract สำเร็จ | Coppergear |
| 5 | **Frostpup** | Mining | Epic | Snowpup → Frostpup → Glacier Wolflet | Chill Guard: ผลกระทบลาวา/พื้นร้อน −40% | Frost Trail (24s): แช่แข็งพื้นลาวารอบตัว 3 ช่อง ปลอดภัย 8 วิ | Calm Step: หินถล่มทำให้มึนสั้นลง 40% | 12 · ปราบ Mining Boss · Extract ที่ Ancient Rift | Frostbound |
| 6 | **Lavasnail** | Mining | Legendary | Ember Snail → Lavasnail → Volcanic Lavasnail | Heat Fracture: ดาเมจขุดแร่ Tier 4+ +8% | Molten Crack (20s): แร่เป้าหมายเปราะ รับดาเมจ +50% 6 วิ | Rare Ore Crack: 5% โอกาสแร่ Tier 5+ แตกได้เพิ่ม 1 ก้อน | 15 · ปราบ Magma Colossus (Mining Boss) · Endless B30+ | Magma |
| 7 | **Emberfox** | Combat | Uncommon | Cindercub → Emberfox → Inferno Emberfox | Burning Bite: การโจมตี 12% ติด Burn | Foxfire Shot (6s): ยิงลูกไฟ 120% ATK + Burn 2 | Blaze Partner: ดาเมจใส่ศัตรูที่ติด Burn +8% | 6 · ชนะ Combat Boss (เลือกเป็นเป้าหมาย) · Combat Contracts | Magma |
| 8 | **Boltcub** | Combat | Rare | Sparkcub → Boltcub → Thunderfang Cub | Static Claw: Shock สะสมเพิ่ม +1 ต่อครั้งที่ติด | Chain Pounce (9s): กระโจน 150% ATK + Shock 2 | Storm Pulse: คอมโบ 10 ฮิต → Attack Speed +15% 4 วิ | 9 · ชนะ Combat Boss · Training (ดาเมจ 5,000+) | Thunder |
| 9 | **Aegishell** | Combat | Rare | Shieldling → Aegishell → Fortress Aegishell | Guard Link: DEF +8 · HP +5% | Shell Barrier (14s): เมื่อ HP < 70% สร้างโล่ 15% HP | Steady Core: ระหว่างมีโล่ ไม่ติดมึน | 9 · ชนะ Combat Boss · Combat Contracts | Stoneguard |
| 10 | **Voidbat** | Combat | Epic | Riftbat → Voidbat → Abyss Voidbat | Weak Mark: ดาเมจต่อบอสที่ถูกทำเครื่องหมาย +10% | Abyss Tag (12s): ทำเครื่องหมายจุดอ่อนบอส 3 วิ | Dimensional Step: Dodge สำเร็จ → การโจมตีถัดไป +30% | 12 · ชนะ Magma Warlord ขึ้นไป | Void |
| 11 | **Crystare** | Combat | Legendary | Gemhare → Crystare → Aurora Crystare | Crystal Focus: Crit Chance +4% | Prism Lance (8s): ลำแสงคริสตัล 200% ATK คริติคอลแน่นอน | Sharp Vision: ดาเมจ Weak Point +12% | 15 · ชนะ Magma Warlord / Void Tyrant | Prism |
| 12 | **Titanpup** | Combat | Mythic | Stonecub → Titanpup → Colossus Titanpup | Crushing Presence: Stagger Power +8 | Titan Roar (15s): คำราม เพิ่ม Stagger Gauge บอส 25 | Unshaken: ดาเมจจากท่าหนัก (มึน/กระแทก) −20% | 20 · ชนะ Void Tyrant / Hybrid Raid | Titanstone |
| 13 | **Driftling** | Hybrid | Epic | Wisplet → Driftling → Aether Driftling | Adaptive Support: Mining: Speed +5 · Combat: DEF +5 | Support Pulse (15s): Mining: ดูดแร่รอบตัว 3 ช่อง · Combat: โล่ 8% HP | Flexible Link: Pet EXP +20% และ Utility เล็กน้อยทั้งสองสาย | 12 · Hybrid Quest: Extract + ชนะ Combat Boss | Zephyr |
| 14 | **Mechafinch** | Hybrid | Rare | Chirpbolt → Mechafinch → Skyforge Finch | Recon Flight: Mining: ตรวจจับ +1 · Combat: เห็นท่าบอสถัดไป | Signal Chirp (20s): Mining: เตือนแร่หายาก · Combat: เตือนท่าหนัก (+0.25 วิ) | Alert Partner: Dodge Cooldown −8% | 9 · Hybrid Quest: Extract + Training | Scrap |

## 4. Part Mastery Database (120 แบบ)
| Family | Rarity | Slot | ชิ้นส่วน | Lv.20 Path A / B | Lv.30 Grandmaster |
|---|---|---|---|---|---|
| Scrap | Common | ⛏ Head | Lantern Scout | Deep Scanner / Focus Lens | **Junk Radar** — ตรวจพบแร่ซ่อนในระยะ 3 ช่องอัตโนมัติทุก 10 วิ |
| Scrap | Common | ⛏ Core | Boiler Core | Power Reactor / Engine Tuning | **Spare Boiler** — เวลาขุดต่อรอบ +10 วิ |
| Scrap | Common | ⛏ Arm | Patchwork Pick | Rapid Miner / Heavy Breaker | **Junkyard Echo** — 14% โอกาสที่การขุดจะตีซ้ำทันที |
| Scrap | Common | ⛏ Legs | Tread Walker | Sprinter / Steady Strider | **Tread Rhythm** — หลังทุบแร่แตก เดินเร็วขึ้น 12% 3 วิ |
| Scrap | Common | ⛏ Back | Scrap Crate | Hauler / Collector | **Scavenger Sack** — 8% โอกาสได้แร่เพิ่ม 1 ก้อนเมื่อแตก |
| Scrap | Common | ⚔ Helmet | Scrap Helmet | Precision / Insight | **Rusty Visor** — Crit Damage +20% |
| Scrap | Common | ⚔ Chest Armor | Scrap Chest Armor | Bulwark / Reactor | **Patchwork Repair** — ฟื้น HP 1% ทุก 3 วิ ระหว่างต่อสู้ |
| Scrap | Common | ⚔ Gauntlet | Scrap Gauntlet | Swift Fist / Heavy Fist | **Scrap Barrage** — ทุกการโจมตีครั้งที่ 4 ปล่อยคลื่นเพิ่ม 80% ATK |
| Scrap | Common | ⚔ Leg Armor | Scrap Leg Armor | Evasion / Charge | **Roll Strike** — Dodge สำเร็จแล้วสวนกลับ 150% ATK |
| Scrap | Common | ⚔ Back Armor | Scrap Back Armor | Barrier / Support | **Iron Lid** — ได้โล่ 8% HP ทุก 10 วิ |
| Stoneguard | Common | ⛏ Head | Bedrock Brow | Deep Scanner / Focus Lens | **Stone Sense** — แร่ Tier 4+ มูลค่า +15% |
| Stoneguard | Common | ⛏ Core | Granite Heart | Power Reactor / Engine Tuning | **Bedrock Core** — ดาเมจขุดแร่ Tier 4+ +12% |
| Stoneguard | Common | ⛏ Arm | Boulder Hammer | Rapid Miner / Heavy Breaker | **Quake Slam** — การขุดกระแทกแร่ข้างเคียง 30% ของดาเมจ |
| Stoneguard | Common | ⛏ Legs | Anchor Stone | Sprinter / Steady Strider | **Stone Footing** — ผลกระทบพื้นอันตราย −60% |
| Stoneguard | Common | ⛏ Back | Shieldstone | Hauler / Collector | **Rampart Hold** — Capacity +20 |
| Stoneguard | Common | ⚔ Helmet | Stoneguard Helmet | Precision / Insight | **Mason Eye** — ทำเครื่องหมาย Weak Point ของบอส 2 วิ ทุก 10 วิ |
| Stoneguard | Common | ⚔ Chest Armor | Stoneguard Chest Armor | Bulwark / Reactor | **Living Wall** — สะท้อนดาเมจ 15% กลับใส่ผู้โจมตี |
| Stoneguard | Common | ⚔ Gauntlet | Stoneguard Gauntlet | Swift Fist / Heavy Fist | **Boulder Fist** — การโจมตีปกติแรงขึ้น 30% ต่อบอสที่ติด Stagger/Freeze |
| Stoneguard | Common | ⚔ Leg Armor | Stoneguard Leg Armor | Evasion / Charge | **Stomp Step** — Dodge Cooldown −15% |
| Stoneguard | Common | ⚔ Back Armor | Stoneguard Back Armor | Barrier / Support | **Warden Wall** — ได้โล่ 8% HP ทุก 8 วิ |
| Coppergear | Uncommon | ⛏ Head | Spark Dial | Deep Scanner / Focus Lens | **Gauge Sight** — 10% โอกาสขุดคริติคอล ×2 |
| Coppergear | Uncommon | ⛏ Core | Rotary Dynamo | Power Reactor / Engine Tuning | **Overclock Core** — ทุบแร่แตกแล้ว Mining Speed +20 3 วิ |
| Coppergear | Uncommon | ⛏ Arm | Piston Pick | Rapid Miner / Heavy Breaker | **Piston Repeat** — 18% โอกาสที่การขุดจะตีซ้ำทันที |
| Coppergear | Uncommon | ⛏ Legs | Copper Spring | Sprinter / Steady Strider | **Spring Gait** — หลังทุบแร่แตก เดินเร็วขึ้น 15% 3 วิ |
| Coppergear | Uncommon | ⛏ Back | Conveyor Hopper | Hauler / Collector | **Gear Collector** — ระยะเก็บแร่ +1.5 ช่อง |
| Coppergear | Uncommon | ⚔ Helmet | Coppergear Helmet | Precision / Insight | **Gauge Lens** — Crit Damage +25% |
| Coppergear | Uncommon | ⚔ Chest Armor | Coppergear Chest Armor | Bulwark / Reactor | **Pressure Valve** — พลัง Ultimate +15% |
| Coppergear | Uncommon | ⚔ Gauntlet | Coppergear Gauntlet | Swift Fist / Heavy Fist | **Piston Rush** — ทุกการโจมตีครั้งที่ 3 ปล่อยคลื่นเพิ่ม 80% ATK |
| Coppergear | Uncommon | ⚔ Leg Armor | Coppergear Leg Armor | Evasion / Charge | **Spring Hop** — Dodge Cooldown −18% |
| Coppergear | Uncommon | ⚔ Back Armor | Coppergear Back Armor | Barrier / Support | **Steam Cycle** — Cooldown Red. +8% |
| Zephyr | Uncommon | ⛏ Head | Gale Finder | Deep Scanner / Focus Lens | **Wind Scout** — ตรวจพบแร่ซ่อนในระยะ 3 ช่องอัตโนมัติทุก 10 วิ |
| Zephyr | Uncommon | ⛏ Core | Breeze Turbine | Power Reactor / Engine Tuning | **Tailwind Core** — ทุบแร่แตกแล้ว Mining Speed +25 3 วิ |
| Zephyr | Uncommon | ⛏ Arm | Wind Cutter | Rapid Miner / Heavy Breaker | **Gale Cut** — การขุดกระแทกแร่ข้างเคียง 20% ของดาเมจ |
| Zephyr | Uncommon | ⛏ Legs | Hoverstride | Sprinter / Steady Strider | **Jetstream** — หลังทุบแร่แตก เดินเร็วขึ้น 22% 3 วิ |
| Zephyr | Uncommon | ⛏ Back | Featherlight Harness | Hauler / Collector | **Updraft Pack** — ระยะเก็บแร่ +2 ช่อง |
| Zephyr | Uncommon | ⚔ Helmet | Zephyr Helmet | Precision / Insight | **Wind Sense** — 6% โอกาสขุดคริติคอล ×2 |
| Zephyr | Uncommon | ⚔ Chest Armor | Zephyr Chest Armor | Bulwark / Reactor | **Breath Weave** — ฟื้น HP 1% ทุก 3 วิ ระหว่างต่อสู้ |
| Zephyr | Uncommon | ⚔ Gauntlet | Zephyr Gauntlet | Swift Fist / Heavy Fist | **Wind Blade** — การโจมตีปกติแรงขึ้น 25% ต่อบอสที่ติด Stagger/Freeze |
| Zephyr | Uncommon | ⚔ Leg Armor | Zephyr Leg Armor | Evasion / Charge | **Gale Counter** — Dodge สำเร็จแล้วสวนกลับ 200% ATK |
| Zephyr | Uncommon | ⚔ Back Armor | Zephyr Back Armor | Barrier / Support | **Wind Veil** — Cooldown Red. +10% |
| Drilltech | Rare | ⛏ Head | Surveyor Visor | Deep Scanner / Focus Lens | **Hazard Scanner** — 14% โอกาสขุดคริติคอล ×2 |
| Drilltech | Rare | ⛏ Core | High-Torque Motor | Power Reactor / Engine Tuning | **Hydraulic Core** — ดาเมจขุดแร่ Tier 4+ +15% |
| Drilltech | Rare | ⛏ Arm | Dual-Bit Drill | Rapid Miner / Heavy Breaker | **Twin Drill** — 20% โอกาสที่การขุดจะตีซ้ำทันที |
| Drilltech | Rare | ⛏ Legs | Stabilizer Caterpillar | Sprinter / Steady Strider | **Tread Grip** — ผลกระทบพื้นอันตราย −50% |
| Drilltech | Rare | ⛏ Back | Coolant Cylinder | Hauler / Collector | **Ore Compactor** — 10% โอกาสได้แร่เพิ่ม 1 ก้อนเมื่อแตก |
| Drilltech | Rare | ⚔ Helmet | Drilltech Helmet | Precision / Insight | **Weak Point Scope** — ทำเครื่องหมาย Weak Point ของบอส 2 วิ ทุก 8 วิ |
| Drilltech | Rare | ⚔ Chest Armor | Drilltech Chest Armor | Bulwark / Reactor | **Spike Plate** — สะท้อนดาเมจ 12% กลับใส่ผู้โจมตี |
| Drilltech | Rare | ⚔ Gauntlet | Drilltech Gauntlet | Swift Fist / Heavy Fist | **Drill Burst** — ทุกการโจมตีครั้งที่ 4 ปล่อยคลื่นเพิ่ม 80% ATK |
| Drilltech | Rare | ⚔ Leg Armor | Drilltech Leg Armor | Evasion / Charge | **Thruster Strike** — Dodge สำเร็จแล้วสวนกลับ 160% ATK |
| Drilltech | Rare | ⚔ Back Armor | Drilltech Back Armor | Barrier / Support | **Drone Shield** — ได้โล่ 8% HP ทุก 12 วิ |
| Titanstone | Rare | ⛏ Head | Titan Crest | Deep Scanner / Focus Lens | **Titan Sense** — แร่ Tier 4+ มูลค่า +20% |
| Titanstone | Rare | ⛏ Core | Mountain Furnace | Power Reactor / Engine Tuning | **Colossus Core** — ดาเมจขุดแร่ Tier 4+ +20% |
| Titanstone | Rare | ⛏ Arm | Colossus Breaker | Rapid Miner / Heavy Breaker | **Titan Quake** — การขุดกระแทกแร่ข้างเคียง 40% ของดาเมจ |
| Titanstone | Rare | ⛏ Legs | Seismic Pillar | Sprinter / Steady Strider | **Monolith Step** — ผลกระทบพื้นอันตราย −70% |
| Titanstone | Rare | ⛏ Back | Mass Hauler | Hauler / Collector | **Monolith Vault** — Capacity +25 |
| Titanstone | Rare | ⚔ Helmet | Titanstone Helmet | Precision / Insight | **Titan Gaze** — Crit Damage +20% |
| Titanstone | Rare | ⚔ Chest Armor | Titanstone Chest Armor | Bulwark / Reactor | **Fortress Body** — สะท้อนดาเมจ 20% กลับใส่ผู้โจมตี |
| Titanstone | Rare | ⚔ Gauntlet | Titanstone Gauntlet | Swift Fist / Heavy Fist | **Ground Breaker** — การโจมตีปกติแรงขึ้น 40% ต่อบอสที่ติด Stagger/Freeze |
| Titanstone | Rare | ⚔ Leg Armor | Titanstone Leg Armor | Evasion / Charge | **Heavy Roll** — Dodge Cooldown −12% |
| Titanstone | Rare | ⚔ Back Armor | Titanstone Back Armor | Barrier / Support | **Monolith Ward** — ได้โล่ 8% HP ทุก 9 วิ |
| Magnetite | Rare | ⛏ Head | Ore Beacon | Deep Scanner / Focus Lens | **Field Sensor** — ตรวจพบแร่ซ่อนในระยะ 3 ช่องอัตโนมัติทุก 10 วิ |
| Magnetite | Rare | ⛏ Core | Induction Core | Power Reactor / Engine Tuning | **Polar Core** — คูลดาวน์สกิลแขน −12% |
| Magnetite | Rare | ⛏ Arm | Polarity Claw | Rapid Miner / Heavy Breaker | **Magnetic Shatter** — การขุดกระแทกแร่ข้างเคียง 25% ของดาเมจ |
| Magnetite | Rare | ⛏ Legs | Rail Magnet | Sprinter / Steady Strider | **Glide Field** — หลังทุบแร่แตก เดินเร็วขึ้น 12% 3 วิ |
| Magnetite | Rare | ⛏ Back | Gravity Hopper | Hauler / Collector | **Grand Magnet** — ระยะเก็บแร่ +3 ช่อง |
| Magnetite | Rare | ⚔ Helmet | Magnetite Helmet | Precision / Insight | **Polarity Mark** — ทำเครื่องหมาย Weak Point ของบอส 2 วิ ทุก 9 วิ |
| Magnetite | Rare | ⚔ Chest Armor | Magnetite Chest Armor | Bulwark / Reactor | **Field Mending** — ฟื้น HP 1.2% ทุก 3 วิ ระหว่างต่อสู้ |
| Magnetite | Rare | ⚔ Gauntlet | Magnetite Gauntlet | Swift Fist / Heavy Fist | **Repulse** — การโจมตีปกติแรงขึ้น 25% ต่อบอสที่ติด Stagger/Freeze |
| Magnetite | Rare | ⚔ Leg Armor | Magnetite Leg Armor | Evasion / Charge | **Mag Glide** — Dodge Cooldown −15% |
| Magnetite | Rare | ⚔ Back Armor | Magnetite Back Armor | Barrier / Support | **Shield Generator** — ได้โล่ 8% HP ทุก 7 วิ |
| Prism | Epic | ⛏ Head | Spectrum Seer | Deep Scanner / Focus Lens | **Prism Appraisal** — มูลค่าแร่ทั้งหมด +8% |
| Prism | Epic | ⛏ Core | Refraction Matrix | Power Reactor / Engine Tuning | **Refraction Core** — คูลดาวน์สกิลแขน −15% |
| Prism | Epic | ⛏ Arm | Facet Splitter | Rapid Miner / Heavy Breaker | **Prismatic Strike** — 15% โอกาสขุดคริติคอล ×2 |
| Prism | Epic | ⛏ Legs | Crystal Runner | Sprinter / Steady Strider | **Light Step** — หลังทุบแร่แตก เดินเร็วขึ้น 14% 3 วิ |
| Prism | Epic | ⛏ Back | Rarestone Reliquary | Hauler / Collector | **Crystal Case** — มูลค่าแร่ทั้งหมด +6% |
| Prism | Epic | ⚔ Helmet | Prism Helmet | Precision / Insight | **Prism Lens** — Crit Damage +35% |
| Prism | Epic | ⚔ Chest Armor | Prism Chest Armor | Bulwark / Reactor | **Crystal Heart** — พลัง Ultimate +20% |
| Prism | Epic | ⚔ Gauntlet | Prism Gauntlet | Swift Fist / Heavy Fist | **Shard Volley** — ทุกการโจมตีครั้งที่ 5 ปล่อยคลื่นเพิ่ม 80% ATK |
| Prism | Epic | ⚔ Leg Armor | Prism Leg Armor | Evasion / Charge | **Mirror Step** — Dodge สำเร็จแล้วสวนกลับ 220% ATK |
| Prism | Epic | ⚔ Back Armor | Prism Back Armor | Barrier / Support | **Refract Shield** — Cooldown Red. +10% |
| Frostbound | Epic | ⛏ Head | Frost Sight | Deep Scanner / Focus Lens | **Frost Sight** — ตรวจพบแร่ซ่อนในระยะ 4 ช่องอัตโนมัติทุก 10 วิ |
| Frostbound | Epic | ⛏ Core | Zero-Degree Core | Power Reactor / Engine Tuning | **Cryo Core** — เวลาขุดต่อรอบ +12 วิ |
| Frostbound | Epic | ⛏ Arm | Glacier Chisel | Rapid Miner / Heavy Breaker | **Shatter Wave** — การขุดกระแทกแร่ข้างเคียง 35% ของดาเมจ |
| Frostbound | Epic | ⛏ Legs | Icewalker | Sprinter / Steady Strider | **Ice Skates** — ผลกระทบพื้นอันตราย −80% |
| Frostbound | Epic | ⛏ Back | Cryo Cargo | Hauler / Collector | **Glacier Pack** — Capacity +18 |
| Frostbound | Epic | ⚔ Helmet | Frostbound Helmet | Precision / Insight | **Frost Crown** — 6% โอกาสขุดคริติคอล ×2 |
| Frostbound | Epic | ⚔ Chest Armor | Frostbound Chest Armor | Bulwark / Reactor | **Permafrost** — ฟื้น HP 1.4% ทุก 3 วิ ระหว่างต่อสู้ |
| Frostbound | Epic | ⚔ Gauntlet | Frostbound Gauntlet | Swift Fist / Heavy Fist | **Frost Arc** — การโจมตีปกติแรงขึ้น 30% ต่อบอสที่ติด Stagger/Freeze |
| Frostbound | Epic | ⚔ Leg Armor | Frostbound Leg Armor | Evasion / Charge | **Ice Slide** — Dodge Cooldown −16% |
| Frostbound | Epic | ⚔ Back Armor | Frostbound Back Armor | Barrier / Support | **Glacial Ward** — ได้โล่ 8% HP ทุก 9 วิ |
| Magma | Legendary | ⛏ Head | Molten Crown | Deep Scanner / Focus Lens | **Heat Sense** — แร่ Tier 4+ มูลค่า +18% |
| Magma | Legendary | ⛏ Core | Inferno Reactor | Power Reactor / Engine Tuning | **Furnace Core** — ทุบแร่แตกแล้ว Mining Speed +20 3 วิ |
| Magma | Legendary | ⛏ Arm | Volcanic Burst | Rapid Miner / Heavy Breaker | **Eruption** — 16% โอกาสที่การขุดจะตีซ้ำทันที |
| Magma | Legendary | ⛏ Legs | Basalt Stomper | Sprinter / Steady Strider | **Lava Walker** — ผลกระทบพื้นอันตราย −100% |
| Magma | Legendary | ⛏ Back | Heat-Vent Furnace | Hauler / Collector | **Forge Crucible** — 9% โอกาสได้แร่เพิ่ม 1 ก้อนเมื่อแตก |
| Magma | Legendary | ⚔ Helmet | Magma Helmet | Precision / Insight | **Horned Fury** — Crit Damage +25% |
| Magma | Legendary | ⚔ Chest Armor | Magma Chest Armor | Bulwark / Reactor | **Molten Skin** — สะท้อนดาเมจ 25% กลับใส่ผู้โจมตี |
| Magma | Legendary | ⚔ Gauntlet | Magma Gauntlet | Swift Fist / Heavy Fist | **Magma Wave** — การโจมตีปกติแรงขึ้น 35% ต่อบอสที่ติด Stagger/Freeze |
| Magma | Legendary | ⚔ Leg Armor | Magma Leg Armor | Evasion / Charge | **Flame Dash** — Dodge สำเร็จแล้วสวนกลับ 200% ATK |
| Magma | Legendary | ⚔ Back Armor | Magma Back Armor | Barrier / Support | **Flame Vent** — Cooldown Red. +8% |
| Thunder | Legendary | ⛏ Head | Volt Tracker | Deep Scanner / Focus Lens | **Storm Eye** — 12% โอกาสขุดคริติคอล ×2 |
| Thunder | Legendary | ⛏ Core | Storm Capacitor | Power Reactor / Engine Tuning | **Tesla Core** — คูลดาวน์สกิลแขน −18% |
| Thunder | Legendary | ⛏ Arm | Chain Spark | Rapid Miner / Heavy Breaker | **Chain Strike** — 22% โอกาสที่การขุดจะตีซ้ำทันที |
| Thunder | Legendary | ⛏ Legs | Flashstep | Sprinter / Steady Strider | **Bolt Step** — หลังทุบแร่แตก เดินเร็วขึ้น 20% 3 วิ |
| Thunder | Legendary | ⛏ Back | Battery Array | Hauler / Collector | **Ion Collector** — ระยะเก็บแร่ +2.5 ช่อง |
| Thunder | Legendary | ⚔ Helmet | Thunder Helmet | Precision / Insight | **Storm Crown** — 7% โอกาสขุดคริติคอล ×2 |
| Thunder | Legendary | ⚔ Chest Armor | Thunder Chest Armor | Bulwark / Reactor | **Tesla Reactor** — พลัง Ultimate +18% |
| Thunder | Legendary | ⚔ Gauntlet | Thunder Gauntlet | Swift Fist / Heavy Fist | **Lightning Rush** — ทุกการโจมตีครั้งที่ 3 ปล่อยคลื่นเพิ่ม 80% ATK |
| Thunder | Legendary | ⚔ Leg Armor | Thunder Leg Armor | Evasion / Charge | **Bolt Counter** — Dodge สำเร็จแล้วสวนกลับ 180% ATK |
| Thunder | Legendary | ⚔ Back Armor | Thunder Back Armor | Barrier / Support | **Storm Drone** — Cooldown Red. +12% |
| Void | Mythic | ⛏ Head | Abyss Oracle | Deep Scanner / Focus Lens | **Abyss Appraisal** — มูลค่าแร่ทั้งหมด +10% |
| Void | Mythic | ⛏ Core | Singularity Heart | Power Reactor / Engine Tuning | **Rift Core** — เวลาขุดต่อรอบ +15 วิ |
| Void | Mythic | ⛏ Arm | Phase Cutter | Rapid Miner / Heavy Breaker | **Dimensional Cut** — การขุดกระแทกแร่ข้างเคียง 30% ของดาเมจ |
| Void | Mythic | ⛏ Legs | Blinkwalker | Sprinter / Steady Strider | **Phase Walk** — หลังทุบแร่แตก เดินเร็วขึ้น 18% 3 วิ |
| Void | Mythic | ⛏ Back | Pocket-Dimension Vault | Hauler / Collector | **Pocket Dimension** — Capacity +22 |
| Void | Mythic | ⚔ Helmet | Void Helmet | Precision / Insight | **Abyss Mark** — ทำเครื่องหมาย Weak Point ของบอส 2 วิ ทุก 7 วิ |
| Void | Mythic | ⚔ Chest Armor | Void Chest Armor | Bulwark / Reactor | **Abyss Heart** — พลัง Ultimate +22% |
| Void | Mythic | ⚔ Gauntlet | Void Gauntlet | Swift Fist / Heavy Fist | **Void Flurry** — ทุกการโจมตีครั้งที่ 4 ปล่อยคลื่นเพิ่ม 80% ATK |
| Void | Mythic | ⚔ Leg Armor | Void Leg Armor | Evasion / Charge | **Phase Strike** — Dodge สำเร็จแล้วสวนกลับ 250% ATK |
| Void | Mythic | ⚔ Back Armor | Void Back Armor | Barrier / Support | **Dimensional Shroud** — ได้โล่ 8% HP ทุก 8 วิ |


## 5. Equipment Rebalance Tables
## 5a. Mining part stats (Lv.1 Standard, หลัง Normalize)
| Family | Rarity | Head | Core | Arm | Legs | Back |
|---|---|---|---|---|---|---|
| Scrap | Common | P+4 | P+8 C+5 | P+12 S+2 | M+8 | C+20 |
| Stoneguard | Common | P+4 | P+13 M-5 | P+16 S-8 | P+11 M-12 | C+20 |
| Coppergear | Uncommon | P+2 S+3 | P+6 S+10 | P+8 S+11 | M+11 | C+27 |
| Zephyr | Uncommon | M+4 | P+3 M+10 | P+7 S+8 | M+9 C-5 | M+8 C+12 |
| Drilltech | Rare | P+2 S+3 | P+5 S+9 | P+7 S+9 | S+6 M+3 | S+11 C+9 |
| Titanstone | Rare | P+4 | P+15 S-8 | P+22 S-24 | P+13 M-18 | C+26 M-10 |
| Magnetite | Rare | P+1 C+4 | P+5 C+10 | P+11 S+4 | M+9 | C+26 M-10 |
| Prism | Epic | P+5 | P+11 S+4 | P+13 S+3 | M+9 | C+24 |
| Frostbound | Epic | P+3 S+2 | P+14 | P+14 S+3 | M+9 | C+24 |
| Magma | Legendary | P+5 | P+16 S-5 | P+20 S-10 | P+12 M-8 | P+4 C+20 |
| Thunder | Legendary | P+1 S+3 | P+6 S+9 | P+12 S+5 | M+12 C-8 | S+9 C+14 |
| Void | Mythic | P+5 | P+17 M-6 | P+16 S+3 | P+1 M+9 | C+28 S-6 |

## 5b. Combat armor stats (Standard +0, หลัง Normalize)
| Family | Rarity | Helm | Chest | Gauntlet | Legs | Back |
|---|---|---|---|---|---|---|
| Scrap | Common | hp+55 def+4 cc+2.5 | hp+135 def+9 atk+5 | atk+12 as+4.5 | hp+60 ms+4.5 def+3 | def+7 hp+55 cdr+2.5 |
| Stoneguard | Common | hp+55 def+5 res+3 | hp+160 def+11 | atk+8 stg+5 def+3 | hp+85 def+6 ms-4 | def+9 hp+55 cdr+1 |
| Coppergear | Uncommon | hp+40 cc+3 as+4 | hp+105 atk+6 as+3.5 def+6 | atk+9 as+6.5 | hp+40 ms+6 as+3 | def+6 cdr+4.5 as+3.5 |
| Zephyr | Uncommon | hp+40 cc+3 ms+4 | hp+135 def+7 ms+6 | atk+9 as+5.5 ms+2 | hp+30 ms+10.5 | def+5 ms+6 cdr+4 |
| Drilltech | Rare | hp+45 cc+4 stg+3 | hp+120 atk+7 def+7 | atk+9 stg+4 as+2 | hp+45 def+4 ms+4.5 | def+5 atk+4 cdr+4 |
| Titanstone | Rare | hp+55 def+5 stg+2 | hp+150 def+9 atk+3 | atk+13 stg+7 as-8 | hp+90 def+7 ms-6 | hp+70 def+8 |
| Magnetite | Rare | hp+40 def+4 res+5.5 | hp+140 def+9 atk+3 | atk+10 def+3 cdr+2 | hp+45 def+4 ms+5 | def+7 cdr+4.5 hp+30 |
| Prism | Epic | hp+30 cc+5 cd+10 | hp+115 atk+7 def+5 cd+6.5 | atk+11 cc+2.5 cd+6.5 | hp+50 ms+7 cc+2 | def+6 cdr+5 cd+9 |
| Frostbound | Epic | hp+50 cc+2 res+6.5 | hp+160 def+9 res+4 | atk+11 stg+3 cdr+1.5 | hp+50 def+4 ms+4.5 | def+7 cdr+5 res+3 |
| Magma | Legendary | hp+40 atk+5 cc+3 | hp+130 atk+9 def+5 | atk+13 stg+3 as+1 | hp+60 ms+3.5 atk+4 | def+6 atk+6 cdr+3 |
| Thunder | Legendary | hp+45 cc+5 as+4 | hp+125 atk+7 def+6 as+3.5 | atk+13 as+5.5 | hp+45 ms+9 as+2 | def+6 cdr+6 as+3.5 |
| Void | Mythic | hp+40 cc+4.5 cd+11.5 | hp+135 atk+9 def+6 cd+3.5 | atk+13 cc+2 cd+7 | hp+50 def+4 ms+6.5 | def+5 atk+4 cdr+6 |


## 5c. สูตรค่าสถานะ (Rebalance)
- Mining: `Stat = Base × Level(1+0.12×(Lv−1), ×2.08 ที่ Lv10) × Evolution(1/1.15/1.30) × Quality(0.9/1/1.1/1.2) × Mastery + Path + Rune(คงที่)` — ค่าติดลบไม่ถูกคูณ
- Armor: `Stat = Base × Quality × Enhance(1+0.04×Enh) × Mastery + Path + Rune + Set Bonus`
- Mastery multiplier: `1 + (Lv/30) × (0.60 + 0.08×(5−rarity))` → GM: Common ×2.00 · Uncommon ×1.92 · Rare ×1.84 · Epic ×1.76 · Legendary ×1.68 · Mythic ×1.60
- งบ Stat ต่อชิ้น = งบประเภท × Rarity step [1.00, 1.06, 1.12, 1.18, 1.24, 1.30] (V4 เดิม 1.00–1.75) · ค่าติดลบคืนงบ 25% · ปรับบทบาทจาก Sim: Coppergear ×1.25, Scrap Armor ×1.18, Stoneguard Armor ×1.15
- Overflow: Speed เกิน 250% → +0.5 P/หน่วย · Movement เกิน 200% → +0.25 P/หน่วย
- ตัวอย่าง P: Starter Scrap = 46 (T1) · Full Stoneguard Lv10 Asc Perfect GM + Path + Power Rune III = **536 (T6)**



## 6. Crafting Economy Tables
### Recipes (Standard · Fine ×1.5 + Basic Stone · Perfect ×2 + Advanced Stone + Essence)
| Family | Rarity | Mining Core (Std) | Armor Chest (Std) | Blueprint |
|---|---|---|---|---|
| Scrap | Common | Stone 18, Copper 9 · 🪙200 | Stone 18, Copper 9 · 🪙200 | - |
| Stoneguard | Common | Stone 29, Iron 11 · 🪙350 | Stone 32, Iron 13 · 🪙400 | - |
| Coppergear | Uncommon | Copper 36, Iron 16 · 🪙700 | Copper 35, Iron 16 · 🪙700 | - |
| Zephyr | Uncommon | Copper 24, Silver 16 · 🪙800 | Copper 24, Silver 16 · 🪙750 | - |
| Drilltech | Rare | Iron 32, Silver 15, Copper 21 · 🪙1300 | Iron 35, Silver 17, Copper 22 · 🪙1400 | - |
| Titanstone | Rare | Stone 39, Iron 26, Gold 13 · 🪙1400 | Stone 42, Iron 28, Gold 14 · 🪙1550 | - |
| Magnetite | Rare | Iron 19, Silver 23, Gold 10 · 🪙1400 | Iron 21, Silver 25, Gold 11 · 🪙1550 | - |
| Prism | Epic | Ruby 32, Diamond 27, Ancient Crystal 8 · 🪙8050 | Ruby 34, Diamond 28, Ancient Crystal 8 · 🪙8400 | Mining: 📄6 ต่อแบบ |
| Frostbound | Epic | Silver 54, Diamond 27, Mythril 8 · 🪙8050 | Silver 56, Diamond 28, Mythril 8 · 🪙8400 | Mining: 📄6 ต่อแบบ |
| Magma | Legendary | Ruby 52, Obsidian 35, Mythril 23 · 🪙20400 | Ruby 53, Obsidian 36, Mythril 24 · 🪙20600 | Mining: 📄10 ต่อแบบ · Armor: 📄10 |
| Thunder | Legendary | Gold 52, Diamond 35, Mythril 23 · 🪙20400 | Gold 53, Diamond 36, Mythril 24 · 🪙20600 | Mining: 📄10 ต่อแบบ · Armor: 📄10 |
| Void | Mythic | Obsidian 67, Ancient Crystal 52, Void Crystal 30 · 🪙55450 | Obsidian 68, Ancient Crystal 54, Void Crystal 33 · 🪙54000 | Mining: 📄15 ต่อแบบ · Armor: 📄15 |

### Rune costs
| Tier | Cost | ปลดล็อก |
|---|---|---|
| I | Iron 6, Silver 4 · 🪙300 | - |
| II | Gold 6, Diamond 4 · 🪙1500 | Rune Crafting Lv.5 |
| III | Mythril 5, Celestial Ore 3 · 🪙6000 | Rune Crafting Lv.12 |
| Rune | Line | Stat | I / II / III |
|---|---|---|---|
| ⛏ Power Rune | Mining | P | 5 / 10 / 18 |
| ⚡ Rapid Rune | Mining | S | 6 / 12 / 20 |
| 🎒 Capacity Module | Mining | C | 8 / 16 / 28 |
| 🦿 Stride Rune | Mining | M | 6 / 12 / 20 |
| ⚔ Might Rune | Combat | atk | 3 / 6 / 10 |
| 🎯 Critical Rune | Combat | cc | 2 / 4 / 7 |
| 🔨 Stagger Rune | Combat | stg | 3 / 6 / 10 |
| 🛡 Shield Module | Combat | def | 4 / 8 / 14 |
| ❤ Vital Module | Combat | hp | 40 / 80 / 140 |
| 🌀 Elemental Rune | Combat | res | 4 / 8 / 14 |
| ⏳ Haste Rune | Combat | cdr | 2 / 4 / 7 |



## 7. แหล่ง Blueprint Fragment & วัสดุ (แน่นอน ไม่สุ่ม)
| แหล่ง | 📄 Blueprint Fragment |
|---|---|
| Quick Mining Extract สำเร็จ (ขุด ≥5 ก้อน) | 1 (+2 First Clear ต่อดันเจี้ยน) |
| Deep Expedition (Extract) | ชั้นละ 1 · ครบ 4 ชั้น +2 |
| Endless Milestone ทุก 10 ชั้น | 2–12 |
| Daily Contract | 1–2 · Weekly Expedition 3–6 |
| Combat Boss Hard / Nightmare | +1 / +2 ต่อชนะ |
| ⚙️ Ancient Scrap | 40 → 1 |
| Ticket เก่า (ครั้งเดียว) | 🎫/🎟 1 → 2 |

แร่ทุกชนิดมาจากดันเจี้ยน/Endless ตาม Tier (ผล Sim ต่อรอบ Quick 180 วิ): Quarry ≈ Stone 17 · Copper 14 · Silver 8 · Iron 4 / Iron Depths ≈ Iron 7 · Silver 8 · Gold 6 · Ruby 5 / Ancient Rift ≈ Diamond 4 · Obsidian 5 · Mythril 3 · Ancient Crystal 5 · Celestial/Void Crystal ต้องมี P 440 (Endless B61+ / Rift)

### จำนวนรอบฟาร์มต่อการคราฟต์ 1 ชิ้น (คำนวณจาก Yield จริง)
| Rarity | เป้าหมาย | Mining (เฉลี่ย 5 ช่อง) | Armor (Chest = ชิ้นแพงสุด) |
|---|---|---|---|
| Common | 1–2 | 1.0–1.8 | 1.3–2.3 |
| Uncommon | 2–3 | 2.4–2.8 | 2.8–3.2 |
| Rare | 3–5 | 3.0–5.0 | 3.9–6.4 |
| Epic | 5–8 | 6.3 | 7.8 |
| Legendary | 8–12 | 9.1–10.5 | 11.1–12.7 |
| Mythic | 12–20 | 15.9 | 19 |


## 8. Boss Reward Tables
| Boss | Win (base) | First Clear | Core Fragments (N/H/NM) |
|---|---|---|---|
| Iron Sentinel | 🪙500 · 🎟1 · 🔹2–3 · 🧩5 · ⚙️35% · ⚙️10 · 📘2 · 📄1 | iron×1 · Iron Guardian Blueprint | Iron Guardian Core: 2 / 3 / 5 |
| Magma Warlord | 🪙1,500 · 🎟2 · 🔷4–6 · 🧩10 · 🔥×1 · ⚙️25 · 💠2 · 📄2 | Inferno Blueprint | Magma Warlord Core: 2 / 3 / 5 |
| Void Tyrant | 🪙4,000 · 🎟3 · 💠7–10 · 🧩20 · 🌑×1 · ✴️25% · 💠4 · 📘8 · 📄3 | Abyss Blueprint | Void Tyrant Core: 2 / 3 / 5 |
| Rank | HP | Damage | Coin bonus |
|---|---|---|---|
| Normal | ×1 | ×1 | ×1 |
| Hard | ×2.2 | ×1.4 | ×1.6 + 📄1 |
| Nightmare | ×4 | ×1.9 | ×2.5 + 📄2 |

Mining Boss (เดิม): Ancient Ore Guardian / Magma Colossus / Void Excavator — รางวัลเดิม + Mastery EXP 90 (ชนะ) + Pet Shard (Frostpup/Lavasnail)

## 9. Endless Progression Design
- ชั้นละ 90 วิ (+0.5 วิ/Lv Deep Exploration) · เป้าหมาย 5→13 ก้อน · HP แร่ ×1 → ×3.5 · Modifier 0/1/2/3 แบบตามความลึก · Checkpoint ทุก 5 ชั้น (รางวัลครั้งแรก 🪙 + 🍪) · Milestone ทุก 10 ชั้นถึง B300 · Expedition Storage: แร่ทุกชั้นรวมเป็น Pending Claim เดียว
| ชั้น | Biome | แร่ |
|---|---|---|
| B1–20 | Crystal Cavern | T1/2/3 (45/35/20) |
| B21–40 | Iron Ruins | T2/3/4 (40/35/25) |
| B41–60 | Magma Depths | T3/4/5 (40/35/25) |
| B61–80 | Ancient Abyss | T4/5/6 (45/35/20) |
| B81–100 | Void Dimension | T4/5/6 (35/35/30) |
| B101+ | Endless Anomaly | T5/6/6 (40/35/25) |

| Milestone | Reward |
|---|---|
| B10 | {"coins":500,"bpf":2,"coreFrag":0,"petShard":2,"ess":0} |
| B20 | {"coins":1000,"bpf":2,"coreFrag":0,"petShard":2,"ess":0} |
| B30 | {"coins":1500,"bpf":3,"coreFrag":0,"petShard":2,"ess":0} |
| B40 | {"coins":2000,"bpf":3,"coreFrag":2,"petShard":2,"ess":0} |
| B50 | {"coins":2500,"bpf":3,"coreFrag":2,"petShard":2,"ess":1} |
| B100 | {"coins":5000,"bpf":5,"coreFrag":3,"petShard":2,"ess":1} |
| B150 | {"coins":7500,"bpf":7,"coreFrag":4,"petShard":2,"ess":1} |
| B200 | {"coins":10000,"bpf":8,"coreFrag":6,"petShard":2,"ess":1} |
| B300 | {"coins":15000,"bpf":12,"coreFrag":8,"petShard":2,"ess":1} |

## Daily pool
| Contract | Goal | Reward |
|---|---|---|
| ขุดแร่ 40 ก้อน | 40 | {"coins":400,"bpf":1} |
| Extract สำเร็จ 2 ครั้ง | 2 | {"coins":300,"petShard":2} |
| คราฟต์ชิ้นส่วน 1 ชิ้น | 1 | {"coins":300,"bpf":1} |
| ได้ Mastery EXP 300 | 300 | {"coins":400,"petTreat":1} |
| Pet ใช้สกิล 8 ครั้ง | 8 | {"coins":300,"petTreat":1} |
| ชนะ Combat Boss 1 ครั้ง | 1 | {"coins":600,"coreFrag":1} |
| เก็บแร่เข้าคลัง 30 ก้อน | 30 | {"coins":300,"bpf":1} |
| Dodge สำเร็จ 8 ครั้ง | 8 | {"coins":350,"coreFrag":1} |
| Endless ผ่าน 3 ชั้น | 3 | {"coins":500,"bpf":2} |

## Weekly themes
- 💎 **Crystal Rush Week** — แร่เกิดเยอะ + แร่หายากเพิ่ม · Modifiers: rush + rare · Blueprint Fragment ×2 จาก Weekly Expedition
- 🏰 **Iron Fortress Week** — แร่แข็ง + ซากเครื่องจักร · Modifiers: heavy + machine · Boss Core Fragment +2
- 🌋 **Volcanic Week** — ลาวาเยอะ + สายแร่อุดม · Modifiers: heat + rich · Pet Shard +3
- 🏺 **Ancient Relic Week** — ห้องลับ + แร่ซ่อน · Modifiers: vault + hidden · Blueprint Fragment ×2
- 🌀 **Void Anomaly Week** — มิติบิดเบี้ยว + ทัศนวิสัยต่ำ · Modifiers: distort + dark · Masterwork Essence +1

## Activity Mastery
| Track | ผล |
|---|---|
| Crystal Mining | มูลค่าแร่ Tier 1–3 +0.5%/Lv |
| Metal Mining | ดาเมจขุดแร่ Tier 4–6 +0.5%/Lv |
| Deep Exploration | Endless: เวลาต่อชั้น +0.5 วิ/Lv |
| Ore Collection | Capacity +1/Lv |
| Weapon Mastery | ATK +0.5%/Lv |
| Defense Mastery | DEF +0.5/Lv |
| Dodge Mastery | Dodge Cooldown −0.5%/Lv |
| Boss Hunter | ดาเมจใส่บอส +0.5%/Lv |
| Elemental Mastery | Res +0.5%/Lv |
| Mining Part Crafting | Lv5 ปลดล็อก Fine · ค่าคราฟต์ −1%/Lv |
| Combat Armor Crafting | Lv5 ปลดล็อก Fine · ค่าคราฟต์ −1%/Lv |
| Rune Crafting | Lv5 ปลดล็อก Rune II · Lv12 Rune III |
| Mechanical Assembly | Upgrade/Enhance ถูกลง 1%/Lv |
| Masterwork Crafting | Lv8 ปลดล็อก Perfect Crafting |


## 10. Responsive UI Design
- **Desktop ≥1200px**: 3 คอลัมน์ — [แท็บ + เนื้อหา] · [แท่นโกเลม + ช่องชิ้นส่วน] · [การ์ดค่าสถานะ/สกิล/เควสต์]
- **Tablet 768–1199px**: 2 คอลัมน์ — แท่นโกเลม + เนื้อหาแท็บ (ซ้าย) · การ์ด (ขวา) · ปุ่มใหญ่ 3 ต่อแถว
- **Mobile <768px**: ซ้อนแนวตั้ง · Bottom Navigation 5 แท็บ (ปุ่ม ≥54px) · Modal เป็น Bottom Sheet (ซ่อน Bottom Nav ขณะเปิด) · ไม่มี Scroll แนวนอน
- **HOME**: Quick Mining ทางลัด · แจ้งเตือน (รางวัลค้าง, Daily สำเร็จ, Wishlist พร้อม, Pet พร้อมวิวัฒนาการ, ไข่พร้อมฟัก, เลือก Specialization, Weekly) · Daily · Weekly · Wishlist
- **GOLEM**: Garage / Combat Loadout / Inventory / Presets / Part Mastery + Mastery ของชิ้นที่ใส่
- **ADVENTURE**: Quick / Deep / Endless / Mining Boss / Arena (เลือกระดับ) / Training / Raid / Contracts + ข้อมูลก่อนเข้า (P แนะนำ, P ของคุณ, Pet, เวลา, รางวัล)
- **WORKSHOP**: แท็บ CRAFT · ENHANCE · MASTERWORK · RUNES · STORAGE · BLUEPRINTS · MASTERY
- **PET**: My Pets · Eggs · Feed & Cook · Codex · รายละเอียด (ตั้งชื่อ, Feed, Evolve, ติดตั้ง Mining/Combat)
- การ์ดชิ้นส่วนทุกจุดแสดง ⭐ Mastery Lv + แถบ EXP · หน้ารายละเอียดชิ้นส่วนมี Mastery Panel + Rune Socket
- บนตัวโกเลม: Mastery Lv.10 ประกายบรอนซ์ · Lv.20 วงแหวนเงิน · Lv.30 ออร่าทอง (ครบ 6 ชิ้น GM มีดาวมงกุฎ) · Pet แต่ละ Stage รูปร่าง/ขนาด/อุปกรณ์ต่างกัน

## 11. Save Migration Report
| ข้อมูล | ผล |
|---|---|
| ตรวจ Save เก่า | อ่าน `migrationVersion` จาก JSON ดิบ (ก่อน merge ค่า default) → Migrate ครั้งเดียว |
| สำรอง | เก็บ Save ก่อน V5 ไว้ที่ `buildAGolem_backup_pre_v5` (ดาวน์โหลดได้ใน Settings) |
| ชิ้นส่วน / เกราะ / Equip / Presets / UID / Level / Evolution / Quality / Trait | คงเดิมทั้งหมด (ทดสอบเทียบ JSON ตรงทุก Field) |
| Ticket | 🎫 → 📄×2 + 🪙50 · 🎟 → 📄×2 + 🔹1 · ยอดเหลือ 0 จึงแปลงซ้ำไม่ได้ |
| Veteran Mastery | ต่อ Design ที่มี: Mining `min(25, 2×Lv + 5×Evo)` · Armor `min(25, 2 + 2×Enh (+3 ถ้า Perfect))` — ไม่ให้ไอเท็ม |
| Endless | Checkpoint = ชั้นลึกสุดเดิมปัดลง 5 + 1 · Milestone V5 ที่ต่ำกว่าสถิติเดิมให้ย้อนหลังครั้งเดียว |
| Field ใหม่ | petInventory · equippedMiningPet · equippedCombatPet · (petExperience/petBond/petEvolution อยู่ในแต่ละ Pet: exp/bond/stage) · partMastery · masterySpecializations · craftingMastery/activityMastery · blueprints · oreStorage · craftingWishlist · bossFragments · endlessCheckpoints · dailyProgress · weeklyProgress · migrationVersion |
| Idempotent | Reload ซ้ำ: ไม่มีไอเท็ม/เงิน/Fragment เพิ่ม (ทดสอบแล้ว) |

**ผลกระทบต่อพลังผู้เล่นเก่า (ตรงไปตรงมา):** เพราะ Rarity ไม่ใช่พลังดิบอีกต่อไป Build Rarity สูงของผู้เล่นเก่าจะ P ลดลงหลังย้ายมา V5 แม้ได้ Veteran Mastery แล้ว เช่น Full Void Lv10 Ascended: V4 P448 (T6) → V5 P236 (T4) · Mix Legendary/Mythic Lv10: P451 → P273 · ส่วน Common ได้ประโยชน์ เช่น Full Stoneguard Lv10 Ascended: P164 (T3) → P310 (T5). ทางกลับสู่ T6 คือ Mastery 30 + Specialization + Power Rune (Full Stoneguard GM = P536 T6). ดันเจี้ยนที่ปลดล็อกแล้วยังเข้าได้เสมอ

## 12. Balance Simulation Results (Engine จริง)
### Mining — 10 Builds · Lv10 · Ascended · Perfect · Mastery 30 · Path ตามบทบาท · Power Rune III · 4 Seeds × 180 วิ
| Build | Rarity | บทบาท | P / Tier | Iron Depths แร่/นาที · 🪙/นาที | Ancient Rift แร่/นาที · 🪙/นาที |
|---|---|---|---|---|---|
| scrap | Common | allround | 372 / T5 | 32.1 · 2660 | 19.9 · 5376 |
| stoneguard | Common | power | 536 / T6 | 30.3 · 2514 | 21.8 · 11970 |
| coppergear | Uncommon | speed | 315 / T5 | 39.3 · 3028 | 26.8 · 7226 |
| drilltech | Rare | speed | 320 / T5 | 45.4 · 3414 | 28.3 · 8175 |
| titanstone | Rare | power | 605 / T6 | 19.7 · 1604 | 16.1 · 9639 |
| magnetite | Rare | haul | 300 / T5 | 27.9 · 2175 | 18.4 · 4918 |
| prism | Epic | allround | 379 / T5 | 36 · 3109 | 22.5 · 6986 |
| magma | Legendary | power | 567 / T6 | 28.1 · 2470 | 20.4 · 12292 |
| thunder | Legendary | speed | 311 / T5 | 47.8 · 3228 | 30.4 · 8150 |
| void | Mythic | allround | 429 / T5 | 32.3 · 2711 | 22.9 · 6600 |

**เทียบในบทบาทเดียวกัน (🪙/นาที):**
- Power: Stoneguard (Common) vs Magma (Legendary) — Depths +2% · Rift −3% ✅ · Titanstone (Rare) ช้ากว่า (Depths −36%, Rift −19%) แต่ P สูงสุด (605) เหมาะบอสเหมือง
- All-round: Scrap (Common) vs Void (Mythic) — Depths −2% · Rift −19% ⚠️ (นอกเป้า ±15% เล็กน้อยใน Rift)
- Speed: Coppergear (Uncommon) vs Thunder (Legendary) — Depths −6% · Rift −11% ✅
- Common GM ขุด Tier 6 ได้จริง (Stoneguard P536)


### Combat — 10 Builds · +10 · Perfect · Mastery 30 · Normal (ก่อนปรับ Common Armor)
| Build | Rarity | CP | Iron Sentinel | Magma Warlord | Void Tyrant |
|---|---|---|---|---|---|
| Full Rust Knight | Common | 915 | 100% · 25s · DPS 280 | 100% · 58s · DPS 347 | 100% · 150s · DPS 288 |
| Full Stone Warden | Common | 846 | 100% · 22s · DPS 360 | 100% · 63s · DPS 326 | 100% · 150s · DPS 287 |
| Full Iron Breaker | Rare | 903 | 100% · 11s · DPS 625 | 100% · 26s · DPS 834 | 100% · 59s · DPS 673 |
| Full Colossus Guardian | Rare | 925 | 100% · 22s · DPS 321 | 100% · 55s · DPS 372 | 100% · 128s · DPS 341 |
| Full Inferno Overlord | Legendary | 977 | 100% · 8s · DPS 915 | 100% · 46s · DPS 436 | 100% · 63s · DPS 653 |
| Full Storm Emperor | Legendary | 1006 | 100% · 8s · DPS 838 | 100% · 26s · DPS 767 | 100% · 55s · DPS 689 |
| Full Abyss Sovereign | Mythic | 932 | 100% · 8s · DPS 1042 | 100% · 26s · DPS 798 | 100% · 98s · DPS 456 |
| Mixed Tank (2 Stone + 4 Titan) | Rare | 894 | 100% · 17s · DPS 422 | 100% · 50s · DPS 417 | 100% · 127s · DPS 354 |
| Mixed Speed (2 Thunder + 2 Copper | Legendary | 996 | 100% · 20s · DPS 370 | 100% · 52s · DPS 387 | 100% · 151s · DPS 286 |
| Mixed Critical (4 Prism + Void + Z | Mythic | 962 | 100% · 14s · DPS 725 | 100% · 31s · DPS 687 | 100% · 93s · DPS 500 |

### Combat — หลังปรับ Common Armor (+18%/+15%) · Normal
| Build | Rarity | CP | Iron Sentinel | Magma Warlord | Void Tyrant |
|---|---|---|---|---|---|
| Full Rust Knight | Common | 1015 | 100% · 18s · DPS 386 | 100% · 57s · DPS 352 | 100% · 138s · DPS 296 |
| Full Stone Warden | Common | 919 | 100% · 22s · DPS 340 | 100% · 59s · DPS 341 | 100% · 141s · DPS 298 |
| Full Colossus Guardian | Rare | 925 | 100% · 22s · DPS 329 | 100% · 56s · DPS 365 | 100% · 134s · DPS 327 |
| Full Abyss Sovereign | Mythic | 932 | 100% · 10s · DPS 743 | 100% · 29s · DPS 714 | 100% · 99s · DPS 472 |

### Combat — Nightmare (×4 HP, ×1.9 Damage) · ก่อนปรับ
| Build | Rarity | CP | Iron Sentinel | Magma Warlord | Void Tyrant |
|---|---|---|---|---|---|
| Full Rust Knight | Common | 915 | 100% · 77s · DPS 355 | 50% · 272s · DPS 296 | 0% · -s · DPS 271 |
| Full Stone Warden | Common | 846 | 100% · 81s · DPS 337 | 0% · -s · DPS 265 | 0% · -s · DPS 254 |
| Full Iron Breaker | Rare | 903 | 100% · 34s · DPS 825 | 50% · 129s · DPS 642 | 0% · -s · DPS 553 |
| Full Colossus Guardian | Rare | 925 | 100% · 61s · DPS 454 | 100% · 246s · DPS 326 | 0% · -s · DPS 330 |
| Full Inferno Overlord | Legendary | 977 | 100% · 42s · DPS 671 | 50% · 165s · DPS 458 | 0% · -s · DPS 706 |
| Full Storm Emperor | Legendary | 1006 | 100% · 27s · DPS 1024 | 100% · 107s · DPS 748 | 50% · 218s · DPS 439 |
| Full Abyss Sovereign | Mythic | 932 | 100% · 30s · DPS 970 | 100% · 98s · DPS 826 | 0% · -s · DPS 480 |
| Mixed Tank (2 Stone + 4 Titan) | Rare | 894 | 100% · 57s · DPS 483 | 100% · 226s · DPS 357 | 0% · -s · DPS 354 |
| Mixed Speed (2 Thunder + 2 Copper | Legendary | 996 | 100% · 72s · DPS 377 | 0% · -s · DPS 369 | 0% · -s · DPS 298 |
| Mixed Critical (4 Prism + Void + Z | Mythic | 962 | 100% · 30s · DPS 919 | 100% · 127s · DPS 634 | 0% · -s · DPS 553 |

### Combat — Nightmare · หลังปรับ Common Armor
| Build | Rarity | CP | Iron Sentinel | Magma Warlord | Void Tyrant |
|---|---|---|---|---|---|
| Full Rust Knight | Common | 1015 | 100% · 75s · DPS 366 | 100% · 233s · DPS 344 | 0% · -s · DPS 325 |
| Full Stone Warden | Common | 919 | 100% · 74s · DPS 375 | 100% · 255s · DPS 315 | 0% · -s · DPS 268 |
| Full Colossus Guardian | Rare | 925 | 100% · 66s · DPS 416 | 100% · 248s · DPS 325 | 0% · -s · DPS 297 |
| Mixed Tank (2 Stone + 4 Titan) | Rare | 922 | 100% · 58s · DPS 480 | 100% · 218s · DPS 368 | 0% · -s · DPS 348 |

### Combat — +10 · Standard · Mastery 0 (เทียบ: ไม่มี Mastery)
| Build | Rarity | CP | Iron Sentinel | Magma Warlord | Void Tyrant |
|---|---|---|---|---|---|
| Full Rust Knight | Common | 465 | 100% · 52s · DPS 133 | 100% · 149s · DPS 135 | 0% · -s · DPS 103 |
| Full Stone Warden | Common | 438 | 100% · 40s · DPS 169 | 100% · 145s · DPS 138 | 0% · -s · DPS 124 |
| Full Iron Breaker | Rare | 493 | 100% · 28s · DPS 245 | 100% · 64s · DPS 317 | 0% · -s · DPS 253 |
| Full Colossus Guardian | Rare | 468 | 100% · 41s · DPS 169 | 100% · 119s · DPS 170 | 100% · 264s · DPS 175 |
| Full Inferno Overlord | Legendary | 529 | 100% · 21s · DPS 336 | 100% · 91s · DPS 223 | 100% · 126s · DPS 331 |
| Full Storm Emperor | Legendary | 556 | 100% · 17s · DPS 406 | 100% · 59s · DPS 337 | 0% · -s · DPS 175 |
| Full Abyss Sovereign | Mythic | 545 | 100% · 24s · DPS 345 | 100% · 61s · DPS 335 | 100% · 249s · DPS 193 |
| Mixed Tank (2 Stone + 4 Titan) | Rare | 465 | 100% · 35s · DPS 200 | 100% · 120s · DPS 167 | 100% · 263s · DPS 159 |
| Mixed Speed (2 Thunder + 2 Copper | Legendary | 540 | 100% · 55s · DPS 125 | 50% · 117s · DPS 148 | 0% · -s · DPS 115 |
| Mixed Critical (4 Prism + Void + Z | Mythic | 514 | 100% · 34s · DPS 217 | 100% · 79s · DPS 257 | 100% · 251s · DPS 184 |

**สรุป Combat:** Tank: Stone Warden (Common) vs Colossus Guardian (Rare) ต่างกัน 3–5% ✅ · Common ที่ GM ชนะบอสทุกตัวในระดับ Normal ✅ · ระดับ Nightmare ของ Void Tyrant ยังแทบไม่มีใครชนะ (เป้าหมายระยะยาว — Sim ยังไม่ใส่ Pet/Rune) · **สาย DPS ยังห่าง**: Rust Knight (Common, All-round) DPS ต่ำกว่า Abyss Sovereign (Mythic) ~37% ใน Tyrant ⚠️ (บทบาทต่างกัน แต่ยังไม่ถึงเป้า ±15%)


## 13. Testing Report
### V5 Acceptance (60 checks ครอบคลุม Part 32 ข้อ 1–45) — **60/60 ผ่าน**
| ผล | Test |
|---|---|
| ✅ | 2 Old (V4) save loads in V5 |
| ✅ | 3 Equipment kept (mining inv/equip/presets + combat inv/equip, UIDs) |
| ✅ | 2b Tickets converted once: 🎫→📄×2+50🪙, 🎟→📄×2+Basic Stone (+ retro Endless milestones), no items granted frag 0→36 |
| ✅ | 2c Veteran mastery granted (no items) + Endless retro checkpoint |
| ✅ | 2d Migration idempotent after reload |
| ✅ | 37a Fresh start: starter golem mines Tier 1, quarry free (no softlock) |
| ✅ | 1 Mining system works |
| ✅ | 27 Quick Mining = 180 s run |
| ✅ | 1b Extract → pending claim, Blueprint Fragments instead of tickets frag +3 |
| ✅ | 13 Mastery EXP from the real run (per equipped design) |
| ✅ | 12 Same arm design on both arms → one mastery entry, no double EXP |
| ✅ | 5 Ore Storage stores exactly the chosen ores, rest sold once |
| ✅ | 13b Extract immediately (0 ores) → no mastery EXP (anti-exploit) |
| ✅ | 11 Scrap Head mastery grows while unequipped Scrap Core does not |
| ✅ | 10 Mastery is per design (keys are design IDs) |
| ✅ | 4 Craft gives exactly the chosen part (no gacha) drilltech_arm |
| ✅ | 6 Crafting deducts materials exactly once |
| ✅ | 6b Not enough materials → nothing deducted |
| ✅ | 7 Blueprint locks Mythic craft, unlocks with fragments, then crafts |
| ✅ | 8 Quality: Fine/Perfect craft deterministic (gated), Improve Quality works |
| ✅ | 9 Rune changes stats for real (+18 P) and returns on unsocket |
| ✅ | 14 Mastery Lv.30 activates the Grandmaster effect |
| ✅ | 14b GM effect changes gameplay (Bedrock Core: +12% damage to Tier 4+ ore) |
| ✅ | 16 Fully developed Common (Stoneguard GM + Power Runes) mines Tier 6 P536 |
| ✅ | 15 Common GM within 15% of Legendary in the same (power) role 536 vs 567 |
| ✅ | 17 Common vs Mythic specialize differently (Stoneguard=Power, Void=Speed/Move/Capacity) |
| ✅ | 18 Higher rarity does not dominate (Common GM beats Mythic GM in Power; Mythic leads only at Lv.0 without mastery) GM mult C 2 M 1.6 |
| ✅ | 19 Pet inventory: first pet free, target eggs → hatch → equip |
| ✅ | 25 Pet Bond Lv.3 has a real effect (Magnetot: Capacity +8) 8 |
| ✅ | 20 All 14 pets have distinct shapes & abilities |
| ✅ | 24 Pet evolution stages change the drawn shape (size/pixels differ per stage) |
| ✅ | 24b Pet level caps at stage (Lv10) → evolve → continues to Lv20 |
| ✅ | 26 Pet synergy (2+ same-family parts) is real: skill cooldown −20% + link visual |
| ✅ | 21 Pet follows the golem in the mine (distance stays small) |
| ✅ | 22 Mining pet uses its skill and helps (Magnet Burst pulls ores) |
| ✅ | 23 Combat pet fights the boss (Emberfox Foxfire damage) |
| ✅ | 31 Boss Core Fragments guaranteed (Normal 2 · Nightmare 5), 5 → 1 Core, same fight never pays twice, ranks unlock |
| ✅ | 32 Enhancement never removes the item (+10 reached, Assembly mastery grows) |
| ✅ | 33 Wishlist shows exact missing materials, farm location, runs, blueprint & ready alert |
| ✅ | 34 Daily reward cannot be claimed twice |
| ✅ | 35 Weekly rollover only resets weekly progress (no permanent loss) |
| ✅ | 28 Deep Expedition: 4 floors → one pending claim + fragments + weekly progress |
| ✅ | 29 Endless checkpoints every 5 floors saved (B6, B11) |
| ✅ | 30 Endless checkpoint/milestone rewards never paid twice (after reload) |
| ✅ | 29b Start from a checkpoint only if unlocked |
| ✅ | 36 No craft→dismantle/sell profit loop; repeated saves never mint currency |
| ✅ | 37 No progression softlock (0 coins → free dungeon → ores → craft materials) |
| ✅ | 42 Save/Load keeps all V5 fields (mastery, pets, runes, wishlist, checkpoints, fragments) |
| ✅ | 42b Export → Import restores the save (with backup) |
| ✅ | 44 Not too fast: one design to Grandmaster ≈ 120+ real runs; Mythic craft ≥ 12 runs |
| ✅ | 45 Long-term goals after a full build (120 GM designs, 30 Endless milestones, Nightmare ranks, 14 pets × 3 stages, 14 activity tracks) |
| ✅ | 43 Balance simulation functions present & runnable |
| ✅ | UI: all V5 screens open without errors |
| ✅ | 38 PC (≥1200: 3 columns) |
| ✅ | 38 PC (≥1200: 3 columns) · Forge modal fits (no horizontal page scroll) |
| ✅ | 39 Tablet (768–1199: 2 columns) |
| ✅ | 39 Tablet (768–1199: 2 columns) · Forge modal fits (no horizontal page scroll) |
| ✅ | 40 Mobile (<768: stacked + bottom nav) |
| ✅ | 40 Mobile (<768: stacked + bottom nav) · Forge modal fits (no horizontal page scroll) |
| ✅ | 41 No JavaScript errors |
| ❌ |  |

### Regression
- V4 Acceptance (72): **69/72** — 3 ข้อที่ไม่ผ่านเป็นการเปลี่ยนตามสเปก V5: (35) Save version = 5 ไม่ใช่ 4 · (16/16b) Starter armor ไม่ให้ Combat Ticket 5 ใบแล้ว แต่ให้ 📄10 แทน (ตรวจแล้ว: ได้ชุดเริ่มต้น 6 ชิ้น สวมครบ ไม่ซ้ำหลัง Reload)
- V3 Regression: ข้อที่เปลี่ยนตามสเปก — สูตร Quality/Level (V5 ใช้ +12%/Lv), Save V2→V3 (Coins ต่างเพราะแปลง Ticket), Mining Boss test ใช้ Build Mythic ที่ไม่มี Mastery (V5 P177 < 190 ตามดีไซน์ Equal Potential)
- ไม่มี JavaScript Error ในการทดสอบทั้งหมด · ภาพหน้าจอ Desktop 1440 / Tablet 1024 / Mobile 390 ตรวจแล้ว ไม่มี Scroll แนวนอน


## 14. รายการที่ยังไม่ได้ทำจริง / ทำได้บางส่วน (ตามจริง)
1. **Grandmaster Effect 120 แบบ** มีชื่อและค่าเฉพาะต่อแบบครบ 120 แต่สร้างจาก *กลไกพื้นฐาน 26 ชนิด* (เช่น Echo, Splash, Refund, Thorns, Counter, Aegis) ที่ปรับตัวเลขต่างกัน — ยังไม่ใช่ 120 กลไกที่ไม่ซ้ำกันเลย
2. **Pet บนแท่นโกเลมหน้า Workshop / Garage ยังไม่แสดง** — Pet เดินตามจริงในดันเจี้ยน, Endless, Deep, Mining Boss และสนาม Combat; หน้า Home/Pet แสดงเป็นภาพ Thumbnail/ภาพเคลื่อนไหวในหน้า Pet
3. **HUD สถานะ Pet** เป็นวงแหวนคูลดาวน์เหนือหัว Pet + ข้อความตอนใช้สกิล — ยังไม่มีแถบ HUD แยกในแถบบน
4. **ความสามารถ Pet บางตัวทำแบบประมาณ**: Mechafinch (Combat) “เห็นท่าบอสถัดไป” = ยืดเวลาสัญญาณเตือนท่าบอส +0.12 วิ (+0.25 วิ ตอนใช้สกิล) · Driftling Bond = Pet EXP +20% (ส่วน “Utility เล็กน้อย” ยังไม่มีผลเพิ่ม) · เอฟเฟกต์สกิล Pet ใช้ชุด FX เดิม (วง/สายฟ้า/ลำแสง) ไม่ใช่แอนิเมชันเฉพาะตัว
5. **Mastery Visual** เป็น Overlay (ประกาย/วงแหวน/ออร่า) บนตำแหน่งชิ้นส่วน — โมเดลชิ้นส่วนไม่ได้เปลี่ยนรูปทรง
6. **Equal Potential ยังไม่สมบูรณ์ทุกบทบาท**: Combat สาย DPS — Common (Rust Knight) ต่ำกว่า Mythic (Abyss) ~37% · Mining All-round ใน Ancient Rift: Scrap −19% vs Void · Titanstone ขุดช้ากว่าสาย Power อื่น 19–36% (แต่ P สูงสุด)
7. **ผู้เล่นเก่าที่ใช้ Rarity สูงจะ P ลดลง** หลังย้ายมา V5 (ดู §11) — ต้องเก็บ Mastery/ใส่ Rune เพื่อกลับไป Tier 6
8. **Weekly Theme** มีผลกับ Deep Expedition เท่านั้น (Quick Mining ใช้ Modifier หมุนเวียนรายสัปดาห์แบบเดิมที่ขยายเป็น 13 แบบ)
9. **Performance**: ไม่ได้เพิ่มระบบ Performance ใหม่ นอกจากเพดานจำนวนข้อความลอย, Pet animation ทำงานเฉพาะตอนเปิดหน้าต่าง Pet และระบบ Graphics Quality เดิม
10. **Balance Simulation** ใช้บอทจำลอง (ทักษะ 0.85) และยังไม่รวม Pet/Rune ใน Combat Sim — ผลจริงของผู้เล่นอาจต่างได้ · ยังไม่มีการทดสอบโดยผู้เล่นจริง
11. Save อยู่ในเครื่อง (localStorage) + Export/Import — ยังไม่มี Cloud Save
12. Test Regression ของ V3/V4 บางข้อไม่ผ่านเพราะสเปก V5 เปลี่ยนกติกา (อธิบายใน §13)
