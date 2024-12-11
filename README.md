DAR AL LÁPIZ O A "CODE" PARA MEJOR LECTURA

Estructura del código:
.data
    # Aquí se va a guardar la información en la memoria del procesador; las strings, los booleanos, los floats, los enteros.
.text
.globl main
    # Aquí es dónde se va a ejecutar o imprimir la información, las funciones, mostrar en pantlla.

main: # Etiqueta main, para estrcuturar mejor el código y sea más legible.
resgistros_de_enteros: $a0, $t0
resgistros_de_flotantes: $f0, $f12

fin: li $v0, 10  # Esta etiqueta marca el fin de toda la "función" main, es como si fuera un return en python.
     syscall

Aqui lo dejo limpio para poder copiar y pegar siempre que vayamos a trabajar:  (recuerda que a mips o qTspim no le importa los espacios, lo hacemos asi para mayor legibilidad)

.data

.text
.globl main
main:
fin: li $v0, 10
     syscall


Mips controla cuatro tipo de variables:
      • Enteros (int)        # Declarar enteros en memoria (.data) --> entero:  .word 10
      • Flotantes (float)    # Declarar flotantes en memoria --------> flotante: .float 10.5
      • Double               # Declarar doubles en memoria ----------> double:   .double 8.34
      • Strings              # Declarar strings en memoria ----------> palabra:  .asciiz "Hola mundo"


Cargar las variables en el main:
      • Enteros (int)        # Cargar enteros en main (.text) --> li $v0, 1
        enteros                                                   lw $a0, entero
        enteros                                                   syscall
      • Flotantes (float)    # Declarar flotantes en memoria ---> li $v0, 2
        flotantes                                                 lwc1 $f12, flotante 
        flotantes                                                 syscall
      • Double               # Declarar doubles en memoria -----> li $v0, 3
        doubles                                                   ldc1 $f12, double
        doubles                                                   syscall
      • Strings              # Declarar strings en memoria -----> li $v0, 4
        strings                                                   la $a0, palabra
        strings                                                   syscall

CÓDIGO de ejemplo:

.data
  palabra: .asciiz "Se van a cargar números: "
  entero: .word 10
  flotante: .float 15.0
  double: .double 2.34
.text
.globl main
    main:
          li $v0, 4
          la $a0, palabra
          syscall
          -
          li $v0, 1
          lw $a0, entero
          syscall
          -
          li $v0, 2
          lwc1 $f12, flotante
          syscall
          -
          li $v0, 3
          ldc1 $f12, double
          syscall
          
fin: li $v0, 10
     syscall



Cosas a tener en cuenta:
   ◉ Reserva de mips
      ➢ $v0         # Memoria que preferiblemente siempre tiene que estar vacía
      ➢ $a0         # Para mostrar en pantalla los enteros y las strings
      ➢ syscall     # Terminación de una función 
   ◉ Palabras reservadas: 
      ➢ li          # Hace una llamada al .data
      ➢ lw          # Indica a mips que va a cargar un entero
      ➢ sw	     # Mueve un valor de v0->(por ejemplo) a una variable
      ➢ move	     # Palabra reservada para enteros, que mueves los valores de unas variables a otras move $t0, $v0
      ➢ la          # Indica a mips que va a cargar una string
      ➢ lwc1        # Indica a mips que va a cargar un flotante
      ➢ ldc1        # Indica a mips que va a cargar un double
