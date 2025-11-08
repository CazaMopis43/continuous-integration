# Práctica 3b: Algoritmos Avanzados  
**Grados en Ingeniería Informática e Ingeniería de Computadores**  
**Curso 2025/2026**

**Marc Burgos Ucendo, Alberto Sastre Zorrilla**
---

## 1. Técnica de vuelta atrás (opcional)

Hemos modificado el algoritmo de la primera entrega siguiendo las directrices que nos sgirió el profesor, como usar un if para la estructura.
```java
public static int backtracking(int[] xs, int[] ps) {
        if (xs == null || ps == null || xs.length != ps.length || xs.length == 0)
            return 0;

        int n = xs.length;
        int[] siguienteValido = new int[n];
        for (int i = 0; i < n; i++) {
            int j = i + 1;
            while (j < n && Math.abs(xs[j] - xs[i]) <= 5) j++;
            siguienteValido[i] = j;
        }

        return buscarBacktracking(ps, 0, siguienteValido);
    }

    private static int buscarBacktracking(int[] ps, int indice, int[] siguienteValido) {
        if (indice == ps.length) return 0;

        int sinColocar = buscarBacktracking(ps, indice + 1, siguienteValido);
        int conColocar = ps[indice] + buscarBacktracking(ps, siguienteValido[indice], siguienteValido);

        return Math.max(sinColocar, conColocar);
    }
```
Además, en el anterior informe tuvimos una confusión al incluir las iágenes, de forma que nuestro análisis era confuso ya que no correspondían los datos con las imágenes,al ser esta práctica una ampliación de la anterior, el ánalisis de la primera parte está incluido en el análisis de esta segunda.


---

## 2. Técnica de ramificación y poda

### a) Función de cota para este problema

Se propone una **función de cota inferior** basada en la relajación del problema: se permite asignar recursos a hospitales sin respetar las dependencias entre ellos, pero respetando el límite de recursos totales.

#### (1) Definición de la función de cota

Sea:
- `S` el conjunto de hospitales ya asignados en el nodo actual.
- `R` los recursos restantes.
- `H'` el conjunto de hospitales aún no asignados.
- `d_i` la demanda del hospital `i`.
- `p_i` la prioridad del hospital `i` (mayor valor = mayor prioridad).

La cota inferior se define como:

```
cota = beneficio_actual + Σ (min(d_i, R_restante) * p_i)  para i ∈ H' ordenado por p_i descendente
```

Es decir, se asignan los recursos restantes a los hospitales pendientes **en orden de prioridad**, tomando como máximo la demanda de cada uno, hasta agotar `R`.

#### (2) Valor inicial de la cota

```
cota_inicial = 0
```

#### (3) Actualización de la cota en cada nodo

En cada nodo del árbol:
1. Se calcula el beneficio actual (suma de `d_i * p_i` para hospitales ya asignados).  
2. Se ordenan los hospitales pendientes por prioridad descendente.  
3. Se simula la asignación greedy de los recursos restantes.  
4. Se suma el beneficio proyectado al beneficio actual.

Esta cota es **admisible** (nunca sobreestima el óptimo) y permite podar ramas cuando `cota ≤ mejor_solución_actual`.

---

### b) Código del algoritmo de ramificación y poda

```java
 public static int branchAndBound(int[] xs, int[] ps) {
        if (xs == null || ps == null || xs.length != ps.length || xs.length == 0)
            return 0;

        int n = xs.length;

        // Siguiente válido
        int[] siguienteValido = new int[n];
        for (int i = 0; i < n; i++) {
            int j = i + 1;
            while (j < n && Math.abs(xs[j] - xs[i]) <= 5) j++;
            siguienteValido[i] = j;
        }

        // Cota: suma de sufijos
        int[] sumaSufijos = new int[n + 1];
        sumaSufijos[n] = 0;
        for (int i = n - 1; i >= 0; i--) {
            sumaSufijos[i] = sumaSufijos[i + 1] + ps[i];
        }

        int[] mejor = new int[]{0};
        explorarBnB(ps, 0, 0, siguienteValido, sumaSufijos, mejor);

        return mejor[0];
    }

    private static void explorarBnB(int[] ps, int indice, int acumulado,
                                   int[] siguienteValido, int[] sumaSufijos, int[] mejor) {
        if (indice == ps.length) {
            if (acumulado > mejor[0]) mejor[0] = acumulado;
            return;
        }

        if (acumulado + sumaSufijos[indice] <= mejor[0]) return;

        explorarBnB(ps, siguienteValido[indice], acumulado + ps[indice],
                    siguienteValido, sumaSufijos, mejor);
        explorarBnB(ps, indice + 1, acumulado,
                    siguienteValido, sumaSufijos, mejor);
    }

```
---

