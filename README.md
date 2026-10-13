# DISEÑO Y CONSTRUCCIÓN DE UNA INCUBADORA NEONATAL A ESCALA

**Luz Marina Valderrama – 5600741**

## INTRODUCCIÓN

Las incubadoras neonatales son equipos biomédicos diseñados para proporcionar un ambiente controlado a los recién nacidos que requieren asistencia para mantener una temperatura corporal adecuada. Su importancia es especialmente relevante en neonatos prematuros, debido a que su capacidad para regular la temperatura puede ser limitada. Por esta razón, el control de variables como la temperatura, la humedad y la circulación del aire es fundamental para favorecer condiciones ambientales apropiadas [1], [2].

Las incubadoras comerciales integran diferentes componentes, como una cubierta transparente, sensores de temperatura, sistemas de calentamiento, ventiladores, controladores electrónicos y alarmas. Dependiendo del modelo, también pueden incorporar mecanismos de humidificación, monitorización clínica y medición del peso [3], [4]. Estos elementos permiten mantener condiciones ambientales controladas y supervisar el funcionamiento del equipo.

En esta práctica se desarrolló un prototipo a escala de una incubadora neonatal mediante un Arduino Uno, un sensor DHT11, un sensor de fuerza FSR400, una pantalla OLED, tres LED indicadores y dos ventiladores. El sistema permite adquirir variables ambientales, visualizar las mediciones y activar un ventilador cuando la temperatura supera el límite establecido. También se incorporó un sensor de fuerza para estimar el peso mediante los cambios de una señal eléctrica.

El proyecto se dividió en dos partes. En la primera se revisaron los componentes de una incubadora neonatal y se plantearon los principios de los circuitos de control térmico y medición del peso. En la segunda se construyó el modelo físico a escala y se integraron los componentes electrónicos. Durante las pruebas de funcionamiento se comprobó que los ventiladores, los indicadores luminosos y la pantalla OLED respondían según lo programado. Asimismo, el sensor FSR400 presentó un error aproximado del 9 % en la estimación del peso. Se observó que la incubadora tardaba en calentarse, lo que evidencia la necesidad de mejorar el comportamiento térmico del prototipo. Además, el bombillo utilizado como fuente de calor no está regulado automáticamente por el Arduino, por lo que el montaje constituye una aproximación académica al funcionamiento de una incubadora y no un equipo apto para uso clínico.

## OBJETIVOS

### Objetivo general

Reconocer la importancia de las incubadoras neonatales en la salud del neonato.

### Objetivos específicos

- Identificar las partes principales que componen una incubadora neonatal.
- Desarrollar un sistema que emule el modo de operación de una incubadora neonatal.
- Evaluar cómo impacta el control de variables como temperatura, humedad y flujo de aire en la salud del neonato.

## METODOLOGÍA

### PARTE A. DISEÑO Y SIMULACIÓN DEL SISTEMA

### Identificación de las partes principales de una incubadora neonatal

Se realizó una revisión del funcionamiento de las incubadoras neonatales para identificar los componentes que permiten mantener condiciones ambientales controladas. Entre los elementos principales se encuentra la cubierta transparente, cuya función es permitir la observación del neonato y reducir el intercambio directo de aire con el ambiente exterior.

La cámara interna constituye el espacio donde se mantiene el ambiente controlado. El sistema de calentamiento proporciona la energía térmica necesaria para elevar la temperatura, mientras que los ventiladores favorecen la circulación del aire y ayudan a distribuir el calor. Los sensores permiten medir las condiciones internas y enviar información al controlador electrónico, que utiliza las mediciones para determinar las acciones de regulación.

Otros componentes importantes son el panel de control, las alarmas y los sistemas de monitorización. En equipos comerciales, estos elementos permiten detectar condiciones fuera de los límites establecidos y alertar al personal encargado. Dependiendo del modelo, también pueden incorporarse sistemas de humidificación y dispositivos para medir el peso [3], [4].

La revisión permitió reconocer que el funcionamiento de una incubadora no depende únicamente de producir calor, sino de medir las condiciones internas, regularlas y detectar posibles fallas. Por ello, el sistema de control y los sensores deben funcionar de manera coordinada.

### Diseño del circuito de control de temperatura

La guía solicita diseñar y simular un circuito eléctrico de control de lazo cerrado capaz de mantener la temperatura entre 36 °C y 37,5 °C. Para este propósito se planteó un sistema compuesto por un sensor de temperatura, un controlador, un elemento calefactor y un ventilador.

El sensor mide continuamente la temperatura interna y envía la información al controlador. Este compara la temperatura medida con el intervalo de referencia y determina las acciones necesarias para regularla. El ventilador permite favorecer la circulación del aire y contribuir a disminuir la temperatura cuando se supera el límite establecido.

