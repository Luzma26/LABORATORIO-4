# DISEÑO Y CONSTRUCCION DE UNA INCUBADORA NEOONATAL A ESCALA

**Luz Marina Valderrama-5600741.**

## INTRODUCCION


### Objetivo General: 
Reconocer la importancia de las incubadoras neonatales en la salud del neonato.
### Objetivos Específicos
• Identificar las partes principales que componen una incubadora neonatal.

• Desarrollar un sistema que emule el modo de operación de una incubadora neonatal.

• Evaluar cómo impacta el control de variables como temperatura, humedad y flujo de aire en la salud del neonato.

## METODOLOGIA
### Revisión de la actividad electrodérmica



### Consideraciones de seguridad eléctrica



### Selección de la región anatómica



### Montaje del dispositivo

El sistema desarrollado estuvo compuesto principalmente por:

• ESP32.
• Sensor GSR.
• Electrodos.
• Cables de conexión.
• Fuente de alimentación mediante power bank.
• Computador con MATLAB.
• Comunicación Bluetooth.


### Programación de la ESP32

Se utilizó la versión 2.0.11 del paquete ESP32 de Espressif Systems en Arduino IDE.

El programa final se encargó de:

1. Inicializar la comunicación Bluetooth.
2. Configurar el GPIO 34 como entrada analógica.
3. Realizar continuamente la lectura del sensor GSR.
4. Enviar el valor obtenido mediante Bluetooth.
5. Mantener la adquisición durante la prueba.

La estructura fundamental empleada fue:

    #include "BluetoothSerial.h"

    BluetoothSerial SerialBT;

    const int GSR_PIN = 34;
    
    void setup()
    {
        Serial.begin(9600);
        SerialBT.begin("GSR_ESP32");
        analogReadResolution(10);
    }
    
    void loop()
    {
        int gsr = analogRead(GSR_PIN);

    SerialBT.println(gsr);

    delay(20);
    }
La resolución del ADC se estableció en 10 bits, por lo que los valores adquiridos se encontraron aproximadamente entre 0 y 1023 ADC.

La transmisión Bluetooth permitió eliminar la conexión física entre la ESP32 y el computador durante la adquisición final, utilizando la alimentación independiente mediante power bank.

### Comunicación inalámbrica

Una vez programada la ESP32, se estableció la comunicación Bluetooth con el computador mediante el dispositivo denominado:

  GSR_ESP32

El computador creó un puerto COM asociado al enlace Bluetooth. MATLAB utilizó dicho puerto para recibir los datos enviados por la ESP32.

Durante la configuración se presentaron inicialmente problemas con los puertos COM y con la conexión Bluetooth. Finalmente se consiguió establecer comunicación y recibir correctamente los valores GSR en MATLAB.

### Adquisición y procesamiento en MATLAB

MATLAB se utilizó para recibir los datos provenientes de la ESP32, generar la gráfica en tiempo real y almacenar los resultados.

El programa se configuró para adquirir aproximadamente:

  Fs = 50Hz

durante:

T=60s

por lo que se esperaban aproximadamente:

N=FsT = 50(60)=3000

muestras.

Los datos fueron almacenados en vectores de tiempo y GSR.

Posteriormente se aplicó un suavizado mediante media móvil:

    xf = movmean(x,10);

Este procesamiento permitió reducir pequeñas fluctuaciones rápidas y facilitar la identificación de la tendencia general de la señal.

A partir de la señal suavizada se estimó la componente tónica:

    SCL = mean(xf);

y se calculó una componente transitoria aproximada:

    SCR = xf - SCL;

De esta forma fue posible observar tanto la variación lenta de la señal como las respuestas transitorias.

### Calibración

Antes de realizar la prueba final se realizaron mediciones en reposo y durante actividades destinadas a producir cambios en la activación fisiológica.

Durante las primeras pruebas se observaron valores cercanos a:

• Mínimo: 934 ADC
• Máximo: 971 ADC
• Promedio: 947,85 ADC
• SCL: 947,85 ADC
• SCR máxima: 7,95 ADC

Posteriormente se realizaron pruebas adicionales para establecer rangos de clasificación.

