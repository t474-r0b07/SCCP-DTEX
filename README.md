```
          · · · · · · · · · · ·
       ·    ╔═══════════════╗    ·
     ·   ╔══╬───────────────╬══╗   ·
    ·  ╔═╬──╬───────────────╬──╬═╗  ·
    · ─╬─╬──╬──── ✛ ────────╬──╬─╬─ ·
    ·  ╚═╬──╬───────────────╬──╬═╝  ·
     ·   ╚══╬───────────────╬══╝   ·
       ·    ╚═══════════════╝    ·
          · · · · · · · · · · ·

  SIGNAL ORIGIN: [REDACTED]
  LAST FIX:      coordenadas del reto
  STATUS:        ⚠ ANOMALÍA DETECTADA
```

---

```bash
$ cat /etc/mission
> Sistema de Control y Custodia Policial
> Módulo: DTEX — Operaciones Externas
> Estado: [DESARROLLO / TESTING] ████████░░ 80%
> Surfaces: WebApp · Android Custodio · Android Supervisor
> Realtime · operaciones tácticas · Level: TACTICAL
```

---

## `> ./overview.sh`

**SCCP Command Center** — plataforma de coordinación y monitoreo operativo en tiempo real.  
Construida desde el lado del desarrollo, pensando también en el lado que intenta romperla.

Tres superficies. Una misma fuente de datos:

```
SCCP ECOSYSTEM
├── SCCP COMMAND CENTER (DTEX)
│   ├── WebApp          → tactical HUD · dashboard · central command
│   ├── DTEX Custodio   → Android · field · GPS · reports · radio
│   └── DTEX Supervisor → Android · mobile command · coordination · alerts
│
└── SCCP MOBILE (specialized armor)
    └── Monitoreo domiciliario
        → Voz · detección de spoofing GPS · geofencing · telemetría
        → github.com/t474-r0b07/SCCP-Mobile
```

---

## `> cat demo.log`

