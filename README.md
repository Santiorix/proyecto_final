# Proyecto final Ironhack -- MRP

![Ironhack logo](https://i.imgur.com/1QgrNNw.png)

## Objetivo
 El objetivo del proyecto es crear un herramienta para la unidad de compras de una empresa cualquiera. La herramienta envía un correo automático a los usuarios deseados en el momento en que un material del inventario alcanza el punto de pedido. 
 
 ![Evolucion_stock](https://www.pdcahome.com/wp-content/uploads/2013/10/evolucion-stock.jpg)
 
 ## Bases de datos utilizadas
 Para llevar a cabo el proyecto se han utilizado ficheros anonimizados de una empresa real. Estos ficheros son:

 * **Fichero de de movimientos de existencias**: Son 3 fiheros compuestos con los movimientos de entrada y salida de mercancías para los años 2017, 2018 y 2019
 * **Fichero de stock actual**: Este fihero indica las existencias reales ha día de hoy por material.
 * **Fichero Maestro de materiales**: En este fichero se indica el stock de seguridad y los lead times por material.
 * **Fichero ltyss**: Fichero generado tras los cálculos y la limpieza con los datos de stock de seguridad y lead times medios por materia prima
 * **Fichero de consumos actuales**: Fichero generado que contiene el consumo diario calculado.
 * **Fichero consolidado**: Fichero limpio con el id del material, el consumo diario, el lead time, el stock de seguridad y el punto de pedido calculado.
 
 ## Metodología
 
 Se ha trabajado con jupyter noteboook para limpiar, transformar, calcular y generar los ficheros necesarios para construir la herramienta.
 
 **Cálculo del consumo diario (Jupyter notebook: Stocks)**
 En un primer lugar se ha limpiado el fichero de movimientos de existencias para llevar a cabo el tratamiento de datos. Este fichero contiene el día y la cantidad de entrada y salida de un material. Con estos datos se ha calculado el consumo medio diario por materia prima. Como output obtenemos el fichero de consumos actuales por material.

**Obtención del plazo de entrega y el stock de seguridad (Jupyter notebook: pp_ss)**
Posteriormente, se ha trabajado el fichero de maestro de materiales del cual obtenemos el lead time por materia prima (Por problemas de tiempo, no se ha hecho una media de los lead times reales del fichero sacando la media por proveedor). A su vez, obtenemos el stock de seguridad que la empresa tiene actualmente calculado. Como output se genera el fichero ltyss.

**Cálculo del punto de pedido (Jupyter notebook: consolidado)**
En este notebook creamos un consolidado sobre el que calculamos el punto de pedido, con la fórmula sencilla:
PP = Stock de seguridad + (Consumo diario x Lead time). 
Como output se obtiene el fichero consolidado. 

**Creación de la herramienta (Jupyter notebook: herramienta_correo)**
En este notebook se carga el fichero consolidado y el fichero de stock actual. Se crea una función que compare si el stock actual es inferior o igual al punto de pedido, y en ese caso, envía un correo al técnico de compras indicando los materiales que necesitan una orden de compra. 

## Visualización

Por último, se ha generado un power bi que bebe de los ficheros consolidado y movimientos de existencias, el cual muestra la evolución de existencias por material desde 2017 a 2019. La visualización contiene un filtro de selección para los materiales. En este cuadro de mando, se observa claramente que el stock de seguridad está mal dimensionado y que las compras históricas de los materiales se han realizado mucho antes de llegar al punto de pedido, por lo que al tener un stock sobredimensionado, se incurre en un alto gasto financiero. No se ha recaluclado el stock de seguridad por motivos de tiempo. 

## Librerias utilizadas

+ Pandas
+ Numpy
+ smtplib
+ pretty_html_table
+ email.mime.text
+ email.mime.multipart