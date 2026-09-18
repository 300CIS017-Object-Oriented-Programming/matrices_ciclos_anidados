# Guía de estudio y práctica, matrices y ciclos anidados en C++

## Propósito

En esta guía vas a representar datos con matrices, recorrerlos mediante índices y usar ciclos anidados para mostrar, contar, reorganizar y transponer información.

El caso inicial usa herraduras de caballos. Después aplicarás el mismo razonamiento a una matriz de temperaturas. Una matriz cambia el contexto, pero no cambia la relación entre filas, columnas e índices.

> **HAZ EL SEGUIMIENTO EN PAPEL**
>
> Si un ejercicio no sale al primer intento, registra los índices y los valores de dos o tres iteraciones. Programar incluye proponer una solución, encontrar diferencias entre lo esperado y lo observado, ajustar y volver a probar.

## 1. Comprender matrices y ciclos anidados

### 1.1 Una matriz organiza datos relacionados

Una matriz guarda datos en dos dimensiones, filas y columnas.

Si debes registrar el estado de las herraduras de dos caballos, puedes usar esta matriz:

```cpp
int main() {
    int necesitaReemplazo[2][4] = {
        {1, 0, 1, 0},
        {0, 1, 0, 1}
    };

    return 0;
}
```

Cada fila representa un caballo y cada columna representa uno de sus cuatro cascos.

| Fila, caballo | Columna, casco | Valor | Significado |
| --- | --- | ---: | --- |
| 0 | 0 | 1 | Reemplazar la herradura |
| 0 | 1 | 0 | Conservar la herradura |
| 0 | 2 | 1 | Reemplazar la herradura |
| 0 | 3 | 0 | Conservar la herradura |
| 1 | 0 | 0 | Conservar la herradura |
| 1 | 1 | 1 | Reemplazar la herradura |
| 1 | 2 | 0 | Conservar la herradura |
| 1 | 3 | 1 | Reemplazar la herradura |

Una posición se escribe con dos índices:

```cpp
necesitaReemplazo[fila][columna]
```

Por ejemplo, `necesitaReemplazo[0][2]` representa el tercer casco del primer caballo.

Sin una matriz tendrías que usar una variable diferente para cada dato. Con una matriz nombras una posición mediante sus índices. Así puedes construir una misma operación para todos los datos, sin escribir una instrucción distinta para cada casco.

### 1.2 Anticipar el recorrido

La matriz tiene dos filas y cuatro columnas. Por tanto, tiene ocho posiciones.

| Caballo | Casco 1 | Casco 2 | Casco 3 | Casco 4 |
| --- | ---: | ---: | ---: | ---: |
| 1 | 1 | 0 | 1 | 0 |
| 2 | 0 | 1 | 0 | 1 |

Antes de escribir código, responde solamente estas tres preguntas:

1. Qué representa una fila.
2. Qué representa una columna.
3. Cuántas posiciones deben recorrerse.

En este caso, una fila representa un caballo, una columna representa un casco y se recorren ocho posiciones.

### 1.3 Qué es un ciclo anidado

Un ciclo anidado es un ciclo dentro de otro. Se usa cuando una tarea tiene dos niveles de repetición.

Esta es la forma general de un ciclo dentro de otro:
```cpp
for (int indiceExterno = inicioExterno;
     indiceExterno <= limiteExterno;
     indiceExterno++) {

    for (int indiceInterno = inicioInterno;
         indiceInterno <= limiteInterno;
         indiceInterno++) {

        // Instrucción que se repite para cada combinación.
    }

    // Instrucción que se ejecuta al terminar un grupo.
}
```
Léelo en este orden:

El ciclo externo selecciona un grupo, por ejemplo, la fila 1.
El ciclo interno empieza su recorrido, por ejemplo, desde el puesto 1.
El ciclo interno termina todos los puestos de esa fila.
El ciclo externo avanza a la fila siguiente.
El ciclo interno vuelve a iniciar desde su valor inicial.

Cuando el ciclo externo pasa a una nueva fila,
el ciclo interno vuelve a iniciar,
porque debe recorrer todos los puestos de esa nueva fila.


En el caso de los caballos, puedes seleccionar un caballo y después recorrer sus cuatro cascos. El ciclo exterior representa el caballo actual. El ciclo interior representa los cascos que se revisan para ese caballo.

```cpp
void mostrarRevisionCaballos() {
    for (int caballo = 0; caballo < 2; caballo++) {
        for (int casco = 0; casco < 4; casco++) {
            cout << "Revisar caballo " << caballo + 1
                 << ", casco " << casco + 1 << endl;
        }
    }
}
```

