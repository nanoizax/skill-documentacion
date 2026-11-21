# Levantar, recorrer y capturar el sistema

Principio: **documento que no se ejecutó = hipótesis.** Un solo recorrido sirve para verificar y para capturar. Para pruebas funcionales amplias y casos de regresión, usa la skill `qa`; aquí solo se recorre lo que el manual describe.

## 1. Entorno de pruebas

- [ ] Clon limpio o contenedor; versiones fijadas. Registra SO, versiones de runtime, commit y fecha (van a `VERIFICATION.md`).
- [ ] Base sembrada con **datos demo** (seed dedicado): personas inventadas, correos `@example.com`, teléfonos de rango ficticio, importes plausibles. Nunca una copia de producción.
- [ ] Nunca probar contra producción. Nunca introducir credenciales reales. Las cuentas demo se crean en el seed y se guardan en el archivo de seeds del proyecto, no en la documentación pública.
- [ ] Antes de crear el seed, averigua quién gobierna la base (esquema y datos demo) y pide los datos demo por el canal acordado si hace falta.
- [ ] Ejecuta el README **literalmente**. Fallo = corregir el doc o abrir bug; nunca "arreglarlo de cabeza".

## 2. Según el entorno

### Windows (emulador Android y navegador web)

Regla: el emulador corre en **Windows**, no en WSL2. Si el código vive en WSL2, compila allí y opera el dispositivo con el `adb` de Windows.

```powershell
& "$env:LOCALAPPDATA\Android\Sdk\platform-tools\adb.exe" devices
& "$env:LOCALAPPDATA\Android\Sdk\emulator\emulator.exe" -list-avds
```
(rutas por defecto de Android Studio; **verificar en el entorno**)

Trampas reales:
- **`adb devices` vacío desde WSL2** = normal. Copia el APK a una ruta de Windows e instala con el `adb` de Windows.
- **Backend dentro de WSL2:** `adb reverse tcp:PUERTO tcp:PUERTO` con el `adb` de Windows; `10.0.2.2` no atraviesa hasta WSL2.
- **Captura corrupta:** `adb exec-out screencap -p > x.png` en PowerShell 5.1 estropea el PNG (BOM y reescritura de bytes). Captura en el dispositivo y baja el archivo con `pull`.
- **No guardes capturas en `/sdcard`:** se cuelan en la galería y en el selector de fotos. Usa `/data/local/tmp/` y `adb pull` desde ahí.
- **Lanzar la app:** `am start -n paquete/paquete.MainActivity` suele fallar; la Activity real es `paquete/.MainActivity`. Más robusto: `adb shell "monkey -p paquete -c android.intent.category.LAUNCHER 1"`.
- **Taps a ciegas fallan:** los campos de un diálogo se reacomodan al tocar el primero. Haz `uiautomator dump` (a `/data/local/tmp/ui.xml` y `pull`) **antes de cada tap** y lee los `bounds` reales. La tecla ESC (`keyevent 111`) cierra el diálogo completo de Flutter, no solo el teclado.
- **Pulsación larga / arrastre:** `input draganddrop` no dispara `LongPressDraggable`; usa `input motionevent DOWN`, espera ~900 ms, varios `MOVE`, `UP`.
- **Selector de fotos vacío:** tras `adb push` a `/sdcard/DCIM/Camera/` lanza `am broadcast -a android.intent.action.MEDIA_SCANNER_SCAN_FILE -d file:///ruta`.
- **PowerShell bloquea `rm` con comodín en `/sdcard/...`:** borra archivos por nombre.
- **Salida de `adb install/pull` por stderr** aparece como `NativeCommandError` en PowerShell 5.1 aunque sea éxito: busca la línea `Success` o `pulled`.
- **Emulador degradado:** si `adb install` falla sin mensaje, `adb push` va lentísimo o `logcat -c` falla, sospecha del emulador, no del APK. Comprueba `adb shell "uptime; service check package"`. Con carga alta o `package` ausente: `adb -s emulator-5554 emu kill` y arranque en frío (`emulator -avd <nombre> -no-snapshot-load -no-boot-anim`), esperar `sys.boot_completed=1`.
- **Servicios de Google (p. ej. escáner de documentos):** la primera vez descargan su módulo (~40 s) y necesitan internet; déjalo anotado en el manual.
- **Criterio de artefacto entregable:** instalarlo **y abrirlo**:
  ```
  adb -s <emu> install -r app.apk
  adb -s <emu> logcat -c
  adb -s <emu> shell am start -n <paquete>/.MainActivity
  adb -s <emu> logcat -d | grep -E "Unhandled Exception|FATAL EXCEPTION"
  ```
  Vacío y la actividad principal al frente = arrancó de verdad. Firma válida, fecha y versión **no** prueban que arranque (un build sin los parámetros de compilación exigidos compila, firma y se cae en `main()`). Usa el script de build del repo o replica **todas** sus banderas.