| Video | Descripción |
|-------|-------------|
| [▶ DEMO — Command Center](https://youtu.be/rMHYnaqIVr0?si=-GGM5YVxFRnkV53z) | Vista general del HUD táctico |
| [▶ DEMO — Modules & Flow](https://youtu.be/EmtY-lQay2o?si=sdV2ma88XMLw34dN) | Inconsistencias · Partes · Oficiales |

---

## `> ls -la modules/`

```
MODULE                   SUPERFICIE     ESTADO     DESCRIPCIÓN
──────────────────────   ──────────    ────────   ──────────────────────────────────
dashboard/               WebApp        IMPLEMENTADO    4 metrics · alerts · navigation
inconsistencias/         WebApp        IMPLEMENTADO    Filters · PIN resolution · audit
partes_sorpresa/         WebApp        IMPLEMENTADO    States · expiration · responses
oficiales/               WebApp        IMPLEMENTADO    ALFA/BRAVO grid · telemetry · glow
auth/                    WebApp        IMPLEMENTADO    Shuffled PinPad · login logs · roles
realtime/                WebApp        IMPLEMENTADO    Supabase subscriptions
dtex_custodio/           Android       IMPLEMENTADO  GPS · reports · radio · telemetría
dtex_supervisor/         Android       IMPLEMENTADO  Mobile command · alerts · live map
```

---

## `> cat stack.txt`

```
FRONTEND
  Flutter Web (Dart)     → WebApp Command Center
  Flutter Android (Dart) → DTEX Custodio + Supervisor
  GetX                   → gestión de estado reactiva
  flutter_animate        → fluid animations
  flutter_map            → CartoDB Dark Matter tiles

BACKEND
  Supabase — PostgreSQL + Realtime
  Autenticación propia · tabla allowed_admins
  Supabase Auth · allowlist · roles · PIN de director
  Registros de autenticación y alertas operativas

ARQUITECTURA
  Organización por capas
  ├─ Presentation  →  Views + GetX Controllers
  ├─ Data          →  Models + Repositories
  └─ Core          →  servicios, constantes y utilidades

UI / UX
  Glassmorphism · BackdropFilter sigma: 15
  Orbitron (headings) · Rajdhani (data)
  Palette: #00FFD1 cyan · #FF006E pink · #0A0E27 base
  Effects: pulse · hover glow · scanner overlay
```

---

## `> cat field_notes.txt`

> Este repositorio documenta una experiencia real de construcción: una aplicación que creció desde una interfaz web táctica hasta un sistema con dos superficies Android, autenticación, Supabase, tiempo real, seguimiento GPS y alertas operativas.
>
> El código conserva decisiones de distintas etapas del proyecto. Algunas son deliberadas; otras forman parte de la deuda técnica. Por eso el estado público se mantiene como **desarrollo / testing**.
>
> La intención es mostrar qué se construyó, qué problemas aparecieron y cómo se fueron resolviendo, no presentar el proyecto como si hubiera nacido terminado.

---

## `> cat threat_model.txt`

```
VECTOR DE ATAQUE      MITIGACIÓN
────────────────     ───────────────────────────────────────
GPS Spoofing    →  Inconsistent coordinate detection
Shoulder Surf   →  Random shuffle PinPad on every use
Unauth access   →  Supabase Auth + allowlist de administradores + PIN para funciones de director
Escalation      →  Strict SUPERVISOR / DIRECTOR roles
Señal GPS       →  Validación de precisión, saltos, velocidad y ubicación simulada
Traceability    →  registros de autenticación y alertas
```

> *Construido con pensamiento ofensivo. Cada función responde a una amenaza concreta.*

---

## `> cat project_snapshot.txt`

```
Arquitectura:            Web + 2 superficies Android
Aplicaciones Android:   2 (Custodio · Supervisor)
Backend:                 Supabase · PostgreSQL · Realtime
Estado público:         desarrollo / testing
Versión declarada:      2.0.0
Modelo de entrega:      prototipo funcional / laboratorio
```

---

## `> ls -la documentation/`

- [`documentation/PROJECT_MEMORY.md`](./documentation/PROJECT_MEMORY.md) — Evolución de la arquitectura y decisiones principales.
- [`documentation/DEVLOG.md`](./documentation/DEVLOG.md) — Problemas encontrados, deuda técnica y decisiones reales.
- [`documentation/CHANGELOG_PUBLIC.md`](./documentation/CHANGELOG_PUBLIC.md) — Historial técnico público y estado de las etapas.

---

## `> ./run.sh`

```bash
git clone https://github.com/t474-r0b07/SCCP-DTEX.git
cd SCCP-DTEX
flutter pub get

# Configuración de credenciales mediante variables de compilación

# WebApp
flutter run -d chrome --dart-define=SUPABASE_URL=... --dart-define=SUPABASE_ANON_KEY=...

# Android Custodio
flutter run --flavor dtex_custodio --target lib/main_custodio.dart

# Android Supervisor
flutter run --flavor dtex_supervisor --target lib/main_dtex_supervisor.dart

# Production build
flutter build web --release
flutter build apk --flavor dtex_custodio --target lib/main_custodio.dart
flutter build apk --flavor dtex_supervisor --target lib/main_dtex_supervisor.dart
```

---

## `> cat roadmap.txt`

```
[ FASE 2 ]  Módulo de internos · Mapbox · exportación PDF
[ FASE 3 ]  Dashboard KPI · chat de supervisión · multi-tenant
[ FASE 4 ]  Analítica ML · API pública · soporte iOS
```

---

## `> tail -n 1 /var/log/build.log`

```
[⚑] 54 68 65 20 73 79 73 74 65 6d 20 77 6f 72 6b 73 2e
    20 54 68 65 20 71 75 65 73 74 69 6f 6e 20 69 73 3a
    20 77 68 6f 20 63 6f 6e 74 72 6f 6c 73 20 74 68 65
    20 73 79 73 74 65 6d 2e
```

---

> `[!]` · [`anomaly in position data`](./CHALLENGE.md) · coordenadas sin verificar

---

```
█████████████████████████████████████████████████████
█                                                   █
█    S C C P  ·  C O M M A N D  C E N T E R        █
█         C O N T R O L .  C U S T O D Y .         █
█                   C O D E .                       █
█                                                   █
█████████████████████████████████████████████████████
```

---

## `> cat /etc/license`

```
© t474-r0b07 · All Rights Reserved
Este código no es open source.
Ver ≠ permiso para usar, copiar o distribuir.
```

---

<!--
  2017. Black Sea. 20+ vessels report impossible position.
  AIS systems place them inland — at an airport.
  No malfunction detected. Hardware nominal.
  The data was lying.

  The first documented large-scale GPS spoofing attack on civilian infrastructure.
  El sistema confió en la señal. La señal era incorrecta.

  Por eso SCCP detecta antes de confiar.

  >> https://www.maritime.dot.gov/msci/2017-005-black-sea-anomalous-gps-signals

  Algo en este README también está mintiendo sobre su posición.
  Encuéntralo.
-->
