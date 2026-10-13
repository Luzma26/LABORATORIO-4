# DISEÑO Y CONSTRUCCIÓN DE UNA INCUBADORA NEONATAL A ESCALA

**Luz Marina Valderrama – 5600741**

## INTRODUCCIÓN

Las incubadoras neonatales son equipos biomédicos diseñados para proporcionar un ambiente controlado a los recién nacidos que requieren asistencia para mantener una temperatura corporal adecuada. Su importancia es especialmente relevante en neonatos prematuros, debido a que su capacidad para regular la temperatura puede ser limitada. Por esta razón, el control de variables como la temperatura, la humedad y la circulación del aire es fundamental para favorecer condiciones ambientales apropiadas [1], [2].

Las incubadoras comerciales integran diferentes componentes, como una cubierta transparente, sensores de temperatura, sistemas de calentamiento, ventiladores, controladores electrónicos y alarmas. Dependiendo del modelo, también pueden incorporar mecanismos de humidificación, monitorización clínica y medición del peso [3], [4]. Estos elementos permiten mantener condiciones ambientales controladas y supervisar el funcionamiento del equipo.

En esta práctica se desarrolló un prototipo a escala de una incubadora neonatal mediante un Arduino Uno, un sensor DHT11, un sensor de fuerza FSR400, una pantalla OLED, tres LED indicadores y dos ventiladores. El sistema permite adquirir variables ambientales, visualizar las mediciones y activar un ventilador cuando la temperatura supera el límite establecido. También se incorporó un sensor de fuerza para explorar la estimación del peso mediante cambios en una señal eléctrica.

El proyecto se dividió en dos partes. En la primera se revisaron los componentes de una incubadora neonatal y se plantearon los principios de los circuitos de control térmico y medición del peso. En la segunda se construyó el modelo físico a escala y se integraron los componentes electrónicos. Sin embargo, el montaje presenta limitaciones: el bombillo utilizado como fuente de calor no está regulado automáticamente por el Arduino, el sensor FSR400 requiere calibración y la pantalla utilizada es OLED, no una LCD convencional. Por lo tanto, el prototipo representa una aproximación académica al funcionamiento de una incubadora y no un equipo apto para uso clínico.

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

La guía solicita diseñar y simular un circuito eléctrico de control de lazo cerrado capaz de mantener la temperatura entre 36 °C y 37,5 °C. Para cumplir este propósito, se plantea un sistema compuesto por un sensor de temperatura, un controlador, un elemento calefactor y un ventilador.

El sensor mide continuamente la temperatura interna y envía la información al controlador. Este compara la temperatura medida con el intervalo de referencia y determina si debe aumentar o reducir la acción de calentamiento. El ventilador permite favorecer la circulación del aire y, cuando se requiere, contribuir a disminuir la temperatura.

El funcionamiento por realimentación puede representarse mediante el siguiente esquema:

Temperatura de referencia → Comparación con la temperatura medida → Controlador → Calefactor y ventilador → Temperatura de la cabina → Sensor de temperatura → Realimentación al controlador.

El intervalo de referencia establecido para el prototipo es de 36 °C a 37,5 °C. La diferencia entre la temperatura de referencia y la temperatura medida se expresa como:

e(t) = T_ref - T_medida

Donde e(t) representa el error de temperatura, T_ref es la temperatura de referencia y T_medida corresponde a la temperatura registrada por el sensor.

Para la implementación se utilizó un sensor DHT11 conectado al Arduino Uno. La lógica de funcionamiento contempla tres estados: temperatura baja, temperatura dentro del intervalo y temperatura alta.

Cuando la temperatura es inferior a 36 °C, se enciende el LED azul. Cuando está entre 36 °C y 37,5 °C, se enciende el LED verde. Si supera los 37,5 °C, se enciende el LED rojo y se activa el ventilador superior.

El ventilador se desactiva cuando la temperatura desciende por debajo de 36 °C. Esta diferencia entre los umbrales de activación y desactivación introduce una histéresis que reduce los cambios constantes de estado cuando la temperatura se encuentra cerca del límite.