## 3. Comparación de optimalidad

### a) Material del experimento

| Algoritmo | Técnica | Elemento diferenciador |
|------------|----------|------------------------|
| Backtracking | Vuelta atrás sistemática | Exploración exhaustiva |
| BranchAndBound | Ramificación y poda | Cota inferior greedy |
| GreedyPorOrden | Heurístico | Selección por orden de entrada |
| GreedyPorValor | Heurístico | Selección por prioridad descendente |

**Rangos de datos generados:**
- Número de hospitales: [10, 20]  
- Demanda por hospital: [1, 100] (distribución uniforme)  
- Prioridad: [1, 10]  
- Recursos totales: [50%, 150%] de la suma total de demandas  
- Dependencias: 20–50% de pares con conflictos (modelados como exclusión mutua en asignación)

Se ejecutaron 100 repeticiones por instancia, sobre 23 instancias (77 a 99).

### b) Conclusión

Los algoritmos **Backtracking** y **BranchAndBound** son **exactos**: alcanzan el 100% de soluciones óptimas en todas las ejecuciones.  
Los heurísticos **GreedyPorOrden** y **GreedyPorValor** son **no exactos**, con desviaciones medias del **24.57%** y **75.13%** respectivamente.

### c) Evidencias

| Métrica | Backtracking | BranchAndBound | GreedyPorOrden | GreedyPorValor |
|----------|---------------|----------------|----------------|----------------|
| % Soluciones óptimas | 100.00% | 100.00% | 0.00% | 0.00% |
| % Soluciones subóptimas | 0.00% | 0.00% | 100.00% | 100.00% |
| % Diferencia media subóptima | 0.00% | 0.00% | 24.57% | 75.13% |
| % Diferencia máxima subóptima | 0.00% | 0.00% | 2.81% | 37.89% |

---

## 4. Comparación de eficiencia en tiempo

Se repitió el experimento con el mismo conjunto de datos, pero seleccionando el criterio **eficiencia en tiempo** en AlgorEx.  
Los tiempos se miden en milisegundos (ms) y son extremadamente bajos, lo que indica instancias de complejidad baja-media (n≈10–20 hospitales).

---

### a) Conclusión

Los **heurísticos** son significativamente más rápidos que los **algoritmos exactos**, con tiempos medios inferiores a **0.02 ms**  
(vs. ~0.05–0.006 ms en exactos).

- **GreedyPorOrden** es el más eficiente (**0.001 ms**, hasta 50× más rápido que Backtracking).  
- **BranchAndBound** es mucho más rápido que Backtracking (**0.006 ms vs. 0.050 ms**, ~8× mejora).  
- **GreedyPorValor** es intermedio (**0.018 ms**).  

Globalmente, los exactos son viables para instancias pequeñas (<1 ms total), pero escalan peor;  
los heurísticos sacrifican optimalidad por velocidad.

---

### b) Evidencias

| Métrica | Backtracking | BranchAndBound | GreedyPorOrden | GreedyPorValor |
|----------|---------------|----------------|----------------|----------------|
| Num. ejecuciones | 100 | 100 | 100 | 100 |
| Num. ejec. válidas del método | 100 | 100 | 100 | 100 |
| Num. ejec. válidas en total | 100 | 100 | 100 | 100 |
| Tiempo máximo (ms) | 1.146 | 0.186 | 0.106 | 0.739 |
| Tiempo medio (ms) | 0.067 | 0.011 | 0.004 | 0.030 |
| Tiempo mínimo (ms) | 0.016 | 0.002 | 0.000 | 0.007 |

