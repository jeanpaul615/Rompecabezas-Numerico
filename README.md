![ChatGPT Image 21 abr 2025, 19_14_26](https://github.com/user-attachments/assets/df2a8809-be68-41a8-a041-3cd499cdc596)
# NUM-PUZZLE-PYTHON-WITH-TURTLE
“”” módulo random: Se utiliza para mezclar los números del rompecabezas antes de colocarlos en las casillas.
time: Se utiliza para medir el tiempo transcurrido durante el juego.
turtle: Se utiliza para dibujar la interfaz gráfica del juego.”””
from random import shuffle
import time
from turtle import *

casillas = {}#Lista de tuplas que representa las posiciones vecinas a una casilla en el rompecabezas.
vecinos = [# Diccionario que almacena las casillas del rompecabezas y sus respectivos números.
    (100, 0),
    (-100, 0),
    (0, 100),
    (0, -100),
]

tiempo_inicio = None #Variable que almacena el momento de inicio del juego.
contador = 0 #Variable que cuenta el número de movimientos realizados.
cronometro_activo = False #esto hace que el cronometro no inicie de inmediato

# Crear el objeto Turtle para mostrar el cronómetro
cronometro_turtle = Turtle()
cronometro_turtle.color("black")
cronometro_turtle.penup()
cronometro_turtle.hideturtle()
cronometro_turtle.goto(0, 300)

“”” función que hace que inicie el cronometro, define unas variables globales y hace el llamado recursivo de la función  actualizar_cronometro()”””
def iniciar_cronometro():
    global tiempo_inicio, cronometro_activo
    tiempo_inicio = time.time()
    cronometro_activo = True
    actualizar_cronometro()

“””esta función tiene el objetivo de que el cronometro aumente cada vez que entra en esta función hasta que el juego termine, hace un llamado recursivo a si mismo”””
def actualizar_cronometro():
    global cronometro_activo

    if not cronometro_activo:
        return

    tiempo_transcurrido = round(time.time() - tiempo_inicio, 2)
    cronometro_turtle.clear()
    cronometro_turtle.write(f"Tiempo: {tiempo_transcurrido} segundos", align="center", font=("Arial", 16, "normal"))
    ontimer(actualizar_cronometro, 100)  # Llamar a la función cada 100 milisegundos

“””En esta función lo que se hace es establecer los limites de cada puzzle en base a el tamaño, por ello se definen 3 limites para cada componente del el tablero,además de eso se determina el tamaño de la longitud del puzzle y se hace un llamado a la función Cargar_Tablero.”””
def Inicio_Juego():
    rango = input("Digite el tamaño del puzzle (3, 4 o 5): ")
    if rango == '3':
        limite1_en_y = -100
        limite2_en_y = 200
        limite3_en_y = 100
        limite1_en_x = -100
        limite2_en_x = 200
        limite3_en_x = 100
        longitud_del_puzzle = 9
        cargar(limite1_en_y, limite2_en_y, limite3_en_y, limite1_en_x, limite2_en_x, limite3_en_x, longitud_del_puzzle)
    elif rango == '4':
        limite1_en_y = 200
        limite2_en_y = -200
        limite3_en_y = -100
        limite1_en_x = -200
        limite2_en_x = 200
        limite3_en_x = 100
        longitud_del_puzzle = 16
        cargar(limite1_en_y, limite2_en_y, limite3_en_y, limite1_en_x, limite2_en_x, limite3_en_x, longitud_del_puzzle)
    elif rango == '5':
        limite1_en_y = 200
        limite2_en_y = -300
        limite3_en_y = -100
        limite1_en_x = -200
        limite2_en_x = 300
        limite3_en_x = 100
        longitud_del_puzzle = 25
        cargar(limite1_en_y, limite2_en_y, limite3_en_y, limite1_en_x, limite2_en_x, limite3_en_x, longitud_del_puzzle)

“””Esta función crea una lista de numero del 1 al L-1, esto representa los números del rompecabezas y le agregar un espacio en vacio al final, mezcla los números con un random.shuffle, además de eso llama la función dibujar y tap.”””
def cargar(y1, y2, y3, x1, x2, x3, L):
    numeros = list(range(1, L))
    shuffle(numeros)#genera número aleatorios en el rango predefino
    numeros.append(None)
    index = 0
    

    for y in range(y1, y2, y3):
        for x in range(x1, x2, x3):
            posicionamiento = (x, y)
            casillas[posicionamiento] = numeros.pop(0)
            index += 1

    dibujar()#llamado a la función para que dibuje el tablero
    setup(750, 750, 600, 0) #tablero general del juego
    hideturtle() #recorrido de la turtle
    tracer(False) #desactiva la animación de la turtle
    onscreenclick(tap) #hace que cuando se cliquee la pantalla este comando llame a la función tap
    iniciar_cronometro() #inicia el cronometro al iniciar el juego
    done() #finaliza la configuración de la interfaz grafica

“””dibuja un cuadrado en la posición dada y rellena su interior, si el número es menor que 10, mueve el lápiz hacia adelante para ajustar el número del cuadrado, recibe dos parámetros, posicionamiento y el número.”””
def cuadrado(posicionamiento, numero)
    up()
    goto(posicionamiento[0], posicionamiento[1])
    down()

    color('green', 'black')
    begin_fill()
    for count in range(4):
        forward(98)
        left(90)
    end_fill()

    if numero is None:
        return
    elif numero < 10:
        forward(20)

    write(numero, font=('Arial', 60, 'normal'))

“””esta función actualiza las variables tiempo_inicio y contador si es la primera casilla seleccionada, si el espacio vacio es vecino intercambia las posiciones del numero con el vacio, llama la función cuadrado para actualizar las nuevas posiciones, también llama la función verificar_ganador() , recibe dos parámetros de orientación, x y y.
def tap(x, y):
    global tiempo_inicio, contador, cronometro_activo

    if tiempo_inicio is None:
        tiempo_inicio = time.time()
        cronometro_activo = True

    contador += 1
    posicionamiento = None

    for casilla in casillas:
        if casilla[0] <= x <= casilla[0] + 100 and casilla[1] <= y <= casilla[1] + 100:
            posicionamiento = casilla
            break

    if posicionamiento is not None:
        for vecino in vecinos:
            posicion = (posicionamiento[0] + vecino[0], posicionamiento[1] + vecino[1])

            if posicion in casillas and casillas[posicion] is None:
                numero = casillas[posicionamiento]
                casillas[posicion] = numero
                casillas[posicionamiento] = None
                cuadrado(posicion, numero)
                cuadrado(posicionamiento, None)
                verificar_ganador()
                break 

esta condición lo que hace es si la función resulta ser verdadera, la función hace lo del if, y imprime un texto con el ganaste, tiempo transcurrido y cantidad de movimiento, se expefica la letra, la orientación y el tamaño.
    if verificar_ganador():
        tiempo_transcurrido = round(time.time() - tiempo_inicio, 2)
        write(f"¡Ganaste!\nTiempo: {tiempo_transcurrido} segundos\nMovimientos: {contador}", align='center', font=('Arial', 20, 'normal'))
        exitonclick()#salir cuando se clickee de nuevo la pantalla
        print("GANASTE, TU TIEMPO ES:",tiempo_transcurrido)
        Guardar_Registro(tiempo_transcurrido)#llamar función para guarda el tiempo

“”” calcula el tiempo entre el incio del juego y el momento dado que es el parámetro que recibe, llama a la función guardar_registro para guardar el tiempo establecido en realizar el puzzle”””
def actualizar_tiempo(termino):
    inicio = tiempo_inicio if tiempo_inicio is not None else 0
    tiempo_transcurrido = round(termino - inicio, 2)
    print("Tardaste", tiempo_transcurrido, "segundos")
    Guardar_Registro(tiempo_transcurrido)

“””Itera sobre las casillas y llama a la función cuadrado() para dibujar cada casilla con su respectivo número.”””
def dibujar():
    for posicionamiento in casillas:
        cuadrado(posicionamiento, casillas[posicionamiento])

“””Crea una lista con el orden correcto de los números del rompecabezas y el espacio vacío, además de eso compara la lista ordenada de las casillas del rompecabezas con el orden correcto.
Si todas las casillas están en el orden correcto, retorna True, indicando que el rompecabezas está resuelto; de lo contrario, retorna False.”””
def verificar_ganador():
    global cronometro_activo

    numeros = list(casillas.values())  # Obtener los números de las casillas

    if numeros[-1] != None:
        return False  # La casilla vacía no está en la última posición

    numeros = numeros[:-1]  # Quitar la casilla vacía de la lista

    numeros_ordenados = sorted(numeros)  # Ordenar los números en forma ascendente

    if numeros != numeros_ordenados:
        return False  # Los números no están en orden ascendente

    cronometro_activo = False  # Detener el cronómetro
    return True

“””Guarda el registro pidiendo el nombre de quien juega y el tiempo registrado, además de eso abre un archivo de texto donde guarda los datos”””
def Guardar_Registro(tiempo_transcurrido):
    respuesta = input("¿Deseas guardar el registro? (sí/no): ")
    nombre = input("Escribe tu nombre: ")

    try:
            with open("Registros.txt", "a") as archivo:#Abrir el archivo
                archivo.write(f"{nombre},{tiempo_transcurrido}\n")#Escribir en el archivo
                archivo.close()#Cerrar el archivo
            print("Registros guardados correctamente.")
    except IOError:
            print("Error al guardar los registros.")

“””Abre el archivo "Registros.txt", y lee todas las líneas del archivo y las almacena en la variable registros, también elimina los espacios en blanco alrededor de cada registro.
Une los registros en una cadena separada por "|". Y imprime los registros en la consola.”””
def mostrar_registros():
    try:
        with open("registros.txt", "r") as archivo:
            registros = archivo.readlines()
            registros = [registro.strip() for registro in registros]
            registros_completos = " | ".join(registros)
            print(registros_completos)
            archivo.close()  # Cerrar el archivo después de leerlo
    except IOError:
        print("Error al leer los registros.")

#es la función puente para acceder a mostrar los mejores puntajes, lleva una condición para que dverifiques si si quieres ver los puntajes.
def Mostrar_R():
    Mejorespuntajes = input(print("Deseas ver los mejores puntajes?: "))
    if Mejorespuntajes.lower() == "si":
        mostrar_registros()
    else:
        Funcion_Principal()



“””Muestra un menú con opciones: Jugar, Mostrar registros y Salir, Dependiendo la opción seleccionada por el usuario, realiza la acción correspondiente.
-Si se selecciona "Jugar", llama a la función inicio().
-Si se selecciona "Mostrar registros", llama a la función Mostrar_R().
-Si se selecciona "Salir", muestra un mensaje y finaliza el programa.
-Si se ingresa una opción inválida, muestra un mensaje de error.”””

def Funcion_Principal():
    print("-------------MENU-----------\n")
    print("1.JUGAR\n")
    print("2.MOSTRAR RECORDS\n")
    print("3.SALIR\n")
    print("digita una opcion: ")
    opcion = input().lower()

    if opcion == "1":
        inicio()

    elif opcion == "2":
        Mostrar_R()

    elif opcion == "3":
        print("SALISTE")

    else:
        print("Opción inválida. Intente nuevamente.")



Funcion_Principal()#inicia el programa
