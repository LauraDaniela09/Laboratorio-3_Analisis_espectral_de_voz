# L𝐚𝐛𝐨𝐫𝐚𝐭𝐨𝐫𝐢𝐨 𝟑 - F𝐫𝐞𝐜𝐮𝐞𝐧𝐜𝐢𝐚 𝐲 V𝐨𝐳
𝙞𝙣𝙩𝙧𝙤𝙙𝙪𝙘𝙘𝙞ó𝙣

En este laboratorio se realizó un análisis espectral de la voz  empleando técnicas de procesamiento digital de señales. Se utilizaron grabaciones de voces masculinas y femeninas para calcular parámetros como la frecuencia fundamental, frecuencia media, brillo e intensidad.

𝙤𝙗𝙟𝙚𝙩𝙞𝙫𝙤

Aplicar la Transformada de Fourier para analizar y comparar las características espectrales de voces masculinas y femeninas, identificando sus diferencias en frecuencia e intensidad para comprender mejor el comportamiento acústico de la voz humana.

𝙞𝙢𝙥𝙤𝙧𝙩𝙖𝙘𝙞ó𝙣 𝙙𝙚 𝙡𝙞𝙗𝙧𝙚𝙧𝙞𝙖𝙨
```python
import scipy.io.wavfile as wav
import matplotlib.pyplot as plt
import numpy as np
from IPython.display import Audio
from scipy.signal import find_peaks
from scipy.io import wavfile
```
Esa parte del código muestra la importación de librerías necesarias para trabajar con archivos de audio y analizarlos:`scipy.io.wavfile` y `wavfile` para leer y escribir archivos de audio `(.wav)`.`matplotlib.pyplot` para graficar señales.`numpy` para realizar operaciones numéricas y de matrices. `IPython.display.Audio` para reproducir el audio directamente en el notebook.`scipy.signal.find_peaks`para detectar picos o puntos importantes en la señal.

<h1 align="center"><i><b>𝐏𝐚𝐫𝐭𝐞 A 𝐝𝐞𝐥 𝐥𝐚𝐛𝐨𝐫𝐚𝐭𝐨𝐫𝐢𝐨</b></i></h1>

En esta parte del codigo se utiliza la función `wav.read()` de `SciPy` para cargar el archivo  y obtener su frecuencia de muestreo y datos de la señal. Si el audio tiene más de un canal, se selecciona solo uno para trabajar en mono. Luego, con `np.linspace()` de `NumPy`, se crea el eje de tiempo para cada muestra. La librería `Matplotlib (plt.plot())` se usa para graficar la señal, mostrando la amplitud frente al tiempo. Finalmente, con `Audio()` de `IPython.display`, se reproduce el sonido directamente en el entorno de ejecución.este procedimiento se realiza con cada una de las señales tanto de mujeres como para hombres.

𝙩𝙧𝙖𝙣𝙨𝙛𝙤𝙧𝙢𝙖𝙙𝙖 𝙙𝙚 𝙛𝙤𝙪𝙧𝙞𝙚𝙧 𝙮 𝙚𝙨𝙥𝙚𝙘𝙩𝙧𝙤 𝙙𝙚 𝙢𝙖𝙜𝙣𝙞𝙩𝙪𝙙

Posteriormente para poder obtener la transformada de fourier y el espectro lo que se realiza es cargar el audio,  conviertirlo a mono y aplicar la Transformada de Fourier (FFT) para obtener sus componentes en frecuencia. Luego se grafican dos resultados: la Transformada de Fourier, que muestra cómo se distribuyen las frecuencias del sonido, y el espectro de magnitud en decibelios, que indica la intensidad de cada frecuencia presente en la señal.