Los datos experimentales mostraron que el valor absoluto de GSR podía desplazarse durante la prueba. Por este motivo, la clasificación no se interpretó únicamente como un valor fisiológico universal, sino como una clasificación experimental relativa a la línea base del sujeto.

Esto es importante porque el nivel de conductancia puede variar entre individuos y también puede verse afectado por temperatura, humedad, movimiento, presión de los electrodos y condiciones de contacto [8], [9].

### Clasificación del nivel de estrés

Se implementó una clasificación experimental en MATLAB mediante tres niveles:

• POCO
• MODERADO
• ELEVADO

Los umbrales se determinaron a partir de los valores observados durante las pruebas de calibración y posteriormente se utilizaron en la prueba final.

El programa recibía cada muestra y determinaba el nivel correspondiente. En la consola se mostraba, por ejemplo:

      GSR: 928 ADC   Nivel: MODERADO
      GSR: 916 ADC   Nivel: POCO
      GSR: 893 ADC   Nivel: ELEVADO

La clasificación se utilizó como indicador relativo de activación fisiológica y no como diagnóstico clínico de estrés.
## RESULTADOS
### Adquisición de la señal GSR

El sistema consiguió adquirir la señal GSR mediante la ESP32 y transmitirla inalámbricamente al computador.

Durante las primeras mediciones se observaron valores aproximadamente entre 934 y 971 ADC en condiciones de reposo.

En las pruebas posteriores se observaron variaciones mayores. En una de las adquisiciones se registraron valores desde aproximadamente 1002 ADC hasta 939 ADC, mostrando una variación suficiente para diferenciar diferentes estados de activación.

### Respuesta durante reposo y respiración

Durante las pruebas de respiración se observó que una inspiración rápida producía un cambio apreciable en la señal. Sin embargo, después del pico la señal no siempre regresaba exactamente al mismo valor inicial.

Este comportamiento no necesariamente representa un error del sistema. La componente tónica de la EDA puede presentar cambios lentos y la recuperación de una respuesta fásica puede tomar tiempo [4]. Además, la señal puede verse afectada por la respiración, movimiento, contacto de los electrodos y otros factores fisiológicos [8].

Por lo tanto, el retorno de la señal no debe interpretarse obligatoriamente como una recuperación instantánea hasta exactamente el mismo valor de reposo.

### Prueba de actividad cognitiva

Durante la prueba final se observó un comportamiento más acorde con el objetivo de la práctica.

En la consola se obtuvieron secuencias como:

    GSR: 935 ADC   Nivel: ELEVADO
    GSR: 928 ADC   Nivel: MODERADO
    GSR: 916 ADC   Nivel: POCO

Durante los momentos en los que se realizó una actividad mental que requería mayor concentración se observaron cambios en el nivel de la señal y clasificación como ELEVADO.

Posteriormente, al disminuir la demanda cognitiva, la señal descendió gradualmente y pasó por los niveles MODERADO y POCO.

Esto permitió observar cambios tanto ascendentes como descendentes de la señal durante una misma adquisición.

### Resultados de la prueba final

Los resultados registrados fueron:

  RESULTADOS PRUEBA GSR

Parámetro	              Resultado
Línea base	             992,03 ADC
Mínimo                   939 ADC
Máximo	                 1002 ADC
Promedio	               958,40 ADC

Los datos fueron almacenados en:

• GSR_prueba_final.csv
• GSR_prueba_final.mat

La diferencia entre el máximo y mínimo registrado fue:

ΔGSR=1002−939
ΔGSR=63 ADC
	​
Este rango muestra que durante la prueba se produjo una variación considerable respecto al rango de fluctuación observado durante algunas condiciones de reposo.

### Componentes SCL y SCR

La señal presentó una variación lenta de su nivel basal, correspondiente a la componente tónica o SCL, sobre la cual aparecieron variaciones rápidas asociadas con la componente fásica o SCR.

Este comportamiento coincide con la descripción utilizada habitualmente en el análisis de EDA, donde el SCL representa cambios lentos mientras que las SCR representan respuestas transitorias ante eventos o estímulos [3], [4].

## ANÁLISIS DE RESULTADOS
  Análisis 1. Eficacia del sistema para monitoreo ambulatorio