La idea se puede relacionar con una revisión de herraduras. Cuando llega el caballo 1, se revisan sus cascos 1, 2, 3 y 4. Al terminar ese recorrido inicia la revisión del caballo 2.

Ese orden no es obligatorio para todos los problemas. Si necesitas comparar el mismo casco en todos los caballos, puedes recorrer primero los cascos y después los caballos. El ciclo exterior controla la dimensión que decides recorrer primero.

| Parte | Lectura |
| --- | --- |
| `caballo = 0` | El recorrido inicia con el primer caballo. |
| `caballo < 2` | Se repite mientras existan caballos pendientes. |
| `casco = 0` | Para el caballo actual, inicia la revisión del primer casco. |
| `casco < 4` | Se recorren los cuatro cascos. |
| `matriz[caballo][casco]` | Accede a la posición actual. |

### 1.4 Tabla de seguimiento

Sigue manualmente las ocho ejecuciones del cuerpo interno.

| Paso | `caballo` | `casco` | Posición | Valor |
| ---: | ---: | ---: | --- | ---: |
| 1 | 0 | 0 | `necesitaReemplazo[0][0]` | 1 |
| 2 | 0 | 1 | `necesitaReemplazo[0][1]` | 0 |
| 3 | 0 | 2 | `necesitaReemplazo[0][2]` | 1 |
| 4 | 0 | 3 | `necesitaReemplazo[0][3]` | 0 |
| 5 | 1 | 0 | `necesitaReemplazo[1][0]` | 0 |
| 6 | 1 | 1 | `necesitaReemplazo[1][1]` | 1 |
| 7 | 1 | 2 | `necesitaReemplazo[1][2]` | 0 |
| 8 | 1 | 3 | `necesitaReemplazo[1][3]` | 1 |

La instrucción más interna se ejecuta ocho veces, dos caballos por cuatro cascos.

### 1.5 Dos ciclos consecutivos no forman un ciclo anidado

```cpp
void mostrarCiclosConsecutivos() {
    for (int caballo = 0; caballo < 2; caballo++) {
        cout << "Caballo " << caballo + 1 << endl;
    }

    for (int casco = 0; casco < 4; casco++) {
        cout << "Casco " << casco + 1 << endl;
    }
}
```

Resultado:

```text
Caballo 1
Caballo 2
Casco 1
Casco 2
Casco 3
Casco 4
```

Los recorridos ocurren uno después del otro. Ningún casco está asociado con un caballo específico.

En un ciclo anidado, el ciclo interior se repite para cada valor del ciclo exterior:

```text
Revisar caballo 1, casco 1
Revisar caballo 1, casco 2
Revisar caballo 1, casco 3
Revisar caballo 1, casco 4
Revisar caballo 2, casco 1
Revisar caballo 2, casco 2
Revisar caballo 2, casco 3
Revisar caballo 2, casco 4
```

### Pausa de práctica 1

#### Ejercicio 1, ubicar posiciones

Completa los valores.

| Posición | Valor |
| --- | ---: |
| `necesitaReemplazo[0][0]` | |
| `necesitaReemplazo[0][3]` | |
| `necesitaReemplazo[1][1]` | |
| `necesitaReemplazo[1][2]` | |

#### Ejercicio 2, completar el seguimiento

| Paso | `caballo` | `casco` | Posición visitada |
| ---: | ---: | ---: | --- |
| 1 | 0 | 0 | `necesitaReemplazo[0][0]` |
| 2 | 0 | 1 | `necesitaReemplazo[0][1]` |
| 3 | | | |
| 4 | | | |
| 5 | | | |
| 6 | | | |
| 7 | | | |
| 8 | | | |

#### Ejercicio 3, mostrar combinaciones

Completa este procedimiento para mostrar las ocho combinaciones de caballo y casco.

```cpp
void mostrarCombinaciones() {
    // Escribe los dos ciclos.
}
```

La salida debe comenzar así:

```text
Caballo 1, casco 1
Caballo 1, casco 2
```

## 2. Patrones de recorrido y operaciones con matrices

### 2.1 Un proyecto con tres archivos

Usa un solo proyecto para los ejemplos.

```text
ciclos_anidados/
│
├── main.cpp
├── operaciones.h
└── operaciones.cpp
```

`main.cpp` prepara las matrices y llama las funciones. `operaciones.cpp` contiene las operaciones. `operaciones.h` declara las funciones.

### 2.2 Recorrer toda la matriz