El funcionamiento por realimentación puede representarse mediante el siguiente esquema:

Temperatura de referencia → Comparación con la temperatura medida → Controlador → Calefactor y ventilador → Temperatura de la cabina → Sensor de temperatura → Realimentación al controlador.

El intervalo de referencia establecido para el prototipo es de 36 °C a 37,5 °C. La diferencia entre la temperatura de referencia y la temperatura medida se expresa como:

e(t) = T_ref - T_medida

Donde e(t) representa el error de temperatura, T_ref es la temperatura de referencia y T_medida corresponde a la temperatura registrada por el sensor.

Para la implementación se utilizó un sensor DHT11 conectado al Arduino Uno. La lógica de funcionamiento contempla tres estados: temperatura baja, temperatura dentro del intervalo y temperatura alta.

Cuando la temperatura es inferior a 36 °C, se enciende el LED azul. Cuando está entre 36 °C y 37,5 °C, se enciende el LED verde. Si supera los 37,5 °C, se enciende el LED rojo y se activa el ventilador superior.

El ventilador se desactiva cuando la temperatura desciende por debajo de 36 °C. Esta diferencia entre los umbrales de activación y desactivación introduce una histéresis que reduce los cambios constantes de estado cuando la temperatura se encuentra cerca del límite.

Durante las pruebas se comprobó que la pantalla OLED indicaba el estado del ventilador y que este respondía a la lógica de control programada. Sin embargo, el montaje construido no constituye un sistema de control térmico completo de lazo cerrado, porque el bombillo permanece encendido y no es regulado automáticamente por el Arduino. El sistema puede medir la temperatura y activar la ventilación, pero no puede modificar directamente la potencia de calentamiento. Por tanto, para cumplir completamente el requisito de la guía se necesitaría incorporar un actuador que permita regular el calentador y verificar su respuesta mediante pruebas experimentales.

### Diseño del circuito de medición del peso

La guía solicita diseñar un circuito capaz de medir el peso y mostrarlo mediante una pantalla LCD o displays de siete segmentos. Para explorar esta función se utilizó un sensor de fuerza FSR400, cuyo principio de funcionamiento se basa en la variación de su resistencia eléctrica cuando se aplica una fuerza sobre su superficie.

El sensor se conectó mediante un divisor de tensión. Una de sus terminales se conectó a 5 V y la otra al nodo de lectura analógica A0 del Arduino Uno. Desde este nodo se conectó una resistencia de 10 kΩ a tierra. De esta forma, los cambios de resistencia del sensor producen variaciones de tensión que pueden ser adquiridas por el microcontrolador.

La lectura analógica obtenida se utilizó para estimar el peso aplicado. Para realizar esta conversión se empleó una relación de calibración que permitió expresar el resultado en gramos y compararlo con una masa de referencia. Durante las pruebas se obtuvo un error aproximado del 9 %, por lo que la estimación fue relativamente cercana al valor patrón, aunque no completamente exacta.

La información se mostró mediante una pantalla OLED con comunicación I²C. Aunque la guía contempla una pantalla LCD o displays de siete segmentos, la OLED utilizada permitió visualizar caracteres, valores numéricos, temperatura, lectura del sensor y estado del ventilador. Por tanto, cumplió una función de visualización equivalente para el prototipo, pero no correspondió literalmente a los tipos de pantalla indicados en la guía.

### PARTE B. CONSTRUCCIÓN DEL MODELO A ESCALA

### Construcción de la estructura

Se construyó una cámara a escala con una cubierta transparente para permitir observar el interior. La estructura representa el espacio de una incubadora neonatal y permite ubicar el sistema de calentamiento, los ventiladores, el sensor ambiental y los componentes electrónicos.

La guía establece unas dimensiones aproximadas de 90 cm × 40 cm × 45 cm. Las dimensiones finales del prototipo corresponden a las de la estructura construida y pueden verificarse mediante medición directa.

La cubierta permite observar el interior y proporciona una barrera física frente al ambiente exterior. El diseño busca reducir la pérdida de calor y facilitar el acceso al espacio interno durante las pruebas.

### Materiales y equipos

Para el montaje se utilizaron los siguientes elementos:

- Arduino Uno.
- Sensor DHT11.
- Sensor de fuerza FSR400.
- Pantalla OLED I²C.
- Tres LED indicadores.
- Dos ventiladores de 5 V.
- Transistor TIP122.
- Diodo 1N4007.
- Resistencias.
- Protoboard.
- Cables jumper.
- Fuente regulada de laboratorio de 5 V.
- Bombillo como fuente de calor.
- Estructura transparente.

<img width="837" height="713" alt="image" src="https://github.com/user-attachments/assets/b90a8368-ab59-4f9b-842d-059e72737a09" />

Fig.1. Montaje y cubierta de la incubadora 

