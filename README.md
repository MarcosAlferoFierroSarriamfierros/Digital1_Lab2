# Digital1_Lab2
# Informe de Laboratorio 2: Reto 1 - Medidor de carga

INTEGRANTES:

Alber Enrique Frias Pacheco - afrias@unal.edu.co

Edgar Alfonso Navarro Perez - enavarro@unal.edu.co

Marcos Alfredo Fierro Sarria - mfierros@unal.edu.co

## Índice
1. [Objetivos de Aprendizaje](#objetivos-de-aprendizaje)
2. [Planteamiento del Problema](#planteamiento-del-problema)
3. [Requisitos Funcionales](#requisitos-funcionales)
4. [Descripción de la Solución](#descripción-de-la-solución)
5. [Simulación y Evidencias](#simulación-y-evidencias)
6. [Implementación en FPGA](#implementación-en-fpga)
7. [Conclusiones](#conclusiones)
8. [Referencias](#referencias)

---

## Objetivos de Aprendizaje

- Implmentar principios de lógica combinacional en hardware.
- Validar el funcionamiento en hardware a partir de los requerimientos.
- Diseñar, crear e implmentar módulos escritos en VHDL.



---

## Planteamiento del Problema

El reto consiste en diseñar e implementar un sistema de monitoreo para supervisar el nivel de carga de un banco de baterías compuesto por dos baterías. Cada batería cuenta con un sensor de tensión que entrega valores entre 0 y 15 en sistema binario, donde:

- **0 (4'b0000):** Batería completamente descargada.
- **15 (4'b1111):** Batería completamente cargada.

El sistema debe detectar condiciones críticas de carga y generar señales de advertencia apropiadas.

---

## Requisitos Funcionales
Con base al problema podemos enumerar los siguientes requisitos:

1. **Detección de baterías descargadas**:  
   - Se necesita generar una señal de alerta si una de las baterías está completamente descargada, es decir, si tiene un valor de: **0 (4'b0000)**.

2. **Aviso de carga crítica**:  
   - Se activa una señal de advertencia si la carga total es igual o menor al **10%** de la carga máxima del banco 30 ** (4'b 1 1110) **.

3. **Indicadores de otros niveles de carga**:  
   - Se necesitan avisos para otros niveles de carga, los cuales se han definido como: **aceptable**, **regular** y **crítico**.

4. **Diseño modular y escalable**:  
   - Se necesita que la solución sea escalable por lo que tendrá que ser hecha a partir de módulos.

---

## Descripción de la Solución

### Diseño del Sistema
1. **Especificación del Problema**  
   - Definición de los estados de carga para cada batería:
     1. Aceptable:
     2. Regular:
     3. Crítico:

   - Identificación de los niveles de carga crítica y aceptable.  
    1. Aceptable:
    2. Crítica:
       
2. **Arquitectura Modular**
   En el siguiente apartado se consignan los módulos creados para la solución usando Verilog y VSCode
   
   - **Módulos principales**:  
     - Advertencia para carga crítica:
    
       ![image](https://github.com/user-attachments/assets/f984c1da-3f3e-4cb6-9f5d-0cd3ea4a5659)

       
     - Cálculo de carga del banco:
    
       ![image](https://github.com/user-attachments/assets/9e9aa821-c171-4ade-9bd6-b6e0438df717)

       
     - Unidad de evaluación de Carga (Detecta en que estado está el banco):
    
       ![image](https://github.com/user-attachments/assets/614b9101-2c76-475c-bc32-56e6808f1e48)

       
     - Módulo principal: Se encarga de calcular el voltaje de batería, determinar el estado de carga y de activar la alarma si así se requiere:
    
       ![image](https://github.com/user-attachments/assets/e257aa77-a2a0-4704-bf0a-82f518184c1f)

    
     - Testbench y casos de prueba: En el testbench se instancia el módulo principal. Se establecen casos de prueba que permitan observar la activación o no de la alarma en condiciones de carga crítica, regular y aceptable.
       
![image](https://github.com/user-attachments/assets/5380a38a-b25f-409f-9124-10cd155226fc)


---

## Simulación y Evidencias

1. **Simulación del Diseño**
2. 
     Una vez ejecutado el testbench, podemos crear el archivo **.vcd**:
   
![image](https://github.com/user-attachments/assets/c2ff4bd3-9413-443d-8fc8-aafdca3dbccd)

   - Validación de resultados: Se muestran los resultados en GTKwave:

  
          
---

## Implementación en FPGA

1. **Proceso de Implementación**
   Para la implmentación física se usó una FPGA, exactamnte la **Cyclone IV** en conjunto con con **Quartus IDE**, el cual será el software de de desarrollo.
   


   A continuación se describe el proceso experimental para la implmentación de los módulos en la FPGA:

3. **Evidencias del Funcionamiento**  
   - Finalmente se muestra la FPGA con los módulos implmentados y evidencia fotográfica de su correcto funcionamiento:
   a. Ambas baterías descargadas: Fíjese en el cursor rojo.


Se observa que la suma de carga de las baterías es cero, además vemos que las señales de carga aceptable y regular están apagadas, mientras que la de carga crítica y Alerta están activadas-

b. Ambas baterías en estado crítico:



En este caso, una batería tiene un

---

## Conclusiones



---

## Referencias

1. DianaNatali. Tutorial de Implementación en la FPGA Cyclone IV usando Quartus Prime lite. Disponible en: https://github.com/digital-electronics-UNAL/2024-2/blob/main/labs/lab02/proyectoQuartus.md

2. DianaNatali. Ejemplos sencillos de descripciones en HDL. Disponible en:
https://github.com/digital-electronics-UNAL/2024-2/blob/main/labs/lab02/ejemplos_hdl.md

3. DianaNatali. Sumador de 1 bit y sumador de 4 bits. Disponible en: https://github.com/digital-electronics-UNAL/2024-2/blob/main/labs/lab02/sumador.md