```python
for archivo in archivos:

    print("\n======================")
    print("Analizando:", archivo)
    print("======================")

    # Leer audio
    fs, y = wavfile.read(archivo)

    # Convertir a mono si es estéreo
    if len(y.shape) > 1:
        y = y[:,0]

    y = y.astype(float)

    # -------------------------
    # 1. DOMINIO DEL TIEMPO
    # -------------------------

    t = np.arange(len(y)) / fs

    plt.figure(figsize=(8,4))
    plt.plot(t, y, color="#FF00FF")
    plt.title("Señal - " + archivo)
    plt.xlabel("Tiempo (s)")
    plt.ylabel("Amplitud")
    plt.grid(True)
    plt.show()

    # -------------------------
    # 2. TRANSFORMADA DE FOURIER
    # -------------------------

    N = len(y)
    Y = np.fft.fft(y)
    f = np.fft.fftfreq(N, 1/fs)
    magnitud = np.abs(Y)

    # Solo frecuencias positivas
    f_pos = f[:N//2]
    mag_pos = magnitud[:N//2]

    # -------------------------
    # 3. FILTRAR 50–1500 Hz
    # -------------------------

    mask = (f_pos >= 50) & (f_pos <= 1500)

    f_range = f_pos[mask]
    mag_range = mag_pos[mask]

    # -------------------------
    # 4. ESPECTRO SEMILOG EN X
    # -------------------------

    plt.figure(figsize=(8,4))
    plt.semilogx(f_range, mag_range, color="#FF00FF")
    plt.title("Espectro de frecuencias (50–1500 Hz) - " + archivo)
    plt.xlabel("Frecuencia (Hz) - Escala log")
    plt.ylabel("Magnitud")
    plt.xlim(50,1500)
    plt.grid(True, which="both")
    plt.show()

    # -------------------------
    # 5. FRECUENCIA FUNDAMENTAL
    # -------------------------

    indice = np.argmax(mag_range)
    f0 = f_range[indice]

    # -------------------------
    # 6. FRECUENCIA MEDIA
    # -------------------------

    f_media = np.sum(f_range * mag_range) / np.sum(mag_range)

    # -------------------------
    # 7. BRILLO
    # -------------------------

    brillo = f_media

    # -------------------------
    # 8. ENERGÍA
    # -------------------------

    energia = np.sum(y**2)

    # -------------------------
    # RESULTADOS
    # -------------------------

    print("Frecuencia fundamental:", round(f0,2), "Hz")
    print("Frecuencia media:", round(f_media,2), "Hz")
    print("Brillo:", round(brillo,2))
    print("Energia:", energia)
```

## Man 1 

<img width="724" height="393" alt="image" src="https://github.com/user-attachments/assets/e43f8898-403a-4a34-86d0-b9f417de6b44" />
<img width="691" height="398" alt="image" src="https://github.com/user-attachments/assets/fc5ae158-30fd-4c3d-b57c-ef845802b9ad" />

## resultados 
-Frecuencia fundamental: 216.6 Hz

-Frecuencia media: 501.48 Hz

-Brillo: 501.48

-Energia: 6918357074850.0

## Man 2 
<img width="724" height="393" alt="image" src="https://github.com/user-attachments/assets/9fb480ec-3050-41f1-8bc7-c93649c76bbb" />
<img width="691" height="398" alt="image" src="https://github.com/user-attachments/assets/6d154acd-443b-478b-85b0-0ef14fbc15db" />


## resultados 
-Frecuencia fundamental: 190.44 Hz

-Frecuencia media: 441.54 Hz

-Brillo: 441.54

-Energia: 2484201169383.0

## Man 3 
<img width="724" height="393" alt="image" src="https://github.com/user-attachments/assets/9fc9448c-303f-43ec-82a7-c1db592ada52" />
<img width="678" height="398" alt="image" src="https://github.com/user-attachments/assets/0622f756-9a21-44aa-b436-58a0378ec95d" />

## resultados
-Frecuencia fundamental: 123.36 Hz

-Frecuencia media: 542.96 Hz

-Brillo: 542.96

-Energia: 3217311983931.0


## Mujer 1 
<img width="724" height="393" alt="image" src="https://github.com/user-attachments/assets/871dcfef-c43a-4e95-ada2-e1e897a03dbe" />
<img width="678" height="398" alt="image" src="https://github.com/user-attachments/assets/4ecf7cd5-c15a-4ac1-9ac2-da06519cccc2" />

## resultados 
-Frecuencia fundamental: 214.12 Hz

-Frecuencia media: 545.6 Hz

-Brillo: 545.6

-Energia: 8596806895859.0


## Mujer 2 
<img width="724" height="393" alt="image" src="https://github.com/user-attachments/assets/c0341665-b956-4b68-8274-9db7c0e05dcd" />
<img width="678" height="398" alt="image" src="https://github.com/user-attachments/assets/f2675e8b-98e0-45ac-907c-d3d1ace12051" />

## resultados 
-Frecuencia fundamental: 177.68 Hz

-Frecuencia media: 560.28 Hz

-Brillo: 560.28

-Energia: 2444546048837.0


## Mujer 3 
<img width="724" height="393" alt="image" src="https://github.com/user-attachments/assets/03d836be-c7b2-4dc3-8339-ba865006b60a" />
<img width="678" height="398" alt="image" src="https://github.com/user-attachments/assets/7778101d-fec7-40f5-824b-22315d0c801b" />

## resultados 
-Frecuencia fundamental: 484.79 Hz

-Frecuencia media: 599.59 Hz

-Brillo: 599.59

-Energia: 7461476423006.0

<h1 align="center"><i><b>𝙋𝙖𝙧𝙩𝙚 𝘽 𝙙𝙚𝙡 𝙡𝙖𝙗𝙤𝙧𝙖𝙩𝙤𝙧𝙞𝙤</b></i></h1>