```cpp
void mostrarMatrizHerraduras(int matriz[][4], int filas) {
    for (int caballo = 0; caballo < filas; caballo++) {
        cout << "Caballo " << caballo + 1 << ": ";

        for (int casco = 0; casco < 4; casco++) {
            cout << matriz[caballo][casco] << " ";
        }

        cout << endl;
    }
}
```

Resultado:

```text
Caballo 1: 1 0 1 0
Caballo 2: 0 1 0 1
```

La operación interna muestra un valor por cada columna. El salto de línea se ejecuta después del ciclo interior, por eso cada caballo aparece en una línea distinta.

### Pausa de práctica 2

#### Ejercicio 4, dibujar una matriz

Escribe un procedimiento que muestre tres filas y cuatro columnas con el símbolo `*`.

```text
* * * *
* * * *
* * * *
```

Dibuja primero la salida y decide en qué lugar debe quedar el salto de línea.

#### Ejercicio 5, mostrar posiciones

Construye un procedimiento que muestre los pares de índices de una matriz de dos filas y cuatro columnas.

```text
[0][0] [0][1] [0][2] [0][3]
[1][0] [1][1] [1][2] [1][3]
```

#### Ejercicio 6, recorrer por columnas

Modifica el orden de los ciclos para producir este resultado:

```text
[0][0] [1][0]
[0][1] [1][1]
[0][2] [1][2]
[0][3] [1][3]
```

### 2.3 Tomar decisiones durante el recorrido

El recorrido puede visitar todas las posiciones y actuar solo sobre algunas de ellas.

```cpp
void revisarHerraduras(int matriz[][4], int filas) {
    for (int caballo = 0; caballo < filas; caballo++) {
        for (int casco = 0; casco < 4; casco++) {
            if (matriz[caballo][casco] == 1) {
                cout << "Reemplazar herradura, caballo "
                     << caballo + 1
                     << ", casco " << casco + 1 << endl;
            }
        }
    }
}
```

Resultado:

```text
Reemplazar herradura, caballo 1, casco 1
Reemplazar herradura, caballo 1, casco 3
Reemplazar herradura, caballo 2, casco 2
Reemplazar herradura, caballo 2, casco 4
```

Los ciclos recorren toda la matriz. La condición decide si se muestra un mensaje.

### 2.4 Contar un resultado global

Un contador global conserva su valor durante todo el recorrido. Se declara antes de ambos ciclos.

```cpp
int contarReemplazos(int matriz[][4], int filas) {
    int total = 0;

    for (int caballo = 0; caballo < filas; caballo++) {
        for (int casco = 0; casco < 4; casco++) {
            if (matriz[caballo][casco] == 1) {
                total = total + 1;
            }
        }
    }

    return total;
}
```

Resultado:

```text
Total de reemplazos: 4
```

| Paso | Posición | Valor | `total` después del paso |
| ---: | --- | ---: | ---: |
| 1 | `[0][0]` | 1 | 1 |
| 2 | `[0][1]` | 0 | 1 |
| 3 | `[0][2]` | 1 | 2 |
| 4 | `[0][3]` | 0 | 2 |
| 5 | `[1][0]` | 0 | 2 |
| 6 | `[1][1]` | 1 | 3 |
| 7 | `[1][2]` | 0 | 3 |
| 8 | `[1][3]` | 1 | 4 |

### 2.5 Contar por cada fila

Un contador por caballo se reinicia al comenzar cada fila. Por eso se declara dentro del ciclo exterior.

```cpp
void mostrarReemplazosPorCaballo(int matriz[][4], int filas) {
    for (int caballo = 0; caballo < filas; caballo++) {
        int reemplazosCaballo = 0;

        for (int casco = 0; casco < 4; casco++) {
            if (matriz[caballo][casco] == 1) {
                reemplazosCaballo = reemplazosCaballo + 1;
            }
        }

        cout << "Caballo " << caballo + 1
             << ": " << reemplazosCaballo
             << " reemplazos" << endl;
    }
}
```

Resultado:

```text
Caballo 1: 2 reemplazos
Caballo 2: 2 reemplazos
```

| Variable | Lugar donde se declara | Razón |
| --- | --- | --- |
| `total` | Antes de ambos ciclos | Conserva los resultados de toda la matriz. |
| `reemplazosCaballo` | Dentro del ciclo exterior | Inicia nuevamente para cada caballo. |
| `casco` | En el ciclo interior | Representa la columna actual. |

### Pausa de práctica 3

#### Ejercicio 7, contar herraduras conservadas

