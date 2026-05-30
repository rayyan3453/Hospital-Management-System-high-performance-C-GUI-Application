# Hospital Management System (HMS)

C++17 OOP-based Hospital Management System with service-layer architecture, CSV persistence, and UI-ready dashboard logic.

## Tech Stack

- Language: C++17
- Build system: CMake
- Persistence: CSV flat files in `data/`
- UI target: SFML 2.6 with optional Dear ImGui (CMake wiring included with graceful fallback)

## Current Project Status

Implemented:

- Domain models (`User` hierarchy, `Appointment`, `MedicalRecord`, `Billing`, `Room`)
- Utility layer (`HashUtils`, `FileParser`, `DateUtils`, `ValidationUtils`)
- Full manager/service layer:
  - `UserManager`
  - `AppointmentManager`
  - `MedicalRecordManager`
  - `BillingManager`
  - `RoomManager`
  - `ReportManager`
- Doctor feature logic (`DoctorDashboard`)
- Patient feature logic (`PatientDashboard`)
- Seed CSV data for immediate testing

## Folder Structure

```text
HMS/
├── CMakeLists.txt
├── README.md
├── data/
│   ├── users.csv
│   ├── appointments.csv
│   ├── medical_records.csv
│   ├── billing.csv
│   └── rooms.csv
├── include/
│   ├── models/
│   ├── managers/
│   ├── ui/
│   └── utils/
└── src/
    ├── models/
    ├── managers/
    ├── ui/
    ├── utils/
    └── main.cpp
```

## Build Instructions (Windows, MinGW/Ninja)

```bash
cmake -B build
cmake --build build
```

Binary output:

- `build/hms.exe`

Data files are copied automatically to `build/data/` after each build.

## QA Validation Run

The project includes an executable QA suite:

```bash
build/hms.exe --phase11-check
```

Expected output:

- `Phase 11 QA suite passed.`

## SFML / ImGui Configuration

The project tries:

1. `find_package(SFML 2.6 ...)`
2. `find_package(ImGui-SFML ...)` when `HMS_ENABLE_IMGUI=ON`

If not found, CMake configures successfully and compiles in non-GUI placeholder mode for now.

To help CMake locate libs, provide your install prefix, for example:

```bash
cmake -B build -DCMAKE_PREFIX_PATH="C:/path/to/SFML;C:/path/to/ImGui-SFML"
```

## Seed Test Accounts

All seed passwords are hashed with djb2. Use these plain passwords at login:

- Admin password: `admin123`
- Doctor password: `doctor123`
- Patient password: `patient123`

Seed users:

- Admins: `admin1@hms.local`, `admin2@hms.local`
- Doctors: `doctor1@hms.local`, `doctor2@hms.local`
- Patients: `patient1@hms.local`, `patient2@hms.local`
- Quick lookup file: `data/seed.csv` (role/email/plain password mapping for demo login)

## Service-Layer Quick Usage

1. Create managers
2. Load from CSV
3. Perform operations (auto-save occurs on mutations once persistence path is set)

Pseudo-flow:

```cpp
UserManager userManager;
userManager.loadFromFile("data/users.csv");

AppointmentManager apptManager;
apptManager.loadFromFile("data/appointments.csv");

auto user = userManager.login("doctor1@hms.local", "doctor123", UserRole::Doctor);
```

## Reporting Support

`ReportManager` provides:

- patient / doctor / admin counts
- appointment status summary
- daily and weekly appointment time-bucket stats

Useful for Admin reports and charts.

## Troubleshooting

- **SFML not found by CMake**
  - Pass `-DCMAKE_PREFIX_PATH=...` to your SFML and ImGui-SFML install roots.
- **Runtime DLL issues**
  - Ensure SFML runtime DLLs are available beside `hms.exe` or in `PATH`.
- **CSV parse issues**
  - Keep CSV headers and column order unchanged from provided seed files.
- **Compiler mismatch**
  - Use one consistent toolchain (e.g., MinGW GCC) for configure + build.

## Architecture Notes

- Detailed architecture and service-boundary notes are in `ARCHITECTURE.md`.