### Filtro pasabanda hombre

Antes de iniciar el codigo se desarrolló a mano el filtro pasabanda para poder encontrar el orden necesario y definir los parametros. 
<p align="center">
<img width="600" height="1280" alt="image" src="https://github.com/user-attachments/assets/e0b9f2fe-c3ac-43a2-966d-092f56510ba2" />
</p>


Primero se importan las librerias, se lee el archivo `/Man1.wav` y guarda la frecuencia de muestreo en `ratem1` y los datos de la señal en `Man1`.
Después define los parametros del filtro pasabanda como la frecuencia de corte baja y alta basandose en los valores teoricos. Para hombres está el rango de 80-400Hz.

```python
from scipy import signal
from scipy.io import wavfile
ratem1, Man1 = wav.read("/Man1.wav")

f_low = 80
f_high = 400
order = 4
fs = ratem1
nyquist = fs / 2
low = f_low / nyquist
high = f_high / nyquist

b, a = signal.butter(order, [f_low/nyquist, f_high/nyquist], btype='bandpass')
Man1_filtrada = signal.filtfilt(b, a, Man1)
t = np.linspace(0, len(Man1)/fs, len(Man1))
```
Se asigna la frecuencia de muestreo `ratem1` a la variable `fs` y se calculan las frecuencias normalizadas y la de Nyquist.
Se diseña el filtro pasabanda tipo butterworth que devuelve los coeficientes del filtro en `b` (numerador) y `a` (denominador).
Aplica el filtro a la señal usando `filtfilt`, que filtra hacia adelante y hacia atrás para eliminar el desfase (fase cero).

```python
plt.figure(figsize=(12,5))
plt.subplot(2,1,1)
plt.plot(t, Man1, color='#005C63')
plt.title("Señal original (voz Hombre)")
plt.xlabel("Tiempo [s]")
plt.ylabel("Amplitud")
plt.subplot(2,1,2)
plt.plot(t, Man1_filtrada, color='#00A5A8')
plt.title("Señal filtrada (pasa banda 80–400 Hz, Butterworth orden 4)")
plt.xlabel("Tiempo [s]")
plt.ylabel("Amplitud")
plt.tight_layout()
plt.show()
```
El código grafica la señal de voz de un hombre antes y después de aplicar un filtro pasa banda Butterworth de 80–400 Hz. La primera gráfica muestra la señal original, y la segunda la señal filtrada, donde se conservan solo las frecuencias propias de la voz masculina y se eliminan ruidos fuera de ese rango.
## resultado
<img width="1189" height="490" alt="image" src="https://github.com/user-attachments/assets/da48f4cc-3d65-4d8e-ad49-1581c34be717" />


```python
w, h = signal.freqz(b, a, worN=4096, fs=fs)  # usamos fs para escalar en Hz

plt.figure(figsize=(10,6))

# Magnitud
plt.subplot(2,1,1)
plt.semilogx(w, 20 * np.log10(abs(h)), color='#00796B')
plt.title('Diagrama de Bode - Magnitud (Filtro pasa banda 80–400 Hz)')
plt.xlabel('Frecuencia [Hz]')
plt.ylabel('Ganancia [dB]')
plt.xlim(10, fs/2)
plt.ylim(-80, 5)
plt.grid(True, which='both', linestyle='--', alpha=0.7)
plt.tight_layout()
plt.show()
```
El código usa `signal.freqz()` para obtener la respuesta en frecuencia del filtro y graficarla en escala logarítmica. Muestra cómo varía la ganancia en decibelios según la frecuencia, destacando la banda de paso entre 80 y 400 Hz y la atenuación fuera de ese rango.
## Grafico de respuesta de frecuencia (bode)

<p align="center">
<img width="700" height="332" alt="image" src="https://github.com/user-attachments/assets/0d890148-9a96-4a74-ac6b-684a0f1f9b52" />
</p>

### Filtro pasabanda mujer

Se repite el mismo proceso para la señal de mujer pero con rango de 150-500Hz.
<img width="1189" height="490" alt="image" src="https://github.com/user-attachments/assets/636e7335-ca71-40a3-88a9-aab50dc182ea" />


