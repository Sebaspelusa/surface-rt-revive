# Surface RT Revive

Convierte una **Surface RT 1** (2012 · Tegra 3 · Windows RT 8.1) en un **lector de libros tipo Kindle** que abre **PDF, EPUB, MOBI, DjVu, FB2, CHM y CBR**. Todo en español, paso a paso, gratis.

La tablet vieja que no corre absolutamente nada hoy sirve para lo que de verdad importa: **leer**.

---

## ⚠️ Aviso legal

- Esto **anula la seguridad de la tableta** (se apaga Secure Boot). Es **por tu cuenta y riesgo**.
- Puede **anular la garantía**. En equipos de 2012 ya expiró, pero déjalo claro.
- **No distribuimos firmware de Microsoft** ni recuperación de fábrica: eso se descarga de la fuente oficial.
- No me hago responsable de datos perdidos, bloqueos o cualquier daño. **Respaldas tus cosas antes.**
- Solo aplica a **Surface RT 1 y Surface 2** (Tegra). No funciona en equipos Qualcomm (Dell XPS 10, Ativ Tab, Lumia 2520).

---

## Qué necesitas

| Material | Detalle |
|---|---|
| Tableta | Surface RT 1, **Modelo 1516**, 32 GB, Windows RT 8.1 |
| PC | Cualquier Windows (para preparar los USB) |
| USB 1 | De **8 GB o más**, todo borrado (FAT32) — recuperación |
| USB 2 | De **8 MB a 8 GB** (FAT32) — jailbreak |
| **Hub USB** | Imprescindible: la Surface tiene **un solo puerto** |
| **Teclado USB físico** | Imprescindible: **el Type Cover NO sirve** en los menús de arranque |
| Cargador | Conéctalo: la restauración **tarda de 30 a 60 minutos** |
| Paciencia | Sí, en serio |

> **Truco:** si el botón **Vol −** de tu Surface está malo (es común), no te preocupes. Este método no lo necesita — hay una entrada por Windows.

---

## 1 · Descargas

### A. Imagen de recuperación de fábrica (BMR)
Oficial de Microsoft — pon el número de serie de tu Surface (12 dígitos, va impreso debajo del soporte trasero):
- https://support.microsoft.com/surface-recovery-image
- Archivo: `SurfaceRT_BMR_10.8.17.0.zip` — **3.106.217.302 bytes**
- SHA256: `B5C60B117951D8064F0CEE4ECB20CE2EB0A8A75EFBFA27D60DDCA7DF2B58CA94`

### B. Tegra Jailbreak USB
Guía oficial de la comunidad (leerla antes de tocar nada):
- https://windows-rt-devices.gitbook.io/windows/tools/tegra-jailbreak-usb
- Paquete USB: `Tegra_Jailbreak_USB.zip` — **5.415.180 bytes**

> 🔒 **Aviso antivirus:** Windows Defender marca `Yahallo.efi` y el paquete como virus. **Es falso positivo conocido desde agosto de 2024.** Son herramientas de jailbreak, que por naturaleza se detectan como amenazas. Añade la excepción o desactívalo temporalmente.

### C. Lector (en este repositorio)
- [`apps/SumatraPDF-3.3.3-Arm32.zip`](apps/SumatraPDF-3.3.3-Arm32.zip) — 6.658.563 bytes
  - SHA256: `CB003F7C0E6801E67DA1F43F7F8ED2DBBC7A8D005275A4821D9EC8A22891C774`
- [`apps/PlumeQi.cer`](apps/PlumeQi.cer) — 500 bytes
  - SHA256: `CD68DF6EFDF0ED40ED51BE6625F71FAA3364E594968A6FA404D77889800264D8`

> **¿Por qué la versión 3.3.3 y no la última?** Desde la 3.3.4 SumatraPDF usa **Edge WebView2** que Windows RT 8.1 no tiene. La 3.3.3 es la última que corre de verdad.

---

## 2 · Paso 1 — Restaurar de fábrica

Esto **borra todo** y repone el sistema limpio. Es necesario porque los updates posteriores a octubre de 2016 **destruyen los llaveros de Secure Boot** y el jailbreak ya no instala.

