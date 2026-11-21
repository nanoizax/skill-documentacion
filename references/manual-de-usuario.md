# Manual de usuario

Audiencia: personas que usan el sistema, no lo construyen. Se organiza por **tareas del usuario**, no por el menú de la app. Un manual por rol cuando los flujos difieren (cliente final, operador, administrador).

## Idioma y variedad del mercado

- [ ] El manual se redacta en el idioma y la variedad del **mercado del cliente**, no en la del equipo. Confirma el mercado en la fase 1.
- [ ] Ejemplos: Chile y cualquier cliente chileno = tuteo, nunca voseo (`dime`, `revisa`, `confirma`, `puedes`, `aquí`; con pronombre, tilde donde corresponde: `actívalo`, `confírmalo`). Brasil = portugués de Brasil. Otro país = su variedad.
- [ ] Producto multi-país sin variedad local única = español neutro con tuteo.
- [ ] Registro neutro y profesional; sin jerga regional ni caricatura.
- [ ] Excepción explícita: si el producto es para EE. UU., la investigación y los entregables formales van en inglés; el español queda para conversar con el equipo. Si dudas sobre un entregable, pregunta.
- [ ] Los textos del manual deben coincidir con los textos reales de la interfaz, en el mismo idioma. Si la interfaz tiene voseo y el mercado pide tuteo, repórtalo como hallazgo; no lo copies al manual.

## Estructura

1. **Qué es** (2-3 frases, sin jerga).
2. **Para quién** (perfiles o roles).
3. **Por qué usarlo** (beneficios concretos).
4. **Cuándo usarlo y cuándo no** (límites honestos).
5. **Antes de empezar** (cuenta, dispositivo, permisos, conexión).
6. **Primeros pasos** (tutorial lineal hasta un primer resultado).
7. **Guía por tarea** (una tarea = una página; ver ficha en `plantillas.md`).
8. **Conceptos básicos** (solo los necesarios).
9. **Preguntas frecuentes**.
10. **Solución de problemas**.
11. **Glosario**.
12. **Contacto y soporte**.
13. **Historial de versiones del manual**.

## Cómo se escribe cada tarea

Cada paso se ejecuta antes de escribirse (regla central). Por tarea:

- [ ] **Para qué sirve**, **quién puede hacerlo**, **antes de empezar**.
- [ ] Pasos numerados; un paso = una acción.
- [ ] Nombres de botones, campos y menús **exactamente** como en pantalla, en **negrita**.
- [ ] Captura por paso relevante, con la marca del elemento (ver `capturas-y-pruebas.md`).
- [ ] Tras el paso clave, qué verá el usuario ("Verás el mensaje **Pedido creado**").
- [ ] Advertencias **antes** del paso riesgoso, no después.
- [ ] **Si algo falla:** síntoma -> causa -> solución.
- [ ] "Ver también" con enlaces internos.
- [ ] Estado de verificación en la cabecera; los pasos no ejecutados llevan `> Sin verificar`.

## Escritura para no técnicos

- Frases de 15-20 palabras; voz activa e imperativo ("Pulsa **Guardar**").
- Un término = un significado; defínelo en el glosario y no cambies de sinónimo.
- Sin "simplemente", "obviamente", "basta con".
- Explica el resultado de cada acción importante; nadie debe preguntarse "¿funcionó?".
- Números, plazos y límites concretos.
- Evita términos internos (nombres de tablas, rutas, códigos). Si el usuario ve un código de error, cítalo tal cual aparece.
- Tono amable y directo, sin infantilizar ni dar sermones. Trata al lector como a una persona capaz que tiene prisa.
- Prueba el manual con alguien que no conozca el sistema (o, si no hay nadie, recórrelo tú siguiendo solo el texto) y corrige lo que tropiece.

## Solución de problemas

Formato de tabla o lista, una entrada por problema real (vistos en las pruebas o en el código de manejo de errores):

| Qué ves | Por qué pasa | Qué hacer |
|---|---|---|
| Mensaje exacto en pantalla | Causa en lenguaje llano | Pasos concretos; a quién escribir si persiste |

- [ ] El texto del mensaje es el que muestra la interfaz (cópialo de la ejecución real).
- [ ] Incluye casos de permisos ("no veo el botón"), conexión, datos inválidos y sesión vencida.
- [ ] No inventes errores que no pudiste provocar o encontrar en el código.

## Preguntas frecuentes

Solo preguntas que el usuario hace de verdad (del soporte, de las pruebas, de las tareas). Máximo ~10. Cada respuesta enlaza a la tarea completa.

## Glosario

Orden alfabético; una línea por término, con el mismo nombre que usa la interfaz. Incluye siglas y conceptos del negocio (estado de un pedido, rol, plan).

## Contacto y soporte

Canal, horario y qué datos tener a mano. Usa los canales oficiales del cliente (correo o teléfono **de soporte**, nunca datos personales de integrantes del equipo).

## Capturas en el manual

- Datos demo únicamente; nunca clientes reales, credenciales ni barras de dirección con sesión.
- Mismo tamaño, tema e idioma en todas; nombre `NN-flujo-paso.png`; texto `alt` descriptivo.
- Fotos de producto o del negocio: usar las reales del cliente cuando existan (con su autorización), no imágenes de stock genéricas.
- Móvil: capturas del emulador o simulador, no de maquetaciones recortadas (ver trampas en `capturas-y-pruebas.md`).

## Entrega del manual

Markdown como fuente; sitio estático, PDF o DOCX generados desde él (ver `calidad-y-mantenimiento.md`). Portada con nombre, versión y fecha; sin nombres de herramientas internas en metadatos ni autoría.