Sin embargo, el montaje construido no constituye un sistema de control térmico completo de lazo cerrado, porque el bombillo permanece encendido y no es regulado automáticamente por el Arduino. El sistema puede medir la temperatura y activar la ventilación, pero no puede modificar directamente la potencia de calentamiento. Por tanto, para cumplir completamente el requisito de la guía se necesitaría incorporar un actuador que permita regular el calentador y verificar su respuesta mediante simulación y pruebas experimentales.

### Diseño del circuito de medición del peso

La guía solicita diseñar un circuito capaz de medir el peso y mostrarlo mediante una pantalla LCD o displays de siete segmentos. Para explorar esta función se utilizó un sensor de fuerza FSR400, cuyo principio de funcionamiento se basa en la variación de su resistencia eléctrica cuando se aplica una fuerza sobre su superficie.

El sensor se conectó mediante un divisor de tensión. Una de sus terminales se conectó a 5 V y la otra al nodo de lectura analógica A0 del Arduino Uno. Desde este nodo se conectó una resistencia de 10 kΩ a tierra. De esta forma, los cambios de resistencia del sensor producen variaciones de tensión que pueden ser adquiridas por el microcontrolador.

La lectura analógica obtenida se utiliza para reconocer cambios en la fuerza aplicada. Sin embargo, esta lectura no representa directamente un peso en gramos. Para convertirla en una estimación de masa es necesario realizar una calibración con masas patrón conocidas y establecer una relación experimental entre las lecturas y los valores de referencia.

La información se muestra mediante una pantalla OLED con comunicación I²C. Aunque la guía contempla una pantalla LCD o displays de siete segmentos, la OLED utilizada permite visualizar caracteres y valores numéricos. Por tanto, cumple una función de visualización equivalente para el prototipo, pero no corresponde literalmente a los tipos de pantalla indicados en la guía.

### PARTE B. CONSTRUCCIÓN DEL MODELO A ESCALA

### Construcción de la estructura

Se construyó una cámara a escala con una cubierta transparente para permitir observar el interior. La estructura representa el espacio de una incubadora neonatal y permite ubicar el sistema de calentamiento, los ventiladores, el sensor ambiental y los componentes electrónicos.

La guía establece unas dimensiones aproximadas de 90 cm × 40 cm × 45 cm. Las dimensiones finales del prototipo deben verificarse mediante medición directa y registrarse según la estructura construida. No se consideran confirmadas las dimensiones propuestas por la guía si no han sido comprobadas físicamente.

La cubierta debe permitir la apertura y el cierre, facilitar el acceso al interior y reducir el intercambio de calor con el ambiente. El material utilizado debe ofrecer visibilidad y proporcionar una barrera física apropiada para las pruebas del prototipo.

### Materiales y equipos

Para el montaje se utilizaron o contemplaron los siguientes elementos: Arduino Uno, sensor DHT11, sensor de fuerza FSR400, pantalla OLED I²C, tres LED indicadores, dos ventiladores de 5 V, transistor TIP122, diodo 1N4007, resistencias, protoboard, cables jumper, fuente regulada de laboratorio de 5 V, bombillo como fuente de calor, estructura transparente y computador con Arduino IDE.

El Arduino Uno se utiliza para adquirir las señales y controlar las salidas digitales. El DHT11 mide temperatura y humedad, mientras que el FSR400 detecta cambios de fuerza. La pantalla OLED muestra los valores y los LED indican el estado térmico.

El ventilador inferior permanece encendido para favorecer la circulación del aire, mientras que el ventilador superior se controla mediante el transistor TIP122. El bombillo proporciona calor, pero en el montaje actual permanece independiente del control automático.

### Consideraciones de seguridad eléctrica

El circuito electrónico se alimentó mediante una fuente regulada de 5 V de corriente continua. Antes de conectar los componentes, se verificó la polaridad de la alimentación y se comprobó que no existieran cortocircuitos entre los rieles de la protoboard.

