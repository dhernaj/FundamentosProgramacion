# Guía de Laboratorio Sesión 6: Ejercicio 1

**Objetivo:** Implementar un sistema de control de sensores térmicos aplicando el alcance de variables (local y global) y principios de diseño modular. Se simulará el encapsulamiento mediante validación controlada.

---

## Fase 0: Preparación de Entorno y Control de Versiones

**Meta:** Inicializar un repositorio y registrar versiones del sistema a medida que evoluciona.

Ejecuta en tu terminal:

```
git init
git add .
git commit -m "v1: Estructura inicial del sistema de sensores"
```

---

## Fase 1: Alcance Global y Función Básica

**Meta:** Implementar una variable global y una función que evalúe el estado de temperatura.

👉 Buena práctica: usar variables globales solo para configuración, no modificarlas directamente.

---

### Paso 1: Código Base

Opción A: Python

```
# Sistema de Monitoreo V1

# Variable global
LIMITE_TEMPERATURA = 50

def mostrar_estado(temp):
    if temp > LIMITE_TEMPERATURA:
        print("ALERTA: Temperatura alta")
    else:
        print("Estado normal")

# Programa principal
temperatura = float(input("Ingrese temperatura: "))
mostrar_estado(temperatura)
```

---

Opción B: C#

```
using System;

class Programa
{
    static int LIMITE_TEMPERATURA = 50;

    static void MostrarEstado(double temp)
    {
        if (temp > LIMITE_TEMPERATURA)
            Console.WriteLine("ALERTA: Temperatura alta");
        else
            Console.WriteLine("Estado normal");
    }

    static void Main()
    {
        Console.Write("Ingrese temperatura: ");
        double temperatura = double.Parse(Console.ReadLine());

        MostrarEstado(temperatura);
    }
}
```

> [!IMPORTANT]
> Primera versión funcional del sistema

Guardar versión:

```
git add .
git commit -m "v2: Evaluación básica de temperatura con variable global"
```

---

## Fase 2: Validación de Datos (Encapsulamiento Simulado)

**Meta:** Validar datos antes de procesarlos.

---

### Paso 2: Implementar validación

Opción A: Python

```
LIMITE_TEMPERATURA = 50

def validar_temperatura(temp):
    if temp < 0:
        print("Error: temperatura inválida")
        return False
    return True

def mostrar_estado(temp):
    if temp > LIMITE_TEMPERATURA:
        print("ALERTA")
    else:
        print("NORMAL")

temperatura = float(input("Ingrese temperatura: "))

if validar_temperatura(temperatura):
    mostrar_estado(temperatura)
```

---

Opción B: C#

```
static bool ValidarTemperatura(double temp)
{
    if (temp < 0)
    {
        Console.WriteLine("Error: temperatura inválida");
        return false;
    }
    return true;
}

static void MostrarEstado(double temp)
{
    if (temp > LIMITE_TEMPERATURA)
        Console.WriteLine("ALERTA");
    else
        Console.WriteLine("NORMAL");
}
```

Guardar versión:

```
git add .
git commit -m "v3: Validación de temperatura implementada"
```

---

## Fase 3: Diseño Modular del Sistema

**Meta:** Separar responsabilidades en funciones independientes.

<details>
<summary><b>📘 Teoría: Diseño Modular</b></summary>

El diseño modular divide el sistema en partes pequeñas:

* Entrada de datos
* Validación
* Procesamiento
* Salida

✔ Mejora la claridad
✔ Facilita mantenimiento
✔ Permite reutilización

</details>

---

### Paso 3: Modularización completa

Opción A: Python

```
LIMITE_TEMPERATURA = 50

def ingresar_temperatura():
    return float(input("Ingrese temperatura: "))

def validar_temperatura(temp):
    if temp < 0:
        print("Error")
        return False
    return True

def evaluar_temperatura(temp):
    if temp > LIMITE_TEMPERATURA:
        return "ALERTA"
    return "NORMAL"

def mostrar_resultado(estado):
    print("Estado:", estado)

# Flujo principal
temp = ingresar_temperatura()

if validar_temperatura(temp):
    estado = evaluar_temperatura(temp)
    mostrar_resultado(estado)
```

---

Opción B: C#

```
static double IngresarTemperatura()
{
    Console.Write("Ingrese temperatura: ");
    return double.Parse(Console.ReadLine());
}

static bool ValidarTemperatura(double temp)
{
    if (temp < 0)
    {
        Console.WriteLine("Error");
        return false;
    }
    return true;
}

static string EvaluarTemperatura(double temp)
{
    if (temp > LIMITE_TEMPERATURA)
        return "ALERTA";

    return "NORMAL";
}

static void MostrarResultado(string estado)
{
    Console.WriteLine("Estado: " + estado);
}
```

Guardar versión:

```
git add .
git commit -m "v4: Sistema modular implementado"
```

---

## Fase 4: Mejora del Alcance (Buenas Prácticas)

**Meta:** Reducir dependencia de variables globales.

<details>
<summary><b>📘 Teoría: Buen uso del alcance</b></summary>

Problema:

* Funciones dependientes de variables globales → difícil mantenimiento

Solución:

* Pasar valores como parámetros

✔ Mejora reutilización
✔ Reduce acoplamiento
✔ Hace el sistema más flexible

</details>

---

### Paso 4: Refactorización

Opción A: Python

```
def evaluar_temperatura(temp, limite):
    if temp > limite:
        return "ALERTA"
    return "NORMAL"
```

---

Opción B: C#

```
static string EvaluarTemperatura(double temp, int limite)
{
    if (temp > limite)
        return "ALERTA";

    return "NORMAL";
}
```

Guardar versión final:

```
git add .
git commit -m "v5: Mejora eliminando dependencia global"
```

---

## Fase 5: Sincronización (Opcional)

**Meta:** Subir el proyecto a repositorio remoto.

```
git push -u origin master
```

---

## 🧠 Reflexión Final

Responde:

* ¿Por qué es importante validar datos antes de procesarlos?
* ¿Qué problema genera depender de variables globales?
* ¿Qué ventaja tiene dividir el sistema en funciones?

---

> [!NOTE]
> Reto Adicional (Nivel Medio)
>
> Extiende el sistema para:
>
> * Pedir 3 temperaturas (una por una, sin arrays)
> * Validarlas individualmente
> * Mostrar cuántas están en estado de alerta
>
> Guarda tu solución en una rama llamada:
>
> ```
> git checkout -b reto-sensores
> ```
