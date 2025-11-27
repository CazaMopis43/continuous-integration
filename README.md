# Informe de Práctica nº 4: Eliminación de Recursividad Múltiple Redundante

## Grados: Ingeniería Informática e Ingeniería de Computadores
## Asignatura: Algoritmos Avanzados
## Curso: 2025/2026
## Autores: Marc Burgos Ucendo,Alberto Sastre Zorrilla
El objetivo de la práctica es que el alumno profundice en la eliminación de la recursividad múltiple redundante.

El algoritmo a optimizar es:
```java
public static int f (int x) {
    return g(x,0);
}

private static int g (int x, int y) {
    if (x==0)
        return 0;
    else if (x==1)
        return y;
    else if (y==0) // Rec. 1: g(x, 0) = g(x-1, 0) + g(x, 1)
        return g(x-1,0) + g(x,1);
    else // y==1 (Rec. 2): g(x, 1) = g(x-2, 0) + g(x-1, 1)
        return g(x-2,0) + g(x-1,1);
}
```
### Análisis de la Redundancia y Diseño de la Tabla

El problema 
f(x)≡g(x,0) exhibe recursividad múltiple redundante.
La optimización se logra almacenando los subproblemas repetidos.

a) Análisis de Redundancia
Ejemplo: 
𝑓(5)≡𝑔(5,0)

#### Árbol de Recursión para g(5,0) (Foto1)

La figura muestra una expansión recursiva con crecimiento exponencial.

Datos de Redundancia (g(5,0)) Foto(2):

Total de nodos: 48

Nodos repetidos: 20 (41.6%)

Ejemplos:

g(2,1) se calcula 8 veces

g(3,1),g(2,0): 4 veces cada uno

#### Grafo de Dependencia Foto(3)
Muestra solo los estados únicos g(x,y).
Los nodos con múltiples arcos entrantes representan subestructuras comunes reutilizables.
### Tabla
Se diseña una tabla $t$ para almacenar el resultado de cada subproblema único $g(i, j)$ 
Dimensiones: La función $g(x, y)$ depende de dos parámetros: $x$ ($0 \le x \le n$) e $y$ ($y \in \{0, 1\}$)  Filas: $x+1$ (índices de $0$ a $x$)Columnas: $2$ (índices $0$ y $1$ para $y$).

```Java
int[][] t = new int[x + 1][2];
```
### Memorización (Top-Down)
#### Código
```Java
public static int f_mem (int x) {
    if (x < 0) return 0;

    int[][] t = new int[x + 1][2];
    for (int i = 0; i <= x; i++) {
        t[i][0] = t[i][1] = -1; // -1: no calculado
    }

    return g_mem(x, 0, t);
}

private static int g_mem (int x, int y, int[][] t) {
    if (x < 0 || y < 0 || y > 1) return 0;

    // CONSULTAR (Cache Hit)
    if (t[x][y] != -1) {
        return t[x][y];
    }

    int resultado;

    // CALCULAR
    if (x == 0)
        resultado = 0;
    else if (x == 1)
        resultado = y;
    else if (y == 0)
        resultado = g_mem(x - 1, 0, t) + g_mem(x, 1, t);
    else
        resultado = g_mem(x - 2, 0, t) + g_mem(x - 1, 1, t);

    // ALMACENAR (SAVE)
    t[x][y] = resultado;
    return resultado;
}
```
#### Árbol de Recursión de 
Ver la foto (3)	​

Solo se calculan valores nuevos.
Las llamadas redundantes se evitan con cache hits.

Redundancia Eliminada:

Nodos repetidos: 0

Total de llamadas reduce de 48 → 18


Tiempo:O(n)→
El tiempo está dominado por la inicialización de la tabla (O(n)) y el cálculo de cada uno de los ≈2n subproblemas únicos. 

Cada subproblema, cuando se calcula por primera vez (no Cache Hit), toma tiempo O(1). Por lo tanto, el tiempo total es T(n)=O(n)+O(n)=O(n)."

Espacio:O(n)→
El espacio es determinado por la tabla t de tamaño (n+1)×2 (O(n)) y la profundidad de la pila de llamadas recursivas, que en el peor caso es proporcional a n (O(n)). 

El espacio total es S(n)=O(n)+O(n)=O(n).
### Tabulación (Bottom-Up)
#### Código
```Java
public static int f_tab (int x) {
    if (x < 0) return 0;
    if (x == 0) return 0;

    int[][] t = new int[x + 1][2];

    // 1. Casos base
    t[0][0] = 0; 
    t[0][1] = 0; 
    t[1][0] = 0; 
    t[1][1] = 1;

    // 2. Rellenar tabla
    for (int i = 2; i <= x; i++) {
        t[i][1] = t[i - 2][0] + t[i - 1][1]; // g(i, 1)
        t[i][0] = t[i - 1][0] + t[i][1];     // g(i, 0)
    }

    return t[x][0];
}

```
#### Complejidades:

Tiempo:O(n)→
El tiempo está dominado por el bucle de llenado de la tabla, el cual itera O(n) veces (desde i=2 hasta x). Dentro del bucle, solo se realizan operaciones de costo constante O(1). 

El costo total en tiempo es T(n)=O(n)⋅O(1)=O(n).

Espacio:O(n)→El espacio es determinado por la tabla t de tamaño (n+1)×2 (O(n)). Dado que es una implementación iterativa (Bottom-Up), no utiliza pila de llamadas recursivas (O(1)).

El espacio total es T(n)=O(n)+O(1)=O(n)."

### Conclusiones

La recursividad múltiple redundante se optimiza mediante Programación Dinámica, pasando de complejidad exponencial a lineal.

Ambas técnicas (Memorización y Tabulación) logran 
O(n), aunque:

Tabulación suele ser más eficiente (menor coste constante y sin pila de llamadas).

### Uso de Herramientas de la IA 

Se ha utilizado la Ia para redactar mejor el informe y para consultas sobre la herramienta srec
Se ha añadido una imagen al final que muestra correctamente el apartado de la tabla, ya que al pasarlo a pdf no mantiene la estructura del markdown y aparecen símbolos de dolar, por tanto, para que se vea el resultado que queriamos mostrar hemos adjuntado dicha foto