El bombillo se mantuvo separado del circuito de baja tensión. Debido a que su alimentación puede involucrar tensión de red, cualquier conexión correspondiente debe ser realizada o verificada por una persona cualificada y bajo supervisión del docente.

El sistema no debe dejarse funcionando sin vigilancia debido al riesgo de sobrecalentamiento. Además, el prototipo no debe utilizarse para mantener las condiciones térmicas de un recién nacido. Las pruebas se realizan exclusivamente con fines académicos y en condiciones controladas.

### Montaje del circuito electrónico

El montaje se realizó sobre una protoboard, utilizando el Arduino Uno como unidad central de procesamiento. La alimentación de 5 V se distribuyó a los rieles de la protoboard y se estableció una tierra común para los componentes de baja tensión.

El sensor DHT11 se conectó a 5 V y GND, mientras que su salida de datos se conectó al pin digital D2 del Arduino. Esta conexión permite obtener las lecturas de temperatura y humedad utilizadas por el programa.

El sensor FSR400 se conectó mediante un divisor de tensión: una terminal a 5 V y la otra al nodo analógico A0. Desde este nodo se conectó una resistencia de 10 kΩ a GND.

La pantalla OLED se conectó mediante el bus I²C, utilizando A4 como línea SDA y A5 como línea SCL. La alimentación y la tierra se conectaron a los terminales correspondientes. La configuración del programa depende de la dirección I²C y de las características de la pantalla utilizada.

Los LED azul, verde y rojo se conectaron a los pines digitales D8, D9 y D10, respectivamente, cada uno con una resistencia limitadora de corriente. El LED azul indica una temperatura inferior al intervalo de referencia, el verde representa una temperatura dentro del intervalo y el rojo indica que se ha superado el límite superior.

El ventilador inferior se conectó para permanecer encendido durante el funcionamiento del sistema. Para controlar el ventilador superior se utilizó un transistor TIP122 como elemento de conmutación, con una resistencia en la base y un diodo 1N4007 para proteger el circuito frente a los transitorios eléctricos generados por el motor. La conexión del transistor, el diodo y la alimentación debe verificarse antes de energizar el sistema.

### Programación del Arduino Uno

El programa del Arduino se diseñó para leer las variables ambientales, presentar la información en la pantalla OLED y controlar los indicadores luminosos y el ventilador superior.

La temperatura medida se compara con el intervalo de referencia comprendido entre 36 °C y 37,5 °C. Cuando la temperatura es inferior a 36 °C, se enciende el LED azul. Cuando la temperatura se encuentra dentro del intervalo, se enciende el LED verde. Si la temperatura supera los 37,5 °C, se enciende el LED rojo y se activa el ventilador superior.

El ventilador se desactiva cuando la temperatura desciende por debajo de 36 °C. Esta lógica permite observar cómo una medición puede utilizarse para modificar el estado de un actuador.

La pantalla OLED se utiliza para mostrar la temperatura, la humedad y la lectura analógica del FSR400. Sin embargo, la lectura del sensor de fuerza no puede interpretarse directamente como gramos sin una calibración previa con masas conocidas.

Es importante señalar que el bombillo no se encuentra conectado a una salida de control del Arduino. Por esta razón, el programa puede activar la ventilación, pero no puede regular automáticamente la potencia de calentamiento.

### Adquisición de temperatura y humedad

Durante las pruebas, el sensor DHT11 se utiliza para obtener las mediciones ambientales de la cámara. Los valores se muestran en la pantalla OLED, lo que permite observar los cambios producidos por el calentamiento y la circulación del aire.

Para evaluar el comportamiento térmico, se deben registrar la temperatura inicial, la temperatura máxima, la temperatura mínima y el tiempo necesario para observar una respuesta del ventilador. También debe comprobarse si la temperatura permanece dentro del intervalo de referencia durante el periodo de observación.

