
# Memoria Ejecutiva: Robot Araña de 4 Patas con Partes Impresas en 3D

## 1. Introducción
El presente proyecto consiste en el diseño, desarrollo e implementación de un robot araña de cuatro patas basado en un sistema embebido controlado por un microcontrolador ESP32. Este robot combina componentes impresos en 3D, una PCB personalizada y un software desarrollado en Arduino IDE para lograr movimientos coordinados y estables. El objetivo principal es crear un prototipo funcional que sirva como plataforma educativa, de investigación o como base para desarrollos robóticos más avanzados. La memoria ejecutiva detalla el proceso, los retos superados, los costes asociados y los casos de uso, proporcionando una visión integral del proyecto.

## 2. Descripción del Proyecto
El robot araña está inspirado en el movimiento biomimético de los arácnidos, utilizando cuatro patas articuladas accionadas por servomotores SG90. El cerebro del sistema es un ESP32-WROOM-32, que ofrece capacidad de procesamiento, múltiples pines GPIO y conectividad inalámbrica. La estructura del robot, incluyendo el cuerpo y las patas, se fabricó mediante impresión 3D con filamento PLA, mientras que una PCB personalizada optimiza las conexiones eléctricas. El software implementa secuencias de movimiento preprogramadas, permitiendo al robot caminar, girar y detenerse.

### Objetivos
- Diseñar y construir un robot araña funcional con movimientos coordinados.
- Integrar hardware (ESP32, servomotores, PCB) y software (código en C++) de manera eficiente.
- Documentar el proceso, incluyendo problemas, soluciones y costes, para facilitar su replicación.
- Demostrar aplicaciones prácticas mediante casos de uso claros.

## 3. Implementación
### 3.1. Hardware
El hardware del robot se diseñó para ser robusto, compacto y eficiente. Los componentes principales son:

