Estructura del código:
.data
    # Aquí se va a guardar la información en la memoria del procesador; las strings, los booleanos, los floats, los enteros.
.text
.globl main
    # Aquí es dónde se va a ejecutar o imprimir la información, las funciones, mostrar en pantlla.

main: # Etiqueta main, para estrcuturar mejor el código y sea más legible.

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
      ➢ la          # Indica a mips que va a cargar una string
      ➢ lwc1        # Indica a mips que va a cargar un flotante
      ➢ ldc1        # Indica a mips que va a cargar un double
      