1. Extrae el contenido del `SurfaceRT_BMR_10.8.17.0.zip` a la raíz del **USB 1** (queda: `Boot`, `EFI`, `sources`, `bootmgr.efi`, etc.).
2. Con la Surface **encendida y en Windows**, ve a:
   **Configuración → Cambiar configuración de PC → Actualización y recuperación → Recuperación → Opciones de arranque avanzadas → Reiniciar ahora.**
3. **Solucionar problemas → Restablecer tu PC.**
4. Elige **"Sí, volver a particionar las unidades"**.
5. Elige **"Limpiar la unidad por completo"** (no la rápida).
6. **Reset** y no toques nada: de 30 a 60 minutos.

### Configuración inicial (ojo, esto protege el jailbreak)
- **Omite el WiFi** (`Skip this step` / configurar sin red).
- **Cuenta local**, NO cuenta de Microsoft.
- Desactiva "configuración rápida" / envío de datos.
- **No conectes WiFi todavía.**

---

## 3 · Paso 2 — Jailbreak (Secure Boot)

Pasa por la **pantalla de arranque oculta**, y ahí solo sirve **teclado USB físico** por el hub.

### 3.1 Instalar Golden Keys (opcional)
1. Conecta **hub → teclado USB + USB 2 de jailbreak** a la Surface.
2. Abre **Equipo → USB**, clic derecho en `Jailbreak_USB_Menu.cmd` → **Ejecutar como administrador**.
3. Revisa el estado. Debería decir:
   ```
   Device:   Surface with Windows RT - UEFI: v3.31.500
   Golden Keys: Disabled        Yahallo: Disabled
   ```
4. Opción **`1`** (Install Golden Keys) + **Enter**.
5. Reinicia (**`R`**). Al arrancar del USB aparece la política de Secure Boot: pulsa **`F3`** en **"Accept and Install"** con el teclado USB.
6. Reinicia a Windows normal.

> En una Surface RT 1 limpia el menú puede marcar **`Golden Keys: Unknown`**. **No es un problema**: para correr aplicaciones solo hacen falta dos cosas, Secure Boot apagado (Yahallo) y UMCI Audit Mode (paso 5).

### 3.2 Instalar Yahallo (obligatorio)
7. Vuelve al menú: opción **`3`** (Install Yahallo) + **Enter** → **`R`**.
8. Arranca `Yahallo.efi` y muestra algo como:
   ```
   Thinking...
   Have fun
   SetupMode = 0x1
   SecureBoot = 0x0
   !!! PLEASE RESET YOUR DEVICE MANUALLY USING THE POWER BUTTON !!!
   ```

### 🛑 3.3 AL TOPE: el bucle de Yahallo (aquí se pierde la gente)
El programa no se reinicia solo. Si le das a reiniciar **con el USB puesto, entra en bucle** y arranca Yahallo para siempre.

**Hazlo en este orden exacto:**

1. **Mantén pulsado el botón de encendido 10-15 segundos** hasta que la pantalla quede **negra del todo**.
2. **Retira el USB de jailbreak.** No lo dejes puesto. ← **ESTE ES EL PASO QUE TODOS OLVIDAN**
3. Ahora sí, **enciende** la tablet.
4. Arranca al disco interno y entra a **Windows**.

> Si metes el USB antes de encender, vuelve el bucle. Saquemoslo siempre.

---

## 4 · Paso 3 — Verificar que funcionó

1. `Win + R` → `msinfo32` → **Enter**.
2. Busca **"Estado de arranque seguro"**.
3. Debe decir **Desactivada**.

```
Nombre del SO        Microsoft Windows RT 8.1
Modelo del sistema   Surface with Windows RT
SKU del sistema      Surface_RT_1_IDP
Estado de arranque   Desactivada ✅
```

---

## 5 · Paso 4 — UMCI Audit Mode

Windows RT tiene **otra barrera además de Secure Boot**: la verificación de código (UMCI) que bloquea todo `.exe` que no venga firmado por Microsoft. Hay que ponerla en modo **auditoría**: registra, pero **no bloquea**.

