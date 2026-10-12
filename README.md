# Windows 11 IoT Enterprise LTSC: Convierte Evaluación → Licencia Completa

> **Edición actualizada 2026.** Convierte tu Windows 11 **IoT Enterprise LTSC Evaluation**
> a una copia con licencia completa — **sin reinstalar, sin perder datos**, en ~5 minutos.

---

## Descarga el ISO (oficial)

Baja el ISO de evaluación oficial desde el **Microsoft Evaluation Center** (gratis, solo pide registro):

- 💻 Disponible para **x64** y **ARM64**
- 🕒 Evaluación de **90 días**
- 🔗 <https://www.microsoft.com/en-us/evalcenter/download-windows-11-iot-enterprise-ltsc-eval>

> Al instalarlo **no hace falta clave**: Microsoft entrega la copia de evaluación sin pedir
> clave de producto. Conviértela después con esta guía.

---

## Lo que necesitas

- Windows 11 **IoT Enterprise LTSC** (evaluación) ya instalado
- Una cuenta de **Administrador**
- Una **clave de producto genuina** de IoT Enterprise LTSC
- Conexión a internet estable

---

## Paso 1 — Comprueba tu edición

Abre **Símbolo del sistema como administrador** y ejecuta:

```cmd
winver
slmgr /dlv
```

Mira la línea de **Sistema operativo / Edición**:

- ✅ Muestra `Windows(R), IoTEnterpriseS` → **el SKU ya está instalado, salta al Paso 2**.
- ❌ Muestra `Core` / `Professional` / `Evaluation` → haz el **Paso 2**.

> El **ISO de evaluación oficial** de Microsoft ya incluye la
> edición **IoT Enterprise LTSC**, así que la mayoría puede saltarse el Paso 2.

---

## Paso 2 — Instala el SKU de IoT Enterprise (solo si el Paso 1 falla)

1. Descarga el [`assets/IoTEnterpriseS.zip`](assets/IoTEnterpriseS.zip) incluido en este repo.
2. Descomprímelo y copia la carpeta **`IoTEnterpriseS`** dentro de:

   ```
   C:\Windows\System32\spp\tokens\skus
   ```

3. Reinstala las licencias:

   ```cmd
   cscript /nologo %windir%\system32\slmgr.vbs /rilc
   ```

4. Vuelve al **Paso 1** y confirma que ahora muestra `IoTEnterpriseS`.

---

## Paso 3 — Borra la licencia de evaluación

Ejecuta estos comandos **en orden** (terminal de administrador, copia uno a uno):

```cmd
cscript /nologo %windir%\system32\slmgr.vbs /upk
cscript /nologo %windir%\system32\slmgr.vbs /ckms
cscript /nologo %windir%\system32\slmgr.vbs /cpky
sc config LicenseManager start= auto
net start LicenseManager
sc config wuauserv start= auto
net start wuauserv
```

| Comando | Qué hace |
|---|---|
| `/upk` | Elimina la clave de evaluación |
| `/ckms` | Limpia las referencias al servidor KMS |
| `/cpky` | Borra la clave en caché del registro |
| `sc` / `net` | Asegura que los servicios de licencias y Windows Update estén activos |

---

## Paso 4 — Instala tu clave genuina

**La forma más fácil:** `Configuración → Sistema → Activación → Cambiar clave de producto`,
introduce tu clave genuina de **IoT Enterprise LTSC** y sigue los pasos.

**O desde la terminal:**

```cmd
cscript /nologo %windir%\system32\slmgr.vbs /ipk <TU-CLAVE>
cscript /nologo %windir%\system32\slmgr.vbs /ato
```

---

## Paso 5 — Verifica

```cmd
slmgr /dlv
slmgr /xpr
```

Deberías ver:

- 🟢 **Estado de licencia: con licencia (Licensed)**
- 🟢 **Windows activado de forma permanente** (sin fecha de expiración)

---

## Solución de problemas

| Problema | Solución |
|---|---|
| `La clave genérica no se usa para esta edición` | La GVLK solo funciona con el SKU `IoTEnterpriseS` → completa el **Paso 2** primero. |
| `slmgr /xpr` sigue mostrando expiración | La clave de evaluación no se borró del todo → repite el **Paso 3** en orden y luego el **Paso 4**. |
| Error de activación `0xC004F074` | No se alcanzó el KMS — espera o activa con una clave **retail / MAK** desde `Configuración`. |
| La edición sigue mostrando "Evaluation" tras el Paso 2 | Verifica que la carpeta sea `C:\Windows\System32\spp\tokens\skus\IoTEnterpriseS` y vuelve a ejecutar `/rilc`. |

---

## Preguntas frecuentes

**¿Tengo que reinstalar Windows?**
No. Todo — aplicaciones, archivos, ajustes — queda intacto.

**¿Qué pasa si no hago nada?**
La evaluación sigue recordándote que actives y **expira (~90 días, prorrogable)**,
con funciones empresariales limitadas.

**¿La clave GVLK genérica es suficiente?**
Detiene la evaluación pero **no activa** el sistema. Aún necesitas una **clave genuina** para activar.

**¿Puedo volver a una edición estándar después?**
No — IoT Enterprise LTSC es otra familia de producto. Necesitarías una instalación limpia.

---

## Fuente

Actualización del tutorial original de [ProSoftKeys](https://prosoftkeys.com/convert-windows-11-iot-enterprise-ltsc-evaluation-to-version/).

---

## `$ ls ~/contacto`

[![X](https://img.shields.io/badge/X-@madmasx-000000?style=for-the-badge&logo=x&logoColor=E63946&labelColor=0d1117&color=E63946)](https://x.com/madmasx)
[![GitHub](https://img.shields.io/badge/GitHub-madmasx-000000?style=for-the-badge&logo=github&logoColor=E63946&labelColor=0d1117&color=E63946)](https://github.com/madmasx)
[![Buy Me a Coffee](https://img.shields.io/badge/Buy_Me_a_Coffee-apóyame-000000?style=for-the-badge&logo=buymeacoffee&logoColor=E63946&labelColor=0d1117&color=E63946)](https://buymeacoffee.com/madmasx)