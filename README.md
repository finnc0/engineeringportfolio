# Hi, I'm Finn 👋

Software Engineering student at the College of Charleston (International Studies minor, class of 2029). I build across the stack, from embedded firmware on STM32 to Spring Boot backends to shipped mobile apps, and I'm happiest when software touches real hardware, especially cars.

📧 finndev00@gmail.com

---

## Projects

### ⚡ Makalu PDM: Automotive Power Distribution Firmware
**[makalu_pdm](https://github.com/ascent-electronics/makalu_pdm) · [makalu_sdk](https://github.com/ascent-electronics/makalu_sdk)** · C, FreeRTOS, STM32F446, CAN, I²C

<!-- 📷 Add: photo of the PDM board on the bench + a CAN log screenshot -->

**Context:** Firmware for a 32-channel solid-state power distribution module that replaces a car's fuse and relay box. It is built for Ascent Electronics on beta hardware, in a two-person team, was started to be tested in a E46 track car.

**My role:** I wrote the CAN driver, the bus protocol, the EEPROM driver stack, and the PDM application layer. That covers the coding/config protocol, commissioning checks, and heartbeat.

**What & how:** Every module is controlled over CAN through a custom protocol with 29-bit IDs. The PDM stores its configuration in a versioned EEPROM register map, including per-channel current limits, fuse ratings, boot states, and the VIN. A PC tool can read and write that configuration over CAN with ACK/NACK responses. Shared code lives in a separate SDK that every future module links against.

**Why:**
- **Priority, source, and message type are packed into the CAN ID**, so bus arbitration follows message priority automatically and frames can be routed without parsing payloads.
- **The ISR only copies frames into a ring buffer, and a task dispatches them.** The hardware FIFO holds just 3 frames, so keeping the interrupt tiny prevents dropped messages during bursts.
- **EEPROM access goes through a driver, adapter, and generic interface**, so moving to a bigger chip is a two-file change with no application edits.
- **Long values like the VIN are sent as chunked writes** and committed only once every byte arrives, because a CAN frame carries 8 bytes and a half-written VIN is worse than none.

**Outcome:** CAN communication, persistent config, and the coding protocol run on hardware. Channel switching, current sensing, and OTA updates are next.

---

### 🔧 Garage Companion: Car Mod & Maintenance Tracker API
**[garage_companion_backend](https://github.com/finnc0/garage_companion_backend)** · Java 21, Spring Boot, PostgreSQL, JWT

<!-- 📷 Add: API architecture diagram or a Postman screenshot of register/login -->

**Context:** A personal project to rebuild my Java skills after two years away from it. It's also something I actually want for tracking work on my BMW. This repo is the backend; a Next.js mobile frontend is planned.

**My role:** Solo, covering design, implementation, and deployment.

**What & how:** A REST API for tracking vehicles, mods, and maintenance. It uses stateless JWT authentication (Spring Security and jjwt), with register/login endpoints and a per-request token filter. It is deployed on Render with a hosted Postgres database.

**Why:**
- **Account (credentials and role) is separate from User (profile)**, so authentication concerns never leak into profile logic, and `app_user` sidesteps Postgres's reserved `USER` keyword.
- **Each feature package is split into `domain/` and `web/`** to keep business logic independent of HTTP request and response types.
- **I tried JdbcTemplate and jOOQ before settling on JPA/Hibernate.** It removed the most boilerplate for a CRUD-heavy domain.
- **Diagnostics and CAN logging were deliberately cut** to ship a focused mod and maintenance tracker first.

**Outcome:** Auth is live and deployed. The vehicle, mod, and maintenance endpoints are in progress.

---

### 📚 Cram AI: AI-Powered Study App
**[cram](https://github.com/finnc0/cram)** · Mobile · **Live on the App Store**

<!-- 📷 Add: 2–3 App Store screenshots + App Store link -->

**Context:** A personal product I took from idea to App Store release.

**My role:** <!-- e.g. Solo developer / co-founder, fill in -->

**What & how:** Cram tracks how a student studies, including time spent per activity, answer choices, and patterns in mistakes. It uses that data to adapt their learning path and study methods.

**Why:** Most study apps use the same method for everyone. Cram adapts the method to each person, based on what actually works for them.

**Outcome:** Shipped and publicly available on the App Store. <!-- add user count / ratings if you have them -->

---

### 🐾 Animal Shelter Manager
**[animalshelter](https://github.com/finnc0/animalshelter)** · Java · CSCI 221 (Projects 1 & 2)

**Context:** A class project. The assignment was a menu-driven console app to manage shelter animals and adopters, loaded from CSV data.

**My role:** Solo.

**What & how:** Supports full create, read, update, and delete operations for animals and owners, an adoption flow, and CSV ingestion that handles missing fields. Dogs, cats, birds, and horses extend an abstract `Animal`, and shared capabilities are defined as interfaces (`Adoptable`, `Displayable`, `Behavior`).

**Why:** I used interfaces for capabilities and inheritance for identity. An animal *is a* dog but *can be* adopted, so adding a new species means writing one subclass without touching the menus. Managers own the data, and menus only handle input and output, which kept the input-validation fixes contained to one layer.

---

### ⚔️ DuelsPlugin: Minecraft PvP Minigame
**[DuelsPlugin](https://github.com/finnc0/DuelsPlugin)** · Java, Spigot API, MongoDB

<!-- The existing repo README already has screenshots; link or embed 1–2 here -->

**Context:** My first real Java project (2023), started about a month into learning the language.

**What & how:** A multiplayer duels plugin with matchmaking queues, several game modes (Classic, BuildUHC, and Bridge, which has its own arena system), duel requests, GUI menus, and player stats stored in MongoDB.

**Why:** Kits, arenas, and locations all live in MongoDB rather than config files, so a server can be set up in-game. Player stats are written only when a player leaves or the server shuts down, which keeps database traffic low.

**Outcome:** It's where I learned OOP, event-driven design, and managing game state. You can compare it with Garage Companion to see how my Java has changed.

---

## Toolbox
**Languages:** C · Java · TypeScript · Swift · Python
**Embedded:** STM32 HAL · FreeRTOS · CAN · I²C · CMake
**Backend:** Spring Boot · PostgreSQL · JPA/Hibernate · MongoDB · JWT
**Frontend / Mobile:** React / Next.js · React Native (Expo) · SwiftUI