El Arduino Uno se utilizó para adquirir las señales y controlar las salidas digitales. El DHT11 midió la temperatura y la humedad, mientras que el FSR400 detectó cambios de fuerza. La pantalla OLED mostró los valores y los LED indicaron el estado térmico.

El ventilador inferior permaneció encendido para favorecer la circulación del aire, mientras que el ventilador superior se controló mediante el transistor TIP122. El bombillo proporcionó calor, pero en el montaje permaneció independiente del control automático.

### Consideraciones de seguridad eléctrica

El circuito electrónico se alimentó mediante una fuente regulada de 5 V de corriente continua. Antes de conectar los componentes, se verificó la polaridad de la alimentación y se comprobó que no existieran cortocircuitos entre los rieles de la protoboard.

El bombillo se mantuvo separado del circuito de baja tensión. Debido a que su alimentación puede involucrar tensión de red, cualquier conexión correspondiente debe ser realizada o verificada por una persona cualificada y bajo supervisión del docente.

El sistema no debe dejarse funcionando sin vigilancia debido al riesgo de sobrecalentamiento. Además, el prototipo no debe utilizarse para mantener las condiciones térmicas de un recién nacido. Las pruebas se realizaron exclusivamente con fines académicos y en condiciones controladas.

### Montaje del circuito electrónico

El montaje se realizó sobre una protoboard, utilizando el Arduino Uno como unidad central de procesamiento. La alimentación de 5 V se distribuyó a los rieles de la protoboard y se estableció una tierra común para los componentes de baja tensión.

El sensor DHT11 se conectó a 5 V y GND, mientras que su salida de datos se conectó al pin digital D2 del Arduino. Esta conexión permitió obtener las lecturas de temperatura y humedad utilizadas por el programa.

El sensor FSR400 se conectó mediante un divisor de tensión: una terminal a 5 V y la otra al nodo analógico A0. Desde este nodo se conectó una resistencia de 10 kΩ a GND.

La pantalla OLED se conectó mediante el bus I²C, utilizando A4 como línea SDA y A5 como línea SCL. La alimentación y la tierra se conectaron a los terminales correspondientes.

Los LED azul, verde y rojo se conectaron a los pines digitales D8, D9 y D10, respectivamente, cada uno con una resistencia limitadora de corriente. El LED azul indicó una temperatura inferior al intervalo de referencia, el verde representó una temperatura dentro del intervalo y el rojo indicó que se había superado el límite superior. Durante las pruebas, los indicadores luminosos respondieron correctamente a la programación.

El ventilador inferior se conectó para permanecer encendido durante el funcionamiento del sistema. Para controlar el ventilador superior se utilizó un transistor TIP122 como elemento de conmutación, con una resistencia en la base y un diodo 1N4007 para proteger el circuito frente a los transitorios eléctricos generados por el motor. La conexión permitió que el ventilador funcionara según la lógica de control programada.

<img width="787" height="672" alt="image" src="https://github.com/user-attachments/assets/459171bc-0078-4549-a398-9d7412bd48f7" />

Fig.2. Conexión de la parte electronica
### Programación del Arduino Uno

El programa del Arduino se diseñó para leer las variables ambientales, presentar la información en la pantalla OLED y controlar los indicadores luminosos y el ventilador superior.

La temperatura medida se compara con el intervalo de referencia comprendido entre 36 °C y 37,5 °C. Cuando la temperatura es inferior a 36 °C, se enciende el LED azul. Cuando la temperatura se encuentra dentro del intervalo, se enciende el LED verde. Si la temperatura supera los 37,5 °C, se enciende el LED rojo y se activa el ventilador superior.

El ventilador se desactiva cuando la temperatura desciende por debajo de 36 °C. Esta lógica permite observar cómo una medición puede utilizarse para modificar el estado de un actuador.

La pantalla OLED muestra la temperatura, la lectura analógica del FSR400, la estimación del peso y el estado del ventilador. Durante las pruebas se comprobó que la información se presentaba correctamente y que la pantalla indicaba cuándo el ventilador debía encenderse o apagarse.

La estimación del peso se mostró en gramos y se comparó con una masa patrón. El error obtenido fue aproximadamente del 9 %, lo que indica que el sistema produjo una estimación relativamente cercana al valor de referencia, aunque todavía presenta diferencias que pueden reducirse mediante una calibración más precisa.

Es importante señalar que el bombillo no se encuentra conectado a una salida de control del Arduino. Por esta razón, el programa puede activar la ventilación, pero no puede regular automáticamente la potencia de calentamiento.

### Adquisición de temperatura y humedad

Durante las pruebas, el sensor DHT11 permitió obtener las mediciones ambientales de la cámara. Los valores se mostraron en la pantalla OLED, lo que facilitó observar los cambios producidos por el calentamiento y la circulación del aire.

