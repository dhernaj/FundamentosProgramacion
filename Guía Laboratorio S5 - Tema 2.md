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
