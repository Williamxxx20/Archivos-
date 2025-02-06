import tkinter as tk

import random

numero_secreto = random.randint(0, 100) 

# Inicializa el módulo de sonido
pygame.mixer.init()

# Cargar sonidos
sound_correct = "correct.wav"  # Sonido cuando adivina correctamente
sound_wrong = "wrong.wav"  # Sonido cuando falla
sound_high = "high.wav"  # Sonido cuando el número es más alto
sound_low = "low.wav"  # Sonido cuando el número es más bajo

# Función para reproducir sonidos
def play_sound(sound_file):
    pygame.mixer.Sound(sound_file).play()

# Función para leer la mejor puntuación almacenada
def leer_mejor_puntuacion():
    if os.path.exists("highscore.txt"):
        with open("highscore.txt", "r") as file:
            return int(file.read().strip())
    return None

# Función para guardar la mejor puntuación
def guardar_mejor_puntuacion(puntos):
    with open("highscore.txt", "w") as file:
        file.write(str(puntos))

# Juego
def jugar():
    numero_secreto = random.randint(0, 100)
    intentos = 0
    mejor_puntuacion = leer_mejor_puntuacion()

    print("🎯 ¡Bienvenido al juego de adivinar el número!")
    print("Intenta adivinar el número entre 0 y 100.")

    while True:
        try:
            intento = int(input("Introduce tu número: "))
            intentos += 1

            if intento < 0 or intento > 100:
                print("⚠️ Número fuera de rango, intenta nuevamente.")
                continue

            if intento < numero_secreto:
                print("🔼 Más alto...")
                play_sound(sound_high)
            elif intento > numero_secreto:
                print("🔽 Más bajo...")
                play_sound(sound_low)
            else:
                print(f"🎉 ¡Correcto! Adivinaste en {intentos} intentos.")
                play_sound(sound_correct)

                if mejor_puntuacion is None or intentos < mejor_puntuacion:
                    print(f"🏆 ¡Nueva mejor puntuación! ({intentos} intentos)")
                    guardar_mejor_puntuacion(intentos)
                break

        except ValueError:
            print("❌ Entrada inválida. Introduce un número válido.")
            play_sound(sound_wrong)

    boton_verificar = tk.Button(ventana, text="Comprobar", command=verificar_numero)

boton_verificar.pack()

mensaje = tk.StringVar()

mensaje.set("Intenta adivinar el número entre 0 y 100")

etiqueta_mensaje = tk.Label(ventana, textvariable=mensaje)

etiqueta_mensaje.pack()

boton_reiniciar = tk.Button(ventana, text="Reiniciar Juego", command=reiniciar_juego)

ventana.mainloop() 