Se comprobó que el sistema respondía a las condiciones de temperatura mediante los indicadores luminosos y la activación del ventilador superior. También se observó que la incubadora tardaba en calentarse, lo que indica que el proceso de aumento de temperatura no era inmediato.

Este comportamiento puede relacionarse con la capacidad de calentamiento del bombillo, las pérdidas de calor hacia el ambiente, el aislamiento de la estructura y la circulación del aire. Debido a que el calentador no estaba regulado automáticamente por el Arduino, el sistema no podía ajustar directamente la potencia térmica para alcanzar el intervalo de referencia con mayor rapidez.

El DHT11 presenta limitaciones de precisión y tiempo de respuesta en comparación con sensores especializados para equipos médicos. Por lo tanto, las mediciones obtenidas se interpretaron como parte de una práctica académica y no como una validación de precisión clínica.

### Estimación del peso mediante el FSR400

El FSR400 es un sensor resistivo cuya resistencia cambia cuando se aplica una fuerza sobre su superficie. En el circuito, este cambio produce una variación de tensión que se lee mediante la entrada analógica A0 del Arduino.

Para establecer una relación entre la lectura analógica y el peso, se realizó una calibración experimental con una masa patrón. El sistema permitió expresar la estimación en gramos y compararla con el valor de referencia.

Durante las pruebas se obtuvo un error aproximado del 9 %. Este resultado indica que la estimación fue relativamente cercana al peso patrón y que el sensor permitió reconocer la carga aplicada. Sin embargo, la diferencia observada evidencia que la medición todavía puede mejorarse.

El error absoluto de la estimación se calcula mediante:

E_a = |m_estimada - m_patron|

El error relativo porcentual se calcula mediante:

E_r(%) = (E_a / m_patron) × 100

Donde m_estimada es la masa estimada por el sistema y m_patron es la masa conocida utilizada como referencia. El error relativo no se calcula cuando la masa patrón es cero.

El error relativo obtenido durante la práctica fue aproximadamente:

E_r ≈ 9 %

La exactitud de la estimación depende de la distribución de la carga, el contacto mecánico con el sensor, la repetibilidad de las mediciones y el intervalo de masas utilizado durante la calibración. Debido a estas limitaciones, el FSR400 no equivale a una balanza clínica.

<img width="838" height="747" alt="image" src="https://github.com/user-attachments/assets/4306e527-c46f-469c-9598-07a8af4fdd07" />

Fig.3. Ubicación sensor FSR400

### Cálculo de costos

Para determinar el costo total del prototipo se deben registrar los precios reales de los componentes y sumar los costos parciales de cada elemento. Se deben incluir el Arduino Uno, el sensor DHT11, el FSR400, la pantalla OLED, los ventiladores, el transistor TIP122, el diodo 1N4007, los LED, las resistencias, la protoboard, los cables, la fuente de alimentación y los materiales de la estructura.

El costo total se obtiene mediante la expresión:

C_total = C_Arduino + C_DHT11 + C_FSR400 + C_OLED + C_ventiladores + C_componentes + C_protoboard + C_estructura + C_bombillo

C_total = 32.000 + 8.000 + 28.000 + 23.000 + 18.000 + 12.000 + 8.000 + 20.000 + 8.000

C_total = $157.000 COP


### Evaluación del sistema

La evaluación del prototipo se realizó mediante pruebas de funcionamiento de sus componentes y del sistema integrado. Se comprobó que el sensor ambiental entregara lecturas, que la pantalla mostrara los valores, que los LED respondieran al intervalo de temperatura programado y que el ventilador superior se activara o desactivara de acuerdo con la lógica establecida.

También se evaluó el sensor FSR400 mediante la comparación de la masa estimada con una masa patrón. El error obtenido fue aproximadamente del 9 %, lo que permitió establecer una referencia cuantitativa del desempeño del sistema de medición del peso.

En las pruebas se observó que los componentes electrónicos funcionaban según lo planeado. Sin embargo, el calentamiento de la cámara era lento y el sistema no contaba con una regulación automática del bombillo. Por esta razón, aunque el sistema respondía a los cambios de temperatura mediante los indicadores y el ventilador, todavía presentaba limitaciones para mantener las condiciones térmicas de forma precisa.

## RESULTADOS

### Funcionamiento general del prototipo

El prototipo integró los principales componentes electrónicos previstos para representar algunas funciones de una incubadora neonatal a escala. El Arduino Uno actuó como unidad de procesamiento, el sensor DHT11 permitió adquirir la temperatura y la humedad, la pantalla OLED facilitó la visualización de las mediciones y los LED indicaron el estado térmico de la cámara.