El DHT11 presenta limitaciones de precisión y tiempo de respuesta en comparación con sensores especializados para equipos médicos. Por lo tanto, las mediciones obtenidas deben interpretarse como parte de una práctica académica y no como una validación de precisión clínica.

### Estimación del peso mediante el FSR400

El FSR400 es un sensor resistivo cuya resistencia cambia cuando se aplica una fuerza sobre su superficie. En el circuito, este cambio produce una variación de tensión que se lee mediante la entrada analógica A0 del Arduino.

Para establecer una relación entre la lectura analógica y el peso, se realiza una calibración experimental. Primero se registra la lectura sin carga y posteriormente se colocan masas conocidas sobre una plataforma que distribuya la fuerza de forma reproducible. Con estos datos se puede construir una curva de calibración y estimar la masa correspondiente a una lectura determinada.

El error absoluto de la estimación se calcula mediante:

E_a = |m_estimada - m_patron|

El error relativo porcentual se calcula mediante:

E_r(%) = (E_a / m_patron) × 100

Donde m_estimada es la masa estimada por el sistema y m_patron es la masa conocida utilizada como referencia. El error relativo no se calcula cuando la masa patrón es cero.

La exactitud de la estimación depende de la distribución de la carga, el contacto mecánico con el sensor, la repetibilidad de las mediciones y el intervalo de masas utilizado durante la calibración. Debido a estas limitaciones, el FSR400 no equivale a una balanza clínica.

### Cálculo de costos

Para determinar el costo total del prototipo se deben registrar los precios reales de los componentes y sumar los costos parciales de cada elemento. Se deben incluir el Arduino Uno, el sensor DHT11, el FSR400, la pantalla OLED, los ventiladores, el transistor TIP122, el diodo 1N4007, los LED, las resistencias, la protoboard, los cables, la fuente de alimentación y los materiales de la estructura.

El costo total se obtiene mediante la expresión:

C_total = C_Arduino + C_DHT11 + C_FSR400 + C_OLED + C_ventiladores + C_componentes + C_estructura + C_alimentacion

Los precios deben registrarse en pesos colombianos y corresponder a los valores reales de compra. Si algunos elementos fueron proporcionados por el laboratorio, deben identificarse por separado para distinguir el costo de los materiales adquiridos del valor total de los recursos utilizados.

No se asignan valores monetarios supuestos a los componentes, ya que no se dispone de las facturas o cotizaciones completas del prototipo.

### Evaluación del sistema

La evaluación del prototipo debe realizarse mediante pruebas de funcionamiento de cada componente y del sistema integrado. Se comprueba que el sensor ambiental entregue lecturas, que la pantalla muestre los valores, que los LED respondan al intervalo de temperatura programado y que el ventilador superior se active cuando se supera el límite establecido.

También se debe evaluar el sensor FSR400 mediante masas patrón para determinar el error de la estimación. Los datos obtenidos deben registrarse para comparar el comportamiento del sistema en diferentes condiciones.

Como no se han proporcionado mediciones experimentales completas de temperatura ni resultados de calibración del peso, estos valores no se presentan como resultados cuantitativos confirmados. La eficacia del sistema debe establecerse a partir de las pruebas realizadas durante la práctica.

## RESULTADOS

### Funcionamiento general del prototipo

El prototipo integra los principales componentes electrónicos previstos para representar algunas funciones de una incubadora neonatal a escala. El Arduino Uno actúa como unidad de procesamiento, el sensor DHT11 permite adquirir la temperatura y la humedad, la pantalla OLED facilita la visualización de las mediciones y los LED indican el estado térmico de la cámara.

El sistema también incorpora dos ventiladores con funciones diferenciadas. El ventilador inferior permanece encendido para favorecer la circulación del aire, mientras que el ventilador superior responde a la temperatura medida por el sensor. De esta forma, el montaje permite representar una estrategia básica de control ambiental mediante sensores y actuadores.

