# english-fun-practice
#this is a python code that helps spanish speakers to learn or practice their english in a fun and easy way, as it was a tiny game
    
    import sounddevice as sd
    import numpy as np
    import scipy.io.wavfile as wav
    import speech_recognition as sr
    import random

    duration = 6
    sample_rate = 44100

    words_by_level = {
        "facil": [
            {"es": "gato", "en": "cat", "pron": "kat"},
            {"es": "perro", "en": "dog", "pron": "dog"},
            {"es": "manzana", "en": "apple", "pron": "a-pol"},
            {"es": "leche", "en": "milk", "pron": "milk"},
            {"es": "sol", "en": "sun", "pron": "san"}
        ],
    
        "medio": [
            {"es": "platano", "en": "banana", "pron": "ba-na-na"},
            {"es": "escuela", "en": "school", "pron": "skul"},
            {"es": "amigo", "en": "friend", "pron": "frend"},
            {"es": "ventana", "en": "window", "pron": "uin-dou"},
            {"es": "amarillo", "en": "yellow", "pron": "ie-lou"}
        ],

        "dificil": [
            {"es": "tecnologia", "en": "technology", "pron": "tec-no-lo-lli"},
            {"es": "universidad", "en": "university", "pron": "iu-ni-ver-si-ti"},
            {"es": "informacion", "en": "information", "pron": "in-for-mei-shon"},
            {"es": "pronunciacion", "en": "pronunciation", "pron": "pro-nun-ci-ei-shon"},
            {"es": "civilizacion", "en": "civilization", "pron": "ci-vi-lai-sei-shon"}
        ]
    }

    level = input("Selecciona el nivel: facil, medio o dificil: ")

    if level in words_by_level:

        score = 0

        palabras = words_by_level[level].copy()
        random.shuffle(palabras)

        for palabra in palabras:

            print("\nTraduce esta palabra al inglés:", palabra["es"])
            print("Abriremos tu micrófono...")

            recording = sd.rec(int(duration * sample_rate),
                               samplerate=sample_rate,
                               channels=1,
                               dtype="int16")

            sd.wait()

            wav.write("audio.wav", sample_rate, recording)

            print("Procesando audio...")

            recognizer = sr.Recognizer()

            with sr.AudioFile("audio.wav") as source:
                audio = recognizer.record(source)


            try:
                text = recognizer.recognize_google(audio)
                print("Dijiste:", text)

                if text.lower() == palabra["en"]:
                    print("¡Bien hecho! 🎉")
                    score += 1

                else:
                    print("Casi...")
                    print("La traducción es:", palabra["en"])
                    print("Se pronuncia:", palabra["pron"])

            except sr.UnknownValueError:
                print("No se pudo reconocer el audio")

            except sr.RequestError:
                print("No se pudo conectar con Google")

        print("\nJuego terminado")
        print("Tu puntuación final es:", score, "/5")

    else:
        print("Nivel inválido. Intenta con: facil, medio o dificil")
