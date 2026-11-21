# skill-documentacion

**Documentación que sale del código y de pruebas reales, no de la memoria: lo que no se ejecutó se marca como "sin verificar".**

Una skill para que tu herramienta de desarrollo estudie un proyecto, lo levante, lo pruebe y entregue documentación técnica y un manual de usuario con capturas, marcando con honestidad lo que no pudo verificar.

---

## ¿Qué es esto?

Una *skill* es un archivo de texto con tu criterio escrito (cuándo se activa, qué reglas no se negocian, cómo se revisa). Tu herramienta lo lee y sigue el método en lugar de improvisar.

Esta skill convierte "documenta este proyecto" en un **proceso repetible de seis fases**: estudiar el código, levantar y probar el sistema, documentar por capa, escribir el manual de usuario, verificar y entregar.

## El problema que resuelve

La documentación casi siempre falla de la misma forma: **se escribe de memoria y envejece el mismo día**. El README dice un comando que ya no funciona, el manual describe una pantalla que cambió, el diagrama muestra una arquitectura que nadie construyó.

Y cuando se la pides a una IA sin método, es peor: produce texto convincente sobre cosas que **nunca ejecutó**.

Esta skill pone una regla simple por encima de todo: **lo que no se ejecutó es una hipótesis y se marca como tal.**

## Para qué sirve

- **Documentar un proyecto ajeno o propio** y que un integrante nuevo lo entienda sin preguntar.
- Entregar un **manual de usuario con capturas reales** (datos demo, mismo tamaño y tema) y un registro de lo verificado y lo no verificado.
- Preparar un **handoff o entrega al cliente**: runbooks, diagramas, diccionario de datos, ADR.
- Explicar **por qué** el repositorio está ordenado así y cuáles son los archivos más importantes.

## Cuándo usarla

- Vas a entregar un proyecto y falta la documentación.
- Llegas a un proyecto que no conoces y necesitas entenderlo y dejarlo explicado.
- Tu documentación existe pero nadie confía en ella.

## Cuándo **no** usarla

- Quieres solo un README de 10 líneas: es demasiado método para eso.
- No puedes ejecutar el sistema (sin entorno de prueba): la skill te obliga a marcar todo como "sin verificar".
- Quieres documentar **producción con datos reales**: la skill exige entornos de desarrollo o prueba y datos demo.

## Cómo instalarla

1. Descarga este repositorio.
2. Copia su contenido (`SKILL.md` y la carpeta `references/`) dentro de una carpeta llamada `documentacion` en el directorio de skills de tu herramienta de desarrollo.
3. Pídele: *"Usa la skill de documentacion para documentar este proyecto"* y dile quién será el lector (equipo técnico, cliente, usuario final).

Cada herramienta guarda sus skills en un sitio distinto; consulta la documentación de la tuya.

## Ejemplo

**Tú:** "Documenta este proyecto para que lo reciba el cliente."

**Con la skill, la herramienta:** primero estudia el código (manifiestos, rutas, modelos, scripts) y traza dos o tres flujos de extremo a extremo; pregunta solo lo que no puede deducir (público, idioma). Después levanta el sistema con datos demo, ejecuta cada paso del README, y solo entonces escribe. Entrega los documentos por carpeta, diagramas Mermaid y un `docs/VERIFICATION.md` que dice qué probó y qué quedó **sin verificar**, con el motivo.

## Qué incluye la skill

- **Diez reglas obligatorias** (derivar, no inventar; cada paso se ejecuta antes de escribirse; explicar el porqué; datos demo siempre; no sobre-documentar…).
- **Seis fases** con checklist: estudio, levantar y probar, documentación por capa, manual de usuario, verificación, entrega.
- Una carpeta `references/` con: estructura de `/docs` (Diátaxis, C4, ADR), cómo leer un proyecto ajeno, qué documentar por capa, diagramas con Mermaid, manual de usuario, capturas y pruebas, control de calidad y plantillas (README, ADR, runbook, ficha de módulo).

## Límites honestos

- Es un método, no magia: la calidad depende de que el sistema se pueda levantar.
- Los comandos de herramientas externas (emuladores, Playwright, linters) llevan la etiqueta **verificar en tu entorno** hasta que los corras.
- Un documento sin lector claro no se escribe: la skill prefiere menos páginas y verdaderas.

## Skills relacionadas

[`skill-qa`](https://github.com/nanoizax/skill-qa) y [`skill-arquitectura-backend`](https://github.com/nanoizax/skill-arquitectura-backend).

## Licencia y autoría

[MIT](LICENSE) · Hecha por Leandro Manuel Perez Montañana.
Más skills y herramientas gratis en **[soytegno.sonholab.com](https://soytegno.sonholab.com)** · TikTok [@soytegno](https://www.tiktok.com/@soytegno)