- **Worktrees/clones:** no traen archivos ignorados por git (configuraciones de servicios, llaves, `.env`); sin ellos el build falla o, peor, compila y falla en ejecución.

Navegador web en Windows:
- Automatiza con un script de navegador (Playwright u otro; **verificar disponibilidad** en la versión de tu sistema operativo) o con el panel de navegador integrado.
- **Dev server en WSL2:** lánzalo con `--host 0.0.0.0` y navega a `http://localhost:PUERTO`, no a la IP interna de WSL2 (dispara una aprobación por acción en el panel).
- Procesos de fondo en WSL2: usa `tmux`; no sobreviven entre invocaciones separadas de `wsl.exe`.
- Invocar `wsl.exe` con rutas absolutas (`~` se contamina desde PowerShell). No uses `$PWD` como origen de `docker run -v` dentro de comandos anidados: monta un directorio equivocado sin avisar. Escribe la ruta absoluta literal.
- **Panel de navegador oculto:** `document.visibilityState === "hidden"` y `requestAnimationFrame` no corre. Animaciones, canvas y librerías que aplican cambios en un frame (metadatos de cabecera, por ejemplo) parecen rotas sin estarlo. Antes de afirmar "esto no se aplica", mide `visibilityState`; contrasta con Chrome headless: `chrome.exe --headless=new --disable-gpu --virtual-time-budget=20000 --dump-dom URL` (**verificar en el entorno**).
- **Canvas WebGL** sale vacío en captura headless si no hay `requestAnimationFrame` corriendo. No pausar el bucle: congela el reloj y usa `preserveDrawingBuffer: true`. Si varias capturas a tiempos distintos pesan exactamente lo mismo, sospecha de esto.
- **Captura móvil con ventana pequeña en modo headless NO aplica el `meta viewport`:** maqueta a ~980 px y recorta a la derecha (desbordes falsos). Para ver móvil real: emulación de dispositivo del navegador o un `iframe` de 411 px en una página de escritorio. Para medir: `document.documentElement.scrollWidth > innerWidth + 1`. Antes de creer un defecto en una captura, reprodúcela sobre la versión anterior.
- **Verifica en la página completa**, con la consola abierta: una vista aislada esconde errores que rompen el resto (un `null` dentro de `render()` decapita lo que viene después).

### Mac (simulador iOS o Android)

Comandos de la investigación previa; **verificar en el entorno** antes de fijarlos:

```bash
xcrun simctl list devices
xcrun simctl boot <udid>
xcrun simctl status_bar booted override --time "9:41" --batteryState charged --batteryLevel 100 --cellularMode active --cellularBars 4 --wifiBars 3
xcrun simctl io booted screenshot --type=png salida.png
xcrun simctl ui booted appearance light
xcrun simctl status_bar booted clear   # al terminar
```
- `simctl` no toca la UI: para navegar usa XCUITest, Maestro o `flutter drive`/`integration_test`.
- Permisos de privacidad: `xcrun simctl privacy`.
- Android en Mac: mismo AVD y mismas reglas de `adb` de arriba (sin las trampas de PowerShell).

