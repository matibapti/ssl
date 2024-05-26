# 💻 TP 2 : Fase de la Traduccion y Errores <img src="https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExZHFlemVwbm42ZmQxdmk0NjN5cXI5ZHhtcmV6ZW02czNrOGp2cm9peSZlcD12MV9zdGlja2Vyc19zZWFyY2gmY3Q9cw/QTfX9Ejfra3ZmNxh6B/giphy.gif" width="100" />

## Indice

- [1- Preprocesador](#preprocesador)
    - [Hello2.c](#hello2.c)
    - [Hello3.c](#hello3.c)
- [2- Compilación](#compilacion)
    - [Hello3.s](#hello3.s)
    - [Hello4.c](#hello4.c)
- [3- Vinculación](#vinculacion)
    - [Hello4.exe](#hello4.exe)
    - [Hello5.c](#hello5.c)
- [4- Corrección de Bug](#correcciondebug)
    - [Hello6.c](#hello6.c)
- [5- Remoción de prototipo](#remociondeprototipo)
    - [Hello7.c](#hello7.c)
- [6- Compilación Separada: Contratos y Módulos](#compilacion)
    - [Hello8.c](#hello8.c)
    - [studio1.c](#studio1.c)
    - [Hello9.c](#hello9.c)
    - [studio2.c](#studio2.c)
- [Crédito Extra](#extra)

# Secuencia de Pasos

## 1- Preprocesador

a) Se escribió hello2.c, que es una variante de hello.c.

b)Analizando el archivo preprocesado se ve que hay varias líneas que hacen referencia a archivos header (.h) siendo estas las dependencias del sitio, incluido en hello2.c, el preprocesador buscará todas las dependencias necesarias del hello2.c indicándolas en hello2.i, siendo este el archivo que pasará a compilarse. Entre las dependencias podemos encontrar tanto el código de usuario como las líneas generadas por el preprocesador  para manejar dependencias y rastrear la precedencia del código.
En este punto se enfatiza en el manejo de dependencias antes de la compilacion, por lo que no surgen errores de compilación en esta fase.

c)Se escribe el archivo hello3.c.

d)Se declara el prototipo de print f: "int printf(const char * restrict s, ...);"

int : Se debe a que la documentación del estándar C y manuales indican que printf devuelve el número de caracteres impresos (excluyendo el carácter nulo de terminación) o un valor negativo si ocurre un error.
printf : Es una función estándar en C que se utiliza para imprimir una cadena de caracteres formateada a la salida estándar.
char * : Indica que "s" es un puntero a un carácter, lo que significa que debe apuntar a una cadena de caracteres.
const : indica que el puntero "s" no debe ser usado para modificar el contenido de la cadena a la que apunta.
restrict : Indica que el puntero  es el único medio por el cual se accederá al objeto que apunta durante la vida útil del puntero. Esto permite al compilador realizar optimizaciones en cuanto al llamado de la función. En este contexto, garantiza que la cadena a la que apuntas no se superpone con ningún otro argumento de printf.
(...) : Indica que printf es una función que acepta un número variable de argumentos, permitiendo así imprimir diferentes tipos y cantidades de datos.

e) Procedimos a preprocesar hello3.c a hello3.i. A simple vista vemos una gran diferencia entre la cantidad de líneas que contiene cada archivo, esto se debe a que hello2.c incluyo "stdio.h", a diferencia de hello3.c que no lo hizo, por lo que todas las dependencias relacionadas con al stdio, inclusiones y definiciones no se tuvieron en cuenta al preprocesar. Se puede observar que en hello3.i solo se dejó el código escrito en hello3.c y las líneas generadas por el preprocesador  para manejar dependencias y rastrear la precedencia del código.

## 2_ Compilación

a)Se procede a compilar el resultado y generar hello3.s, como era de esperarse, el compilador arroja errores de compilación:
La función main no termina con "}", nos indica un error de estructura incompleta en nuestro código.
Asimismo, se indica un posible error tipográfico, dado que la función prontf no fue declarada, sugiere que quizás quisimos utilizar otra función (printf).

b)Se renombró hello3.c a hello4.c y se le agregó la llave que faltaba, por lo que hello4.c logró compilar.

c)

d) Se compila y ensambla hello4.c, y el compilador arroja un archivo Objeto, el cual contiene código máquina.

## 3_ Vinculación

a)Se intentó vincular hello4.c con la biblioteca estándar pero, como era de esperarse, no resuelve la referencia a 'prontf', informando el error: "undefined reference to `prontf'.

b)Se corrigió el error que impedía la vinculación con la biblioteca estándar en hello5.c, este archivo se compiló y vinculó obteniendo así el ejecutable hello5.exe

c)Se procede a ejecutar hello5.exe y se detecta un bug dado que no retorna lo esperado, incluso retorna valores distintos cada vez que el mismo es ejecutado.

## 4_ Corrección del Bug

a) Se procede a corregir el código incorporando la variable "i" en printf en un nuevo archivo hello6.c, luego de compilar y ejecutar se observa que funciona como se esperaba.

## 5_ Remoción de prototipo

a)Se removió la declaración de 'printf' en hello6.c, y se renombró el archivo a hello7.c. El programa sigue funcionando como se esperaba.

b)Esto se debe principalmente a que dentro del programa gcc hace inferencias basándose en el nombre de la función "printf" y hace que nuestro código se vincule directamente con la biblioteca estándar, ya que asume que la función printf es la que se encuentra en dicha biblioteca.

(ESTO FUE LO QUE SE HABLO EN CLASE)

## 6_Compilación Separada: Contratos y Módulos

a)Se escribe studio1.c, la cual tiene la definición de prontf y dicha función es usada en hello8.c

b)Se generó el archivo hello8.exe pasando a gcc los dos archivos fuente por medio del siguiente comando:
gcc studio1.c hello8.c -o hello8
Luego se corrió el ejecutable y el mismo funcionó como se esperaba.

c)Si eliminamos o agregamos argumentos a la invocación prontf en hello8.c; seguimos sin tener errores en la etapa de compilación dado que en hello8.c no se le indica al compilador cómo debe usarse prontf.
Por otro lado, a la hora de generar el ejecutable, este no funciona como se espera, dado que en la etapa de vinculación, al combinar los archivos objetos para generar el ejecutable, las referencias a funciones no coincidirán.

d)Se escribieron los archivos; studio.h (contrato), hello9.c (cliente que incluye el contrato) y studio2.c (proveedor que incluye el contrato). En este caso, agregar o quitar argumentos a la invocación de la función prontf devuelve errores de compilación dado que no se está cumpliendo con el prototipo o contrato declarado en hello9.c por medio del include.

Al incluir el contrato tanto en los clientes como en el proveedor, aseguramos que ambas partes tengan una visión consistente de las funciones y tipos disponibles.
Permite que el compilador pueda verificar que las funciones están siendo llamadas con los argumentos correctos y que las definiciones coinciden con las declaraciones.
Todo esto resulta en un código más robusto pero menos propenso a errores.

## Crédito Extra