El sistema incorporó dos ventiladores con funciones diferenciadas. El ventilador inferior permaneció encendido para favorecer la circulación del aire, mientras que el ventilador superior respondió a la temperatura medida por el sensor. De esta forma, el montaje permitió representar una estrategia básica de control ambiental mediante sensores y actuadores.

Durante las pruebas se comprobó que los ventiladores y los indicadores luminosos funcionaban correctamente. La pantalla OLED también mostró la información prevista e indicó cuándo el ventilador debía encenderse o apagarse. Estos resultados evidenciaron que la integración entre el Arduino, los sensores, la pantalla y las salidas de control funcionó según lo planeado.

No obstante, se observó que la incubadora tardaba en calentarse. Este comportamiento mostró que el proceso de calentamiento dependía de las características de la fuente de calor, el aislamiento de la estructura y las condiciones de circulación del aire. Además, el bombillo no estaba regulado automáticamente por el Arduino, por lo que el sistema no podía ajustar directamente la potencia de calentamiento.

### Respuesta del sistema ante los cambios de temperatura

El programa estableció tres estados de funcionamiento según la temperatura medida. Por debajo de 36 °C, el sistema indicó una condición de temperatura baja mediante el LED azul. Entre 36 °C y 37,5 °C, el LED verde representó el intervalo establecido como referencia. Por encima de 37,5 °C, se encendió el LED rojo y se activó el ventilador superior.

Durante las pruebas se observó que los indicadores luminosos respondían correctamente a la lógica programada y que la pantalla OLED mostraba el estado del ventilador. Cuando la temperatura alcanzaba el umbral superior, el sistema indicaba que debía activarse la ventilación, y cuando descendía por debajo del umbral inferior, indicaba que debía apagarse.

Este comportamiento demostró que una medición ambiental puede utilizarse para modificar el estado de un actuador. Sin embargo, la activación del ventilador no garantiza por sí sola que la temperatura regrese al intervalo deseado, ya que el resultado depende de la potencia del bombillo, el aislamiento de la cámara, la circulación del aire y las condiciones ambientales.

Una limitación importante es que el bombillo permaneció encendido sin regulación automática. En consecuencia, el prototipo no dispuso de un control completo del calentamiento y la refrigeración. Aunque los componentes respondieron según lo programado, se observó que el calentamiento de la cámara era lento.

Para mejorar este comportamiento, sería necesario incorporar un sistema que permita regular automáticamente el calentador, mejorar el aislamiento térmico y evaluar la distribución del calor dentro de la cámara. También sería conveniente registrar la temperatura durante un periodo definido para cuantificar el tiempo requerido para alcanzar el intervalo de referencia y determinar cuánto tiempo permanece dentro de él.

### Adquisición de humedad

El sensor DHT11 permitió obtener una lectura de humedad relativa junto con la temperatura. Esta medición resultó útil para observar las condiciones ambientales dentro de la cámara y reconocer que la humedad constituye una variable importante en el cuidado neonatal.

No obstante, el prototipo no incorporó un humidificador ni un mecanismo automático para regular la humedad. Por esta razón, el sistema podía medir y mostrar esta variable, pero no garantizar que permaneciera dentro de un intervalo específico.

### Lectura del sensor FSR400

El sensor FSR400 permitió detectar cambios en la fuerza aplicada mediante variaciones de su resistencia eléctrica. La conexión con el divisor de tensión permitió convertir esos cambios en lecturas analógicas que el Arduino procesó para estimar el peso.

Durante las pruebas, el sistema mostró el peso estimado en gramos y se comparó el resultado con una masa patrón. Se obtuvo un error aproximado del 9 %, por lo que el valor calculado fue relativamente cercano al valor de referencia.

Este resultado indica que el sensor pudo utilizarse para realizar una estimación del peso dentro de las condiciones de la práctica. Sin embargo, la diferencia entre el valor estimado y el patrón evidencia que la medición no fue completamente exacta.

El error puede estar relacionado con la respuesta no lineal del FSR400, la calibración utilizada, la distribución de la carga y la forma en que la fuerza se transmite al sensor. Para mejorar la medición, sería conveniente utilizar varias masas patrón, repetir las mediciones y construir una curva de calibración que represente mejor la respuesta del sensor.

A pesar del error observado, el resultado permitió comprobar que el FSR400 puede integrarse con el Arduino y una pantalla OLED para desarrollar un sistema experimental de estimación del peso. Sin embargo, este montaje no debe considerarse equivalente a una balanza clínica calibrada.

### Comparación general con una incubadora comercial

El prototipo permite demostrar principios de medición, visualización y control, pero presenta diferencias importantes frente a las incubadoras comerciales. Equipos como la Dräger Isolette 8000 plus incorporan funciones avanzadas de control térmico, monitorización y gestión de humedad, según su configuración [3], [4].