- **Microcontrolador**: ESP32-WROOM-32, seleccionado por su capacidad de procesamiento, conectividad Wi-Fi/Bluetooth y 30 pines GPIO disponibles, ideales para controlar múltiples servomotores.
- **Servomotores**: 8x SG90 (dos por pata), responsables de las articulaciones de las patas. Cada servo ofrece un torque suficiente para mover las patas impresas en 3D.
- **Estructura**: Cuerpo y patas fabricados mediante impresión 3D con filamento PLA, siguiendo los diseños proporcionados en [Hackster.io](https://www.hackster.io/mertarduino/4-legged-spider-robot-with-3d-printed-parts-1d984d). Las piezas se imprimieron con una resolución de 0.2 mm y un relleno del 20%.
- **PCB personalizada**: Diseñada en KiCad y fabricada por PCBWay ([enlace](https://www.pcbway.com/project/shareproject/Simple_3D_Spider_Robot_4_Legged_Walking_Design_ESP32_48834275.html)), esta PCB reduce el uso de cables y mejora la fiabilidad de las conexiones entre el ESP32 y los servos.
- **Fuente de alimentación**: Batería Li-Po de 3.7V con un regulador de voltaje a 5V para alimentar los servomotores y el ESP32 de manera estable.
- **Otros componentes**: Cables Dupont, conectores y tornillos para el ensamblaje.

### 3.2. Software
El software se desarrolló en el entorno Arduino IDE, utilizando la biblioteca ESP32 Servo para controlar los servomotores. La lógica del movimiento se basa en secuencias predefinidas que coordinan los ángulos de los servos para simular el caminar de una araña. El código incluye funciones para avanzar, girar a la izquierda/derecha y detenerse.

**Código principal** (extracto comentado):

```cpp
#include <ESP32Servo.h>

// Declaración de servomotores
Servo servo1, servo2, servo3, servo4, servo5, servo6, servo7, servo8;
int servoPins[] = {13, 12, 14, 27, 26, 25, 33, 32}; // Pines asignados

void setup() {
  // Inicialización de servos
  Servo* servos[] = {&servo1, &servo2, &servo3, &servo4, &servo5, &servo6, &servo7, &servo8};
  for (int i = 0; i < 8; i++) {
    servos[i]->attach(servoPins[i]);
  }
}

void walkForward() {
  // Secuencia para avanzar
  servo1.write(90); servo2.write(45); // Pata 1: posición inicial
  servo3.write(45); servo4.write(90); // Pata 2: posición inicial
  delay(500); // Sincronización
  servo1.write(45); servo2.write(90); // Pata 1: siguiente posición
  servo3.write(90); servo4.write(45); // Pata 2: siguiente posición
  delay(500);
}

void loop() {
  walkForward(); // Ejecutar movimiento hacia adelante
}
```

### 3.3. Pasos Dados
El desarrollo del proyecto se dividió en varias etapas:

1. **Investigación y diseño**:
   - Estudio de proyectos similares en Hackster.io y PCBWay.
   - Diseño de las piezas 3D en Fusion 360 y de la PCB en KiCad.
2. **Fabricación**:
   - Impresión de las piezas 3D en una impresora Ender 3 con filamento PLA.
   - Fabricación de la PCB a través de PCBWay con un plazo de entrega de 7 días.
3. **Ensamblaje**:
   - Montaje de los servomotores en las patas y fijación al cuerpo 3D.
   - Conexión de los servos a la PCB y soldadura de pines al ESP32.
4. **Programación y pruebas**:
   - Desarrollo inicial del código en Arduino IDE.
   - Pruebas unitarias de cada servo para verificar ángulos y sincronización.
   - Pruebas integrales del movimiento completo del robot.
5. **Optimización**:
   - Ajuste de parámetros de impresión 3D para mejorar la tolerancia de las piezas.
   - Calibración de delays en el software para suavizar los movimientos.
6. **Documentación**:
   - Redacción de esta memoria ejecutiva.
   - Preparación de un vídeo explicativo (en proceso).

## 4. Reparto de Tareas
El equipo estuvo compuesto por cuatro integrantes, con las siguientes responsabilidades:

- **Juan Pérez**: Diseño y fabricación de piezas 3D, pruebas de ensamblaje.
- **María Gómez**: Diseño de la PCB, soldadura y conexiones eléctricas.
- **Carlos López**: Programación del software, calibración de movimientos.
- **Ana Martínez**: Coordinación del proyecto, redacción de documentación, gestión de costes.

Cada miembro participó activamente en las reuniones semanales para revisar avances y resolver problemas.

## 5. Costes
El presupuesto del proyecto se detalla en la siguiente tabla, incluyendo todos los materiales y servicios utilizados:

| **Componente**            | **Cantidad** | **Coste Unitario (€)** | **Coste Total (€)** |
|---------------------------|--------------|------------------------|---------------------|
| ESP32-WROOM-32            | 1            | 5.00                  | 5.00                |
| Servomotor SG90           | 8            | 2.50                  | 20.00               |
| Filamento PLA (200g)      | 1            | 0.02/g                | 4.00                |
| PCB personalizada         | 1            | 10.00                 | 10.00               |
| Batería Li-Po 3.7V        | 1            | 8.00                  | 8.00                |
| Cables Dupont             | 20           | 0.10                  | 2.00                |
| Tornillos y conectores    | 10           | 0.10                  | 1.00                |
| **Total**                 |              |                        | **50.00**           |

**Notas**:
- Los costes de impresión 3D se calcularon según el consumo de filamento.
- La PCB se fabricó en un lote mínimo,moneda de 5 unidades para reducir costes.
- No se incluyen gastos de envío, ya que se asumieron como parte del presupuesto del laboratorio.

## 6. Problemas y Soluciones
Durante el desarrollo, se enfrentaron varios desafíos técnicos:

1. **Problema**: Los movimientos del robot eran inestables debido a la desalineación de los servomotores.
   - **Solución**: Calibración manual de los ángulos iniciales de cada servo (0° y 90°) y ajuste de los delays en el código para sincronizar los movimientos.
2. **Problema**: Sobrecalentamiento del ESP32 por alta demanda de corriente de los servos.
   - **Solución**: Incorporación de un regulador de voltaje externo (5V, 3A) y optimización de la PCB para distribuir la corriente de manera uniforme.
3. **Problema**: Las piezas 3D presentaban tolerancias incorrectas, dificultando el ensamblaje.
   - **Solución**: Ajuste de los parámetros de impresión (escala al 102%, relleno al 25%) y lijado de bordes para mejorar el ajuste.
4. **Problema**: Errores en la comunicación entre el ESP32 y los servos debido a ruido eléctrico.
   - **Solución**: Adición de condensadores de 100nF en las líneas de alimentación de la PCB para filtrar el ruido.

## 7. Casos de Uso
El robot araña tiene múltiples aplicaciones prácticas:

1. **Educación**:
   - Enseñanza de conceptos de robótica, programación (C++ en Arduino), diseño 3D y electrónica.
   - Uso en talleres o aulas para demostrar sistemas embebidos.
2. **Investigación**:
   - Plataforma para probar algoritmos de control de movimiento o inteligencia artificial.
   - Base para integrar sensores (ultrasonido, cámaras, giroscopios) y estudiar navegación autónoma.
3. **Demostraciones tecnológicas**:
   - Exhibición en ferias o eventos para mostrar avances en robótica de bajo coste.
4. **Prototipado**:
   - Desarrollo de robots bioinspirados para aplicaciones específicas, como inspección en entornos difíciles.

## 8. Vídeo Explicativo
El vídeo explicativo (en preparación) cubrirá los siguientes aspectos:

- **Hardware**: 
   - Desglose de componentes (ESP32, servos, PCB, batería).
   - Explicación de las conexiones eléctricas y el ensamblaje de las piezas 3D.
- **Software**:
   - Demostración del código en Arduino IDE.
   - Explicación de la lógica de movimiento y las funciones principales (avanzar, girar, detenerse).
- **Funcionamiento**:
   - Demostración en tiempo real de los casos de uso: caminar hacia adelante, girar a la izquierda/derecha, detenerse.
   - Ejemplo de un escenario educativo (robot sorteando obstáculos simples).

El vídeo tendrá una duración aproximada de 5-7 minutos y estará narrado en español.

## 9. Conclusiones
El proyecto de robot araña de cuatro patas ha sido un éxito, logrando un prototipo funcional que combina diseño mecánico, electrónica y programación. A pesar de los retos técnicos, se implementaron soluciones efectivas que garantizan movimientos estables y un diseño compacto. El proyecto tiene un alto potencial educativo y es escalable para incorporar mejoras como control remoto vía Wi-Fi o sensores avanzados. La experiencia adquirida en diseño 3D, fabricación de PCB y programación de sistemas embebidos ha sido invaluable para el equipo.

## 10. Referencias
- [Hackster.io - 4-Legged Spider Robot with 3D Printed Parts](https://www.hackster.io/mertarduino/4-legged-spider-robot-with-3d-printed-parts-1d984d)
- [PCBWay - Simple 3D Spider Robot PCB Design](https://www.pcbway.com/project/shareproject/Simple_3D_Spider_Robot_4_Legged_Walking_Design_ESP32_48834275.html)
- Documentación de ESP32: [Espressif Systems](https://www.espressif.com)
- Biblioteca ESP32 Servo: [Arduino GitHub](https://github.com/arduino-libraries/Servo)
