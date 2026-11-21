# Documentación por capa

Documenta solo las capas que existen. Cada capa tiene `README.md` (qué es, cómo se arranca, enlaces) más referencia y how-to. Todo dato sale del código o de una prueba real; lo demás, "sin verificar".

## Backend (`docs/backend/`)

Contenido:
- [ ] Propósito y límites del servicio; qué **no** hace.
- [ ] Arquitectura: capas o módulos, regla de dependencia. Criterios de diseño: skill `arquitectura-backend` (enlazar, no copiar).
- [ ] Flujo de una petición (secuencia): entrada, middleware, validación, servicio, persistencia, respuesta.
- [ ] Autenticación y autorización: cómo se emite y valida el token, roles, políticas.
- [ ] Manejo de errores: formato, códigos de negocio.
- [ ] Tareas en segundo plano: colas, cron, reintentos, idempotencia.
- [ ] Integraciones externas: para qué, qué pasa si caen.
- [ ] Configuración: tabla de variables (abajo).
- [ ] Logging y observabilidad: dónde están los logs, qué se registra, qué **no** (datos personales).
- [ ] Cómo correr las pruebas y qué cubren.

Tabla de variables de entorno (referencia):

| Variable | Obligatoria | Ejemplo ficticio | Efecto | Dónde se usa |
|---|---|---|---|---|
| `DATABASE_URL` | Sí | `postgres://usuario:clave@localhost:5432/app_dev` | Conexión a la base | `src/config/db.ts:12` |

- Los ejemplos son **ficticios**; nunca valores reales de ningún entorno.
- Verifica en ambos sentidos: toda variable del código está en la tabla y viceversa.

## API (`docs/reference/api/`)

- **Fuente de verdad: OpenAPI 3.x**, generado del código o contrato-primero. Valida con un linter de OpenAPI (Spectral o Redocly, **verificar en el entorno**).
- `components/securitySchemes` (Bearer, API key, OAuth2) aplicado global o por operación.
- Errores definidos una vez en `components/responses` y reutilizados; formato recomendado RFC 9457 (`application/problem+json`).
- **Ejemplos reales**: request y response capturados ejecutando el sistema con datos demo, éxito y cada error relevante (401, 403, 404, 409, 422, 429).
- Probar caminos de error y permisos por rol, no solo el GET feliz.

Plantilla por endpoint:

```md
### POST /orders
Crea un pedido. Roles: `operator`, `admin`. Auth: Bearer.
| Campo | Tipo | Obligatorio | Descripción |
Request (real, datos demo):  ```json ... ```
Respuesta 201 (real):        ```json ... ```
Errores: 401 · 403 · 422 (ejemplo real) · 429
Notas: paginación, idempotencia, límites de tasa.
```

Extras: guía de autenticación paso a paso, versionado, colección `.http`/Bruno/Postman generada desde los ejemplos probados, diferencias entre entornos. Desajustes contrato-implementación: herramientas de prueba de contrato (Schemathesis, Dredd, Prism; **verificar en el entorno**).

## Base de datos (`docs/reference/base-de-datos/`)

Si el proyecto tiene una guía de base de datos (modelado, inventario, trampas conocidas), léela antes de consultar. Consulta solo con rol de solo lectura, acotada, sin volcar datos.

- [ ] **Diccionario de datos** por tabla: propósito, columnas (tipo, nulo, default, clave, descripción de negocio, valores de enums), índices, FK con `ON DELETE`, datos personales sí/no.
- [ ] **Diagrama ER** principal en Mermaid (`erDiagram`), máximo ~8 entidades por diagrama; dividir por dominio.
- [ ] Migraciones: cómo se aplican, en qué orden, quién las ejecuta.
- [ ] Política de datos: retención, respaldo, multi-tenant y RLS si aplican.
- [ ] Generar desde el esquema vivo con una herramienta (tbls o SchemaSpy, **verificar en el entorno**) y completar con descripciones de negocio. Sin descripción en `COMMENT`/migración: escribe "sin descripción", no la inventes.
- [ ] Distinguir desarrollo de producción; documentar solo esquema.