TABLAS CON TODO LAS DECLARACIONES:
![Captura de pantalla 2024-11-25 104640](https://github.com/user-attachments/assets/0d9df29a-5224-499a-8349-0b6e177f918a)
![image](https://github.com/user-attachments/assets/b65e0ad2-fc2f-499a-bac1-14e905ade35c)
![Captura de pantalla 2024-11-25 105703](https://github.com/user-attachments/assets/10543ca5-84fd-4473-8d6a-c2f0b634dace)
![Captura de pantalla 2024-11-25 105710](https://github.com/user-attachments/assets/f7365592-01ae-4fb6-9290-04f03281b8f3)

####################################################################################################################################################################################################################
Almacenamiento

Imáginemos que queremos hacer un ejercicio, donde pidamos al usuario que ingres un número y luego le digamos que este es su número:
"Ingrasa un número: "
"Este es tu número: 40"
Bien pues para hacer este ejercicio debemos entender ciertas cosas:

▶ ¿Qué es imprimir o cargar/mostar?
Imprimir en mips lo interpretamos como en pytohn, hacer un print() de una constante: print("Hello world") ---> Hello world
.data
mensaje1: .asciiz "Introduce un numero: "
.text
    li $v0, 4         # Mostar mensaje1
	la $a0, mensaje1
	syscall

▶ ¿Qué es leer y guardar?
Leer es cargar un número (no tiene por que estar declarado) li $v0, 5 para posteriormente guardarlo en una variable que SI hemos declarado sw $v0, numero
fijando que ahora ya no usamos un "4" para imprimir o cargar un entero, que ya está declarado, sino un "5" ya que indicamos que vamos a LEER entero

.data
    	numero: .word 0
.text
        li $v0, 5        # Lectura de un entero
        syscall
        sw $v0, numero    # Guarda el numero de la memoria v0 a la variable numero

Bien, pues para resolver este ejercico podemos seguir esta estrucutura (inténtalo antes de ver la solución):
    ◦ Cargar mensaje1
    ◦ Leer entero + guardar
    ◦ Cargar mensaje2
    ◦ Cargar el número almacenado (recuerda que esto no es python, no salta de línea si no se lo dices, no te machaques la cabeza pensando más de la cuenta, no hay que 
    encganchar mensaje2 con el guardado del número)

Solución:
.data
	mensaje1: .asciiz "Introduce un numero: " 	# Mensaje1
	mensaje2: .asciiz "\n Este es tu numero: "	# Mensaje2
	numero: .word 0					# Variable vacía donde se va a almacenar el numero
.text
.globl main
    main:
	li $v0, 4         # Cargar mensaje1
	la $a0, mensaje1
	syscall
    - 
	li $v0, 5         # Leer entero del usuario, que se almacenerá en v0
	syscall
	sw $v0, numero    # Pasar el numero de v0 a la variable numero
    -
	li $v0, 4	  # Cargar mensaje2
	la $a0, mensaje2
	syscall
    -
	li $v0, 1	# Imprimir el entero
	lw $a0, numero	# Cargar el numero almacenado
	syscall
	
          
fin: li $v0, 10
     syscall

*Nota*: - Date cuenta que la memoria v0 es un valor en el cual se guarda temporalmente los enteros y las strings, por eso tenemos que mover el valor temporal de v0 --> VARIABLE
		pero en los flotantes y doubles en vez de ser "v0" es "f0"
        - Y para que se muestre el nuevo valor de numero tienes que imprimirlo
	- Hay otra forma de hacerlo que es sin tener que declarar una variable numero, para almacenar el dato, utilizar "move" en vez de "sw": 
 		li $v0, 5
   		move $t0, $v0
     		syscall
        - Cabe recalcar que si para leer un entero utilizamos el "5" (li $v0, 5) para un flotante usaremos el "6"; para un double el "7"; y para una string el "8"

 ####################################################################################################################################################################################################################
5. Vamos a comenzar con las Instrucciones aritméticas, lógicas y de desplazamiento.
( Para encontrar ejercicios de suma, resta... Ir a la sección de ejercicio 5. Intrucciones artiméticas ) 
La siguiente tabla resume las cuatro operaciones básicas: suma, resta, multiplicación y división. 
****
Vamos ha hacer un ejercicio que haga una suma, una resta... Pero primero este será el caso base, que tendremos que hacer siempre para resolver ejercicios de este tipo:

.data               
    prompt1: .asciiz "Introduce el primer número: "  		# Mensaje para el primer número
    prompt2: .asciiz "Introduce el segundo número: " 		# Mensaje para el segundo número
    result_msg: .asciiz "El resultado de sumar es: "            # Mensaje para mostrar el resultado
# Recuerda cambiar el texto de resltado para que se adecue a la operación
.text               
    .globl main      

main:

    li $v0, 4            # Cargar el código de servicio para imprimir una cadena
    la $a0, prompt1      # Cargar la dirección de la cadena prompt1
    syscall              
    
    li $v0, 5            # Cargar el código de servicio para leer un entero
    syscall              # Llamada al sistema (leer entero)
    move $t0, $v0        # Guardar el primer número en $t0

    
    li $v0, 4            # Cargar el código de servicio para imprimir una cadena
    la $a0, prompt2      # Cargar la dirección de la cadena prompt2
    syscall              # Llamada al sistema (imprimir mensaje)

    
    li $v0, 5            # Cargar el código de servicio para leer un entero
    syscall              # Llamada al sistema (leer entero)
    move $t1, $v0        # Guardar el segundo número en $t1
 # Esta siempre sera la parte cambiante, ya que ahora queremos que sume, pero si queremos que reste solo tendremos que cambiar la intrucción
 # add $t2, $t0, $t1    # Sumar los valores de $t0 y $t1 y guardar el resultado en $t2

    
    li $v0, 4            # Cargar el código de servicio para imprimir una cadena
    la $a0, result_msg   # Cargar la dirección de la cadena result_msg
    syscall              # Llamada al sistema (imprimir mensaje)

    
    li $v0, 1            # Cargar el código de servicio para imprimir un entero
    move $a0, $t2        # Pasar el resultado de la suma (en $t2) a $a0
    syscall              # Llamada al sistema (imprimir el entero)

# Para restar
# Haremos: sub $t2, $t0, $t1
# Para dividir
# Haremos: div $t2, $t0, $t1 Si hay una division impar, al ser enteros nos redondeara al menor: 13/2 = 6
# Para multiplicar
# Haremos: mul $t2, $t0, $t1
# Recuerda ver bien la tabla, dado que dependiendo lo que necesites tendras que utilizar una instrucción u otra.

####################################################################################################################################################################################################################
									# PARTE MÁS IMPORTANTE
####################################################################################################################################################################################################################
6. Estructuras de control: Saltos condicionales e incondicionales
Este apartado se divide en tres grandes grupos: Condicionales, Bucles mientras y Bucles para.

# CONDICIONALES
Algoritmos:
	Si (condición) entonces
 		accion1
	si no
 		acción2
	fin si


Si a=b entonces
 c=a
si no
 c=a+b
fin si