No se dispone de un registro experimental completo que permita establecer el porcentaje de tiempo durante el cual la temperatura permaneció dentro del intervalo de referencia. Por lo tanto, la eficacia térmica debe determinarse mediante las mediciones obtenidas durante las pruebas, sin asumir que el sistema mantiene una temperatura constante.

### Respuesta del sistema ante los cambios de temperatura

El programa establece tres estados de funcionamiento según la temperatura medida. Por debajo de 36 °C, el sistema indica una condición de temperatura baja mediante el LED azul. Entre 36 °C y 37,5 °C, el LED verde representa el intervalo establecido como referencia. Por encima de 37,5 °C, se enciende el LED rojo y se activa el ventilador superior.

Esta lógica permite demostrar cómo una medición puede utilizarse para modificar el estado de un actuador. Sin embargo, la activación del ventilador no garantiza por sí sola que la temperatura regrese al intervalo deseado, ya que el resultado depende de la potencia del bombillo, el aislamiento de la cámara, la circulación del aire y las condiciones ambientales.

Además, el bombillo permanece encendido sin regulación automática. En consecuencia, el prototipo no dispone de un control completo del calentamiento y la refrigeración. Para determinar su desempeño real, es necesario registrar las temperaturas durante un periodo definido y comprobar la respuesta del sistema ante los cambios térmicos.

### Adquisición de humedad

El sensor DHT11 permite obtener una lectura de humedad relativa junto con la temperatura. Esta medición resulta útil para observar las condiciones ambientales dentro de la cámara y reconocer que la humedad constituye una variable importante en el cuidado neonatal.

No obstante, el prototipo no incorpora un humidificador ni un mecanismo automático para regular la humedad. Por esta razón, el sistema puede medir y mostrar esta variable, pero no garantizar que permanezca dentro de un intervalo específico.

### Lectura del sensor FSR400

El sensor FSR400 permite detectar cambios en la fuerza aplicada mediante variaciones de su resistencia eléctrica. La conexión con el divisor de tensión permite convertir esos cambios en lecturas analógicas que el Arduino puede procesar.

Sin embargo, una lectura analógica no corresponde directamente a un peso expresado en gramos. Para estimar la masa se requiere una curva de calibración construida con masas patrón conocidas. Como no se cuenta con los datos completos de estas pruebas, no es posible establecer un error numérico confirmado para la medición.

La calibración debe realizarse colocando las masas sobre una plataforma estable y repitiendo cada medición para observar la variabilidad. Posteriormente, se pueden comparar los valores estimados con los valores de referencia para determinar el error absoluto y relativo.

### Comparación general con una incubadora comercial

El prototipo permite demostrar principios de medición, visualización y control, pero presenta diferencias importantes frente a las incubadoras comerciales. Equipos como la Dräger Isolette 8000 plus incorporan funciones avanzadas de control térmico, monitorización y gestión de humedad, según su configuración [3], [4].

La incubadora YP2000 comercializada por Instrumentalia ofrece funciones de control ambiental y una estructura destinada al cuidado neonatal [5]. Por su parte, la incubadora B3 de LEEX Medical incorpora funciones comerciales de control de temperatura y humidificación, según su configuración [6].

En comparación con estos equipos, el prototipo académico tiene la ventaja de permitir la modificación del programa y la observación directa del funcionamiento de los sensores y actuadores. Sin embargo, no cuenta con regulación automática del calentamiento, control activo de humedad, alarmas clínicas ni un sistema de medición del peso validado.

Por lo tanto, los resultados obtenidos permiten estudiar el funcionamiento básico de una incubadora, pero no demuestran que el sistema tenga la precisión, confiabilidad o seguridad necesarias para sustituir un equipo médico.

## 13. ANÁLISIS DE RESULTADOS

### Análisis 1. Eficacia del sistema para controlar la temperatura de la cabina y error en el cálculo del peso

