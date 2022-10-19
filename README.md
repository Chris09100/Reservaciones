# Reservaciones
import random 
import datetime
import time
import csv
import openpyxl 
import pandas as pd

fecha_actual = datetime.date.today()
reserva_sala = {}
tabla = {}
registro_clientes = {}
id_cliente = []
Registro_sala = {}

while True:
    print("***********")
    print("*   MENÚ DE OPCIONES        *")
    print("***********")
    print("1) Registrar la reservación de una sala")
    print("2) Editar el nombre de un evento de una reservacion ya hecha")
    print("3) Consultar las reservaciones existentes")
    print("4) Registrar un nuevo cliente")
    print("5) Registrar una sala")
    print("6) Salir")
    respuesta = input("¿Qué opción desea?: ")
   
    try:
     respuesta_int = int(respuesta)
    except ValueError:
        print("* NO ES VALIDA LA RESPUESTA *")
    except Exception:
        print("** SE HA PRESENTADO UNA EXCEPCIÓN")
   
    if respuesta_int == 1:
        print("*********")
        print("    RESERVACIÓN DE SALA    ")
        print("*********")
        cliente_registrado = input("Clave de cliente: ")
        if cliente_registrado in id_cliente:
          cliente = input("Nombre del cliente: ")
          print(f"Salas disponibles: {Registro_sala}")
          sala_reservada = input(f"Nombre de la sala a reservar: ")
          evento = input("Nombre del evento: ")
          if sala_reservada in Registro_sala:
             fecha_capturada = input("Fecha de reserva de tu sala (dd/mm/aaaa):" )
             fecha_reserva = datetime.datetime.strptime(fecha_capturada, "%d/%m/%Y").date()
             fr_procesada = fecha_reserva.strftime("%d/%m/%Y")
             if fecha_actual < fecha_reserva:
                 Turno = input("Turno (mañana, tarde, noche): ")
                 reserva_sala[sala_reservada] = evento, fr_procesada, Turno 
                 tabla[fr_procesada] = sala_reservada, cliente, evento, Turno
             else:
                print("NO SE PUEDE RESERVAR EN ESTA FECHA")
             
          else:
            print("Esta sala no existe")
        else:
            print("CLIENTE NO REGISTRADO/ENCONTRADO")
    elif respuesta_int == 2:
        print("************")
        print("  EDITAR EL NOMBRE DE UNA SALA  ")
        print("************")
        print(list(reserva_sala.keys()))
        sala_elegida = input("¿A qué sala desea cambiar el nombre : ")
        nuevo_nombre = input("Que nombre desea: ")
        reserva_sala[nuevo_nombre] = reserva_sala[sala_elegida]
        del reserva_sala[sala_elegida]
        print("Sala editada")
        print(list(reserva_sala.keys()))
    elif respuesta_int == 3:
        print("************")
        print("    RESERVACIONES EXISTENTES    ")
        print("************")
        import pandas as pd
        reservaciones = [[clave_sala],
         [nombre_sala],
         [Turno]]
        columnas = [fecha_capturada] # definimos los nombres de las columnas
        filas = ["Clave de la sala", "Nombre de la sala", "Turno"] # definimos los nombres de las filas
        df = pd.DataFrame(reservaciones, columns=columnas, index=filas)
        print(df)
        df.to_csv('reservaciones.csv')
        df = pd.read_csv('reservaciones.csv')
        df.to_excel('reservaciones.xlsx', index=False)


    
    elif respuesta_int == 4:
       print("************")
       print("       REGISTRAR CLIENTE        ")
       print("************")
       num = [random.randrange(1,1001) for valor in range(1000)]
       clave_cliente = format(id(num), )
       print(f"Clave del cliente: {clave_cliente}")
       id_cliente.append(clave_cliente)
       nombre_cliente = input("Nombre del cliente: ")
       registro_clientes[nombre_cliente] = clave_cliente
       print(f"Cliente registrado: {registro_clientes}")
    elif respuesta_int == 5:
        print("************")
        print("        REGISTRAR SALA          ")
        print("************")
        numero = [random.randrange(1,1001) for valor in range(1)]
        clave_sala = format(id(numero),"x")
        print(f"Clave de la sala: {clave_sala}")
        nombre_sala = input("Nombre de la sala: ")
        cupo_sala = input("Cupo de sala: ")
        Registro_sala[nombre_sala] = clave_sala,cupo_sala
        print(f"la sala ha sido registrada")
    elif respuesta_int == 6:
        break
    else:
        print(f"Su opción no es valida.")
