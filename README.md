# Lab02 - Medidor de carga

## Integrantes

- Alber Enrique Frias Pacheco - afrias@unal.edu.co  
- Edgar Alfonso Navarro Perez - enavarro@unal.edu.co  
- Marcos Alfredo Fierro Sarria - mfierros@unal.edu.co  

---

## Informe

### Índice

1. [Diseño implementado](#diseño-implementado)  
2. [Simulaciones](#simulaciones)  
3. [Implementación](#implementación)  
4. [Preguntas](#preguntas)  
5. [Conclusiones](#conclusiones)  
6. [Referencias](#referencias)  

---

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
