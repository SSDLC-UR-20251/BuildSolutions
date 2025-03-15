
# Ejercicios con Maven y NPM
## Creación del Proyecto Maven  
Para iniciar, necesitamos inicializar un nuevo proyecto de Maven, en CodeSpaces, ejecutemos el siguiente comando para generar la estructura del proyecto Maven:

```bash
mvn archetype:generate -DgroupId=com.example -DartifactId=maven-banking -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```

Posetiormente, navegamos al directorio del proyecto:  

```bash
cd maven-banking
```

Deberiamos ver la siguiente estructura generada:
```
maven-banking
├── pom.xml
├── src
│   ├── main
│   │   ├── java
│   │   │   ├── com
│   │   │   │   ├── example
│   │   │   │   │   ├── App.java
│   ├── test
│   │   ├── java
│   │   │   ├── com
│   │   │   │   ├── example
│   │   │   │   │   ├── AppTest.java
│   ├── resources
│   │   ├── transactions.txt  
```

## Implementación del Extracto Bancario  
📌 **Objetivo:**  
Continuando con la lógica de nuestra aplicación bancaria, para este ejrcicio desarrollaremos una aplicación en Java que:  

1. Lea un archivo `transactions.txt` con transacciones bancarias en formato JSON.

2. Permita ingresar un correo de usuario y extraiga sus transacciones.  

3. Genere un extracto bancario en un archivo `.txt`.

**Ejemplo de entrada (`transactions.txt`)**
En el repositorio BuildSolutions encontrarán un archivo de trnsacciones un poco más completo.
```json
{
  "juan.jose@urosario.edu.co": [
    {"balance": "50", "type": "Deposit", "timestamp": "2025-02-11 14:17:21.921536"},
    {"balance": "-20", "type": "Withdrawal", "timestamp": "2025-02-15 10:30:15.123456"}
  ]
}
```

**Ejemplo de salida (`juan_jose_urosario_edu_co_extracto.txt`):**  
```
Extracto Bancario - juan.jose@urosario.edu.co
====================================
Fecha: 2025-02-11 14:17:21.921536
Tipo: Deposit
Monto: 50
------------------------------------
Fecha: 2025-02-15 10:30:15.123456
Tipo: Withdrawal
Monto: -20
------------------------------------
```


## Esqueleto de Implementación en `App.java`  
Dado que no todos hemos programado en Java, les estoy brindando un esqueleto con las funciones que deben implementar. En cada una de ellas encontraran un `TODO` para ser completado.

📌**Nota:** Está bien si utilizmos alguna IA para completar el ejercicio, sin embargo hagamos el esfuerzo de entender la lógica implementada y que no sea solo copiar y pegar.

```java
package com.example;

import java.io.*;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.util.*;
import org.json.*;

public class App {

    // 🔹 1. Leer el archivo JSON desde un .txt
    public static String leerArchivo(String rutaArchivo) {
        // TODO: Implementar la lectura del archivo
        return null;
    }

    // 🔹 2. Obtener transacciones de un usuario específico
    public static List<JSONObject> obtenerTransacciones(String jsonData, String usuario) {
        // TODO: Extraer transacciones del usuario
        return null;
    }

    // 🔹 3. Generar extracto bancario en un archivo .txt
    public static void generarExtracto(String usuario, List<JSONObject> transacciones) {
        // TODO: Guardar el extracto bancario en un archivo
    }

    public static void main(String[] args) {
        // TODO: Solicitar usuario, leer transacciones y generar extracto
    }
}
```

## Implementación de Pruebas Unitarias  
Una vez implementada la lógica, debemos validar el correcto funcionamiento. Para eso crearemos algunas ruebas unitarias. 

📌 **Pruebas que debes implementar en `AppTest.java`:** 

1. **Prueba para `leerArchivo()`** → Verifica que el archivo `transactions.txt` se lee correctamente. Es posible utilizar métodos como `assertNotNull` para validar que el archivo no es nulo y `assertFalse` para validar que no esta vacío.

