# Ejercicios con Ant y Gradle

Este ejercicio te guiará en la creación de un **repositorio en GitHub**, donde implementarás dos programas:
- **Un script en Python ejecutado con Apache Ant** para generar **códigos de recuperación**.
- **Un programa en Kotlin con Gradle** para generar **códigos de recuperación**.

Aprenderás cómo **automatizar la ejecución y construcción** de proyectos usando estas herramientas.

---

## 1️⃣ Crear el Repositorio en GitHub y Abrirlo en Codespaces

1. **Crea un nuevo repositorio en GitHub** (vacío, solo con este `README.md`).
2. **Abre el repositorio en Codespaces**:
   - Ve a **"Code" → "Open with Codespaces" → "New Codespace"**.
3. **Abre la terminal en Codespaces** (`Ctrl + Shift + \`` para abrir una nueva pestaña).

---

## 2️⃣ Ejercicio en Python con Apache Ant 🐍

**Objetivo:** Crear un script que genere **códigos de recuperación** aleatorios de **6 caracteres alfanuméricos**. Luego, **modificar un `build.xml` básico** para ejecutar el script con **Ant**.

### Paso 1: Crear el Archivo `main.py`

📌 **En el directorio raíz del repositorio, crea el archivo `main.py` con el siguiente código:**  

```python
import sys
import random
import string

# TODO: Implementar la función para generar códigos de recuperación
def generar_codigo():
    """
    Debe generar un código de 6 caracteres alfanuméricos sin caracteres ambiguos.
    Caracteres ambiguos a evitar: '0', 'O', '1', 'l'
    """
    pass  # TODO: Implementar la generación del código

if __name__ == "__main__":
    if len(sys.argv) != 2 or not sys.argv[1].isdigit():
        print("Uso: python main.py <cantidad_de_codigos>")
        sys.exit(1)

    cantidad = int(sys.argv[1])
    print("Generando códigos de recuperación...")

    for i in range(cantidad):
        print(f"Código {i+1}: {generar_codigo()}")

```
### Explicación

- La función `generar_codigo()` debe generar un código de **6 caracteres alfanuméricos**.  
- Se deben excluir caracteres ambiguos (`0, O, 1, l`).  
- El programa toma un argumento desde la línea de comandos que indica **cuántos códigos generar**.  

---

### Paso 2: Modificar el Archivo `build.xml`

Aquí tienes un `build.xml` básico. Este script no ejecuta Python todavía, pero lo usaremos como punto de partida.  

**Crea un archivo `build.xml` con este contenido:**

```xml
<?xml version="1.0"?>
<project name="Hello World Project" default="info">
   <target name="info">
      <echo>Hello World - Welcome to Apache Ant!</echo>
   </target>
</project>
```
### ¿Qué hace este `build.xml`?

- Define un proyecto llamado **"Hello World Project"**.  - Tiene un **target** llamado `info` que imprime un mensaje con `<echo>`.  
- Si ejecutamos `ant` sin especificar un target, se ejecuta `info` por defecto.  

---

### Ejecuta el archivo `build.xml` con:

```bash
ant
```
### Paso 3: Agregar la Ejecución de Python


Modifica el `build.xml` para que incluya un nuevo **target** llamado `run`.  
Este target debe **ejecutar `main.py`** pasando el argumento `3`.  
Investiga en la documentación de Ant cómo usar la tarea `<exec>` para ejecutar scripts de Python.  Busca responder a ¿Cómo definir un target que ejecute un comando con <exec>? ¿Cómo pasar argumentos a un script desde build.xml?


## 3️⃣ Ejercicio en Kotlin con Gradle 🛠️

### 📌 Objetivo

Crear un programa en **Kotlin** que genere **códigos de recuperación** aleatorios de **6 caracteres alfanuméricos**, excluyendo los caracteres ambiguos (`0, O, 1, l`). Luego, configurar **Gradle** para compilar y ejecutar el código.

---

### Paso 1: Crear un Proyecto de Gradle

Desde la raíz del repositorio, en la terminal ejecuta:

```bash
gradle init --type kotlin-application
```

Cuando se te solicite, selecciona las siguientes opciones:

- Selecciona el DSL de script de construcción: Escoge Kotlin (1).
- Acepta los valores por defecto para el nombre del proyecto y el paquete.

### Paso 2: Implementar la Lógica de Generación de Códigos

 **Modifica `src/main/kotlin/App.kt` ppara incluir la lógica dentro de la clase App y permitir la ejecución con argumentos:**

```kotlin
package org.example

import kotlin.random.Random

class App {
    fun generarCodigo(): String {
        /**
         * Genera un código de 6 caracteres alfanuméricos sin caracteres ambiguos.
         * Caracteres ambiguos a evitar: '0', 'O', '1', 'l'.
         */
        val caracteresValidos = "ABCDEFGHJKLMNPQRSTUVWXYZabcdefghijkmnopqrstuvwxyz23456789"
        return (1..6)
            .map { caracteresValidos[Random.nextInt(caracteresValidos.length)] }
            .joinToString("")
    }

    fun ejecutar(cantidad: Int) {
        if (cantidad > 0) {
            println("Generando códigos de recuperación...")
            repeat(cantidad) { i ->
                println("Código ${i + 1}: ${generarCodigo()}")
            }
        } else {
            println("Cantidad inválida. Usa: gradle run --args='3'")
        }
    }
}

fun main(args: Array<String>) {
    val cantidad = args.firstOrNull()?.toIntOrNull()
    App().ejecutar(cantidad ?: -1)
}

```

### 📝 Paso 3: Modificar AppTest.kt para Validar la Lógica

📌 **Modifica el archivo `src/test/kotlin/org/example/AppTest.kt` para probar la generación de códigos de recuperación:**  

```kotlin
package org.example

import kotlin.test.Test
import kotlin.test.assertEquals
import kotlin.test.assertTrue

class AppTest {
    @Test
    fun testGenerarCodigoLongitudCorrecta() {
        val classUnderTest = App()
        val codigo = classUnderTest.generarCodigo()
        
        // Verificar que el código generado tiene 6 caracteres
        assertEquals(6, codigo.length, "El código debe tener exactamente 6 caracteres")
    }

    @Test
    fun testGenerarCodigoSinCaracteresAmbiguos() {
        val classUnderTest = App()
        val codigo = classUnderTest.generarCodigo()
        val caracteresInvalidos = setOf('0', 'O', '1', 'l')

        // Verificar que el código generado no contiene caracteres ambiguos
        assertTrue(codigo.none { it in caracteresInvalidos }, "El código no debe contener caracteres ambiguos")
    }
}

```
Asegúrate de que `settings.gradle.kts` contenga:

```kotlin
rootProject.name = "GradleCodeGenerator"
```

### Paso 4: Compilar y Ejecutar el Proyecto con Gradle
Desde la terminal en la carpeta raíz del proyecto, ejecuta:

```bash
gradle build
gradle run --args="3"
````