El sistema desarrollado permite representar algunos de los principios de funcionamiento de una incubadora neonatal mediante la adquisición de temperatura y humedad con el sensor DHT11, la visualización de los datos en una pantalla OLED y la activación de indicadores luminosos y ventiladores mediante un Arduino Uno. De esta manera, se puede observar cómo la medición de una variable ambiental se utiliza para modificar el funcionamiento de un actuador.

Para evaluar la respuesta térmica, se estableció como referencia un intervalo de 36 °C a 37,5 °C. Cuando la temperatura es inferior a 36 °C, se enciende el LED azul; cuando se encuentra dentro del intervalo, se enciende el LED verde; y cuando supera los 37,5 °C, se enciende el LED rojo y se activa el ventilador superior. El ventilador inferior permanece encendido para favorecer la circulación del aire.

Este comportamiento demuestra una estrategia básica de control por realimentación. Sin embargo, la eficacia real depende de las mediciones experimentales, del tiempo de respuesta del sensor, de la distribución de la temperatura dentro de la cámara y de la capacidad de los ventiladores para disipar el calor acumulado.

Una limitación importante es que el bombillo utilizado como fuente de calor permanece encendido y no está regulado automáticamente por el Arduino. Por esta razón, el sistema puede responder al aumento de temperatura mediante ventilación, pero no puede reducir directamente la potencia de calentamiento. En consecuencia, no se puede afirmar que la cabina mantenga una temperatura constante sin contar con registros experimentales que lo demuestren.

Para cuantificar la eficacia térmica, se debe registrar la temperatura durante un periodo definido y calcular el porcentaje de mediciones que permanecen dentro del intervalo de referencia:

P(%) = (N_dentro / N_total) × 100

Donde N_dentro corresponde al número de mediciones entre 36 °C y 37,5 °C, y N_total representa el número total de mediciones registradas.

En cuanto a la estimación del peso, el FSR400 permite detectar cambios en la fuerza aplicada mediante variaciones de su resistencia eléctrica. No obstante, la señal analógica obtenida por el Arduino no representa directamente una masa en gramos. Para establecer esta conversión es necesario realizar una calibración experimental utilizando masas patrón conocidas.

El error absoluto de la estimación se determina mediante:

E_a = |m_estimada - m_patron|

El error relativo porcentual se calcula mediante:

E_r(%) = (E_a / m_patron) × 100

Donde m_estimada corresponde a la masa estimada por el sistema y m_patron representa la masa conocida utilizada como referencia. El error relativo no se calcula cuando la masa patrón es cero.

La precisión de esta medición depende de la calibración del sensor, la distribución de la fuerza sobre su superficie y la estabilidad de la plataforma donde se coloca la carga. Además, el FSR400 no debe considerarse equivalente a una balanza clínica calibrada. Sin mediciones realizadas con masas conocidas, no es posible establecer un porcentaje de error experimental.

En general, el sistema permite comprender los principios de medición y control aplicados a una incubadora neonatal a escala. Sin embargo, para evaluar cuantitativamente su eficacia es necesario registrar los datos experimentales y reconocer que el control térmico y la estimación del peso presentan limitaciones que deben resolverse antes de considerar aplicaciones más exigentes.

### Análisis 2. Comparación del sistema desarrollado con los equipos comerciales

El prototipo construido se diferencia de las incubadoras comerciales principalmente por su complejidad, precisión, seguridad y cantidad de variables controladas. Mientras que el sistema académico utiliza componentes electrónicos para demostrar principios básicos de instrumentación, los equipos comerciales están diseñados para proporcionar condiciones ambientales controladas y ofrecer funciones específicas para la atención neonatal.

La incubadora Dräger Isolette 8000 plus incorpora funciones avanzadas de control térmico, monitorización y gestión de humedad según su configuración, además de opciones de pesaje. Estas características permiten un control más completo que el del prototipo, aunque su adquisición y mantenimiento requieren proveedores especializados [3], [4].

