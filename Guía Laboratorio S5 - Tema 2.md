# Guía de Laboratorio Sesión 5: Funciones y Diseño Modular

**Objetivo:** Desarrollar el núcleo de un sistema de monitoreo de sensores térmicos industriales, aplicando la declaración de funciones, el manejo del alcance de variables y los principios de encapsulamiento. Adicionalmente, se optimizará el control de versiones utilizando alias y se sincronizará el código mediante `push` y `pull`.

---

## Fase 0: Preparación de Entorno y Optimización de Git

**Meta:** Crear el repositorio y configurar atajos (alias) en Git para agilizar el trabajo en el terminal.

<details>
<summary><b>📘 Teoría: ¿Qué es un Alias en Git?</b></summary>

Un **Alias** es un atajo personalizado para comandos de Git. En ingeniería, donde la eficiencia es clave, los alias permiten ejecutar flujos complejos (como ver el historial gráfico) con solo 2 o 3 letras, reduciendo errores de escritura y fatiga.
</details>

<details>
<summary><b>⚡ Ejercicio Rápido: Alias de Historial</b></summary>

Configura un alias llamado `lgall` para ver el historial de forma compacta y colorida:
```
git config --global alias.lgall "log --oneline --graph --all"
# Pruébalo usando:
git lg
```
</details>

Configura alias para hacer los comandos más cortos y rápidos:
```
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.cm commit
```

## Fase 1: Funciones Básicas y Alcance Global 
Meta: Crear funciones con y sin retorno, e implementar una variable global que defina el límite de temperatura segura.

<details>
<summary><b>📘 Teoría: Funciones: ¿Por qué usarlas?</b></summary>

En ingeniería, una función actúa como una **"Caja Negra"**. Recibe una entrada, realiza un proceso y genera una salida.
- **Sin retorno:** Útiles para tareas repetitivas de interfaz o reportes (ej. mostrar un banner).
- **Con retorno:** Fundamentales para cálculos y lógica de decisión donde el programa principal necesita el resultado para continuar.
</details>

<details>
<summary><b>2. El Ámbito de las Variables (Scope)</b></summary>

- **Variables Globales:** Se definen fuera de cualquier función. Son útiles para configuraciones maestras (como límites de seguridad) que no deben cambiar pero sí ser consultadas por todo el sistema.
- **Variables Locales:** Viven solo dentro de su función. Esto evita errores de "contaminación" de datos entre diferentes partes del programa.
</details>


Paso 1: Escribir el Código Base
Crea tu archivo principal y desarrolla lo siguiente:

Opción A: Python
```
# Módulo de Monitoreo V1

# 1. Alcance Global: Variable accesible por cualquier función
LIMITE_ALERTA_GLOBAL = 85.0 

# 2. Función sin retorno (Procedimiento)
def mostrar_encabezado():
    print("========================================")
    print("    SISTEMA DE MONITOREO INDUSTRIAL     ")
    print("========================================")

# 3. Función con retorno
def validar_temperatura(temp_actual):
    # Uso de variable local (temp_actual) y global (LIMITE_ALERTA_GLOBAL)
    if temp_actual > LIMITE_ALERTA_GLOBAL:
        return True # Hay alerta
    return False # Todo normal

# Ejecución principal
mostrar_encabezado()
lectura_sensor = float(input("Ingrese la temperatura del motor (°C): "))

if validar_temperatura(lectura_sensor):
    print("¡PELIGRO! Temperatura excede el límite operativo.")
else:
    print("Estado del motor: Operativo y estable.")
```

Opción B: C#
```
using System;

class MonitoreoIndustrial
{
    // 1. Alcance Global (a nivel de clase)
    static double LimiteAlertaGlobal = 85.0;

    // 2. Función sin retorno (void)
    static void MostrarEncabezado()
    {
        Console.WriteLine("========================================");
        Console.WriteLine("    SISTEMA DE MONITOREO INDUSTRIAL     ");
        Console.WriteLine("========================================");
    }

    // 3. Función con retorno
    static bool ValidarTemperatura(double tempActual)
    {
        // Uso de variable local y global
        if (tempActual > LimiteAlertaGlobal)
        {
            return true; // Hay alerta
        }
        return false; // Todo normal
    }

    static void Main()
    {
        MostrarEncabezado();
        Console.Write("Ingrese la temperatura del motor (°C): ");
        double lecturaSensor = double.Parse(Console.ReadLine());

        if (ValidarTemperatura(lecturaSensor))
        {
            Console.WriteLine("¡PELIGRO! Temperatura excede el límite operativo.");
        }
        else
        {
            Console.WriteLine("Estado del motor: Operativo y estable.");
        }
    }
}
```
> [!IMPORTANT]  
> primera Versión de tu código

