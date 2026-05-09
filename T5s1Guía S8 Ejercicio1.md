# 🗄️ Guía de Laboratorio: Operación "Archivo Maestro"

## 🎯 Misión del Agente de Datos

Has sido reclutado como **Agente de Persistencia** de la Agencia Nacional de Datos (AND). Tu misión: construir un sistema de registro de campo capaz de **guardar, recuperar y analizar información crítica** usando archivos. Los datos que no se persisten… se pierden para siempre.

> ⚠️ **Clasificación:** CIIN1205P — Tema 5 | Persistencia de datos con archivos  
> 🛠️ **Lenguajes:** C# / Python  
> 🧩 **Competencia:** Identificar diferencias entre archivos de texto y binarios, y aplicar operaciones básicas de E/S para guardar y recuperar datos de manera persistente.

---

## 🏅 Sistema de Insignias

Completa cada fase para desbloquear tu insignia de agente:

| Fase | Nombre | Insignia | XP |
|------|--------|----------|----|
| 1 | Cuartel General (Git) | 🔐 Agente Iniciado | +10 XP |
| 2 | Primer Contacto (Escritura) | 📡 Transmisor | +20 XP |
| 3 | Recuperación de Datos (Lectura) | 📥 Receptor | +20 XP |
| 4 | Archivo Estructurado (CSV) | 🗂️ Archivista | +30 XP |
| 5 | Análisis de Campo (Procesamiento) | 🔬 Analista | +30 XP |
| 6 | Sincronización (GitHub) | 🛰️ Satélite Activo | +10 XP |
| Reto | Agente Élite | 🏆 Maestro del Archivo | +50 XP |

**Total máximo: 170 XP** — ¿Puedes conseguirlos todos?

---

## Fase 1 🔐 — Cuartel General (Configuración Git)

Antes de cualquier misión, el agente asegura su entorno de trabajo.

```bash
mkdir ArchivoMaestro
cd ArchivoMaestro
git init
```

Crea tu archivo principal según tu lenguaje:
- C# → `agente.cs`
- Python → `agente.py`

Registra el inicio de operaciones:

```bash
git add .
git commit -m "Inicio de operación: Archivo Maestro"
```

> ✅ **+10 XP desbloqueados** — Insignia 🔐 **Agente Iniciado** obtenida.

---

## Fase 2 📡 — Primer Contacto (Escritura de archivos de texto)

El agente debe enviar su primer reporte al cuartel. Tu misión: **escribir un archivo de texto** con información de un agente de campo.