El sistema desarrollado permitió realizar una medición continua y portátil de la respuesta galvánica cutánea y transmitir los datos de manera inalámbrica.

Una de las principales ventajas fue que la ESP32 pudo funcionar alimentada mediante una power bank, evitando que el sujeto permaneciera conectado físicamente al computador.

Los sistemas portátiles de EDA son una alternativa utilizada para realizar mediciones fuera de condiciones estrictamente de laboratorio; sin embargo, la calidad de la señal puede verse afectada por movimiento, colocación de electrodos, temperatura, humedad y otros factores [8], [10].

Por lo tanto, el dispositivo desarrollado resulta apropiado como prototipo académico de monitoreo, aunque sería necesario realizar una validación más rigurosa antes de considerarlo un sistema de medición clínica.

  Análisis 2. Alcance y limitaciones del sistema

El principal alcance del sistema es permitir observar cambios relativos de la actividad electrodérmica durante diferentes situaciones.

Los resultados obtenidos muestran que el sistema puede identificar cambios durante actividades cognitivas. Esto es consistente con investigaciones donde la EDA ha mostrado sensibilidad ante tareas que producen estrés cognitivo [11].

Sin embargo, la GSR no mide directamente el estrés psicológico. La señal representa principalmente cambios relacionados con la activación autonómica y puede verse modificada por diferentes factores fisiológicos y ambientales [1], [8].

Por ello, un aumento de GSR debe interpretarse como un aumento de activación fisiológica, y no necesariamente como una prueba definitiva de estrés.

### ¿Por qué la señal tarda en regresar a su nivel inicial?

Durante las pruebas se observó que, después de producirse una respuesta elevada, la señal podía permanecer durante un tiempo en un nivel diferente antes de regresar hacia valores menores.

Esto puede explicarse por la propia dinámica de la EDA. Las respuestas fásicas presentan una fase de aumento y posteriormente una fase de recuperación, mientras que el componente tónico puede cambiar lentamente durante períodos de varios segundos o minutos [4].

Además, la recuperación puede verse afectada por el estado fisiológico del sujeto y por las condiciones de medición.

Por tanto, que la señal no vuelva inmediatamente al valor inicial no implica necesariamente que el sensor esté funcionando incorrectamente.

### Relación entre actividad cognitiva y GSR

Durante la prueba final se observó que, al realizar actividades que requerían mayor concentración, aparecieron periodos clasificados como ELEVADO.

Posteriormente se observaron periodos MODERADOS y POCO cuando la activación disminuía.

Este resultado es compatible con investigaciones que han demostrado sensibilidad de la EDA ante el estrés cognitivo [11]. La explicación fisiológica propuesta es que una tarea mental exigente puede aumentar la activación del sistema nervioso simpático, lo cual puede modificar la actividad de las glándulas sudoríparas y producir cambios en la conductancia cutánea [1], [3].

No obstante, la respuesta no debe atribuirse exclusivamente al estrés, debido a que movimiento, respiración y otros factores también pueden generar variaciones en la EDA [8].

### Influencia del movimiento

La señal presentó fluctuaciones durante algunas pruebas. Esto es especialmente relevante porque el sistema fue diseñado como dispositivo vestible.

Los movimientos pueden modificar la presión y el contacto entre los electrodos y la piel, produciendo cambios que no necesariamente corresponden a una respuesta fisiológica real. La literatura identifica específicamente el movimiento como una de las fuentes importantes de artefactos en mediciones portátiles de EDA [8].

Esto representa una de las principales limitaciones del prototipo.

### Hipótesis fisiológica

A partir de los resultados obtenidos se plantea la siguiente hipótesis:

Cuando una persona realiza una actividad que requiere mayor concentración o genera una mayor activación emocional, aumenta la activación del sistema nervioso simpático, lo que puede incrementar la actividad de las glándulas sudoríparas y producir cambios detectables en la conductancia de la piel.

Esta hipótesis es coherente con la utilización de la EDA como indicador periférico de activación simpática [1], [3].

## CONCLUSIONES

## REFERENCIAS

[1] A. H. C. van der Kraaij et al., “Role and Status of Biomarkers in Technostress Research: A Systematic Review,” International Journal of Environmental Research and Public Health, 2024.

[2] 
