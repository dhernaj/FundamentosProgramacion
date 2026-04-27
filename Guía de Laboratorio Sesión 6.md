
# Guía de Laboratorio Sesión 6: Alcance de Variables y Encapsulamiento

**Objetivo:** Desarrollar un sistema de monitoreo de sensores térmicos aplicando el alcance de variables (local, global y no local), principios de diseño modular y encapsulamiento mediante funciones. Adicionalmente, se gestionará el control de versiones utilizando Git para registrar la evolución del sistema.

---

## Fase 0: Preparación de Entorno y Optimización de Git

**Meta:** Crear el repositorio y configurar atajos (alias) en Git para agilizar el trabajo en el terminal.

<details>
<summary><b>📘 Teoría: ¿Qué es un Alias en Git?</b></summary>

Un **Alias** es un atajo personalizado para comandos de Git. En ingeniería, donde la eficiencia es clave, los alias permiten ejecutar comandos largos con pocas letras, reduciendo errores y mejorando la productividad.

</details>

<details>
<summary><b>⚡ Ejercicio Rápido: Alias de Historial</b></summary>

Configura un alias llamado `lgall`:

```
git config --global alias.lgall "log --oneline --graph --all"
git lg
```

</details>

Configura alias adicionales:

```
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.cm commit
```

---

## Fase 1: Funciones y Alcance de Variables

**Meta:** Implementar funciones con variables globales y locales.

<details>
<summary><b>📘 Teoría: Alcance de Variables</b></summary>

* **Global:** Se define fuera de funciones, accesible en todo el programa.
* **Local:** Solo existe dentro de la función.
* **No Local (Python):** Permite modificar variables de una función externa.

👉 Buen diseño: usar globales solo como configuración.

</details>

---

### Paso 1: Código Base

Opción A: Python

```
# Sistema de Monitoreo V1

LIMITE_GLOBAL = 80

def mostrar_encabezado():
    print("==== SISTEMA INDUSTRIAL ====")

def evaluar_temperatura(temp):
    if temp > LIMITE_GLOBAL:
        return True
    return False

mostrar_encabezado()
temp = float(input("Ingrese temperatura: "))

if evaluar_temperatura(temp):
    print("ALERTA")
else:
    print("NORMAL")
```

---

Opción B: C#

```
using System;

class Programa
{
    static int LIMITE_GLOBAL = 80;

    static void MostrarEncabezado()
    {
        Console.WriteLine("==== SISTEMA INDUSTRIAL ====");
    }

    static bool EvaluarTemperatura(double temp)
    {
        if (temp > LIMITE_GLOBAL)
            return true;
        return false;
    }

    static void Main()
    {
        MostrarEncabezado();
        Console.Write("Ingrese temperatura: ");
        double temp = double.Parse(Console.ReadLine());

        if (EvaluarTemperatura(temp))
            Console.WriteLine("ALERTA");
        else
            Console.WriteLine("NORMAL");
    }
}
```

> [!IMPORTANT]
> Primera versión del sistema

Guardar:

```
git add .
git commit -m "v1: Sistema base con variable global"
```

---

## Fase 2: Encapsulamiento mediante Funciones

**Meta:** Validar datos antes de procesarlos.

<details>
<summary><b>📘 Teoría: Encapsulamiento sin clases</b></summary>

El encapsulamiento se puede lograr:

* Controlando acceso mediante funciones
* Validando datos antes de usarlos
* Evitando valores inválidos

👉 Protege la lógica del sistema.

</details>

---

### Paso 2: Validación

Opción A: Python

```
LIMITE_GLOBAL = 80

def validar_temperatura(temp):
    if temp < 0:
        print("Error: valor inválido")
        return False
    return True

def evaluar_temperatura(temp):
    if temp > LIMITE_GLOBAL:
        return "ALERTA"
    return "NORMAL"

temp = float(input("Ingrese temperatura: "))

if validar_temperatura(temp):
    estado = evaluar_temperatura(temp)
    print("Estado:", estado)
```