```python
from scipy import signal
from scipy.io import wavfile
ratem1, Mujer1 = wav.read("/Mujer1.wav")

f_low = 150
f_high = 500
order = 2
fs = ratem1
nyquist = fs / 2
low = f_low / nyquist
high = f_high / nyquist

b, a = signal.butter(order, [f_low/nyquist, f_high/nyquist], btype='bandpass')
Mujer1_filtrada = signal.filtfilt(b, a, Mujer1)
t = np.linspace(0, len(Mujer1)/fs, len(Mujer1))

plt.figure(figsize=(12,5))
plt.subplot(2,1,1)
plt.plot(t, Mujer1, color='#6A5ACD')
plt.title("Señal original (voz mujer)")
plt.xlabel("Tiempo [s]")
plt.ylabel("Amplitud")
plt.subplot(2,1,2)
plt.plot(t, Mujer1_filtrada, color='#9A00FF')
plt.title("Señal filtrada (pasa banda 150–500 Hz, Butterworth orden 2)")
plt.xlabel("Tiempo [s]")
plt.ylabel("Amplitud")
plt.tight_layout()
plt.show()
```
```python
plt.figure(figsize=(12,5))
plt.subplot(2,1,1)
plt.plot(t, Mujer1, color='#6A5ACD')
plt.title("Señal original (voz mujer)")
plt.xlabel("Tiempo [s]")
plt.ylabel("Amplitud")
plt.subplot(2,1,2)
plt.plot(t, Mujer1_filtrada, color='#9A00FF')
plt.title("Señal filtrada (pasa banda 150–500 Hz, Butterworth orden 2)")
plt.xlabel("Tiempo [s]")
plt.ylabel("Amplitud")
plt.tight_layout()
plt.show()
```
<img width="1189" height="490" alt="image" src="https://github.com/user-attachments/assets/1dd0273b-41a5-465f-b03c-f86d0beefd98" />
Se grafica la respuesta de frecuencia (Diagrama Bode de magnitud)

## Grafico de respuesta de frecuencia (bode)

```pythom
w, h = signal.freqz(b, a, worN=4096, fs=fs)

plt.figure(figsize=(10,5))
plt.semilogx(w, 20 * np.log10(abs(h)), color='#9A00FF')
plt.title('Diagrama de Bode - Magnitud (Filtro pasa banda 150–500 Hz)')
plt.xlabel('Frecuencia [Hz]')
plt.ylabel('Ganancia [dB]')
plt.xlim(10, fs/2)
plt.ylim(-80, 5)
plt.grid(True, which='both', linestyle='--', alpha=0.7)
plt.tight_layout()
plt.show()

```
<p align="center">
<img width="600" height="489" alt="image" src="https://github.com/user-attachments/assets/6a71ad18-38f6-4558-8b29-9511c5e2fdc8" />
</p>


## Medición jitter y shimmer

Se importan las librerias, se define la función `jitter_shimmer` donde se recibe la señal de voz, se calculan el jitter y shimmer y sus porcentajes.
Finalmente se visualiza una grafica donde se ve en azul la señal original de voz, en rojo los cruces por ceros y en verde los picos. Y una tabla con los resultados de cada señal.

```pythom
import pandas as pd

resultados = []

for nombre, ruta in archivos.items():

    print("Procesando:", nombre)

    fs, audio = wavfile.read(ruta)

    if audio.ndim > 1:
        audio = audio[:,0]

    ja, jr, sa, sr, cruces, peaks, audio = jitter_shimmer(audio, fs)

    t = np.linspace(0, len(audio)/fs, len(audio))

    plt.figure(figsize=(10,4))

plt.figure(figsize=(10,4))

fig, axs = plt.subplots(2, 3, figsize=(18, 8))
axs = axs.flatten()

color_senal = '#5A4FCF'
color_cruces = '#D81B60'
color_picos = '#311B92'

resultados = []

for i, (nombre, ruta) in enumerate(archivos.items()):

    print("Procesando:", nombre)

    fs, audio = wavfile.read(ruta)

    if audio.ndim > 1:
        audio = audio[:, 0]

    ja, jr, sa, sr, cruces, peaks, audio = jitter_shimmer(audio, fs)

    t = np.linspace(0, len(audio)/fs, len(audio))

    ax = axs[i]

    ax.plot(t, audio, color=color_senal, linewidth=1)

    if len(cruces) > 0:
        ax.scatter(t[cruces], audio[cruces], color=color_cruces, s=10)

    if len(peaks) > 0:
        ax.scatter(t[peaks], audio[peaks], color=color_picos, s=10)

    ax.set_title(nombre)
    ax.set_xlabel("Tiempo [s]")
    ax.set_ylabel("Amplitud")
    ax.grid(True, alpha=0.3)

    resultados.append([nombre, ja, jr, sa, sr])

plt.tight_layout()
plt.show()
```
<img width="1790" height="789" alt="image" src="https://github.com/user-attachments/assets/ab26f90e-aa1b-40af-9247-5e086bebd458" />
<img width="758" height="225" alt="image" src="https://github.com/user-attachments/assets/00867a27-7f9a-492f-a094-b81f387036cd" />