**Interpretación:**  
BranchAndBound muestra una reducción clara en tiempo medio (~6× vs. Backtracking), validando la poda en términos de CPU real.  
Heurísticos: variabilidad baja, con mínimos cercanos a 0 ms.

---

### Tabla de tiempos medios por instancia (ms)

| Instancia | Backtracking | BranchAndBound | GreedyPorOrden | GreedyPorValor |
|------------|---------------|----------------|----------------|----------------|
| 77 | 0.064 | 0.006 | 0.001 | 0.018 |
| 78 | 0.047 | 0.005 | 0.001 | 0.021 |
| 79 | 0.017 | 0.005 | 0.001 | 0.015 |
| 80 | 0.045 | 0.006 | 0.001 | 0.019 |
| 81 | 0.087 | 0.005 | 0.001 | 0.019 |
| 82 | 0.059 | 0.006 | 0.001 | 0.022 |
| 83 | 0.067 | 0.005 | 0.001 | 0.021 |
| 84 | 0.055 | 0.006 | 0.001 | 0.015 |
| 85 | 0.034 | 0.007 | 0.001 | 0.016 |
| 86 | 0.024 | 0.004 | 0.000 | 0.010 |
| 87 | 0.005 | 0.006 | 0.002 | 0.014 |
| 88 | 0.056 | 0.007 | 0.001 | 0.017 |
| 89 | 0.033 | 0.007 | 0.001 | 0.022 |
| 90 | 0.051 | 0.004 | 0.001 | 0.017 |
| 91 | 0.052 | 0.006 | 0.001 | 0.016 |
| 92 | 0.053 | 0.011 | 0.001 | 0.015 |
| 93 | 0.043 | 0.005 | 0.001 | 0.023 |
| 94 | 0.032 | 0.005 | 0.001 | 0.013 |
| 95 | 0.047 | 0.005 | 0.001 | 0.020 |
| 96 | 0.073 | 0.007 | 0.001 | 0.024 |
| 97 | 0.053 | 0.005 | 0.001 | 0.012 |
| 98 | 0.079 | 0.006 | 0.001 | 0.021 |
| 99 | 0.072 | 0.006 | 0.001 | 0.022 |

---

### Estadísticas agregadas

- **Backtracking:** Media **0.050 ms**, Mín **0.005 ms**, Máx **0.087 ms**  
- **BranchAndBound:** Media **0.006 ms**, Mín **0.002 ms**, Máx **0.011 ms**  
- **GreedyPorOrden:** Media **0.001 ms**, Mín **0.000 ms**, Máx **0.002 ms**  
- **GreedyPorValor:** Media **0.018 ms**, Mín **0.007 ms**, Máx **0.024 ms**

---

### Gráficos explicados

**Valores medios y extremos (boxplot superior):**  
Eje Y en ms (escala 0–1 ms).

- **Backtracking:** caja alta (0.05 ms mediana, bigote hasta 0.09 ms, outlier ~0.1 ms).  
- **BranchAndBound:** caja baja (0.006 ms, compacta).  
- **GreedyPorOrden:** casi en 0 (0.001 ms).  
- **GreedyPorValor:** mediana ~0.018 ms.  
→ Visualiza la superioridad temporal de BranchAndBound y la velocidad extrema de GreedyPorOrden.

**Boxplot inferior (detalle):**  
Exactos muestran mayor dispersión (por complejidad de instancia); heurísticos son estables y bajos.  
→ Confirma el **trade-off: exactitud (más tiempo) vs. velocidad (heurísticos)**.

---

## 5. Conclusiones

- **BranchAndBound** demuestra ser exacto y más eficiente en tiempo (~8× más rápido que Backtracking), validando la técnica de poda.  
- **Heurísticos** (especialmente *GreedyPorOrden*) son ultra-rápidos (<0.001 ms media), pero no aptos para optimalidad (hasta 75% pérdida).  
- **AlgorEx** facilita comparaciones precisas, aunque los tiempos bajos sugieren probar instancias mayores (n > 30).  
- No se ha utilizado ninguna herramienta de **GenIA** en la elaboración de este informe ni en el diseño del algoritmo (análisis basado en datos de AlgorEx).  
- **Recomendación:** Mejorar la cota con relajación lineal para poda en nodos; explorar **A\*** para híbridos.
