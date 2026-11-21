# Fase 1 — Estudiar un proyecto ajeno

Meta: entender el sistema lo bastante para explicarlo sin inventar. Todo hallazgo se anota con su origen (`archivo:línea`, comando, salida).

## 1. Orden de lectura

1. `README`, `CONTRIBUTING`, `CHANGELOG`, `docs/` existentes (¿están vigentes? compáralos con el código).
2. **Manifiestos:** `package.json`, `pnpm-workspace.yaml`, `pubspec.yaml`, `composer.json`, `pom.xml`/`build.gradle`, `pyproject.toml`, `go.mod`, `Cargo.toml`. Lee `scripts`, versiones mínimas, dependencias y lockfile.
3. **Entorno:** `.env.example` (nunca `.env` reales; si los abres para entender, no copies valores), `docker-compose*.yml`, `Dockerfile`, `Makefile`, `scripts/`.
4. **Puntos de entrada:** `main`, `index`, `app`, `bootstrap`, `server`, `artisan`, `manage.py`, `lib/main.dart`, `MainActivity`/`AppDelegate`.
5. **Rutas y contratos:** archivos de rutas, controladores, `openapi.*`, resolvers GraphQL, DTO/esquemas de validación.
6. **Datos:** migraciones, modelos/entidades, `schema.prisma`, seeds. Para el esquema vivo, usa una consulta de solo lectura (nunca escribas en la base para documentar).
7. **Seguridad transversal:** middleware de auth, guards, políticas, CORS, limitadores, manejo de errores.
8. **Despliegue:** CI (`.github/workflows`, etc.), proxy, procesos (systemd, PM2), nombres de variables de producción (sin valores).
9. **Pruebas existentes:** dicen qué comportamiento el equipo considera importante.
10. **Historial:** `git log --stat` de los últimos meses, autores, mensajes; fuente principal del porqué.

## 2. Inventario interno (antes de escribir)

Anota en una hoja de trabajo (no se entrega tal cual):

| Campo | Contenido |
|---|---|
| Stack y versiones | lenguajes, frameworks, runtime |
| Capas presentes | backend, API, BD, web, admin, móvil, infra |
| Entradas | comandos de arranque por capa |
| Rutas / pantallas | lista completa (generada, no de memoria) |
| Modelos / tablas | lista completa |
| Variables de entorno | nombre, obligatoria, efecto (sin valores) |
| Integraciones externas | pagos, correo, mapas, notificaciones, almacenamiento |
| Roles y permisos | quién puede qué |
| Tareas en segundo plano | colas, cron, workers |

Extrae listas con comandos (`grep`, el generador de rutas del framework, `tree`), no a ojo. Las variables del código frente a la tabla documentada deben coincidir en ambos sentidos.

## 3. Trazar flujos de extremo a extremo

Elige 2 o 3 casos de uso que representen el negocio (p. ej. registro, compra, publicación). Para cada uno sigue el dato **desde donde comienza hasta donde termina**:

`acción del usuario -> pantalla/componente -> llamada a la API -> ruta -> middleware -> controlador/servicio -> base de datos o servicio externo -> respuesta -> cambio en pantalla -> efectos secundarios (correo, cola, notificación)`

- [ ] Cada salto con `archivo:línea`.
- [ ] Confirma al menos uno ejecutándolo de verdad (fase 2) y comparando con lo leído.
- [ ] Dibuja cada flujo como diagrama de secuencia (ver `diagramas.md`).

## 4. Cómo decidir cuáles son los archivos más importantes

Criterios objetivos; un archivo entra si cumple al menos dos:

| Criterio | Cómo medirlo |
|---|---|
| Punto de entrada | arranque de cada proceso o app |
| Configuración central | config, variables, flavors, rutas base |
| Contrato | rutas, OpenAPI, esquemas, DTO |
| Modelo de datos | esquema, migraciones, entidades |
| Seguridad | auth, permisos, cifrado, firma |
| Fan-in alto | archivos que más módulos importan (`grep -rn "import .*<archivo>"` y cuenta) |
| Hotspot | más cambios: `git log --format= --name-only \| sort \| uniq -c \| sort -rn \| head -20` |
| Despliegue | CI, Dockerfile, proxy, scripts de release |
| Riesgo | si se rompe, el sistema no arranca o pierde datos |

Tabla a entregar (`docs/explanation/archivos-clave.md`):

| Archivo | Rol | Por qué importa | Qué se rompe si se toca | Dónde leer más |
|---|---|---|---|---|
| `src/main.ts` | Entrada del servidor | Registra módulos, pipes y CORS | Nada arranca | `backend/README.md` |

- [ ] Máximo ~15-25 filas. Más es ruido.
- [ ] La columna "por qué importa" cita evidencia (fan-in, hotspot, entrada). Sin evidencia: "hipótesis".

## 5. Explicar por qué el repo está ordenado así

Documento `docs/explanation/estructura-del-repo.md`:

1. Árbol comentado a 2 niveles (listado real, luego anotado).
2. Por carpeta: responsabilidad, qué **no** debe ir ahí, regla de dependencia (quién importa a quién) y motivo.
3. Distinguir **convención del framework** ("Laravel exige `app/Http`") de **decisión propia** ("separamos dominio de infraestructura"); lo segundo merece ADR.
4. "Dónde toco para...": añadir un endpoint, una pantalla, una tabla, una variable, un job.
5. Reglas de ubicación para código nuevo.
6. Fuentes del porqué: ADR, commits, PR, comentarios, convenciones del framework. Sin evidencia: "motivo no documentado; hipótesis: ...".

## 6. Señales de alarma a documentar (sin juzgar)

Código muerto, rutas sin pantalla, tablas sin uso, variables no declaradas, migraciones que no reproducen el esquema, secretos en el árbol o en el historial (repórtalo al responsable, **no lo copies** a la documentación), dependencias abandonadas. Van a "Riesgos y deuda", no al manual.

## 7. Preguntas que sí conviene hacer

Solo las que no se pueden deducir: público del manual, idioma y variedad, nivel de detalle, si hay entorno de pruebas y accesos demo, si hay capas fuera del repo (panel en otro proyecto, app en otro repo). Agrupa todas en un único mensaje.
