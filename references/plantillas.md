# Plantillas

Copia, rellena con datos reales del proyecto y elimina lo que no aplique. Todo dato de ejemplo es ficticio.

## README

```md
# Nombre del proyecto — frase de valor en una línea

![Captura principal](docs/assets/hero.png)

## Qué es y para quién
Dos o tres frases sin jerga. Quién lo usa y qué problema resuelve.

## Requisitos
- Runtime y versión · Base de datos · Otras herramientas

## Arranque rápido
1. `git clone <url-del-repo>`
2. `cp .env.example .env`   (completa los valores de desarrollo; ver docs/reference/configuracion/variables-entorno.md)
3. `<comando de instalación>`
4. `<comando de arranque>`
5. Abre `http://localhost:<puerto>`
Resultado esperado: <qué se ve>. Cuenta demo: ver `docs/user-manual/primeros-pasos.md`.

## Documentación
Mapa completo en [docs/index.md](docs/index.md). Manual de usuario en [docs/user-manual](docs/user-manual/index.md).

## Estructura resumida
`src/` ... · `migrations/` ... · `docs/` ...

## Contribuir
Ver [CONTRIBUTING.md](CONTRIBUTING.md).

## Licencia y contacto
```

Verificación: cada comando del arranque rápido se ejecutó tal cual (fila en `VERIFICATION.md`).

## ADR

```md
# 0007. Título corto de la decisión
- Estado: aceptada | reemplazada por 00xx | reconstruida (inferida)
- Fecha: AAAA-MM-DD
- Decisores: rol o equipo

## Contexto y problema
## Factores de decisión
## Opciones consideradas
1. Opción A — pros / contras
2. Opción B — pros / contras
## Decisión
## Consecuencias
(+) ...  (−) ...
## Confirmación
Cómo se verifica que se cumple (prueba, regla de CI, revisión).
## Evidencia
Commits, PR o archivos que sustentan la decisión (obligatorio si el estado es "reconstruida").
```

## Runbook

```md
---
title: <Procedimiento o incidente>
ultima_prueba: AAAA-MM-DD
entorno_de_prueba: staging | contenedor local
---
# <Procedimiento>
**Propósito:** ...
**Disparador:** alerta o síntoma que lo activa.
**Prerrequisitos y accesos:** rol necesario (los accesos se piden por el canal seguro; no se escriben aquí).
**Impacto esperado:** ...

## Pasos
1. Acción. -> **Resultado esperado:** ... -> **Verificación:** comando o pantalla.
2. ...

## Rollback
## Escalamiento
A quién y por qué canal.
## Enlaces
Dashboards, logs, documentación relacionada.
```

Un runbook no probado lleva `ultima_prueba: nunca` y la marca `Sin verificar`.

## Ficha de módulo (documentación técnica)

```md
# Módulo: <nombre>
**Propósito:** una frase.
**Ubicación:** `src/modules/<nombre>/`
**Responsabilidades / Fuera de alcance:** ...
**Entradas y salidas:** rutas, eventos, colas, funciones públicas.
**Depende de / Lo usan:** ...
**Datos que toca:** tablas y campos (enlace al diccionario).
**Flujos principales:** enlaces a secuencias.
**Reglas de negocio y validaciones:** ...
**Errores y cómo se manejan:** ...
**Configuración:** variables relevantes.
**Pruebas:** dónde están y qué cubren.
**Archivos clave:** tabla archivo | rol | por qué importa.
**Verificación:** fecha, método, resultado.
```

## Ficha de pantalla / tarea del manual de usuario

```md
---
title: Cómo crear un pedido
tipo: how-to
audiencia: operador
ultima_verificacion: AAAA-MM-DD
verificado_contra: v1.4.0
verificado_con: navegador web, datos demo
estado: verificado
---
# Cómo crear un pedido
**Para qué sirve:** ...
**Quién puede hacerlo:** rol(es).
**Antes de empezar:** tener iniciada la sesión; tener al menos un producto cargado.

> **Importante:** advertencia previa si el paso es riesgoso o irreversible.

1. Entra a **Pedidos** ▸ **Nuevo pedido**.
   ![Pantalla Nuevo pedido con el botón resaltado](assets/screenshots/pedidos/01-pedidos-crear-01.png)
2. Elige el cliente en **Cliente**.
3. Pulsa **Guardar**.
   ![Mensaje de confirmación](assets/screenshots/pedidos/01-pedidos-crear-03.png)

**Resultado:** verás el mensaje **Pedido creado** y el pedido aparecerá en la lista con estado **Borrador**.

**Si algo falla**
| Qué ves | Por qué pasa | Qué hacer |
|---|---|---|
| "<texto exacto del mensaje>" | ... | ... |

**Ver también:** [Cancelar un pedido](cancelar-pedido.md) · [Glosario](../glosario.md)
```

Campos de la ficha para pantallas de aplicación móvil: añade **Plataforma** (Android / iOS), **Versión de la app** y **Permisos necesarios**.

## Entrada de `screenshots.yml` (manifiesto)

```yaml
- archivo: docs/user-manual/assets/screenshots/pedidos/01-pedidos-crear-01.png
  pagina: docs/user-manual/tareas/crear-pedido.md
  paso: 1
  script: tools/screenshots/pedidos.spec.ts
  entorno: navegador 1280x800, datos demo
  alt: Pantalla Nuevo pedido con el botón Guardar resaltado
```

## Cabecera de `VERIFICATION.md`

Ver formato completo en `capturas-y-pruebas.md`.