Por su parte, la incubadora YP2000 comercializada por Instrumentalia cuenta con una estructura y funciones destinadas al control ambiental neonatal [5]. Durante la consulta comercial se identificó un precio publicado de aproximadamente $53.391.350 COP, incluidos impuestos. Este valor debe confirmarse con el proveedor, ya que puede variar según la disponibilidad, la configuración y las condiciones de venta.

La incubadora B3 de LEEX Medical también ofrece funciones comerciales de control de temperatura y humidificación, según la configuración del equipo. Su precio debe solicitarse directamente al fabricante o distribuidor [6].

Frente a estas alternativas, el prototipo académico presenta como ventaja su bajo costo relativo y la posibilidad de modificar el programa para observar la relación entre las mediciones de los sensores y la respuesta de los actuadores. Además, permite aplicar conocimientos de electrónica, programación, adquisición de señales y control de variables físicas.

No obstante, sus limitaciones son importantes. El bombillo no cuenta con regulación automática, no existe un sistema activo de control de humedad, el FSR400 requiere calibración para estimar el peso y el montaje no incorpora las alarmas, redundancias ni las funciones de seguridad de una incubadora clínica. Tampoco se han documentado pruebas de uniformidad térmica, precisión o seguridad eléctrica.

Por tanto, el prototipo es útil como herramienta educativa para comprender el funcionamiento general de una incubadora neonatal, pero no ofrece las mismas capacidades que los equipos comerciales. La diferencia no depende únicamente del precio, sino también de la precisión de los sensores, la capacidad de control, la confiabilidad y las medidas de seguridad necesarias para proteger al paciente.

## 14. CONCLUSIONES

La construcción del prototipo de incubadora neonatal permitió reconocer la importancia de controlar las condiciones ambientales para favorecer la estabilidad térmica del neonato, así como identificar los componentes principales que intervienen en su funcionamiento. El sistema desarrollado integra un Arduino Uno, un sensor DHT11, un FSR400, una pantalla OLED, indicadores luminosos y ventiladores, permitiendo representar algunos de los procesos de medición y control presentes en estos equipos. Sin embargo, debido a que el bombillo permanece encendido sin regulación automática, no se puede garantizar un control preciso de la temperatura; de igual manera, la estimación del peso requiere calibrar el FSR400 con masas patrón conocidas para determinar su error. Estos resultados evidencian que, aunque es posible implementar funciones básicas mediante componentes electrónicos de bajo costo, controlar variables como la temperatura y el peso con gran precisión requiere sensores calibrados, mecanismos de regulación adecuados y pruebas experimentales que permitan validar el desempeño del sistema. Como siguiente paso, se propone incorporar el control automático del calentamiento, mejorar la regulación de la humedad y realizar mediciones repetidas de temperatura y peso para cuantificar los errores y evaluar las mejoras del prototipo.

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

[3] Dräger, “Isolette 8000 plus.” [En línea]. Disponible en: https://www.draeger.com/en-us_us/Products/isolette-8000-plus. [Accedido: 13-oct-2026].

[4] Dräger, “Isolette 8000 plus: información del producto.” [En línea]. Disponible en: https://www.draeger.com/Content/Documents/Products/PI/en_sa/isolette-8000-plus-Product-Information-en-sa.pdf. [Accedido: 13-oct-2026].

[5] Instrumentalia S.A.S., “Incubadora YP2000.” [En línea]. Disponible en: https://instrumentalia.com.co/medicina/61297-incubadora-yp2000.html. [Accedido: 13-oct-2026].

[6] LEEX Medical, “Incubadora neonatal B3.” [En línea]. Disponible en: https://leexmedical.com/product/incubadora-neonatal-b3/. [Accedido: 13-oct-2026].

[7] LEEX Medical, “Cuna radiante BQ80.” [En línea]. Disponible en: https://leexmedical.com/product/servocuna/. [Accedido: 13-oct-2026].

[8] Interlink Electronics, “FSR 400 Series Data Sheet.” [En línea]. Disponible en: https://www.interlinkelectronics.com/downloads/datasheets/fsr-400-series-datasheet.pdf. [Accedido: 13-oct-2026].