Construye la función:

```cpp
int contarHerradurasConservadas(int matriz[][4], int filas) {
    // Escribe la solución.
}
```

Debe contar los valores iguales a `0`. Completa primero una tabla de seguimiento con las ocho posiciones.

#### Ejercicio 8, contar reemplazos por casco

Construye un procedimiento que muestre cuántos reemplazos aparecen en cada columna.

```text
Casco 1: 1 reemplazo
Casco 2: 1 reemplazo
Casco 3: 1 reemplazo
Casco 4: 1 reemplazo
```

#### Ejercicio 9, comparar dos ubicaciones de un contador

Declara temporalmente `reemplazosCaballo` antes del ciclo exterior. Registra el resultado para cada caballo. Después declara nuevamente el contador dentro del ciclo exterior y explica la diferencia mediante una tabla.

### 2.6 Recorrer por columnas

```cpp
void mostrarPorColumnas(int matriz[][4], int filas) {
    for (int casco = 0; casco < 4; casco++) {
        cout << "Casco " << casco + 1 << ": ";

        for (int caballo = 0; caballo < filas; caballo++) {
            cout << matriz[caballo][casco] << " ";
        }

        cout << endl;
    }
}
```

Resultado:

```text
Casco 1: 1 0
Casco 2: 0 1
Casco 3: 1 0
Casco 4: 0 1
```

La matriz no cambia. Cambia el orden en que lees las posiciones.

### 2.9 Código integrado

#### operaciones.h

```cpp
#ifndef OPERACIONES_H
#define OPERACIONES_H

void mostrarRevisionCaballos();
void mostrarMatrizHerraduras(int matriz[][4], int filas);
void revisarHerraduras(int matriz[][4], int filas);
int contarReemplazos(int matriz[][4], int filas);
void mostrarReemplazosPorCaballo(int matriz[][4], int filas);

#endif
```

#### operaciones.cpp

```cpp
#include "operaciones.h"
#include <iostream>

using std::cout;
using std::endl;

void mostrarRevisionCaballos() {
    for (int caballo = 0; caballo < 2; caballo++) {
        for (int casco = 0; casco < 4; casco++) {
            cout << "Revisar caballo " << caballo + 1
                 << ", casco " << casco + 1 << endl;
        }
    }
}

void mostrarMatrizHerraduras(int matriz[][4], int filas) {
    for (int caballo = 0; caballo < filas; caballo++) {
        cout << "Caballo " << caballo + 1 << ": ";

        for (int casco = 0; casco < 4; casco++) {
            cout << matriz[caballo][casco] << " ";
        }

        cout << endl;
    }
}

void revisarHerraduras(int matriz[][4], int filas) {
    for (int caballo = 0; caballo < filas; caballo++) {
        for (int casco = 0; casco < 4; casco++) {
            if (matriz[caballo][casco] == 1) {
                cout << "Reemplazar herradura, caballo "
                     << caballo + 1
                     << ", casco " << casco + 1 << endl;
            }
        }
    }
}

int contarReemplazos(int matriz[][4], int filas) {
    int total = 0;

    for (int caballo = 0; caballo < filas; caballo++) {
        for (int casco = 0; casco < 4; casco++) {
            if (matriz[caballo][casco] == 1) {
                total = total + 1;
            }
        }
    }

    return total;
}

void mostrarReemplazosPorCaballo(int matriz[][4], int filas) {
    for (int caballo = 0; caballo < filas; caballo++) {
        int reemplazosCaballo = 0;

        for (int casco = 0; casco < 4; casco++) {
            if (matriz[caballo][casco] == 1) {
                reemplazosCaballo = reemplazosCaballo + 1;
            }
        }

        cout << "Caballo " << caballo + 1
             << ": " << reemplazosCaballo
             << " reemplazos" << endl;
    }
}

void mostrarPorColumnas(int matriz[][4], int filas) {
    for (int casco = 0; casco < 4; casco++) {
        cout << "Casco " << casco + 1 << ": ";

        for (int caballo = 0; caballo < filas; caballo++) {
            cout << matriz[caballo][casco] << " ";
        }

        cout << endl;
    }
}


```

#### main.cpp

```cpp
#include "operaciones.h"
#include <iostream>

using std::cout;
using std::endl;

int main() {
    int necesitaReemplazo[2][4] = {
        {1, 0, 1, 0},
        {0, 1, 0, 1}
    };

    mostrarRevisionCaballos();

    cout << endl;
    mostrarMatrizHerraduras(necesitaReemplazo, 2);

    cout << endl;
    revisarHerraduras(necesitaReemplazo, 2);

    cout << endl;
    cout << "Total de reemplazos: "
         << contarReemplazos(necesitaReemplazo, 2) << endl;

    cout << endl;
    mostrarReemplazosPorCaballo(necesitaReemplazo, 2);

    return 0;
}
```

