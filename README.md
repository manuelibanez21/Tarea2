1\. Código ASCII: historia, funcionamiento y conceptos clave

1.1 ¿Qué es ASCII?



ASCII (American Standard Code for Information Interchange) es un estándar que asigna un número a cada carácter (letras, dígitos, signos y controles). El ASCII original usa 7 bits (128 códigos: 0–127). Permite la interoperabilidad de texto entre computadoras, impresoras, terminales y dispositivos.



Ejemplos:



A → 65 (decimal) → 0x41 (hex) → 1000001 (binario, 7 bits)



a → 97



0 → 48



Espacio → 32



1.2 Breve historia



Surge a inicios de los años 60 impulsado por ANSI y la industria de los teletipos.



Se volvió el estándar dominante en sistemas de tiempo compartido, Unix, Internet temprana y protocolos de texto.



Con el tiempo, el conjunto de 7 bits resultó insuficiente para otros alfabetos; aparecieron extensiones de 8 bits (no estándar entre sí), y finalmente Unicode (UTF-8, UTF-16, etc.) se convirtió en el estándar moderno para representar casi todos los idiomas.



1.3 ¿Cómo funciona?



Cada carácter tiene un código numérico fijo.



En transmisión serie, esos números se envían bit a bit.



El ASCII básico usa 7 bits; muchos sistemas añaden un octavo bit de paridad o lo dejan en 0.



1.4 Caracteres de control más usados (0–31 y 127)

Nombre	Dec	Hex	Uso típico

NUL	0	0x00	Valor nulo / relleno

BEL	7	0x07	Campana / alerta acústica

BS	8	0x08	Retroceso

TAB	9	0x09	Tabulación horizontal

LF	10	0x0A	Salto de línea

CR	13	0x0D	Retorno de carro

ESC	27	0x1B	Secuencia de escape

DEL	127	0x7F	Suprimir (histórico)



Nota: En sistemas modernos, Unicode/UTF-8 es el estándar recomendado; ASCII es un subconjunto de UTF-8 (los códigos 0–127 son idénticos).



2\. Pines de los conectores RS-232 (DB9 y DB25)

2.1 Contexto rápido (DTE vs DCE y tipo de cable)



DTE: Data Terminal Equipment (PC, terminal).



DCE: Data Circuit-terminating Equipment (módem, equipo de comunicaciones).



Direcciones indicadas abajo son vistas desde un DTE (un PC típico).



Cables:



Directo (straight-through): DTE ↔ DCE (PC ↔ módem).



Null-modem: DTE ↔ DTE (PC ↔ PC). Cruza TX/RX y señales de control (p. ej., TXD↔RXD, RTS↔CTS, DTR↔DSR/DCD).



2.2 Conector DB9 (en realidad DE-9) – pinout típico de PC (DTE)

Pin	Señal	Acrónimo	Dirección (desde DTE)	Descripción

1	Detecta portadora	DCD	Entrada	El DCE indica que hay portadora (enlace válido).

2	Recibir datos	RXD	Entrada	Datos que llegan al DTE.

3	Transmitir datos	TXD	Salida	Datos enviados por el DTE.

4	Terminal listo	DTR	Salida	El DTE está listo/activo.

5	Tierra de señal	GND	—	Retorno/Referencia de señales.

6	Equipo listo	DSR	Entrada	El DCE está listo.

7	Solicitud de envío	RTS	Salida	DTE solicita permiso para transmitir.

8	Listo para enviar	CTS	Entrada	DCE concede permiso para transmitir.

9	Indicador de timbre	RI	Entrada	Señal de timbrado entrante (modem).



Prueba de “loopback” DB9: Puentea 2↔3 (RXD↔TXD) para comprobar eco local con un terminal serie.



2.3 Conector DB25 – pinout más habitual en RS-232 (DTE)



En DB25 existen más pines definidos; aquí se listan los más usados en datos y control.



Pin	Señal	Acrónimo	Dirección (desde DTE)	Descripción

1	Tierra de protección (chasis)	PGND	—	Blindaje / chasis.

2	Transmitir datos	TXD	Salida	Datos enviados por el DTE.

3	Recibir datos	RXD	Entrada	Datos hacia el DTE.

4	Solicitud de envío	RTS	Salida	Solicita permiso para enviar.

5	Listo para enviar	CTS	Entrada	Concede permiso de envío.

6	Equipo listo	DSR	Entrada	DCE está listo.

7	Tierra de señal	GND	—	Referencia de señales (principal).

8	Detecta portadora	DCD	Entrada	Portadora presente.

20	Terminal listo	DTR	Salida	DTE activo/listo.

22	Indicador de timbre	RI	Entrada	Timbre entrante (modem).



Diferencia importante: Pin 1 (DB25) = tierra de protección,

Pin 7 (DB25) = tierra de señal. En DB9 sólo hay GND en el pin 5.



3\. Formato del protocolo RS-232

3.1 Capa física (niveles eléctricos)



Señales single-ended referidas a GND.



Niveles lógicos (estándar RS-232C):



‘1’ (MARK/ reposo): voltaje negativo, típicamente entre −3 V y −15 V (muchos equipos usan ≈ −12 V).



‘0’ (SPACE / activo): voltaje positivo, típicamente entre +3 V y +15 V (≈ +12 V en muchos equipos).



Zona entre −3 V y +3 V: indefinida.



Reposo de línea = ‘1’ (negativo).

El bit de inicio fuerza la línea a ‘0’ (positivo).



3.2 Trama asíncrona típica



Una trama RS-232 es asíncrona y se compone de:



Start bit: 1 bit en ‘0’.



Bits de datos: 5–8 bits (lo más común 8). Orden LSB→MSB.



Paridad (opcional): None / Even / Odd / Mark / Space.



Stop bit(s): 1, 1.5 o 2 bits en ‘1’.



Configuración popular: 9600-8-N-1

(9600 baudios, 8 bits de datos, N sin paridad, 1 bit de parada).

Tiempo por bit a 9600 bps ≈ 1 / 9600 s = 0.000104166… s ≈ 104.17 μs.



3.3 Control de flujo



Hardware: RTS/CTS (permiso para transmitir) y DTR/DSR (estado de terminal/equipo).



Software: XON/XOFF (Ctrl-Q / Ctrl-S) dentro del flujo de datos.



3.4 Longitud de cable y velocidad (reglas prácticas)



Regla común: hasta ~15 m (~50 ft) a 19 200 bps con cable y terminaciones adecuados.



A menor velocidad (p. ej., 9 600 bps) pueden alcanzarse decenas de metros (incluso ~100 m) con buen cable y baja capacitancia.



RS-232 no es diferencial; es sensible a ruido y a diferencias de tierra → usar cable apantallado y buena referencia de GND.



3.5 Tipos de cableado



Straight-through (PC–módem/DCE): pines 1:1.



Null-modem (PC–PC/DTE–DTE): cruza al menos TXD↔RXD y GND, y normalmente RTS↔CTS y DTR↔DSR/DCD.