1. Con el USB de jailbreak puesto, corre `Jailbreak_USB_Menu.cmd` (admin).
2. Opción **`8`** → **Enable UMCI Audit Mode** → espere la confirmación.
3. El estado debe cambiar a **`UMCI Audit Mode: Enabled`**.
4. Opción **`E`** para salir. **NO uses `R`** (recuerda el bucle).
5. **Reinicia** desde Windows: Inicio → Encendido → **Reiniciar**.

> Si un día quieres desactivarlo: opción **`9`**.

---

## 6 · Paso 5 — Congelar Windows Update

**Un solo update puede romper el jailbreak.** Congélalo:

1. Menú del USB (admin) → opción **`13`** → **Disable Windows Update**.
2. `E` + Enter para salir.

---

## 7 · Paso 6 — El truco oculto: el certificado PlumeQi

Este es el paso donde **toda la gente se atasca** y nadie lo explica.

Si ejecutas `SumatraPDF.exe` y te sale:

> **"Windows no puede comprobar la firma digital en este archivo. Un cambio reciente en el hardware o en el software podría haber instalado un archivo con una firma incorrecta o dañada..."**

**no es un virus y no está corrupto el archivo.** El programa **sí está firmado**, pero con un certificado autofirmado (`CN=PlumeQi`) que Windows RT no reconoce como de confianza. El autor de los puertos lo dice en su propio repositorio:

> *"the exe ... is not signed. It can not work under Windows RT 8.1 ... **I signed my certification, it works well.**"*

### Solución: instalar el certificado

1. Copia `apps/PlumeQi.cer` a la Surface (sobre el Escritorio, por ejemplo).
2. **Doble clic en `PlumeQi.cer`**.
3. **Instalar certificado… → Siguiente.**
4. Marca **"Colocar todos los certificados en el siguiente almacén"** → **Examinar…**
5. Marca **"Mostrar los almacenes"** y elige **"Entidades de certificación raíz de confianza"** (*Trusted Root Certification Authorities*).
6. **Siguiente → Finalizar.** Acepta la advertencia: **Sí**.

> ⚠️ Sin instalar este certificado, **ningún `.exe` de los puertos de PlumeQi funcionará**, aunque tengas el jailbreak impecable. Si aún así falla, reabre el menú del USB y confirma que `UMCI Audit Mode` diga **Enabled**.

> 🔐 **Nota de seguridad (léela, es importante):** ese almacén —**Entidades de certificación raíz de confianza**— es el más sensible de Windows: cualquier clave que viva ahí puede firmar archivos o sitios que el sistema aceptará como confiables. Es el procedimiento estándar de esta escena y el certificado es público, pero quedas **confiando en una clave de un tercero**. Si no te convence: **(a)** compila tú mismo SumatraPDF desde el código fuente del puerto (GPL-3.0, sin certificado de nadie) → https://github.com/plumeqi/RT_Arm32_Porting (`SumatraPDF3.3.3/SourceCodes`), o **(b)** cuando dejes de usar el lector, elimina el certificado: Panel de control → Certificados → Entidades de certificación raíz de confianza → `PlumeQi` → Eliminar (el lector dejará de abrir hasta reinstalarlo).

---

## 8 · Paso 7 — Instalar el lector

1. Descomprime `SumatraPDF-3.3.3-Arm32.zip`. Trae:
   - `SumatraPDF.exe` — portable (6.085.568 bytes) → **este es el que usas**
   - `SumatraPDF-Installer.exe` — instalador (4.322.240 bytes) → opcional