### ¿Por qué archivos de texto?
Los archivos de texto son legibles por humanos y otras herramientas (bloc de notas, Excel, Python, C#). Ideales para reportes, logs y datos de intercambio.

### Opción A: C#

```csharp
using System;
using System.IO;

class Agente {
    static void Main() {
        // Datos del agente de campo
        string nombre = "Ramírez";
        string zona = "Lima Norte";
        int casos = 7;

        // Escribir reporte en archivo de texto
        string ruta = "reporte.txt";
        using (StreamWriter sw = new StreamWriter(ruta, false)) {
            sw.WriteLine("=== REPORTE DE CAMPO ===");
            sw.WriteLine($"Agente   : {nombre}");
            sw.WriteLine($"Zona     : {zona}");
            sw.WriteLine($"Casos    : {casos}");
            sw.WriteLine($"Fecha    : {DateTime.Now}");
        }

        Console.WriteLine("✅ Reporte transmitido correctamente.");
        Console.WriteLine($"📁 Archivo creado: {ruta}");
    }
}
```

### Opción B: Python

```python
from datetime import datetime

# Datos del agente de campo
nombre = "Ramírez"
zona   = "Lima Norte"
casos  = 7

# Escribir reporte en archivo de texto
ruta = "reporte.txt"
with open(ruta, "w", encoding="utf-8") as f:
    f.write("=== REPORTE DE CAMPO ===\n")
    f.write(f"Agente   : {nombre}\n")
    f.write(f"Zona     : {zona}\n")
    f.write(f"Casos    : {casos}\n")
    f.write(f"Fecha    : {datetime.now()}\n")

print("✅ Reporte transmitido correctamente.")
print(f"📁 Archivo creado: {ruta}")
```

### 🔍 ¿Qué observar?

Abre el archivo `reporte.txt` con el bloc de notas o cualquier editor de texto. ¿Puedes leerlo directamente? Eso es la ventaja de los **archivos de texto**.

> 💡 **Nota técnica:** El parámetro `false` en C# (y `"w"` en Python) indica que el archivo se **sobrescribe** si ya existe. Veremos el modo *append* en la Fase 3.

> **Punto de Control Git:**
> ```bash
> git add .
> git commit -m "Fase 2: escritura de reporte de campo en archivo de texto"
> ```

> ✅ **+20 XP desbloqueados** — Insignia 📡 **Transmisor** obtenida.

---

## Fase 3 📥 — Recuperación de Datos (Lectura + Append)

El cuartel necesita leer los reportes anteriores y agregar nuevos eventos sin borrar el historial. Dos operaciones críticas: **leer** y **agregar** (append).

### 3.1 — Leer el reporte existente

#### Opción A: C#

```csharp
// Leer el archivo completo
string ruta = "reporte.txt";

if (File.Exists(ruta)) {
    string contenido = File.ReadAllText(ruta);
    Console.WriteLine("📥 Contenido del reporte:");
    Console.WriteLine(contenido);
} else {
    Console.WriteLine("⚠️  Archivo no encontrado.");
}
```

#### Opción B: Python

```python
ruta = "reporte.txt"

import os
if os.path.exists(ruta):
    with open(ruta, "r", encoding="utf-8") as f:
        contenido = f.read()
    print("📥 Contenido del reporte:")
    print(contenido)
else:
    print("⚠️  Archivo no encontrado.")
```

### 3.2 — Agregar un nuevo evento (Append)

El agente registra un nuevo evento sin borrar el reporte anterior:

#### Opción A: C#

```csharp
// Agregar evento al final del archivo (append)
using (StreamWriter sw = new StreamWriter("reporte.txt", true)) {
    sw.WriteLine($"[EVENTO] Nuevo contacto registrado: {DateTime.Now}");
}
Console.WriteLine("📝 Evento registrado en el historial.");
```

#### Opción B: Python

```python
# Agregar evento al final del archivo (modo "a" = append)
with open("reporte.txt", "a", encoding="utf-8") as f:
    f.write(f"[EVENTO] Nuevo contacto registrado: {datetime.now()}\n")

print("📝 Evento registrado en el historial.")
```

### 📊 Tabla de modos de apertura

| Modo | C# (StreamWriter) | Python | ¿Borra contenido? |
|------|-------------------|--------|-------------------|
| Escribir | `new StreamWriter(ruta, false)` | `"w"` | ✅ Sí |
| Agregar | `new StreamWriter(ruta, true)` | `"a"` | ❌ No |
| Solo leer | `File.ReadAllText()` | `"r"` | ❌ No |

> **Punto de Control Git:**
> ```bash
> git add .
> git commit -m "Fase 3: lectura y modo append para historial de eventos"
> ```

> ✅ **+20 XP desbloqueados** — Insignia 📥 **Receptor** obtenida.

---

## Fase 4 🗂️ — Archivo Estructurado (CSV de Agentes)

La agencia necesita manejar **múltiples agentes**. Para eso, usamos un archivo CSV: un formato de texto estructurado donde cada línea es un registro.

### El archivo `agentes.csv` tendrá esta estructura:

```
id,nombre,zona,casos
1,Ramírez,Lima Norte,7
2,Torres,Lima Sur,12
3,Vega,Callao,5
```

### 4.1 — Crear y escribir el CSV

#### Opción A: C#

```csharp
using System.IO;

string ruta = "agentes.csv";

// Escribir encabezado y registros
using (StreamWriter sw = new StreamWriter(ruta, false)) {
    sw.WriteLine("id,nombre,zona,casos");
    sw.WriteLine("1,Ramírez,Lima Norte,7");
    sw.WriteLine("2,Torres,Lima Sur,12");
    sw.WriteLine("3,Vega,Callao,5");
}

Console.WriteLine("🗂️  Base de datos de agentes creada: agentes.csv");
```

#### Opción B: Python

```python
import csv

ruta = "agentes.csv"
agentes = [
    ["id", "nombre", "zona", "casos"],
    [1, "Ramírez", "Lima Norte", 7],
    [2, "Torres",  "Lima Sur",   12],
    [3, "Vega",    "Callao",     5],
]

with open(ruta, "w", newline="", encoding="utf-8") as f:
    escritor = csv.writer(f)
    escritor.writerows(agentes)

print("🗂️  Base de datos de agentes creada: agentes.csv")
```

### 4.2 — Leer y mostrar el CSV

#### Opción A: C#

```csharp
string[] lineas = File.ReadAllLines("agentes.csv");
Console.WriteLine("\n📋 LISTA DE AGENTES ACTIVOS:");
Console.WriteLine("─────────────────────────────────");

for (int i = 1; i < lineas.Length; i++) {   // i=1 para saltar encabezado
    string[] campos = lineas[i].Split(',');
    Console.WriteLine($"  ID: {campos[0]} | Agente: {campos[1],-10} | Zona: {campos[2],-12} | Casos: {campos[3]}");
}
```

#### Opción B: Python

```python
import csv

print("\n📋 LISTA DE AGENTES ACTIVOS:")
print("─" * 50)

with open("agentes.csv", encoding="utf-8") as f:
    lector = csv.reader(f)
    next(lector)  # saltar encabezado
    for fila in lector:
        print(f"  ID: {fila[0]} | Agente: {fila[1]:<10} | Zona: {fila[2]:<12} | Casos: {fila[3]}")
```

> **Punto de Control Git:**
> ```bash
> git add .
> git commit -m "Fase 4: creación y lectura de base de datos CSV de agentes"
> ```

> ✅ **+30 XP desbloqueados** — Insignia 🗂️ **Archivista** obtenida.

---

## Fase 5 🔬 — Análisis de Campo (Procesamiento de datos)

El Cuartel General necesita un **reporte de inteligencia**: total de casos, agente más activo y promedio de casos por zona. Leeremos el CSV y procesaremos los datos.

### Opción A: C#

```csharp
using System;
using System.IO;

string[] lineas = File.ReadAllLines("agentes.csv");

int totalCasos   = 0;
int maxCasos     = 0;
string mejorAgente = "";

for (int i = 1; i < lineas.Length; i++) {
    string[] campos = lineas[i].Split(',');
    string nombre   = campos[1];
    int casos       = int.Parse(campos[3]);

    totalCasos += casos;

    if (casos > maxCasos) {
        maxCasos    = casos;
        mejorAgente = nombre;
    }
}

int numAgentes = lineas.Length - 1;   // descontar encabezado
double promedio = (double)totalCasos / numAgentes;

// Guardar reporte de inteligencia
string reporte = $"""
=== REPORTE DE INTELIGENCIA ===
Total de agentes : {numAgentes}
Total de casos   : {totalCasos}
Promedio         : {promedio:F2}
Agente destacado : {mejorAgente} ({maxCasos} casos)
Generado         : {DateTime.Now}
""";

File.WriteAllText("inteligencia.txt", reporte);
Console.WriteLine(reporte);
Console.WriteLine("📄 Reporte guardado en: inteligencia.txt");
```

### Opción B: Python

```python
import csv
from datetime import datetime

total_casos   = 0
max_casos     = 0
mejor_agente  = ""

with open("agentes.csv", encoding="utf-8") as f:
    lector = csv.reader(f)
    next(lector)  # saltar encabezado
    filas = list(lector)

for fila in filas:
    nombre = fila[1]
    casos  = int(fila[3])
    total_casos += casos
    if casos > max_casos:
        max_casos    = casos
        mejor_agente = nombre

num_agentes = len(filas)
promedio    = total_casos / num_agentes

reporte = f"""=== REPORTE DE INTELIGENCIA ===
Total de agentes : {num_agentes}
Total de casos   : {total_casos}
Promedio         : {promedio:.2f}
Agente destacado : {mejor_agente} ({max_casos} casos)
Generado         : {datetime.now()}
"""

with open("inteligencia.txt", "w", encoding="utf-8") as f:
    f.write(reporte)

print(reporte)
print("📄 Reporte guardado en: inteligencia.txt")
```

### 🧠 Conexión con temas anteriores

| Concepto usado | Tema del sílabo |
|----------------|-----------------|
| Arreglos / listas para almacenar filas | Tema 3 |
| Funciones modulares de lectura | Tema 2 |
| Bucle `for` para recorrer registros | Tema 1 |
| Escritura de resultado en archivo | Tema 5 ← ¡Hoy! |

> **Punto de Control Git:**
> ```bash
> git add .
> git commit -m "Fase 5: análisis de datos y generación de reporte de inteligencia"
> ```

> ✅ **+30 XP desbloqueados** — Insignia 🔬 **Analista** obtenida.

---

## Fase 6 🛰️ — Sincronización con el Satélite (GitHub)

Toda misión se documenta. Sube tu trabajo al repositorio remoto para que el cuartel central tenga acceso.

```bash
git remote add origin https://github.com/TU_USUARIO/ArchivoMaestro.git
git push -u origin main
```

Verifica en tu perfil de GitHub que los archivos `agente.cs` / `agente.py`, `reporte.txt`, `agentes.csv` e `inteligencia.txt` estén visibles.

> ✅ **+10 XP desbloqueados** — Insignia 🛰️ **Satélite Activo** obtenida.

---

## 🏆 Reto Nivel Élite — Agente de Campo Autónomo

**Situación:** El sistema actual tiene los agentes escritos a mano en el código. Eso no es profesional para la Agencia.

### Misión élite (3 partes):

**Parte 1 — Registro interactivo:**  
Crea una función `registrar_agente()` que pida por consola el nombre, zona y número de casos, y lo **agregue como una nueva fila al CSV** (modo append) sin borrar los registros anteriores.

**Parte 2 — Búsqueda por nombre:**  
Crea una función `buscar_agente(nombre)` que lea el CSV y muestre los datos del agente buscado. Si no existe, muestra un mensaje de error.

**Parte 3 — Menú de opciones:**  
Encierra todo en un menú de consola con bucle:

```
=== AGENCIA NACIONAL DE DATOS ===
1. Ver todos los agentes
2. Registrar nuevo agente
3. Buscar agente por nombre
4. Generar reporte de inteligencia
5. Salir
```

> **Haz un commit por cada parte completada:**
> ```bash
> git commit -m "Reto Élite Parte 1: registro interactivo de agentes"
> git commit -m "Reto Élite Parte 2: búsqueda de agente por nombre"
> git commit -m "Reto Élite Parte 3: menú principal de la agencia"
> ```

> 🏆 **+50 XP desbloqueados** — Insignia 🏆 **Maestro del Archivo** obtenida.

---

## 📋 Resumen de Conceptos Clave

| Concepto | Descripción | Ejemplo |
|----------|-------------|---------|
| Archivo de texto | Legible, codificado UTF-8 | `.txt`, `.csv`, `.json` |
| Archivo binario | Bytes crudos, más compacto | `.dat`, `.bin`, `.png` |
| Modo escritura `w` | Crea o sobrescribe | Nuevo reporte |
| Modo append `a` | Agrega al final | Log de eventos |
| Modo lectura `r` | Solo lee, no modifica | Ver historial |
| `using` / `with` | Cierre automático del archivo | Buena práctica |
| CSV | Texto con campos separados por coma | Base de datos ligera |
| File pointer | Cursor de posición dentro del archivo | Avanza al leer/escribir |

---

## 🗂️ Archivos del Proyecto al Finalizar

```
ArchivoMaestro/
├── agente.cs  o  agente.py     ← Código fuente principal
├── reporte.txt                 ← Fase 2 y 3
├── agentes.csv                 ← Fase 4
└── inteligencia.txt            ← Fase 5
```

---