La incubadora YP2000 comercializada por Instrumentalia ofrece funciones de control ambiental y una estructura destinada al cuidado neonatal [5]. Por su parte, la incubadora B3 de LEEX Medical incorpora funciones comerciales de control de temperatura y humidificación, según su configuración [6].

En comparación con estos equipos, el prototipo académico tiene la ventaja de permitir la modificación del programa y la observación directa del funcionamiento de los sensores y actuadores. Durante la práctica se comprobó que los ventiladores, los indicadores luminosos y la pantalla funcionaban según lo planeado, y que el sensor FSR400 permitía estimar el peso con un error aproximado del 9 %.

Sin embargo, el prototipo no cuenta con regulación automática del calentamiento, control activo de humedad, alarmas clínicas ni un sistema de medición del peso validado. Además, el calentamiento lento de la cámara muestra que todavía existen aspectos por mejorar en el desempeño térmico.

Por lo tanto, los resultados obtenidos permiten estudiar el funcionamiento básico de una incubadora, pero no demuestran que el sistema tenga la precisión, confiabilidad o seguridad necesarias para sustituir un equipo médico.

## 13. ANÁLISIS DE RESULTADOS

### Análisis 1. Eficacia del sistema para controlar la temperatura de la cabina y error en el cálculo del peso

El sistema desarrollado permitió representar algunos de los principios de funcionamiento de una incubadora neonatal mediante la adquisición de temperatura y humedad con el sensor DHT11, la visualización de los datos en una pantalla OLED y la activación de indicadores luminosos y ventiladores mediante un Arduino Uno. Durante las pruebas se comprobó que los componentes electrónicos funcionaban según lo planeado y que la pantalla OLED indicaba cuándo el ventilador debía encenderse o apagarse.

Para evaluar la respuesta térmica, se estableció como referencia un intervalo de 36 °C a 37,5 °C. Cuando la temperatura era inferior a 36 °C, se encendía el LED azul; cuando se encontraba dentro del intervalo, se encendía el LED verde; y cuando superaba los 37,5 °C, se encendía el LED rojo y se activaba el ventilador superior. El ventilador inferior permaneció encendido para favorecer la circulación del aire.

Este comportamiento demostró una estrategia básica de control por realimentación, ya que la temperatura medida se utilizó para determinar el estado del ventilador y de los indicadores luminosos. Sin embargo, durante las pruebas se observó que la incubadora tardaba en calentarse. Esto evidencia que, aunque el sistema de ventilación respondía correctamente, el proceso de calentamiento no era inmediato.

El tiempo requerido para elevar la temperatura puede estar relacionado con la potencia de la fuente de calor, las pérdidas térmicas hacia el ambiente, el aislamiento de la cámara y la circulación del aire. Una limitación importante es que el bombillo utilizado como fuente de calor permanecía encendido y no estaba regulado automáticamente por el Arduino. Por esta razón, el sistema podía responder al aumento de temperatura mediante ventilación, pero no podía reducir directamente la potencia de calentamiento.

En consecuencia, se comprobó el funcionamiento de los elementos de indicación y ventilación, pero no puede afirmarse que la cabina mantuviera una temperatura constante dentro del intervalo de referencia durante todo el periodo de prueba. Para cuantificar la eficacia térmica sería necesario registrar la temperatura a intervalos regulares y calcular el porcentaje de mediciones que permanecen dentro del intervalo deseado.

Este porcentaje puede determinarse mediante:

P(%) = (N_dentro / N_total) × 100

Donde N_dentro corresponde al número de mediciones entre 36 °C y 37,5 °C, y N_total representa el número total de mediciones registradas.

En cuanto a la estimación del peso, el FSR400 permitió detectar cambios en la fuerza aplicada mediante variaciones de su resistencia eléctrica. La señal analógica obtenida por el Arduino se convirtió en una estimación expresada en gramos y se comparó con una masa patrón.

Durante las pruebas se obtuvo un error relativo aproximado del 9 %. Este resultado indica que el peso estimado fue relativamente cercano al valor de referencia, aunque presentó una diferencia que puede mejorarse mediante una calibración más precisa.

El error absoluto de la estimación se determina mediante:

E_a = |m_estimada - m_patron|

El error relativo porcentual se calcula mediante:

E_r(%) = (E_a / m_patron) × 100

Donde m_estimada corresponde a la masa estimada por el sistema y m_patron representa la masa conocida utilizada como referencia. El error relativo no se calcula cuando la masa patrón es cero.

El resultado del 9 % evidencia que el sensor permitió realizar una estimación funcional del peso, pero también muestra que la medición no fue completamente exacta. La diferencia puede estar relacionada con la calibración, la distribución de la fuerza sobre el sensor, la repetibilidad de las mediciones y las características propias del FSR400.