---

Opción B: C#

```
static bool ValidarTemperatura(double temp)
{
    if (temp < 0)
    {
        Console.WriteLine("Error: valor inválido");
        return false;
    }
    return true;
}

static string EvaluarTemperatura(double temp)
{
    if (temp > LIMITE_GLOBAL)
        return "ALERTA";
    return "NORMAL";
}
```

Guardar:

```
git add .
git commit -m "v2: Validación implementada"
```

---

## Fase 3: Variable No Local (Python)

**Meta:** Entender el uso de `nonlocal`.

<details>
<summary><b>📘 Teoría: Variable No Local</b></summary>

Se usa en funciones anidadas para modificar variables externas.

👉 No afecta variables globales, solo del contexto inmediato.

</details>

---

### Paso 3: Ejemplo

Python

```
def sistema():
    estado = "apagado"

    def encender():
        nonlocal estado
        estado = "encendido"

    encender()
    print("Estado:", estado)

sistema()
```

---

Guardar:

```
git add .
git commit -m "v3: Uso de nonlocal en Python"
```

---

## Fase 4: Diseño Modular

**Meta:** Dividir el sistema en funciones independientes.

<details>
<summary><b>📘 Teoría: Modularidad</b></summary>

Un sistema modular:

* Divide responsabilidades
* Facilita mantenimiento
* Mejora claridad

👉 Cada función cumple una sola tarea.

</details>

---

### Paso 4: Modularización completa

Opción A: Python

```
LIMITE_GLOBAL = 80

def ingresar():
    return float(input("Ingrese temperatura: "))

def validar(temp):
    if temp < 0:
        return False
    return True

def evaluar(temp):
    if temp > LIMITE_GLOBAL:
        return "ALERTA"
    return "NORMAL"

def mostrar(resultado):
    print("Estado:", resultado)

temp = ingresar()

if validar(temp):
    resultado = evaluar(temp)
    mostrar(resultado)
```

---

Opción B: C#

```
static double Ingresar()
{
    Console.Write("Ingrese temperatura: ");
    return double.Parse(Console.ReadLine());
}

static bool Validar(double temp)
{
    if (temp < 0)
        return false;
    return true;
}

static string Evaluar(double temp)
{
    if (temp > LIMITE_GLOBAL)
        return "ALERTA";
    return "NORMAL";
}

static void Mostrar(string resultado)
{
    Console.WriteLine("Estado: " + resultado);
}
```

Guardar:

```
git add .
git commit -m "v4: Sistema modular completo"
```

---

## Fase 5: Mejora del Alcance

**Meta:** Evitar dependencia de variables globales.

<details>
<summary><b>📘 Teoría: Buenas prácticas</b></summary>

Problema:

* Dependencia global

Solución:

* Pasar parámetros

✔ Mejora reutilización
✔ Reduce errores

</details>

---

### Paso 5: Mejora

Python

```
def evaluar(temp, limite):
    if temp > limite:
        return "ALERTA"
    return "NORMAL"
```

---

C#

```
static string Evaluar(double temp, int limite)
{
    if (temp > limite)
        return "ALERTA";
    return "NORMAL";
}
```

Guardar:

```
git add .
git commit -m "v5: Mejora de alcance"
```

---

## Fase 6: Sincronización (Push y Pull)

**Meta:** Trabajar con repositorio remoto.

```
git push -u origin master
```

Simular cambio en GitHub y luego:

```
git pull origin master
```

---

> [!NOTE]
> Reto Adicional de Ingeniería (Nivel Medio)
>
> Diseña un sistema que:
>
> * Solicite 3 temperaturas (una por una, sin arrays)
> * Valide cada una
> * Cuente cuántas están en alerta
> * Use funciones modulares
>
> Crear rama:
>
> ```
> git checkout -b reto-sensores
> ```
