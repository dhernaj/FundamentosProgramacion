# ⚓ Guía de Laboratorio: Operación "Batalla Naval"

## 🎯 Objetivo de la Práctica
Diseñar e implementar un simulador interactivo del juego "Batalla Naval" en consola. Esta práctica consolida el manejo de **arreglos bidimensionales (matrices)**, estructuras de control y modularidad empleando los lenguajes de programación del curso, además de gestionar el control de versiones con **Git y GitHub**[cite: 1].

---

## 🛠️ Fase 1: Configuración del Cuartel General (Git)

Antes de escribir código, debemos asegurar nuestro entorno de trabajo y el control de versiones.

1. Abre tu terminal y crea la carpeta del proyecto:
   ```bash
   mkdir BatallaNaval
   cd BatallaNaval
   ```
2. Inicializa el repositorio local:
   ```bash
   git init
   ```
3. Crea un archivo llamado `Juego.cs` (C#) o `juego.py` (Python).
4. Realiza tu primer *commit* de configuración:
   ```bash
   git add .
   git commit -m "feat: Inicialización del proyecto Batalla Naval"
   ```

---

## 🗺️ Fase 2: El Radar (Recorrido de Matrices)

Crearemos una función modular que se encargará de traducir los números de nuestra matriz a símbolos visuales para que el jugador pueda ver el estado del tablero.
*   `~` : Agua (0)
*   `B` : Barco (1)
*   `X` : Impacto (8)
*   `O` : Fallo (9)

### Opción A: C#
```csharp
using System;

class Program {
    // Función para dibujar el mapa
    static void DibujarMapa(int[,] mapa) {
        Console.WriteLine("\n  0 1 2 3 4"); 
        for (int i = 0; i < 5; i++) {
            Console.Write(i + " "); 
            for (int j = 0; j < 5; j++) {
                char simbolo;
                switch (mapa[i, j]) {
                    case 1: simbolo = 'B'; break; 
                    case 8: simbolo = 'X'; break; 
                    case 9: simbolo = 'O'; break; 
                    default: simbolo = '~'; break; 
                }
                Console.Write(simbolo + " ");
            }
            Console.WriteLine();
        }
    }
```

### Opción B: Python
```python
# Función para dibujar el mapa
def dibujar_mapa(mapa):
    print("\n  0 1 2 3 4")  
    for i in range(len(mapa)):
        print(f"{i} ", end="") 
        for j in range(len(mapa[i])):
            valor = mapa[i][j]
            if valor == 1:
                simbolo = "B"
            elif valor == 8:
                simbolo = "X"
            elif valor == 9:
                simbolo = "O"
            else:
                simbolo = "~"
            print(f"{simbolo} ", end="")
        print() 
```

> **🛡️ Punto de Control Git:** 
> `git add .`
> `git commit -m "feat: Función modular para visualizar el radar del océano"`

---

## 🌊 Fase 3: Despliegue de la Flota (Declaración e Inserción)

Dentro de la función principal de nuestro programa, declararemos una cuadrícula de 5x5 llena de ceros (agua) y posicionaremos estratégicamente nuestros barcos (unos).

### Opción A: C#
```csharp
    static void Main() {
        // 1. Declarar la matriz de 5x5
        int[,] oceano = new int[5, 5];

        // 2. Posicionar 3 barcos manualmente
        oceano[1, 2] = 1; 
        oceano[3, 4] = 1;
        oceano[4, 0] = 1;

        Console.WriteLine("¡Océano generado y flota desplegada!");
        DibujarMapa(oceano); // Mostramos el radar inicial
```

### Opción B: Python
```python
# 1. Declarar la matriz de 5x5 llena de ceros
oceano = [
    [0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0]
]

# 2. Posicionar 3 barcos manualmente
oceano[1][2] = 1
oceano[3][4] = 1
oceano[4][0] = 1

print("¡Océano generado y flota desplegada!")
dibujar_mapa(oceano) # Mostramos el radar inicial
```

> **🛡️ Punto de Control Git:** 
> `git add .`
> `git commit -m "feat: Creación de matriz 5x5 y posicionamiento de barcos"`

---

## 💥 Fase 4: ¡Fuego a Discreción! (Búsqueda y Modificación)

Pediremos al jugador que ingrese coordenadas para atacar. Evaluaremos esa posición en la matriz para determinar si fue un impacto o un fallo al agua.

### Opción A: C#
```csharp
        // 3. Sistema de ataque
        Console.Write("\nIngresa la Fila para atacar (0-4): ");
        int fila = Convert.ToInt32(Console.ReadLine());

        Console.Write("Ingresa la Columna para atacar (0-4): ");
        int columna = Convert.ToInt32(Console.ReadLine());

        // Búsqueda y Modificación
        if (oceano[fila, columna] == 1) {
            Console.WriteLine("¡IMPACTO CRÍTICO! Hundiste un barco.");
            oceano[fila, columna] = 8; 
        } else {
            Console.WriteLine("Fallaste. Solo le diste al agua.");
            oceano[fila, columna] = 9; 
        }

        // Mostramos el resultado del ataque
        DibujarMapa(oceano);
    }
}
```

### Opción B: Python
```python
# 3. Sistema de ataque
fila = int(input("\nIngresa la Fila para atacar (0-4): "))
columna = int(input("Ingresa la Columna para atacar (0-4): "))

# Búsqueda y Modificación
if oceano[fila][columna] == 1:
    print("¡IMPACTO CRÍTICO! Hundiste un barco.")
    oceano[fila][columna] = 8 
else:
    print("Fallaste. Solo le diste al agua.")
    oceano[fila][columna] = 9 

# Mostramos el resultado del ataque
dibujar_mapa(oceano)
```

> **🛡️ Punto de Control Git:** 
> `git add .`
> `git commit -m "feat: Lógica de ataque y actualización dinámica de matriz"`

---

## 📡 Fase 5: Reporte al Satélite (Sincronización)

Es momento de subir el código al repositorio remoto para entrega y evaluación.

1. Vincula tu repositorio local con el remoto en GitHub:
   
```bash
   git remote add origin https://github.com/TU_USUARIO/BatallaNaval.git
   ```
2. Envía tu código a la rama principal:
   ```bash
   git push -u origin main
   ```

---

## 🧠 Reto Nivel Ingeniero (Actividad Autónoma)
Actualmente, el radar muestra dónde están escondidos los barcos (`B`). Modifica la función `DibujarMapa` / `dibujar_mapa` para que los barcos permanezcan ocultos (mostrándose como agua `~`) hasta que sean impactados y se conviertan en una `X`. Adicionalmente, encierra el bloque de ataque en un bucle (condicional repetitiva) para permitir múltiples disparos hasta hundir la flota completa.
```
