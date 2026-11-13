# ABS 3.0 — Analysis of Biomechanical Signals

**ABS 3.0** (*Análisis Biomecánico de Señales*) es una aplicación de escritorio
multiplataforma para importar, visualizar, filtrar y analizar datos de fuerza
exportados en CSV desde la plataforma dinamométrica AMTI.

Fue desarrollada para el **Laboratorio de Investigación en Biomecánica y
Análisis del Movimiento (LIBiAM)** — DCB — Cenur Litoral Norte — UdelaR, como
proyecto final de la carrera Tecnólogo en Informática de la **Universidad
Tecnológica (UTEC)**, sede Paysandú.

ABS 3.0 se puede adaptar sin tocar el código: interpreta archivos CSV con
distintas estructuras de columnas y encabezados, y permite crear, guardar,
importar y exportar fórmulas propias desde la interfaz.

---

## Índice

- [Características principales](#características-principales)
- [Requisitos](#requisitos)
- [Ejecución desde el código fuente](#ejecución-desde-el-código-fuente)
  - [Windows](#windows)
  - [macOS](#macos)
  - [GNU/Linux](#gnulinux)
- [Ejecución de la versión portable](#ejecución-de-la-versión-portable)
- [Guía rápida de uso](#guía-rápida-de-uso)
- [Funcionalidades en detalle](#funcionalidades-en-detalle)
- [Dónde se guardan los datos](#dónde-se-guardan-los-datos)
- [Estructura del proyecto](#estructura-del-proyecto)
- [Tecnologías](#tecnologías)
- [Autoría y cómo citar](#autoría-y-cómo-citar)

---

## Características principales

- **Carga flexible de CSV**: detecta el separador, la cabecera real, la fila de
  unidades, la frecuencia de muestreo y los subframes, aunque el archivo tenga
  metadatos al principio.
- **Mapeo de columnas configurable**: reconoce Fuerza, Momento y Centro de
  Presión, aprende los nombres de columna y permite reordenarlos y ocultarlos.
- **Gráficas interactivas**: una por señal, con unidades en el eje, zoom
  configurable y reducción automática de puntos en archivos grandes.
- **Filtros Butterworth** pasa-bajos, pasa-altos y pasa-banda de fase cero; la
  señal filtrada se superpone a la original sin reemplazarla.
- **Intervalos y subintervalos** de análisis, con notas, replicación entre
  gráficas y selección masiva (todos, pares, impares, ninguno).
- **Fórmulas incorporadas y personalizadas**: aceleración, potencia, impulso,
  impacto, fuerza resultante y un constructor visual para crear nuevas.
- **Proyectos**: se guarda y se reabre el análisis completo.
- **Exportación** de datos, intervalos y resultados en CSV o en un único ZIP.
- **Accesibilidad**: paletas para distintos tipos de daltonismo, estilos de
  línea diferenciados e interfaz que se adapta al tamaño de la ventana.
- **Editor CSV integrado** para definir a mano secciones y cabeceras cuando la
  detección automática no alcanza.

---

## Requisitos

| Requisito | Detalle |
|---|---|
| Sistema operativo | Windows 10 o posterior, macOS 13 o posterior, o una distribución GNU/Linux actual de 64 bits |
| Python (solo desde el código fuente) | 3.11 a 3.13, de 64 bits |
| Espacio en disco | Al menos 1 GB entre aplicación, entorno virtual y datos generados |

Las dependencias de Python se instalan desde `requirements.txt`:

| Biblioteca | Versión |
|---|---|
| NumPy | 2.4.4 |
| pandas | 3.0.3 |
| PyQtGraph | 0.14.0 |
| PySide6 | 6.11.0 |
| SciPy | 1.18.0 |
| SQLAlchemy | 2.0.49 |

---

## Ejecución desde el código fuente

En los tres sistemas el procedimiento es el mismo: clonar el repositorio, crear
un entorno virtual, instalar las dependencias y ejecutar `main.py`. Solo
cambian los comandos.

```bash
git clone <url-del-repositorio> ABS-3.0
cd ABS-3.0
```

### Windows

1. Instalar Python 3.11–3.13 desde [python.org](https://www.python.org/downloads/windows/)
   y marcar la opción **Add python.exe to PATH** durante la instalación.
2. Abrir **PowerShell** en la carpeta del proyecto y crear el entorno virtual:

   ```powershell
   py -3 -m venv .venv
   ```

3. Activar el entorno:

   ```powershell
   .venv\Scripts\Activate.ps1
   ```

   En **CMD** se usa `.venv\Scripts\activate.bat`.

4. Instalar las dependencias y ejecutar:

   ```powershell
   python -m pip install --upgrade pip
   pip install -r requirements.txt
   python main.py
   ```

Si PowerShell no permite ejecutar scripts, se puede habilitar solo para la
sesión actual:

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
```

También se puede usar el intérprete del entorno sin activarlo:

```powershell
.venv\Scripts\python.exe main.py
```

### macOS

1. Instalar Python 3.11–3.13 desde [python.org](https://www.python.org/downloads/macos/)
   o con Homebrew:

   ```bash
   brew install python@3.12
   ```

2. Abrir **Terminal** en la carpeta del proyecto, crear y activar el entorno:

   ```bash
   python3 -m venv .venv
   source .venv/bin/activate
   ```

3. Instalar las dependencias y ejecutar:

   ```bash
   python -m pip install --upgrade pip
   pip install -r requirements.txt
   python main.py
   ```

Las dependencias ofrecen versiones nativas para Apple Silicon (M1, M2, M3 o
posteriores) y para Intel; `pip` elige automáticamente la que corresponde.

### GNU/Linux

1. Instalar Python, el módulo `venv` y las bibliotecas del sistema que Qt
   necesita para abrir ventanas.

   **Ubuntu, Debian, Linux Mint:**

   ```bash
   sudo apt update
   sudo apt install python3 python3-venv python3-pip libxcb-cursor0 libxkbcommon-x11-0 libegl1 libgl1
   ```

   **Fedora:**

   ```bash
   sudo dnf install python3 python3-pip xcb-util-cursor libxkbcommon-x11 mesa-libEGL
   ```

   **Arch Linux / Manjaro:**

   ```bash
   sudo pacman -S python python-pip xcb-util-cursor libxkbcommon-x11
   ```

2. Crear y activar el entorno virtual:

   ```bash
   python3 -m venv .venv
   source .venv/bin/activate
   ```

3. Instalar las dependencias y ejecutar:

   ```bash
   python -m pip install --upgrade pip
   pip install -r requirements.txt
   python main.py
   ```

---

## Ejecución de la versión portable

Para quien solo quiere **usar** la aplicación, ABS 3.0 también se distribuye
como versión portable. Incluye Python y todas las bibliotecas, así que no hace
falta instalar nada ni ejecutar un instalador. Conviene mantener juntos los
archivos de la carpeta. En la primera apertura se crea la base de datos local de
configuraciones.

### Windows

**Descargar:** [Windows](https://github.com/jairobandera/ABS-3.0/releases/download/v3.0.0/ABS-3.0-Windows-x64.zip)

1. Si se recibió un ZIP, extraer su contenido en una carpeta del equipo, por
   ejemplo en el Escritorio.
2. Hacer doble clic sobre el ejecutable **ABS 3.0**.
3. Si Windows SmartScreen muestra una advertencia, y el archivo proviene de la
   fuente oficial del proyecto, elegir **Más información → Ejecutar de todos
   modos**.

### macOS

**Descargar:**
[Apple Silicon (M1, M2, M3)](https://github.com/jairobandera/ABS-3.0/releases/download/v3.0.0/ABS-3.0-macOS-Apple-Silicon.zip) //
[Intel](https://github.com/jairobandera/ABS-3.0/releases/download/v3.0.0/ABS-3.0-macOS-Intel.zip)

1. Elegir la carpeta según el procesador: abrir el menú Apple → **Acerca de esta
   Mac** y revisar la línea **Chip**. Apple M1, M2, M3 o posterior corresponde a
   la versión **Apple-Silicon**; un procesador Intel corresponde a la versión
   **Intel**.
2. Hacer clic derecho sobre **ABS 3.0** y elegir **Abrir**. La primera vez,
   macOS avisa que no puede verificar la aplicación porque no proviene de la App
   Store; cerrar el aviso con **Listo**.
3. Ir a **Configuración del Sistema → Privacidad y seguridad**, bajar a
   **Seguridad** y pulsar **Abrir de todos modos** junto al mensaje sobre ABS
   3.0. Confirmar con **Abrir de todos modos** (no elegir *Mover al basurero*)
   e ingresar la contraseña de administrador.
4. Este paso solo se hace una vez. Desde entonces la aplicación se abre con
   doble clic.

### GNU/Linux (AppImage)

**Descargar:** [ABS-3.0-x86_64.AppImage](https://github.com/jairobandera/ABS-3.0/releases/download/v3.0.0/ABS-3.0-Linux-x86_64.zip)

La versión para GNU/Linux es una AppImage para equipos x86_64 (Intel o AMD),
pensada para Ubuntu 22.04+, Debian 12+ o Linux Mint 21+.

1. Copiar `ABS-3.0-x86_64.AppImage` a una carpeta con permisos de escritura,
   por ejemplo Descargas o Documentos.
2. Darle permiso de ejecución. Desde el administrador de archivos: clic derecho
   → **Propiedades** → activar **Ejecutable como un programa**. Desde la
   terminal:

   ```bash
   chmod +x ABS-3.0-x86_64.AppImage
   ```

3. Abrirla con doble clic o con:

   ```bash
   ./ABS-3.0-x86_64.AppImage
   ```

En la primera apertura se crea la carpeta `ABS 3.0-datos` junto a la AppImage,
donde quedan fórmulas, preferencias, configuraciones y proyectos. Para hacer una
copia de seguridad, se cierra la aplicación y se copia esa carpeta. Si la
carpeta de la AppImage no permite escritura, los datos se guardan en
`~/.local/share/ABS 3.0`.

---

## Guía rápida de uso

1. **Cargar un CSV** con el botón *Cargar archivo CSV* del panel izquierdo o
   arrastrándolo al área central.
2. **Revisar el mapeo** en *Variables* (panel derecho): qué columnas se
   muestran y en qué orden.
3. **Completar las variables del sujeto** en el panel izquierdo: masa corporal,
   masa total y estatura. La gravedad viene en 9.8 y se puede editar.
4. **Filtrar** (opcional) desde *Filtros*: elegir señales, modo y frecuencia de
   corte.
5. **Marcar intervalos** con *Seleccionar intervalo*: un clic en el inicio y otro
   en el final. `Esc` cancela el punto pendiente o sale del modo.
6. **Calcular** desde *Recorte y fórmulas*: elegir intervalos, la fuente de
   datos (original o filtrada) y la fórmula, y pulsar *Aplicar*.
7. **Guardar** el proyecto o **Exportar** los resultados desde el panel
   superior.

El detalle de cada pantalla está en el *Manual de usuario*.

---

## Funcionalidades en detalle

### Carga de CSV

- Detecta el separador: coma, punto y coma, tabulación o barra vertical.
- Localiza la cabecera real cuando hay metadatos antes de los datos.
- Separa la fila de unidades, para que valores como `N`, `N.mm` o `mm` no se
  mezclen con las muestras.
- Conserva la frecuencia de muestreo declarada en la exportación.
- Reconoce `SubFrame`, `Sub Frame` y `Sub_Frame`. Cuando hay subframes, promedia
  las muestras de cada frame antes de graficar.
- Usa el motor C de pandas y evita recorrer celdas con bucles de Python.

### Gráficas

- Una gráfica por señal, con la unidad en el eje vertical (`N`, `N.mm`, `mm`).
- Sin factores de escala confusos: `Cz` se muestra como `0.000020 mm` y no como
  `20 × 10⁻⁶ mm`.
- *Configurar → Zoom de las gráficas* define si la rueda actúa sobre X, Y o
  ambos ejes. El valor predeterminado es solo X y la elección se recuerda.
- *Configurar → Colores de las gráficas* cambia los colores de las curvas
  original y filtrada al instante. *Restaurar colores* vuelve al azul y ámbar.
- El modo accesible usa su propia paleta y, al desactivarlo, reaparecen los
  colores personalizados.

### Filtrado

- Butterworth de orden 4 en tres modos: pasa-bajos, pasa-altos y pasa-banda.
- Se aplica hacia adelante y hacia atrás, con secciones de segundo orden, para
  no desplazar los eventos en el tiempo.
- *Frecuencia usada* se completa con la frecuencia detectada y se puede
  corregir. Con subframes se usa la frecuencia efectiva; por ejemplo, 2000 Hz
  con 8 subframes se procesa a 250 Hz.
- Muestra el límite de Nyquist y bloquea combinaciones inválidas.
- *Quitar filtro* restaura solo las señales seleccionadas.
- Filtrar no es obligatorio para calcular. El usuario elige si cada cálculo usa
  la señal original o la filtrada, y la procedencia queda registrada.

### Intervalos y subintervalos

- Los límites se redondean al frame entero más cercano (`30.15` → `30`,
  `30.76` → `31`).
- Por defecto no se superponen: si una selección invade otra, se ajusta al tramo
  libre siguiendo la dirección del gesto. Con `20–30` ocupado, seleccionar
  `24–50` crea `31–50`. La superposición se habilita en *Configurar*.
- Se ordenan de izquierda a derecha; los nombres automáticos siguen esa
  posición sin perder notas ni subintervalos.
- Con *Replicar en gráficas visibles*, todas las copias comparten el color.
- Cada intervalo admite una nota y se elimina con la cruz de su fila; al
  eliminarlo se eliminan también sus subintervalos.
- El doble clic sobre un intervalo abre su ventana de detalle para crear y
  calcular subintervalos.
- *Calcular sobre: Intervalos | Subintervalos* permite aplicar una fórmula en
  masa a los subintervalos de todos los intervalos de la señal. Par e impar se
  determinan dentro de cada padre (A2, B2, C2…).
- Los cálculos de intervalos y de subintervalos se mantienen separados y se
  restauran al reabrir las ventanas mientras el CSV siga cargado.
- Un intervalo marcado en una señal puede delimitar el cálculo de otra: un
  recorte replicado en Fx puede acotar una fórmula de Fz, cuyos valores se
  siguen tomando de Fz.

### Fórmulas

| Fórmula | Expresión | Unidad |
|---|---|---|
| Aceleración | `a = (Fz − m·g) / m` | m/s² |
| Potencia | `P = Fz · v`, con `v = ∫ a dt` | W |
| Impulso | `J = ∫ (Fz − m·g) dt` | N·s |
| Impacto | `maximo(Fz)` | N |
| Fuerza resultante | `√(Fx² + Fy² + Fz²)` | N |

- La integración se hace una vez sobre todo el registro, desde el primer frame
  con el sujeto quieto. El intervalo solo recorta el resultado. El impulso neto
  de un intervalo es la diferencia entre los extremos del recorte, y se informa
  también el reparto propulsivo/frenado.
- La fuerza resultante requiere el mismo intervalo en Fx, Fy y Fz (se eligen
  con *Todas las señales visibles*), se superpone en las tres gráficas y se abre
  también en una ventana propia con su resumen. No se ofrece para
  subintervalos.
- El **constructor de fórmulas** permite armar expresiones sin escribir código,
  con señales, variables (`masa`, `masa_total`, `estatura`, gravedad,
  frecuencia, tiempo, π, e), operadores y funciones (absoluto, raíz, integral,
  derivada, promedio, máximo, mínimo, RMS, suma, logaritmo, exponencial).
  Incluye plantillas de inicio rápido y permite reutilizar fórmulas existentes.
- Las fórmulas se exportan a `.txt` y se importan en otra copia de ABS. Al
  importar se validan, se omiten duplicados exactos y se renombran las
  coincidencias para no reemplazar fórmulas existentes.

### Proyectos

- *Guardar* conserva una copia del CSV, las anotaciones (intervalos,
  subintervalos y notas) y un archivo de estado con las variables del sujeto,
  gravedad, frecuencia, filtros, orden y visibilidad de las gráficas.
- *Abrir* reconstruye el análisis. Los proyectos de versiones anteriores, sin
  archivo de estado, se siguen abriendo.
- *Configurar → Archivos guardados* muestra cuántos proyectos hay, cuánto
  ocupan, y permite eliminarlos.
- *Inicio* cierra el análisis actual y vuelve al estado previo a cargar un CSV,
  sin borrar proyectos ni fórmulas guardadas.

### Exportación

- Datos de las señales, muestras de cada intervalo, resumen de fórmulas o el
  análisis completo en un único ZIP.
- Los resultados tienen una fila por fórmula e intervalo, con promedio, máximo
  y frame del máximo. Incluyen los cálculos de subintervalos aunque su ventana
  esté cerrada.
- Se puede exportar todo el registro o solo los intervalos marcados. La lista
  reúne todos los intervalos y subintervalos de todas las señales.

### Editor CSV

- Permite definir manualmente la fila de cabeceras y el fin de los datos,
  marcar varias secciones y asignar cada cabecera a una variable.
- Usa una tabla virtualizada que solo crea las celdas visibles. Con un archivo
  de 49 309 filas y 11 columnas, la construcción de la ventana pasó de unos
  23 s a 0,013 s en el entorno de desarrollo.

### Interfaz

- Se adapta al tamaño de la ventana y a la escala del sistema; en ventanas
  angostas reduce márgenes y reorganiza los paneles.
- La ventana principal y los diálogos recuerdan su tamaño y posición. Si cambió
  el monitor, la geometría se ajusta para que la ventana quede visible.
- *Acerca de* muestra versión, autoría, tutores, institución y la referencia
  sugerida en inglés y español, que se puede copiar al portapapeles.

---

## Dónde se guardan los datos

| Ubicación | Contenido | Duración |
|---|---|---|
| `libiam_config.db` (SQLite) | Alias de columnas aprendidos, secciones manuales, variables por archivo | Permanente |
| `archivos/` | Proyectos guardados: `<nombre>.csv`, `<nombre>_anotaciones.csv` y `<nombre>_estado.json` | Hasta que el usuario los elimina |
| Preferencias de Qt (`QSettings`) | Tamaño y posición de ventanas, colores, modo de zoom | Permanente, por usuario |
| Memoria | Opciones de superposición de la sesión | Se reinician al cerrar |

---

## Estructura del proyecto

```text
ABS-3.0/
├── main.py                    Punto de entrada: base de datos, estilos y ventana principal
├── requirements.txt           Dependencias con versiones fijas
├── libiam_config.db           Configuración local (SQLite)
├── archivos/                  Proyectos guardados
├── logica/                    Lógica de negocio, sin interfaz y probada
│   ├── app_info.py            Versión, autoría y referencia
│   ├── cargador_csv.py        Carga y detección de señales
│   ├── lector_csv.py          Lectura rápida y metadatos del CSV
│   ├── mapeo_columnas.py      Mapeo automático y manual
│   ├── config_db.py           Esquema y migraciones de SQLite
│   ├── filtros_senales.py     Filtro Butterworth
│   ├── formulas.py            Registro, validación y cálculo de fórmulas
│   ├── intercambio_formulas.py Importación y exportación de fórmulas
│   ├── rangos.py              Intervalos sin superposición
│   ├── intervalos.py          Utilidades de intervalos
│   ├── exportacion.py         Exportación de datos y resultados
│   ├── proyecto.py            Guardado y carga de proyectos
│   ├── paleta.py              Paletas estándar y accesibles
│   ├── accesibilidad.py       Opciones del modo accesible
│   ├── preferencias_graficas.py Colores y zoom
│   ├── interfaz_adaptativa.py Ajustes según tamaño y escala
│   └── idioma.py              Textos de la interfaz
├── ui/
│   ├── cabecera/              Barra superior, configuración y Acerca de
│   ├── ventanaPrincipal/      Panel izquierdo, área central de gráficas y panel derecho
│   └── ventanaRegion/         Ventana de detalle de intervalos y subintervalos
├── utilidades/
│   ├── estilos/               Hojas de estilo .qss
│   ├── icons/                 Iconos SVG
│   └── resources/             CSV de ejemplo
└── tests/                     Pruebas con unittest
```

La lógica (`logica/`) está separada de la interfaz (`ui/`). Los paneles se
comunican mediante señales de Qt conectadas en `VentanaPrincipal.init_ui()`.
Para agregar una fórmula incorporada basta con registrarla en `FORMULAS`
(`logica/formulas.py`): la interfaz la incorpora sin otros cambios.

Para publicar otra versión se modifica `VERSION` en `logica/app_info.py`; el
título de la ventana, la cabecera, el diálogo *Acerca de* y la referencia se
actualizan a partir de ese valor.

---

## Tecnologías

- **Python** como lenguaje.
- **PySide6** (Qt 6) para la interfaz de escritorio.
- **PyQtGraph** para las gráficas interactivas.
- **pandas** y **NumPy** para la lectura y el procesamiento de datos.
- **SciPy** para los filtros digitales.
- **SQLAlchemy** sobre **SQLite** para la configuración persistente.

---

## Autoría y cómo citar

**Autores:** Jairo Bandera, Alan Ceballos y Juan Macchi.
**Tutores:** Marcelo Scotto (UTEC) y Artur Bonezi (UdelaR).
**Institución:** Universidad Tecnológica (UTEC) — Tecnólogo en Informática, sede Paysandú.
**Laboratorio:** LIBiAM — DCB — Cenur Litoral Norte — UdelaR.

ABS 3.0 continúa el trabajo de las versiones anteriores de ABS, desarrolladas
en proyectos previos de la misma carrera.

**Referencia sugerida**

> Análisis Biomecánico de Señales (ABS) — versión 3.0 para Windows y macOS.
> Paysandú: Aplicación LIBiAM (UdelaR) y Universidad Tecnológica (UTEC) (2026).

> Analysis of Biomechanical Signal (ABS) — version 3.0 for Windows & macOS.
> Paysandú: Software LIBiAM (UdelaR) & Universidad Tecnológica (UTEC) (2026).