### WSL2 / servidor Linux

- Sin pantalla: navegador headless para web; backend y API se prueban con `curl`/`.http`.
- El emulador Android no corre aquí; si la app móvil es parte del alcance y no hay emulador accesible, márcala **sin verificar** en vez de asumir.
- iOS no existe en Windows ni Linux: "sin verificar en iOS" en `VERIFICATION.md` y en el texto.

## 3. Recorrido

- [ ] Sigue cada tarea del manual con el texto escrito, nada más.
- [ ] Prueba éxito y errores relevantes y permisos por rol.
- [ ] API: ejecuta cada ejemplo contra el entorno de pruebas y guarda la respuesta **real**; contrástala con el OpenAPI.
- [ ] Runbooks: ejecútalos en staging o contenedor; el resultado esperado de cada paso debe coincidir.
- [ ] Datos de ensayo: bórralos por el mismo camino que los creó (borrar a mano una sola tabla deja huérfanos y rompe pantallas).

## 4. Capturas

Automatizadas:
- Un script por flujo: navega, escribe datos demo, captura. Regenerable con un comando (`docs:screenshots`).
- Web: viewport fijo (p. ej. 1280x800 y 390x844 con emulación de móvil), `deviceScaleFactor: 2`, idioma y zona horaria fijos, reloj congelado, animaciones desactivadas, esperar a selectores o a red inactiva. Máscara sobre elementos sensibles o dinámicos.
- Android: barra de estado limpia con el modo demo del sistema (`settings put global sysui_demo_allowed 1` y `am broadcast -a com.android.systemui.demo -e command ...`; **verificar en el entorno**); mismo AVD, resolución y tema en todas.
- iOS: `status_bar override` como arriba.

Anotadas:
- Números en círculos, recuadros y flechas, con el mismo color, grosor y estilo en todo el manual.
- En web, inyecta los overlays en el DOM antes de capturar (nítidos y reproducibles); en móvil, procesa la imagen con un script y coordenadas obtenidas de `uiautomator dump`.
- Recorta al área relevante.

Consistencia y orden:
- Nombre `NN-flujo-paso.png`, carpeta `docs/user-manual/assets/screenshots/<flujo>/`, manifiesto `screenshots.yml` (captura -> paso del manual -> script).
- Optimiza el peso (oxipng o WebP; **verificar en el entorno**) y escribe `alt`.

Privacidad (obligatorio):
- [ ] Solo datos demo. Sin tokens, URLs con claves, barra de direcciones con sesión, notificaciones del sistema ni nombre de usuario del sistema operativo.
- [ ] Revisa **cada** imagen a ojo; elimina metadatos.
- [ ] Ante la duda, rehacer la captura.

## 5. `docs/VERIFICATION.md`

Se abre al empezar y se llena durante las pruebas. Es la evidencia de que nada se inventó.

```md
# Registro de verificación
Entorno: SO ..., runtime ..., commit ..., fecha ...

| Doc / paso | Método | Entorno | Fecha | Resultado | Notas |
|---|---|---|---|---|---|
| user-manual/tareas/crear-pedido.md pasos 1-6 | Recorrido manual + script de captura | Navegador, datos demo | AAAA-MM-DD | OK | |
| mobile/README.md build release | Instalar y abrir en emulador | Android 14 | AAAA-MM-DD | Corregido | Faltaba un parámetro de build; ya documentado |
| ios/... | - | - | - | No verificable | No hay Mac disponible |
```

Resultados permitidos: `OK`, `Corregido` (el doc estaba mal), `Parcial`, `No verificable` (con motivo), `Sin verificar`. Todo `No verificable` o `Sin verificar` también aparece marcado en la página correspondiente.
