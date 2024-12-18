# Informe de Laboratorio 2: Medidor de Cargas

### Integrantes

- Alber Enrique Frias Pacheco - afrias@unal.edu.co  
- Edgar Alfonso Navarro Perez - enavarro@unal.edu.co  
- Marcos Alfredo Fierro Sarria - mfierros@unal.edu.co  

---

### Índice

1. [ Descripción General](#descripción-general) 
2. [Diseño implementado](#diseño-implementado)  
3. [Simulaciones](#simulaciones)  
4. [Implementación](#implementación)  
5. [Preguntas](#preguntas)  
6. [Conclusiones](#conclusiones)  
7. [Referencias](#referencias)  

---

## Descripción General
Para la correcta implementación de la lógica del laboratorio “Medidor de Cargas”, se decidió abordar el problema general dividiéndolo en planteamientos más pequeños. Esta estrategia nos permitió estructurar el proyecto en distintos módulos independientes, facilitando así su diseño, implementación y análisis. A continuación, se explica en detalle cada una de las etapas desarrolladas y los módulos implementados.

---

## 1. Sumador Completo de 4 Bits

### Descripción del Sumador Completo
En primera instancia, definimos el funcionamiento del sumador completo, siguiendo la lógica estudiada en clase. Para extender esta funcionalidad a una operación de 4 bits, se realizó la instanciación del sumador completo 4 veces en cascada.
#### Objetivo del Sumador Completo de 4 Bits
El propósito de implementar un sumador completo de 4 bits es poder analizar el estado del banco de baterías en el cual cada una de las baterías puede tener un valor máximo de 15. Se consideró que cada batería debía evaluarse como un conjunto y, a partir de su suma binaria, determinar en qué estado se encuentra el banco de baterías en su conjunto.
## 2. Estado de las Baterías
Para el análisis del estado de las baterías, se establecieron dos puntos importantes:
1. **Análisis Individual de Baterías:**
Se identificó cuándo una batería específica tenía un voltaje de 0. En términos de hardware, esto se traduce a que la batería presenta un valor binario de “0000”. Para este caso, se diseñó un sistema de alerta auditiva utilizando un buzzer integrado en la tarjeta de desarrollo. Esta alerta se activa automáticamente al detectar que cualquiera de las dos baterías tiene un valor de “0000”.
---
2. **Módulos Implementados:**
   - **sumadorCompleto:** Implementa el sumador de 4 bits para cada batería.
   - **cargasBateria:** Se encarga de representar y gestionar el nivel de carga de cada batería.
   - **calculoCarga:** Realiza el cálculo total de la carga sumando ambas baterías y proporciona el resultado en un formato que se pueda evaluar fácilmente.
  
---
     
## 3. Evaluación de la Carga del Banco de Baterías
Además de evaluar cada batería de forma individual, el laboratorio requiere analizar el banco de baterías como un conjunto. Para ello, se implementó el módulo evaluacionCarga, que permite clasificar el estado del banco de baterías según tres categorías:
#### Carga Crítica:
Se activa cuando los tres bits más significativos son 0 (es decir, los bits 3, 2 y 1 son 0).
Esto indica que la carga del banco es extremadamente baja.
#### Carga Regular:
Se activa cuando al menos uno de los bits 3 o 2 es 1.
Indica que la batería tiene una carga intermedia y requiere monitoreo, pero no es crítica aún.
#### Carga Aceptable:
Se activa cuando el bit más significativo (bit 3) es 1.
Indica que la batería tiene un nivel de carga adecuado.
### Implementación del Comparador
Para llevar a cabo esta clasificación, utilizamos un comparador que evalúa los bits más y menos significativos de la salida del sumador. Los parámetros establecidos para las condiciones son:
- **Carga Crítica:**
Bits 3, 2 y 1= 0
- **Carga Regular:**
Bits 3 o 2  = 1
- **Carga Aceptable:**
Bit 3 = 1
---
## 4. Alertas Auditivas y Visuales
El sistema de alertas se diseñó con el fin de proporcionar notificaciones inmediatas al usuario. Se implementó un buzzer que se activa en los siguientes casos:
### 1. Batería con Voltaje Nulo:
Cuando una de las baterías presenta un valor de “0000”, se activa una alerta auditiva para indicar que una de las baterías está completamente descargada.
### 2. Carga Crítica del Banco de Baterías:
Si el banco de baterías tiene un nivel de carga equivalente al 10% o menos de su capacidad total, se activa también una alerta auditiva mediante el buzzer

---

## 5. Estructura del Proyecto y Módulo Principal
Con todos los módulos desarrollados e implementados, se procedió a diseñar un módulo principal (main) que permite instanciar y llamar a cada uno de los módulos de forma ordenada. Este módulo principal garantiza la correcta implementación y funcionamiento de la lógica completa del medidor de cargas.
### Resumen de Módulos Implementados
- sumadorCompleto – Realiza la suma binaria de 4 bits.
- cargasBateria – Representa y gestiona el nivel de carga de cada batería.
- calculoCarga – Calcula la carga total del banco de baterías.
- evaluacionCarga – Evalúa y clasifica el estado del banco de baterías según las condiciones establecidas.
---
## Problemas encontrados
- Durante el desarrollo de la práctica de implementación del “Medidor de Cargas”, se presentaron diversas dificultades, principalmente en la etapa de implementación física del diseño en la FPGA (Field-Programmable Gate Array). A pesar de que en las simulaciones realizadas mediante el testbench el funcionamiento del sistema era correcto y las salidas coincidían con las expectativas, al trasladar el diseño a la FPGA, las salidas obtenidas no correspondían con la lógica esperada ni con lo visualizado en las simulaciones. Este desfase entre las simulaciones y la implementación real generó múltiples inconvenientes que requirieron una investigación más profunda y una comprensión más detallada de las características específicas de la FPGA para poder solucionarlos de manera efectiva.
- Una de las principales fuentes de estos problemas fue la lógica negada propia de la FPGA. Esta característica implica que los valores lógicos en la FPGA pueden estar invertidos respecto a lo que se observa en las simulaciones realizadas en un entorno digital convencional. En otras palabras, mientras que en la simulación se interpreta un valor lógico “1” como un nivel alto y un “0” como un nivel bajo, en la FPGA esta interpretación puede estar invertida debido a la naturaleza del hardware y su configuración interna. Esta lógica negada influyó directamente en el diseño, especialmente en aquellos módulos donde el manejo de bits y la propagación de señales eran críticos para el correcto funcionamiento del sistema.
- Para resolver estos inconvenientes, fue necesario identificar cuidadosamente en qué partes del diseño la lógica negada estaba afectando los resultados y aplicar correcciones precisas. Por ejemplo, en la implementación del sumador completo de 4 bits, se debía considerar la correcta definición del carry-in inicial. En las simulaciones, el carry-in se establecía como “0” para el primer sumador; sin embargo, debido a la lógica negada de la FPGA, este bit necesitaba ser negado para que la propagación del acarreo entre los sumadores funcionara correctamente. Sin esta corrección, los resultados de la suma se veían afectados, ya que el acarreo no se propagaba de manera adecuada, lo que generaba salidas incorrectas.
- Además, la lógica negada también influyó en el análisis y evaluación del estado del banco de baterías. En esta etapa, el sistema debía realizar comparaciones bit a bit para determinar si las baterías se encontraban en alguno de los estados definidos: Carga Crítica, Carga Regular o Carga Aceptable. Estas comparaciones dependían de los valores binarios resultantes del cálculo de carga realizado por el sumador. Sin embargo, debido a la lógica negada de la FPGA, las comparaciones directas no funcionaban como se esperaba, ya que los bits estaban invertidos. Fue necesario aplicar negaciones a estos bits antes de realizar las comparaciones para asegurar que los resultados fueran consistentes con los criterios establecidos para cada estado de carga.
- Otro aspecto crítico fue el manejo de las señales de alerta, particularmente la señal generada para activar el buzzer en caso de detectar una batería con un voltaje de “0000”. Durante las simulaciones, esta condición se evaluaba directamente y se generaba la señal de alerta correspondiente. No obstante, en la FPGA, esta lógica se vio afectada por la inversión de los bits, lo que hacía que el buzzer no se activara correctamente cuando una de las baterías estaba descargada. Para solucionar este problema, fue necesario aplicar la negación de los bits involucrados en la comparación para que la señal de alerta se generara en el momento adecuado.
- Las dificultades encontradas en la implementación física del proyecto en la FPGA se debieron en gran medida a la lógica negada característica de este tipo de dispositivos. Esta lógica implicó que las señales y los bits debían ser negados sistemáticamente en puntos críticos del diseño para asegurar que las operaciones se llevaran a cabo correctamente. Se requirió un análisis detallado y un enfoque meticuloso para identificar cada una de las partes del diseño afectadas por esta lógica negada, lo que incluyó la negación del carry-in en el sumador completo, la negación en las comparaciones para evaluar el estado de carga y la correcta activación de las alertas mediante el buzzer.
Gracias a estas correcciones, se logró superar las dificultades y asegurar que el sistema funcionara de manera consistente tanto en las simulaciones como en la FPGA, garantizando que los resultados obtenidos fueran acordes con los objetivos planteados en la práctica.

## Diseño implementado

### Descripción

El reto consiste en diseñar e implementar un sistema de monitoreo para supervisar el nivel de carga de un banco de baterías compuesto por dos baterías. Cada batería cuenta con un sensor de tensión que entrega valores entre 0 y 15 en sistema binario, donde:

- **0 (4'b0000):** Batería completamente descargada.  
- **15 (4'b1111):** Batería completamente cargada.  

El sistema debe detectar condiciones críticas de carga y generar señales de advertencia apropiadas.  

### Diagramas

#### Módulos principales
1. Advertencia para carga crítica:  
   ![image](https://github.com/user-attachments/assets/f984c1da-3f3e-4cb6-9f5d-0cd3ea4a5659)  

2. Cálculo de carga del banco:  
   ![image](https://github.com/user-attachments/assets/9e9aa821-c171-4ade-9bd6-b6e0438df717)  

3. Unidad de evaluación de carga (Detecta en qué estado está el banco):  
   ![image](https://github.com/user-attachments/assets/614b9101-2c76-475c-bc32-56e6808f1e48)  

4. Módulo principal: Calcula el voltaje de batería, determina el estado de carga y activa la alarma:  
   ![image](https://github.com/user-attachments/assets/e257aa77-a2a0-4704-bf0a-82f518184c1f)  

5. Testbench y casos de prueba:  
   ![image](https://github.com/user-attachments/assets/5380a38a-b25f-409f-9124-10cd155226fc)  

---

## Simulaciones

### Simulación del bloque de alarma de descarga

Se realizaron simulaciones para comprobar la activación correcta de la alarma en las siguientes condiciones:

1. **Ambas baterías descargadas:**  
   ![image](https://github.com/user-attachments/assets/a14d12f8-adf0-4938-8b74-b371d3df91dd)  

2. **Baterías en estado crítico:**  
   ![image](https://github.com/user-attachments/assets/c175e8aa-ba85-4589-8266-b0a2cd386918)  

3. **Una batería en carga regular y otra en estado crítico:**  
   ![image](https://github.com/user-attachments/assets/47f511ab-874a-480d-9b03-c0bf4f392766)  

4. **Ambas baterías completamente cargadas:**  
   ![image](https://github.com/user-attachments/assets/5f8e303e-7177-4470-bfb9-9ccf9eb6b70e)  

5. **Baterías en estado regular de carga:**  
   ![image](https://github.com/user-attachments/assets/5d5152c9-a339-452c-91f4-32cf85be5f9f)  

---

## Implementación

### Proceso de implementación

Se utilizó la FPGA **Cyclone IV** en conjunto con **Quartus Prime Lite**. A continuación, se detalla el proceso experimental para la implementación de los módulos en la FPGA.  

#### Evidencias del funcionamiento

1. Ambas baterías descargadas:  
   La suma de carga de las baterías es cero. Las señales de carga aceptable y regular están apagadas, mientras que las de carga crítica y Alerta están activadas.  

2. Ambas baterías en estado crítico:  
   Una batería tiene un voltaje bajo, mientras que la otra está en un nivel crítico. Las señales responden adecuadamente.

---

## Preguntas

1. ¿Qué desafíos pueden surgir al implementar en *hardware* un diseño que funcionaba correctamente en simulación?  

Pueden surgir distintos incovenientes, como por ejemplo, que se tenga ruido por la degradación de señales, lo que afecta el funcionamiento del sistema. Por otro lado también se deben tener en cuenta desfases en el reloj, debido a que en la simulación estos suelen ser ideales, pero en hardware, los relojes mal configurados generan errores en comunicaciones o registros, estos problemas se pueden ver agravados por los tiempos de propagación, debido a que en hardware las señales pueden tardar más en propagarse en rutas físicas.

2. Describa el enfoque estructural y comportamental en el contexto de electrónica digital y cómo los implementó en el reto. ¿Qué hace Quartus con cada uno?  

En este caso, el enfoque estructural nos permite  modelar el circuito mediante sumadores, comparadores, multiplexores, etc. En este laboratorio se diseñó módulos que permitieran las suma de tensiones entre las baterías, así como la evaluación de carga del banco.

Por otro lado el enfoque comportamental nos invita a ver el sistemas desde un nivel abstracto, mediante la descripción de sus entradas y salidas, lo que hace que se pueda modelar la reacción del sistema a las entradas de interés. Para el reto se usó para definir arbitrariamente los estados de carga y las alarmas.


3. ¿Cómo afecta el diseño del sumador y de comparadores al uso de recursos en la FPGA (LUTs, FFs, BRAMs, etc.)? Muestren el uso de recursos de su diseño.  

Implmentar sumadores suele consumir LUTs porque implmenta lógica combinacional para realizar operaciones aritméticas. Por otro lado, los LUTs también son usados por los comparadores para evaluar condiciones y determinar estados. Finalmente, incrementar los bits, hará que requiramos repetir las operaciones más veces, así como procesar más entradas, lo que se verá directamente reflejado en la cantidad de LUTs requeridos. Los Flip-flops se usan solo si los resultados se almacenan o sincronizan con el reloj

Teniendo en cuenta que se usó un sumador de 4 bits, y sabiendo que cada bit requiere un LUT para su suma y otro para su acarreo, se necesitarán 8 LUTs.



4. ¿Qué impacto tiene aumentar el número de bits de la lectura de cada batería? ¿Qué impacto tiene aumentar el número de baterías del banco?  

Aumentar los bits de cada batería haría que el consumo de recursos LUTs y FFs aumenten, debido a que las operaciones como las sumas y las comparaciones necesitarán más entradas y salidas. 

Por otro lado, aumentar el número de baterías hará que aumente la cantidad de veces que tenemos que repetir las operaciones. En caso de que se requiera para escalabilidad, puede ser necesario redefinir los criterios para los distintos estados de carga del banco.

5. Describa las diferencias entre los tipos de dato `wire` y `reg` en Verilog y compare ambos con el tipo de dato `logic` en System Verilog.  

- wire: Es la representación de los cables, que a su vez transportan señales que son asignadas mediante la lógica combinacional.

- reg: Este término se usa para definir variables, generalmente se pueden usar en procesos de lógica secuancial o dentro de bloques "always".

- logic:
  Es un un tipo de dato de System Verilog que brinda las funcionalidades de "wire" y "reg". Facilita tareas de diseño dado que no se debe distinguir activamente entre la lógica combinacional y la secuencial.

6. Únicamente con lo que se vio en clase, describa cómo se usó el bloque `always`. Enfoque su respuesta hacia la implementación de lógica combinacional.  

---

## Conclusiones

- La implmentación en FPGA no suele ser trivial en la mayoría de casos. Se deben tener en cuanta por ejemplo, cosas como la lógica negada al implmentar en la FPGA.

---

## Referencias

1. DianaNatali. Tutorial de Implementación en la FPGA Cyclone IV usando Quartus Prime Lite.  
   Disponible en: [https://github.com/digital-electronics-UNAL/2024-2/blob/main/labs/lab02/proyectoQuartus.md](https://github.com/digital-electronics-UNAL/2024-2/blob/main/labs/lab02/proyectoQuartus.md)  

2. DianaNatali. Ejemplos sencillos de descripciones en HDL.  
   Disponible en: [https://github.com/digital-electronics-UNAL/2024-2/blob/main/labs/lab02/ejemplos_hdl.md](https://github.com/digital-electronics-UNAL/2024-2/blob/main/labs/lab02/ejemplos_hdl.md)  

3. DianaNatali. Sumador de 1 bit y sumador de 4 bits.  
   Disponible en: [https://github.com/digital-electronics-UNAL/2024-2/blob/main/labs/lab02/sumador.md](https://github.com/digital-electronics-UNAL/2024-2/blob/main/labs/lab02/sumador.md)  
