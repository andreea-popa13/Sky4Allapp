# Sky4All — In-Flight Passenger & Cabin Crew Communication Platform

**Sky4All** is an offline-first, real-time in-flight communication and cabin management application designed for **Passengers** and **Cabin Crew (Stewards)**. It enables seamless onboard interaction during flight using **Ditto Peer-to-Peer (P2P) Mesh** and synchronizes flight manifests, orders, medical alerts, seat complaints, and ratings with **Ditto Big Peer Cloud** and **Microsoft SQL Server (SSMS 22)** before takeoff and after landing.

---

## 1. Application Features

### A. Passenger Application Features
- **3-Step Flight Boarding & Seat Verification**:
  1. **Flight Check-In**: Passengers enter their flight number and select their flight date on an interactive calendar, greeted by the Sky4All hero banner (*"Your comfort, our priority"*).
  2. **Seat Selection**: Passengers select their assigned seat letter (`A`–`F`) and row (`1`–`30`), validated against the flight reservation manifest.
  3. **Passenger Profile & Medical Onboarding**: Passengers confirm their name and optionally register pre-existing medical conditions (*Diabetes*, *Asthma*, *Heart condition*, *Epilepsy*, *Pregnancy*, *Allergies*, or *None*). Selecting *Allergies* opens a detailed multi-select sub-menu (*Peanuts*, *Tree nuts*, *Milk / lactose*, *Eggs*, *Gluten*, *Soy*, *Fish*, *Shellfish*, *Sesame*, *Penicillin*, *Latex*, *Pollen*, or custom *Other* text) so cabin crew are immediately informed in case of an emergency.
- **Instant Medical Emergency Alert**:
  - Dedicated high-visibility red **Medical Emergency** button with confirmation protection.
  - Passengers can specify the exact nature of the emergency (*Cardiac problem*, *Hypoglycemia*, *Fainting*, *Dizziness*, *Breathing problems*, or custom description), which immediately triggers an audible and visual red pulsing alert on the Cabin Crew tablet along with the passenger's pre-registered medical conditions and allergies.
- **Call a Steward**:
  - One-tap cabin crew call button that notifies the steward of the exact seat requesting assistance.
- **Onboard Snacks & Drinks Menu (40 Products in EUR)**:
  - Interactive catalog of **40 branded food and beverage items** with product photography, volume/weight details (`ml` / `g`), and prices in **EUR (€)**.
  - Category filtering (*All*, *Drinks*, *Snacks*), real-time quantity controls (`+` / `-`), automatic cart total calculation, and instant order transmission to the crew.
- **5-Category Flight Rating & Reviews**:
  - Passengers can rate the flight from 1 to 5 stars across 5 categories: **Turbulence**, **Staff behaviour**, **Cleanliness**, **Legroom**, and **Bag space**, plus an optional written review comment.
- **Interactive Cabin Seat Map & Seat Complaints**:
  - Visual 30-row (`1`–`30`, `A`–`F`) aircraft cabin map highlighting the passenger's own seat.
  - Passengers can tap any other seat in the cabin to report a disturbance categorized as **Noise**, **Behaviour**, or **Other**, with a written description.
- **Live Request Status Tracker**:
  - Real-time status badges on the passenger's home screen showing whether each request is *Sent over air*, *Seen by steward*, *In progress*, or *Resolved*.
- **Bilingual Interface (`EN` / `RO`)**:
  - Instant language switching between **English (`EN`)** and **Romanian (`RO`)** across all screens.

---

### B. Cabin Crew (Steward) Application Features
- **Crew Authentication & Automatic Flight Autofill**:
  - Secure Steward Login screen requiring crew username and password when the application opens.
  - Upon authentication, the steward's assigned **Flight number** and **Day of flight** are automatically populated on the dashboard.
- **Real-Time 4-Tab Notification Center**:
  - **Urgent**: Displays active medical emergencies with red pulsing indicators, emergency type, and the passenger's medical conditions/allergies. Triggers an audible alert chime when a new medical emergency arrives.
  - **Calls**: Lists active steward call requests by seat number.
  - **Orders**: Displays open snack and drink orders with itemized quantities, calculated EUR total, and one-tap status updates (*Mark seen* / *Seen*).
  - **Complaints & Feedback**: Consolidates seat-to-seat complaints (`From Seat → Target Seat`, category badge, and description) alongside anonymous passenger flight ratings and average star scores.