En general, los resultados permitieron comprobar el funcionamiento de los principales componentes del prototipo. Los ventiladores y los indicadores luminosos respondieron según lo programado, la pantalla OLED mostró la información prevista y el sensor de fuerza produjo una estimación relativamente cercana al peso patrón. No obstante, el calentamiento lento y el error de medición evidencian la necesidad de mejorar la regulación térmica y la calibración del sistema de pesaje.

### Análisis 2. Comparación del sistema desarrollado con los equipos comerciales

El prototipo construido se diferencia de las incubadoras comerciales principalmente por su complejidad, precisión, seguridad y cantidad de variables controladas. Mientras que el sistema académico utiliza componentes electrónicos para demostrar principios básicos de instrumentación, los equipos comerciales están diseñados para proporcionar condiciones ambientales controladas y ofrecer funciones específicas para la atención neonatal.

La incubadora Dräger Isolette 8000 plus incorpora funciones avanzadas de control térmico, monitorización y gestión de humedad según su configuración, además de opciones de pesaje. Estas características permiten un control más completo que el del prototipo, aunque su adquisición y mantenimiento requieren proveedores especializados [3], [4].

Por su parte, la incubadora YP2000 comercializada por Instrumentalia cuenta con una estructura y funciones destinadas al control ambiental neonatal [5]. Durante la consulta comercial se identificó un precio publicado de aproximadamente $53.391.350 COP, incluidos impuestos. Este valor debe confirmarse con el proveedor, ya que puede variar según la disponibilidad, la configuración y las condiciones de venta.

La incubadora B3 de LEEX Medical también ofrece funciones comerciales de control de temperatura y humidificación, según la configuración del equipo. Su precio debe solicitarse directamente al fabricante o distribuidor [6].

Frente a estas alternativas, el prototipo académico presenta como ventaja su bajo costo relativo y la posibilidad de modificar el programa para observar la relación entre las mediciones de los sensores y la respuesta de los actuadores. Además, permite aplicar conocimientos de electrónica, programación, adquisición de señales y control de variables físicas.

Los resultados obtenidos durante el laboratorio muestran que fue posible integrar los sensores, los indicadores luminosos, la pantalla OLED y los ventiladores. El sistema de medición del peso también presentó un error aproximado del 9 %, lo que demuestra que fue posible obtener una estimación relativamente cercana a una masa patrón.

No obstante, las limitaciones siguen siendo importantes. El bombillo no cuenta con regulación automática, no existe un sistema activo de control de humedad, el FSR400 presenta un margen de error y el montaje no incorpora las alarmas, redundancias ni las funciones de seguridad de una incubadora clínica. Asimismo, se observó que la cámara tardaba en calentarse, lo que evidencia que el desempeño térmico puede mejorarse.

Por tanto, el prototipo es útil como herramienta educativa para comprender el funcionamiento general de una incubadora neonatal, pero no ofrece las mismas capacidades que los equipos comerciales. La diferencia no depende únicamente del precio, sino también de la precisión de los sensores, la capacidad de control, la confiabilidad y las medidas de seguridad necesarias para proteger al paciente.

## 14. CONCLUSIÓN

La construcción y evaluación del prototipo de incubadora neonatal permitió reconocer la importancia de controlar las condiciones ambientales para favorecer la estabilidad térmica del neonato e identificar los componentes principales que intervienen en el funcionamiento de estos equipos. La integración del Arduino Uno, el sensor DHT11, el FSR400, la pantalla OLED, los indicadores luminosos y los ventiladores permitió desarrollar un sistema que respondió según lo programado durante las pruebas. La pantalla mostró la información prevista e indicó cuándo el ventilador debía encenderse o apagarse, mientras que los indicadores luminosos y los ventiladores funcionaron correctamente. En la estimación del peso, el FSR400 presentó un error aproximado del 9 %, por lo que el valor obtenido fue relativamente cercano al peso patrón, aunque todavía puede mejorarse mediante una calibración más precisa. En cuanto al comportamiento térmico, se observó que la incubadora tardaba en calentarse y que el bombillo no estaba regulado automáticamente por el Arduino, lo que limitó la capacidad del sistema para mantener la temperatura de manera precisa. Estos resultados muestran que es posible representar funciones básicas de una incubadora mediante componentes electrónicos de bajo costo, pero que la regulación térmica y la medición del peso requieren mejoras para obtener resultados más exactos y confiables. Como siguiente paso, se propone incorporar un sistema de control automático del calentamiento, mejorar el aislamiento térmico, realizar mediciones repetidas de temperatura y peso y evaluar el desempeño del prototipo mediante pruebas cuantitativas. El sistema desarrollado constituye una herramienta académica para comprender los principios de instrumentación biomédica, pero no debe utilizarse como sustituto de una incubadora clínica.

## 15. PREGUNTAS PARA LA DISCUSIÓN