2. Copia `SumatraPDF.exe` a una carpeta estable, por ejemplo:
   `C:\Users\tu_usuario\Documentos\Lector\`
3. **Doble clic.** Ya abre.

### Hacerlo lector por defecto
- Clic derecho en un **PDF** → **Abrir con → Elegir otra aplicación → SumatraPDF** → marcar **"Siempre"**.
- Repite con un **EPUB**.

### Formatos que abre
PDF · EPUB · MOBI · DjVu · FB2 · CHM · CBZ · CBR · XPS · DJVU · TXT

---

## 9 · Problemas comunes

| Síntoma | Causa | Solución |
|---|---|---|
| No puedo entrar al menú de arranque | Botón **Vol −** malo | Configuración → Cambiar configuración de PC → Actualización y recuperación → Recuperación → Opciones de arranque avanzadas → Reiniciar ahora |
| `Yahallo.efi` error `0xc0000428` | Secure Boot sigue activo / llaveros corruptos | Restaura de fábrica (paso 1) y repite el jailbreak |
| Llega a `Golden Keys: Disabled` y no cambia con F3 | Updates posteriores a oct-2016 | Restaura de fábrica (paso 1) |
| `Golden Keys: Unknown` | Normal en RT 1 limpia | No bloquea nada. Sigue con Yahallo |
| Pantalla de Yahallo se repite en bucle | USB puesto al encender | **Apaga 15 s → retira el USB → enciende** |
| "no puede comprobar la firma digital" | Certificado PlumeQi no instalado | Paso 6 |
| Defender lo borra | Falso positivo conocido | Excepción de carpeta |
| Los `.exe` no corren tras el jailbreak | Falta UMCI Audit Mode | Opción `8` + reinicio (paso 4) |
| Se reinicia solo y descarga updates | Windows Update activo | Opción `13` (paso 5) |

---

## 10 · Checklist final

Antes de darlo por terminado, comprueba todo esto:

- [ ] `msinfo32` → **Estado de arranque seguro = Desactivada**
- [ ] Menú del USB → **Yahallo: Enabled**
- [ ] Menú del USB → **UMCI Audit Mode: Enabled**
- [ ] Windows Update congelado (opción 13)
- [ ] Certificado `PlumeQi.cer` en **Entidades de certificación raíz de confianza**
- [ ] `SumatraPDF.exe` abre sin errores
- [ ] Abre un **PDF** y pasa de página
- [ ] Abre un **EPUB** y pasa de página
- [ ] SumatraPDF es el lector por defecto de PDF y EPUB
- [ ] **WiFi sigue desconectado** (para que no llegue ningún update)

---

## 11 · Lo que NO hace falta

- ❌ **Windows 10** en la Surface: solo existe la compilación 15035 de prueba, es inestable, y la app moderna de Kindle exige ARM64 (no existe para ARM32). Por eso esta ruta: **lectores locales, sin nube**.
- ❌ **Android**: no hay puerto estable y fiable.
- ❌ **Monitoreo por cable**: la Surface RT 1 solo tiene salida micro-HDMI, no sirve como segundo monitor por cable.
- ❌ **App propia**: al final no hizo falta. SumatraPDF cubre PDF y EPUB sin complicarte la vida.

---

## 12 · Créditos y licencias

| Proyecto | Autor | Licencia | Uso aquí |
|---|---|---|---|
| **Yahallo** (apagar Secure Boot) | Bingxing Wang y colaboradores | GPLv2 | Enlazado, no redistribuido |
| **Golden Keys** (Secure Boot Debug Policy) | my123 (@never_released) y slipstream (@TheWack0lian) | Writeup de RoL | Enlazado |
| **Tegra Jailbreak USB** | lgibson02 y comunidad | Comunidad | Enlazado |
| **Guía Tegra Jailbreak** | windows-rt-devices.gitbook.io | — | Enlazado |
| **SumatraPDF 3.3.3 (ARM32)** | Krzysztof Kowalczyk · puerto por **PlumeQi** | GPL-3.0 | Binario en este repo |
| Puertos de apps ARM32 | **PlumeQi** | — | https://github.com/plumeqi/RT_Arm32_Porting |

SumatraPDF se distribuye bajo **GPL-3.0**. El código fuente del puerto a ARM32 está en el repositorio de Plumeqi en la carpeta `SumatraPDF3.3.3/SourceCodes`:
https://github.com/plumeqi/RT_Arm32_Porting

Este repositorio en particular se publica bajo **CC BY 4.0**: úsalo, cámbialo, compártelo. Solo no redistribuyas firmware de Microsoft ni la imagen de recuperación.

---

## Sobre este proyecto

Escrito para que **cualquier persona con una Surface RT 1 vieja** la convierta en un lector de libros en vez de tirarla a la basura. Proyecto en español, pensado para Latinoamérica.

Si te sirvió, **dalle estrella al repo** y comparte la guía.