## 3. Trabajo independiente y revisión

### 3.1 Ejercicios de práctica

#### Ejercicio 10, temperaturas por día

Una matriz almacena temperaturas de tres días, en cuatro franjas horarias.

```cpp
int main() {
    int temperaturas[3][4] = {
        {18, 23, 27, 21},
        {17, 22, 26, 20},
        {19, 24, 28, 22}
    };

    return 0;
}
```

Escribe un procedimiento que muestre la temperatura de cada día y cada franja.

```text
Día 1, franja 1: 18
Día 1, franja 2: 23
```

#### Ejercicio 11, contar temperaturas altas

Construye una función que cuente cuántas temperaturas son mayores o iguales a `25`.

Completa primero una tabla de seguimiento con los doce valores y el contador acumulado.

#### Ejercicio 12, promedio por día

Crea un procedimiento que muestre el promedio de las cuatro temperaturas de cada día.

Usa una variable acumuladora que se reinicie al comenzar un nuevo día.

```text
Promedio del día 1: 22
Promedio del día 2: 21
Promedio del día 3: 23
```

#### Ejercicio 13, recorrer por franjas

Modifica el recorrido para que el ciclo exterior represente la franja horaria y el ciclo interior represente los días.

```text
Franja 1: 18 17 19
Franja 2: 23 22 24
```

#### Ejercicio 14, detectar una posición

Construye una función que busque si existe un valor menor que `18` en la matriz de temperaturas.

La función debe retornar `1` si existe un valor menor que `18` y `0` si no existe. No uses `break`. Puedes usar `return` cuando encuentres el valor.


#### Ejercicio 15, integrar un reporte

Amplía el proyecto de los caballos con una función que muestre, por cada caballo, el número del primer casco que necesita reemplazo.

```text
Caballo 1, primer reemplazo: casco 1
Caballo 2, primer reemplazo: casco 2
```

Si un caballo no requiere reemplazos, muestra:

```text
Caballo 3, no requiere reemplazo
```

Declara el prototipo en `operaciones.h`, escribe la función en `operaciones.cpp` y llámala desde `main.cpp`.

### 3.2 Prompt de apoyo para revisar y aprender

```text
Actúa como tutor socrático de programación en C++.

Estoy aprendiendo matrices, ciclos anidados, recorridos de índices y operaciones con matrices.

Voy a compartir una imagen o captura de mi solución. Ayúdame a revisarla mediante preguntas y no resuelvas el ejercicio por mí.

Sigue estas reglas:

- Lee primero el código y pide el enunciado si no aparece.
- Si una parte de la imagen no se puede leer, pregunta qué dice. No inventes código.
- Haz una pregunta a la vez.
- Pídeme identificar qué representa el ciclo exterior y qué representa el ciclo interior.
- Ayúdame a construir una tabla de seguimiento con los índices, la posición de la matriz y las variables que cambian.
- Revisa que los límites de los ciclos correspondan con las dimensiones de la matriz.
- Revisa si un contador debe conservar su valor para toda la matriz o reiniciar al comenzar una fila.
- Si estoy copiando datos, ayúdame a diferenciar la posición origen de la posición destino.
- Si estoy transponiendo una matriz, ayúdame a verificar que se intercambien fila y columna.
- No escribas una solución completa.
- No uses using namespace std.
- No introduzcas vectores, clases, referencias ni const.
- Si respondo incorrectamente, dame una pista corta y vuelve a preguntarme.
- Si mi solución funciona, pídeme explicar por qué el orden de los ciclos produce esa salida.

Cuando crea que terminé, revisa nuevamente la solución.

Comienza diciendo qué entiendes que intenta hacer mi código y formula solamente la primera pregunta.
```

### 3.3 Revisión final

| Revisión | Evidencia esperada |
| --- | --- |
| Índices | Cada índice permanece dentro de los límites de la matriz. |
| Recorrido | La cantidad de posiciones visitadas coincide con filas por columnas. |
| Contadores | Un contador global conserva su valor, un contador por fila se reinicia. |
| Salida | La salida coincide con la tabla de seguimiento o con el dibujo previsto. |
| Transformaciones | La matriz origen conserva sus valores y la matriz destino recibe las posiciones calculadas. |