- **Live Interactive 30-Row Cabin Heatmap**:
  - Color-coded aircraft seat map (`Rows 1–30`, `Seats A–F`) providing instant situational awareness:
    - **Green (`OK`)**: Normal status / no open issues.
    - **Yellow (`Minor`)**: Active snack order or seat complaint involving that seat.
    - **Red Pulsing (`Urgent`)**: Active medical emergency or steward call on that seat.
- **Seat Inspection & Passenger Management Panel**:
  - Clicking any seat on the cabin heatmap opens a detailed inspection card for that seat showing:
    - Passenger name and highlighted medical conditions / allergies.
    - All requests and complaints associated with that seat.
    - Workflow progression controls (*Mark seen* → *Start* → *Resolve*).
    - **Rate Passenger** tool allowing the steward to record a `1–5` behavior score and crew note for that seat.
- **End Flight & Post-Flight Database Upload**:
  - One-click **End flight & upload** button with real-time progress indicator that synchronizes all in-flight data (`passengers`, `requests`, `orders`, `flightRatings`, `seatComplaints`, `passengerRatings`) to the **Ditto Big Peer Cloud** and commits all records into the **Microsoft SQL Server (`Sky4All`)** database.

---

## 2. Platforms, Technologies & Programming Languages Used

### Programming Languages
| Language | Role in Project |
| :--- | :--- |
| **Dart** | Cross-platform mobile/tablet/web application (`lib/`) and standalone SQL Server ⇄ Ditto sync server (`backend/sql_ditto_sync_server.dart`). |
| **TypeScript (`.ts` / `.tsx`)** | React web applications (`sky4all-split`, `sky4all-pasager`, `sky4all-stewardesa`), Ditto data layer, and Vite SQL Server middleware. |
| **JavaScript (ES2022)** | Standalone browser runtime and cross-tab P2P state synchronization (`web_html/index.html`). |
| **HTML5 & CSS3** | Document structure, responsive mobile/tablet mockups, and keyframe animations. |
| **T-SQL (Transact-SQL)** | Relational database schema, constraints, seed data, and stored procedures for SQL Server Management Studio 22 (`database/Sky4All.sql`). |
| **DQL (Ditto Query Language)** | Document collection creation, upserts (`ON ID CONFLICT DO UPDATE`), and subscriptions for Ditto Big Peer (`database/ditto_collections_setup.dql`). |

### Frameworks, Libraries & Tools
- **Flutter SDK & Material 3**: Cross-platform native UI for Passenger phone and Crew Steward tablet views (`provider`, `http`, `google_fonts`, `ditto_live`).
- **React 19 & TanStack Start / TanStack Router**: File-based routing (`/`, `/p`, `/s`), reactive state management (`useSyncExternalStore`), and SSR/client hydration.
- **Vite**: Fast frontend build tool and development server with custom Node.js middleware (`sky4all-sql-sync`) for direct local SQL Server persistence.
- **Tailwind CSS & Radix UI / shadcn-ui**: Responsive styling, custom aviation color palette, popovers, calendars (`date-fns`), and `lucide-react` iconography.
- **Ditto.live Platform (`@dittolive/ditto` & HTTP DQL API)**: Offline-first CRDT document store, P2P mesh synchronization during flight, and Big Peer cloud sync.
- **Microsoft SQL Server & SSMS 22 (`sqlcmd` ODBC 18)**: Global relational database (`Sky4All`) storing pre-flight manifests, onboard catalog, post-flight records, and audit logs.

### Supported Platforms
- **Web (Desktop & Mobile Browsers)**: Chrome, Edge, Firefox, Safari — supports side-by-side Split View (`/`) as well as multi-tab operation (`/p` for Passenger tab and `/s` for Steward tab).
- **Mobile & Tablet**: Android & iOS (via Flutter).
- **Desktop**: Windows, macOS, and Linux (via Flutter & Web).

---

## 3. Ditto Big Peer Connection Configuration

- **Database ID**: `965957f9-6fd7-4d2f-8794-d8f1544c5724`
- **Cloud URL**: `https://i83inp.cloud.dittolive.app`
- **Cloud URL Endpoint**: `i83inp.cloud.dittolive.app/965957f9-6fd7-4d2f-8794-d8f1544c5724`
- **HTTP DQL Endpoint**: `https://i83inp.cloud.dittolive.app/api/v4/store/execute`

---

## 4. Architecture Breakdown: Frontend, Backend & Database Files

