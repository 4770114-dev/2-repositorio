# 🛡️ Guía Completa de Seguridad Informática: Malware, Clasificación, Diagnóstico por CMD y Protección

## 1. Tipos de Virus / Malware y el Daño que Causan

| Tipo de Malware | Mecanismo de Acción | Daño / Impacto |
| :--- | :--- | :--- |
| **Gusano (Worm)** | Se propaga automáticamente por la red duplicándose a sí mismo. | Satura la red y consume ancho de banda y memoria del sistema. |
| **Troyano (Trojan)** | Se oculta dentro de software aparentemente inofensivo. | Crea una puerta trasera para permitir el acceso no autorizado de terceros a tus archivos. |
| **Ransomware** | Cifra la información del disco duro. | Bloquea los archivos personales o el sistema operativo completo e impide su uso. |
| **Spyware** | Funciona en segundo plano recolectando actividad. | Espía hábitos de navegación y recopila credenciales o información personal sin autorización. |
| **Adware** | Diseñado para desplegar publicidad masiva. | Genera ventanas emergentes molestas y degrada el rendimiento del navegador. |
| **Keylogger** | Registra la entrada de datos del teclado. | Captura contraseñas, números de tarjetas de crédito y mensajes privados. |
| **Rootkit** | Se oculta a nivel profundo en el sistema operativo. | Oculta otros programas maliciosos y otorga control administrativo al atacante. |
| **Botnet** | Malware que toma control de múltiples computadoras. | Convierte el equipo en un "zombi" usado para ataques masivos de denegación de servicio (DDoS). |
| **Virus de sector de arranque (Boot Sector)** | Infecta el registro principal de arranque (MBR). | Impide que el sistema operativo inicie correctamente. |
| **Virus de macro** | Escrito en lenguajes de programación integrados en documentos de ofimática. | Modifica o borra archivos al abrir documentos infectados de Word o Excel. |

---

## 2. Categorías de Virus

### Según su ubicación:
* **Residentes en memoria:** Se alojan en la memoria RAM y permanecen activos desde que se enciende el equipo.
* **No residentes:** Se ejecutan únicamente cuando se abre el archivo infectado.

### Según su capacidad de modificación:
* **Polimórficos / Metamórficos:** Cambian su código o estructura cada vez que infectan un archivo para evitar ser detectados por firmas de antivirus.

---

## 3. Creación de Scripts de Prueba (Entorno Controlado y Daño Bajo)

En la formación académica e investigación de seguridad, se estudian programas de "daño bajo" o demostraciones de concepto (PoC). Son scripts inofensivos creados en entornos de prueba para comprender el funcionamiento de un proceso sin comprometer la integridad del sistema:

* **Simulación en Batch (.bat):** Un script que genera un bucle infinito abriendo la ventana del bloc de notas o mostrando un mensaje recurrente en pantalla. No elimina archivos ni roba datos, pero permite analizar cómo un bucle consume recursos del procesador y memoria RAM.

---

## 4. Método Único de Análisis y Detección de Infecciones por CMD

Para analizar de forma creativa e independiente si un equipo está infectado sin usar software de terceros, se pueden emplear comandos nativos de la consola de comandos (CMD) de Windows siguiendo un flujo de auditoría por pasos:

### Paso 1: Auditoría de conexiones de red sospechosas
El malware como troyanos, spyware o botnets suele comunicarse con servidores externos.
```cmd
netstat -ano | findstr "ESTABLISHED"

```

* **Análisis:** Examina las direcciones IP de destino. Si detectas conexiones hacia ubicaciones desconocidas o puertos inusuales, anota el número de la última columna: el **PID (Identificador de Proceso)**.

### Paso 2: Identificación del ejecutable tras el proceso

Usa el PID obtenido para descubrir qué programa está manteniendo la conexión activa:

```cmd
tasklist /FI "PID eq [Número_PID]"

```

* **Análisis:** Muestra el nombre exacto del archivo `.exe`. Si el nombre imita procesos legítimos del sistema con errores ortográficos (ej. `syssttem32.exe`) o se ejecuta desde carpetas temporales, es una señal clara de malware.

### Paso 3: Inspección de persistencia en el arranque del sistema

El malware suele configurarse para ejecutarse automáticamente al encender el equipo:

```cmd
wmic startup get caption, command, location

```

* **Análisis:** Revisa la columna `Command`. Si hay programas desconocidos apuntando a rutas como `C:\Users\...\AppData\Local\Temp`, podrían ser gusanos o spyware residentes.

### Paso 4: Comprobación de integridad de archivos del sistema

Los virus de sector de arranque o rootkits suelen alterar archivos críticos de Windows:

```cmd
sfc /scannow

```

* **Análisis:** Este comando analiza la firma digital e integridad de los archivos del sistema operativo y repara automáticamente aquellos que hayan sido alterados o dañados por malware.

### Paso 5: Aislamiento y finalización del proceso sospechoso

Si confirmas un proceso malicioso en ejecución, puedes detenerlo inmediatamente:

```cmd
taskkill /PID [Número_PID] /F

```

---

## 5. Tipos de Protección

* **Protección Preventiva:** Antivirus con análisis heurístico, parches de seguridad del sistema operativo y educación sobre correos sospechosos (Phishing).
* **Protección Detectiva:** Cortafuegos (Firewalls) para monitorear el tráfico entrante y saliente, análisis por comandos en CMD y sistemas de detección de intromisiones (IDS).
* **Protección Correctiva:** Copias de seguridad (Backups) actualizadas e imágenes de restauración del sistema.

```

```
