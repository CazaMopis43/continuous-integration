# Informe de Experimento con AlgorEx  
**Algoritmos Avanzados**  


**Marc Burgos Ucendo**
  

---

## Problema de optimización seleccionado  
El problema que he elegido es el problema de la mochila 0-1 
Es uno de los problemas más famosos cuando hablamos de optimizción con combinación, básicamente se nos plantea un problema en el que tenemos una mochila con una capacidad **X** y unos objetos con un peso  y un valor determinado cada uno, el objetivo es máximizar el valor que contiene la mochila


---

## Rangos de valores usados para la generación aleatoria de los datos de entrada  
Podemos observar que se trabaja con los siguientes datos  
- Los pesos ( representados por ps)  y beneficios ( representados por bs) se han generado con valores enteros entre 1 y 20.
- Por otra parte la capacidad de la mochila ( representada por c) toma valores que fluctuan entre 4 y 20 , esto hace que varíe el umbral de dificultad.
Todos estos datos se pueden ver en la primera captura adjuntada al final del informe
---

## Conclusión sobre métodos exactos o inexactos según los resultados  
A partir de la comparación de métodos:  
- mochila01_PD  y mochila01_back  han mostrado resultados exactos en todas las ejecuciones.  
- mochila01_aprox  coincide con los resultados óptimos en varios casos, aunque en algunos difiere en beneficio.  
- mochila01_voraz  es el que presenta más casos subóptimos. Según la gráfica, en torno al 20% de las ejecuciones no alcanza el valor óptimo.  
La gráfica se puede encontrar en la segunda imagen al final del informe
Por lo tanto podemos concluir que :
- Métodos exactos son PD y Backtracking , mientras que nos queda como inexacto el Aproximado y Voraz (este último el menos preciso).  

---

## d) Evidencias  
- En la tabla de resultados numéricos, se comparan los beneficios obtenidos por cada algoritmo en 10 ejecuciones distintas. Se observan diferencias entre el valor óptimo (PD/Backtracking) y los algoritmos Aproximado/Voraz.  
- En el resumen gráfico, el histograma de resultados muestra que el algoritmo voraz consigue soluciones óptimas en un 80% de los casos, pero en un 20% cae en subóptimos.  
- El diagrama de caja confirma que los valores obtenidos por los algoritmos inexactos tienden a estar próximos al óptimo, aunque con desviaciones.  

---

## e) Incidencias  
Hemos tenido problemas a la hora de iniciar con AlgorEx ya que el JDK que teníamos intalado(22) no era compatible y no permitía el correcto funcionamiento ya que se necesitaba un JDK 19 o inferior.

---