## Fase 2: Paso de Parámetros y Modificadores

Meta: Entender cómo los datos se envían a las funciones. Demostraremos el paso por valor (donde se envía una copia) y el paso por referencia (donde se modifica la variable original).


<details>
<summary><b>📘 Teoría: Funciones: ¿Cuál es la diferencia de paso por valor y por referencia?</b></summary>

**Paso por Valor (Copy):** Se crea una copia exacta del dato en una nueva ubicación de memoria. Si la función modifica el valor, la variable original permanece intacta. Es la opción más segura para evitar efectos secundarios no deseados.

**Paso por Referencia (Pointer/Ref):** Se envía la dirección de memoria de la variable original. La función no trabaja con una copia, sino con el dato real. Cualquier cambio se refleja en todo el sistema. Es eficiente para manejar grandes volúmenes de datos (como arreglos de sensores) sin duplicar memoria.

<b>⚡ Imagina</b>

<p style="padding-left: 30px;">
Imagina un sensor de presión con una lectura de 100 PSI. Ejecutas una función 
Calibrar(presion) que le suma +10.

Si el paso es por valor, después de la función, la presión sigue siendo 100.

Si el paso es por referencia, después de la función, la presión es 110.

Pregunta para la clase: Si estás programando el sistema de frenado de un tren, ¿usarías paso por valor o por referencia para actualizar la velocidad actual? ¿Por qué?
</p>
</details>

> [!IMPORTANT]  
> Paso 1: Crear una nueva rama usando el alias

Paso 2: Implementar la calibración de sensores

Opción A: Python
(En Python, los tipos primitivos se pasan por valor. Para simular el cambio, retornaremos el nuevo valor y reasignaremos, respetando el diseño modular).
```
# Añadir al código existente:

# Paso por valor (el parámetro 'ajuste' es una copia local)
def calibrar_sensor(temperatura, ajuste):
    temperatura_calibrada = temperatura + ajuste
    return temperatura_calibrada

# En el bloque principal, agregar:
print("\n--- Modo Calibración ---")
ajuste_tecnico = 2.5
lectura_sensor = calibrar_sensor(lectura_sensor, ajuste_tecnico)
print(f"Temperatura tras calibración de +{ajuste_tecnico}°C: {lectura_sensor}°C")
```
Opción B: C#
(C# permite un control estricto del paso por referencia usando la palabra clave ref).
```
// Añadir al bloque de métodos:

// Paso por referencia (modifica la variable original en memoria)
static void CalibrarSensor(ref double temperatura, double ajuste)
{
    temperatura += ajuste;
}

// En el método Main(), agregar al final:
Console.WriteLine("\n--- Modo Calibración ---");
double ajusteTecnico = 2.5;
// Usamos 'ref' para indicar que lecturaSensor cambiará
CalibrarSensor(ref lecturaSensor, ajusteTecnico); 
Console.WriteLine($"Temperatura tras calibración de +{ajusteTecnico}°C: {lecturaSensor}°C");
```

> [!IMPORTANT]  
> Paso 3: Guardar y Fusionar

##Fase 3: Sincronización de Equipos (Push y Pull)

Meta: En un entorno real, varios ingenieros trabajan en el mismo código. Sincronizaremos nuestro repositorio local con la nube.

Paso 1: Enviar al repositorio remoto (Push)
Sube tu código validado a GitHub:
```
git push -u origin master
````

Paso 2: Simular un cambio externo y descargar (Pull)

- Entra a tu repositorio en la página web de GitHub.

- Abre tu archivo de código y usa el lápiz de edición web para agregar un comentario al inicio: // Revisado por el Ing. Jefe de Planta. Haz el commit en la web.

- Vuelve a tu terminal local. Tu código local ahora está desactualizado. Para traer los cambios de la nube a tu computadora, ejecuta:
```
git pull origin master
```
- Abre tu archivo en tu editor local y verifica que el comentario ha aparecido.

> [!NOTE]  
> Reto Adicional de Ingeniería (Avanzado)
> Aplica el principio de diseño modular:
> Crea un módulo (o una clase independiente en C#) completamente separado de la ejecución principal. Este módulo debe contener una función que reciba un arreglo/lista de 5 lecturas de sensores, aplique la calibración a cada una mediante un bucle, e imprima el promedio descartando automáticamente cualquier lectura que arroje números negativos (error de hardware). Sube este reto en una rama llamada reto-modular.


