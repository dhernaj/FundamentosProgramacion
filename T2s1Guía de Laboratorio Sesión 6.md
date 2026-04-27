
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

## Fase 3: Diseño Modular

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
double LIMITE_GLOBAL = 80;

static double Ingresar()
{
    Console.Write("Ingrese temperatura: ");
    double temperatura = double.Parse(Console.ReadLine());
    return temperatura;
}

static bool Validar(double temp)
{
    if (temp < 0)
    {
    Console.WriteLine("Temperatura inválida");
    return false;
    }
    else return true;
}

string Evaluar(double temp)
{
    if (temp > LIMITE_GLOBAL) return "ALERTA";
    else return "NORMAL";
}

static void Mostrar(string resultado)
{
    Console.WriteLine("Estado: " + resultado);
}

    double temp = Ingresar();
    if (Validar(temp))
    {
        string estado = Evaluar(temp);
        Mostrar(estado);
    }
```

Guardar:

```
git add .
git commit -m "v3: Sistema modular completo"
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
git commit -m "v4: Mejora de alcance"
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

Aquí tienes **4 problemas adicionales de nivel medio**, con **contextos distintos** al industrial y manteniendo coherencia con tu enfoque (funciones, alcance, sin clases ni arrays):

---

## Ejercicios de Nivel Medio

---

### 🧩 Ejercicio 5: Control de Consumo Eléctrico Doméstico

**Enunciado:**
Un hogar desea monitorear su consumo eléctrico diario para evitar sobrecargas. Se requiere un programa que evalúe el consumo ingresado por el usuario y determine si se encuentra dentro de un rango seguro.

**Requerimientos:**

* Definir un límite máximo de consumo como variable global
* Crear una función para ingresar el consumo diario
* Validar que el valor ingresado no sea negativo
* Crear una función que determine si el consumo es **Normal** o **Excesivo**
* Mostrar el resultado final al usuario
* Usar funciones para estructurar el programa

---

### 🧩 Ejercicio 6: Evaluación de Calificaciones Académicas

**Enunciado:**
Un docente necesita evaluar el rendimiento de un estudiante a partir de tres notas ingresadas manualmente.

**Requerimientos:**

* Solicitar 3 notas 
* Validar que cada nota esté entre 0 y 20
* Calcular el promedio usando variables simples
* Crear una función que determine si el estudiante está:

  * **Aprobado** (≥ 12)
  * **Desaprobado** (< 12)
* Evitar uso innecesario de variables globales
* Mostrar el resultado final

---

### 🧩 Ejercicio 7: Sistema de Control de Acceso

**Enunciado:**
Una empresa desea controlar el acceso de personas a una instalación restringida mediante un código de verificación.

**Requerimientos:**

* No usar variable global
* Crear una función que solicite el código al usuario
* Crear una función que valide si el código es correcto
* Mostrar:

  * **Acceso permitido**
  * **Acceso denegado**
* Implementar una función adicional que registre (imprima) el intento

---

### 🧩 Ejercicio 8: Monitoreo de Nivel de Agua en Tanque

**Enunciado:**
Un sistema básico debe controlar el nivel de agua en un tanque para evitar desbordes o vacíos críticos.

**Requerimientos:**

* Definir niveles mínimo y máximo (no usar variables globales)
* Crear una función para ingresar el nivel actual
* Validar que el nivel no sea negativo
* Crear una función que determine el estado:

  * **Bajo** (por debajo del mínimo)
  * **Normal**
  * **Alto** (por encima del máximo)
* Mostrar el estado del sistema
* Organizar el código de forma modular

---