```md
### orders
Propósito: ...
| Columna | Tipo | Nulo | Default | Clave | Descripción de negocio |
Índices · FK · Reglas · Datos personales: sí|no
```

## Frontend web (`docs/frontend/`)

- [ ] Mapa de rutas y pantallas (generado de la config de rutas).
- [ ] Árbol de componentes principales y dónde viven los compartidos.
- [ ] Gestión de estado y de datos remotos; cómo se consume la API (cliente, interceptores, manejo de 401).
- [ ] Matriz rol x pantalla/acción.
- [ ] Design system: tokens, tipografía, componentes base.
- [ ] i18n: idiomas, dónde están las cadenas, cómo añadir una.
- [ ] Build y variables públicas (las que viajan al navegador son públicas por definición; dilo).
- [ ] Accesibilidad y navegadores soportados.
- [ ] Verificación en la **página completa** con consola abierta, no solo en vistas aisladas.

## Panel de administración (`docs/admin/`)

Documentación técnica como el frontend, más:
- [ ] Quién accede, cómo se crean administradores, separación de roles.
- [ ] Acciones destructivas o irreversibles (borrados, reembolsos, envíos masivos) con advertencia.
- [ ] Auditoría: qué queda registrado.
- [ ] Manual propio en `user-manual/admin/` (el admin es un usuario final distinto del cliente).

## Móvil (`docs/mobile/`)

- [ ] Plataformas y versiones mínimas; herramientas de build y versión del SDK.
- [ ] Arquitectura, navegación, gestión de estado.
- [ ] Permisos solicitados y **por qué** se piden.
- [ ] Almacenamiento seguro y qué se guarda en el dispositivo.
- [ ] Entornos o flavors y **a qué backend apunta cada uno** (un flavor no decide por sí solo el backend: verifica la configuración real).
- [ ] **Build de release reproducible con parámetros exactos** (p. ej. todos los `--dart-define` o variables de compilación requeridas). Un build que compila y firma pero omite parámetros puede arrancar y morir; busca el script de release del repo y documenta ese, no un comando inventado.
- [ ] Firma y publicación: qué llaves existen y **dónde se respaldan** (sin incluir las llaves ni sus contraseñas); sin ellas no se puede volver a publicar la app.
- [ ] Deep links, notificaciones push, permisos en tiempo de ejecución.
- [ ] Criterio de "listo": el artefacto se instala **y se abre** en emulador o simulador (ver `capturas-y-pruebas.md`).

## Infraestructura y despliegue (`docs/infra/`, `docs/runbooks/`)

- [ ] Diagrama de despliegue (C4 contenedores + hosts).
- [ ] Inventario: servicios, puertos, dominios, proxy y TLS, contenedores y redes, procesos (systemd/PM2).
- [ ] CI/CD paso a paso: disparadores, etapas, artefactos, secretos usados (**solo nombres**).
- [ ] Variables y secretos: dónde viven, quién los rota, procedimiento de respaldo. Nunca valores ni IPs en el documento público.
- [ ] Backups: qué, dónde, cada cuánto, y **restauración probada** (un respaldo sin prueba de restauración no cuenta).
- [ ] Monitoreo y alertas; espacio en disco y límites.
- [ ] Procedimiento de despliegue y rollback como runbook.
- [ ] "Qué se pierde si se reinicia": procesos o redes configurados a mano, servicios que no arrancan solos.
- [ ] Antes y después de un reinicio: foto del estado (contenedores, procesos, puertos) para comparar.

Comandos de ejemplo en runbooks: pásalos por un archivo y revisa la sintaxis antes de ejecutarlos en servidores; no anides heredocs ni recortes configuraciones con `sed`/`awk` sin respaldo.

## Ficha de módulo

Para módulos de negocio grandes (pagos, notificaciones, inventario) usa la ficha de `plantillas.md`: propósito, entradas y salidas, dependencias, datos que toca, flujos, errores, pruebas, archivos clave.