### Pregunta 1. ¿Qué otras variables, y por qué, además de las mencionadas son críticas en el monitoreo neonatal?

Además de la temperatura, la humedad y el flujo de aire, existen otras variables relevantes para evaluar el estado del recién nacido. Entre ellas se encuentran la frecuencia cardiaca, la frecuencia respiratoria, la saturación de oxígeno y, cuando está indicado clínicamente, la presión arterial. Estas variables proporcionan información sobre el funcionamiento cardiovascular y respiratorio.

La concentración de oxígeno también es importante cuando se administra oxígeno suplementario, mientras que la temperatura cutánea puede complementar la evaluación térmica. Asimismo, el ruido y la iluminación del entorno deben considerarse porque forman parte de las condiciones ambientales que rodean al neonato.

La medición de estas variables requiere sensores y equipos apropiados para cada función. En una incubadora clínica, su integración depende de las necesidades del paciente y de las características del equipo utilizado [2], [3].

### Pregunta 2. ¿Qué haría falta para convertir el sistema desarrollado en una incubadora neonatal real?

Para convertir el prototipo en una incubadora neonatal apta para uso clínico sería necesario realizar modificaciones importantes en el sistema de control, los sensores, la estructura y las medidas de seguridad.

En primer lugar, se necesitaría un sistema de calentamiento regulado automáticamente mediante un controlador que mantenga la temperatura de referencia y reduzca el riesgo de sobrecalentamiento. También sería necesario incorporar sensores calibrados, verificar la uniformidad térmica dentro de la cabina e implementar un sistema de humidificación con medición y regulación de la humedad.

Además, deberían incorporarse alarmas para detectar temperaturas fuera de rango, fallas de sensores y pérdida de alimentación. La circulación del aire tendría que evaluarse para comprobar que la distribución del calor sea adecuada y que no se produzcan corrientes perjudiciales.

El sistema de medición del peso también requeriría una plataforma apropiada y una calibración verificable. De igual manera, la estructura debería construirse con materiales seguros, resistentes y fáciles de limpiar y desinfectar.

Finalmente, sería indispensable realizar pruebas de seguridad eléctrica, gestión de riesgos, confiabilidad y desempeño, además de cumplir los requisitos regulatorios aplicables a los dispositivos médicos. Estas condiciones demuestran que convertir un prototipo académico en una incubadora real exige mucho más que integrar sensores y programar un microcontrolador.

### Pregunta 3. ¿Qué semejanzas hay entre una incubadora neonatal y una servocuna?

La incubadora neonatal y la servocuna tienen como finalidad contribuir al mantenimiento de una temperatura apropiada para el recién nacido. Ambas pueden utilizar sensores, sistemas de calentamiento, controles electrónicos y alarmas para vigilar las condiciones térmicas.

La principal diferencia se encuentra en su estructura y en la forma de transferir el calor. La incubadora neonatal utiliza una cámara cerrada que proporciona un microambiente protegido y permite controlar las condiciones internas. La servocuna o cuna radiante utiliza un elemento calefactor situado por encima del paciente, que transmite calor principalmente mediante radiación y permite un acceso más directo durante los procedimientos clínicos.

En consecuencia, ambos equipos comparten el objetivo de favorecer la estabilidad térmica del neonato, pero se diferencian en la forma de proporcionar calor, el nivel de aislamiento y la facilidad de acceso al paciente [3], [7].

## REFERENCIAS

[1] Universidad Militar Nueva Granada, “Diseño y construcción de una incubadora neonatal a escala,” *Guía de laboratorio de Instrumentación Biomédica y Biosensores*, 2025.

[2] World Health Organization, *Standards for Improving the Quality of Care for Small and Sick Newborns in Health Facilities*. Geneva, Switzerland: WHO, 2020.

[3] Dräger, “Isolette 8000 plus.” [En línea]. Disponible en: https://www.draeger.com/en-us_us/Products/isolette-8000-plus.

[4] Dräger, “Isolette 8000 plus: información del producto.” [En línea]. Disponible en: https://www.draeger.com/Content/Documents/Products/PI/en_sa/isolette-8000-plus-Product-Information-en-sa.pdf. 

[5] Instrumentalia S.A.S., “Incubadora YP2000.” [En línea]. Disponible en: https://instrumentalia.com.co/medicina/61297-incubadora-yp2000.html. 

[6] LEEX Medical, “Incubadora neonatal B3.” [En línea]. Disponible en: https://leexmedical.com/product/incubadora-neonatal-b3/.

[7] LEEX Medical, “Cuna radiante BQ80.” [En línea]. Disponible en: https://leexmedical.com/product/servocuna/. 

[8] Interlink Electronics, “FSR 400 Series Data Sheet.” [En línea]. Disponible en: https://www.interlinkelectronics.com/downloads/datasheets/fsr-400-series-datasheet.pdf. 
