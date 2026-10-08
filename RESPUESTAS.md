## Escenarios que elegí y por qué

Elegí los solapamientos parciales, la contención, los intervalos idénticos y el inicio coincidente para cubrir distintas formas de intersección; los casos están definidos en `specs/001-reservas-sala/spec.md:24`, `specs/001-reservas-sala/spec.md:29`, `specs/001-reservas-sala/spec.md:32`, `specs/001-reservas-sala/spec.md:35` y `specs/001-reservas-sala/spec.md:37`, y probados en `test/crear_reserva_test.dart:43`, `test/crear_reserva_test.dart:81`, `test/crear_reserva_test.dart:100`, `test/crear_reserva_test.dart:119` y `test/crear_reserva_test.dart:138`. También cubrí el límite contiguo y otra sala para confirmar que solo hay conflicto cuando se cruzan horarios de la misma sala (`specs/001-reservas-sala/spec.md:26`, `specs/001-reservas-sala/spec.md:40`; `test/crear_reserva_test.dart:62`, `test/crear_reserva_test.dart:157`).

## Riesgo más grave del repositorio

La pantalla evita el caso de uso e inserta directamente en Supabase (`lib/presentation/reserva_page.dart:58`, `lib/presentation/reserva_page.dart:60`), por lo que la validación de dominio (`lib/domain/crear_reserva.dart:16`, `lib/domain/crear_reserva.dart:22`) no se aplica al flujo real. Además, la migración solo impone que el fin sea posterior al inicio, sin una restricción contra solapamientos (`supabase/migracion.sql:11`).

## ¿La regla protege la app real?

No. Aunque `main` inyecta `CrearReserva` en la pantalla (`lib/main.dart:13`, `lib/main.dart:14`), el botón acaba haciendo el `insert` directo (`lib/presentation/reserva_page.dart:103`, `lib/presentation/reserva_page.dart:60`). También existe riesgo de carrera entre la consulta y el guardado separados en el caso de uso (`lib/domain/crear_reserva.dart:16`, `lib/domain/crear_reserva.dart:28`); la base de datos no impide ese solapamiento (`supabase/migracion.sql:11`).