2. **Prueba para `obtenerTransacciones()`** → Asegura que extrae correctamente las transacciones de un usuario.  Puede tener una transacción de prueba, llamar a la funcion `obtenerTransacciones` y validar la salida usando `assertNotNull` y `assertEquals` montando un caso especifico, supongamos validar que el balance es un número exacto,

3. **Prueba para `generarExtracto()`** → Verifica que el extracto bancario se genera en un archivo `.txt`.  Es posible usar `assertTrue` para validar la existencia del archivo.

## Esqueleto de `AppTest.java`  
📌 **Completa las pruebas unitarias marcadas con `TODO`:**  
```java
package com.example;

import org.json.JSONObject;
import org.junit.jupiter.api.Test;
import java.io.File;
import java.time.LocalDate;
import java.util.List;

import static org.junit.jupiter.api.Assertions.*;

public class AppTest {

    @Test
    public void testLeerArchivo() {
        // TODO: Implementar prueba para verificar que el archivo se lee correctamente
    }

    @Test
    public void testObtenerTransacciones() {
        // TODO: Implementar prueba para verificar que las transacciones se extraen correctamente
    }

    @Test
    public void testGenerarExtracto() {
        // TODO: Implementar prueba para verificar que el extracto se genera correctamente
    }
}
```
## Modificación del archivo `pom.xml`
Nuestro archivo de construccion debe tener todas las dependencias necesrias para poder ejecutar la aplicación.

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
         http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.example</groupId>
    <artifactId>maven-java</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>
        <!-- JUnit 5 para pruebas unitarias -->
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter-api</artifactId>
            <version>5.8.1</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter-engine</artifactId>
            <version>5.8.1</version>
        </dependency>
        <dependency>
          <groupId>org.json</groupId>
          <artifactId>json</artifactId>
          <version>20210307</version>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <!-- Plugin para compilar código con Java 17 -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.8.1</version>
                <configuration>
                    <source>17</source>
                    <target>17</target>
                </configuration>
            </plugin>

            <!-- Plugin para ejecutar la aplicación con 'mvn exec:java' -->
            <plugin>
                <groupId>org.codehaus.mojo</groupId>
                <artifactId>exec-maven-plugin</artifactId>
                <version>3.1.0</version>
                <executions>
                    <execution>
                        <goals>
                            <goal>java</goal>
                        </goals>
                    </execution>
                </executions>
                <configuration>
                    <mainClass>com.example.App</mainClass>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>

```


---

## Comandos para Compilar y Ejecutar  
 **1. Compilar el proyecto:**  
```bash
mvn compile
```

**2. Ejecutar la aplicación:**  
```bash
mvn exec:java
```

**3. Ejecutar las pruebas unitarias:**  
```bash
mvn test
```

**4. Empaquetar el proyecto en un `.jar`:**  
```bash
mvn package
```

**5. Ejecutar el `.jar` generado:**  
```bash
java -jar target/maven-banking-1.0-SNAPSHOT.jar
```

## Creación de proyecto con NPM
Para comenzar, necesitamos inicializar un nuevo proyecto Node.js utilizando npm.

Para esto crearemos una nueva carpeta dedicada a npm
```bash
mkdir nm
```

Y ejecutamos los siguientes comandos para inicializar un nuevo proyecto npm:
```bash
cd npm
npm init -y
```
Esto generará un archivo package.json con la configuración básica del proyecto.

## Creación estructura de carpetas
Crearemos una estructura para dividir el código entre la logica de la aplicación y las pruebas unitarias, par aesto necesitamos dos carpeta:
```bash
mkdir src
mkdir test
```
## Instalación de librerías
Para este ejercicio, utilizaremos Jest para las pruebas unitarias. Instalémoslo como una dependencia de desarrollo.

```bash