### Frontend — React / TypeScript Web Applications
- `src/components/SplitDemo.tsx` — Side-by-side Passenger phone and Steward tablet view (`/`).
- `src/routes/p.tsx` — Standalone Passenger web view (`/p`).
- `src/routes/s.tsx` — Standalone Crew Steward web view (`/s`).
- `src/components/PassengerApp.tsx` — Complete Passenger onboarding and in-flight interface.
- `src/components/StewardApp.tsx` — Crew Steward login, autofilled flight header, 4-tab notification center, cabin heatmap, seat panel, and post-flight upload button.
- `src/components/OrderMenu.tsx` & `src/lib/menu.ts` — 40-item onboard snack & drink menu with EUR formatting.
- `src/components/SeatMap.tsx` — Interactive 30-row (`A`–`F`) aircraft cabin seat map.
- `src/lib/i18n.tsx` — Bilingual (`EN` / `RO`) translation system.
- `src/lib/ditto-config.ts` & `src/lib/ditto.ts` — Ditto store, cross-tab `BroadcastChannel` + `localStorage` sync, credential validation, and `uploadFlightData()` post-flight sync.

### Frontend — Flutter / Dart Application
- `pubspec.yaml` — Flutter dependencies and bundled image assets (`assets/images/`).
- `lib/main.dart` — Split-screen entrypoint (`Passenger` + `Steward`).
- `lib/main_passenger.dart` — Standalone Passenger app entrypoint.
- `lib/main_steward.dart` — Standalone Crew Steward app entrypoint.
- `lib/screens/split_demo_screen.dart` — Split-screen layout and view switcher.
- `lib/screens/passenger/passenger_app.dart` — Flutter Passenger implementation.
- `lib/screens/steward/steward_app.dart` — Flutter Crew Steward implementation.
- `lib/widgets/order_menu_widget.dart` & `lib/widgets/seat_map_widget.dart` — Reusable menu and seat map widgets.
- `lib/services/i18n_service.dart` & `lib/models/sky4all_models.dart` — Translations, models, and menu catalog.

### Frontend — Standalone HTML Web Version
- `web_html/index.html` — Zero-build HTML5/Tailwind/JS web application.

### Backend & Synchronization Layer
- `vite.config.ts` — Contains the `sky4allSqlSyncPlugin` middleware that writes post-flight data directly to SQL Server (`Sky4All`) via `sqlcmd` when clicking **End flight & upload**.
- `backend/sql_ditto_sync_server.dart` — Standalone Dart HTTP Sync Server (`http://localhost:3001`) bridging Ditto Big Peer and SQL Server (`Sky4All`).
- `lib/services/ditto_service.dart` & `lib/config/ditto_config.dart` — Flutter Ditto DQL & SQL Server sync service.

### Global Database & Ditto Collections
- `database/Sky4All.sql` — T-SQL script for **SQL Server Management Studio 22 (SSMS 22)** creating the `Sky4All` database, 10 tables (`Flights`, `Stewards`, `Passengers`, `Products`, `Requests`, `Orders`, `FlightRatings`, `SeatComplaints`, `PassengerRatings`, `SyncAuditLogs`), seed data, and sync stored procedures (`usp_PreFlightSyncToDitto`, `usp_PostFlightSyncFromDitto`).
- `database/ditto_collections_setup.dql` — DQL statements for initializing the 7 Ditto collections (`passengers`, `stewards`, `requests`, `orders`, `seatComplaints`, `flightRatings`, `passengerRatings`).

---

## 5. How to Run on Localhost

### 1. Run the React / Vite Web Interface (Split View & Multi-Tab)
```cmd
cd C:\Users\PC\.gemini\antigravity\brain\777b3dec-3107-4120-852e-1df728dff82b\scratch\sky4all-split
npm install
npm run dev
```
Open in your browser:
- **Split View (Passenger + Steward side-by-side)**: `http://localhost:5173/`
- **Passenger View in its own tab**: `http://localhost:5173/p`
- **Crew Steward View in its own tab**: `http://localhost:5173/s`

### 2. Inspect Uploaded Flight Data in SQL Server Management Studio 22 (SSMS 22)
After clicking **End flight & upload** in the Steward interface, open a New Query on your **`Sky4All`** database in SSMS 22 and run:
```sql
USE Sky4All;
GO

SELECT * FROM dbo.Orders ORDER BY CreatedAt DESC;
SELECT * FROM dbo.Requests ORDER BY CreatedAt DESC;
SELECT * FROM dbo.FlightRatings ORDER BY CreatedAt DESC;
SELECT * FROM dbo.SeatComplaints ORDER BY CreatedAt DESC;
SELECT * FROM dbo.PassengerRatings ORDER BY CreatedAt DESC;
SELECT * FROM dbo.Passengers;
SELECT * FROM dbo.SyncAuditLogs ORDER BY LogId DESC;
```