npm install --save-dev jest
```
## Implementación funcionaidad transferencia bancaria
Implementaremos una funcionalidad denro del Bankingsystem que le permita al usuario transferir dinero de una cuenta a otra. par aesto debemos implementar:
1. LeerArchivo: Lee el archivo transactions.txt y devuelve su contenido en formato JSON.
2. guardarArchivo: Guarda los datos de transacciones en el archivo transactions.txt en formato JSON.
3. CalcularSaldo: Calcula el saldo total de un usuario sumando depósitos y restando retiros de sus transacciones.
4. transferir: Realiza una transferencia de dinero entre dos cuentas, verificando saldo y actualizando el archivo de transacciones.

## Esqueleto de `App.js`para la implementación
En el archivo src/app.js, deberán implementar la lógica de las funciones bancarias. A continuación, les proporciono el esqueleto de las funciones:

```javascript
const fs = require('fs');

// Función para leer el archivo transactions.txt
function leerArchivo() {
    // TODO: Implementar la lógica para leer el archivo transactions.txt.
    // Devuelve el contenido del archivo en formato JSON.
    return {}; // Completar la lógica
}

// Función para escribir el archivo transactions.txt
function escribirArchivo(data) {
    // TODO: Implementar la lógica para escribir los datos de vuelta en el archivo transactions.txt.
    // Los datos deben guardarse en formato JSON.
    // Completar la lógica
}

// Función para calcular el saldo actual de un usuario, basado en sus transacciones
function calcularSaldo(usuario) {
    // TODO: Implementar la lógica para calcular el saldo del usuario.
    // El saldo se calcula sumando los depósitos y restando los retiros.
    let saldo = 0;
    return saldo; // Completar la lógica
}

// Función para realizar la transferencia entre cuentas
function transferir(de, para, monto) {
    // TODO: Implementar la lógica para realizar la transferencia entre dos cuentas.
    // Verificar que el saldo de la cuenta de origen sea suficiente.
    // Restar el monto de la cuenta de origen y sumarlo a la cuenta de destino.
    // Actualizar el archivo transactions.txt.
    return {
        exito: true, 
        mensaje: `Transferencia de ${monto} realizada correctamente de ${de} a ${para}.`
    }; // Completar la lógica
}

const resultado = transferir('juan.jose@urosario.edu.co', 'sara.palaciosc@urosario.edu.co', 50);
console.log(resultado.mensaje);

// Exportar las funciones para pruebas
module.exports = { transferir };

```

## Creación de `App.test.js` para pruebas
En el archivo `test/app.test.js`, agregarán las pruebas unitarias utilizando Jest. A continuación, les proporciono el esqueleto de las pruebas, dejando espacio para que completen las validaciones.

```javascript
const { transferir } = require('../src/app');

test('Transferencia entre cuentas', () => {
    // TODO: Crear una prueba que verifique si la transferencia fue exitosa.
    const resultado = transferir('juan.jose@urosario.edu.co', 'sara.palaciosc@urosario.edu.co', 30);
    // Completar
});

test('Transferencia con saldo insuficiente', () => {
    // TODO: Crear una prueba que verifique que la transferencia falla si el saldo de la cuenta de origen es insuficiente.
    const resultado = transferir('juan.jose@urosario.edu.co', 'sara.palaciosc@urosario.edu.co', 1000);
    // Completar
});

```

## Modificar `package.json`
Ahora, debemos configurar los scripts de start y test en el archivo package.json para que puedan ejecutar el código y las pruebas fácilmente. Agrega los siguientes valores al parametro `scripts`
```json

  "scripts": {
    "start": "node src/app.js",
    "test": "jest"
  },
 
```
## Comandos necesarios para ejecutar la aplicación
1. Instalar las dependencias:
```bash
npm install
```
2. Ejecutar el proyecto con npm start:
```bash

npm start
```
Esto ejecutará node src/app.js y correrá el código de la aplicación.

3. Ejecutar las pruebas con npm test:
```bash
npm test
```

Esto ejecutará Jest y correrá las pruebas definidas en test/app.test.js.