
---

# 📘 Semana 1: Sistemas Numéricos, Códigos y Compuertas Lógicas

---

## 🎯 Mi Objetivo de Aprendizaje en esta Semana

En esta primera semana mi propósito fue comprender las bases físicas y matemáticas sobre las que funciona cualquier computador moderno. Antes de diseñar circuitos complejos, necesité responder una pregunta fundamental: **¿cómo logra una máquina electrónica representar números, signos negativos, letras y decisiones lógicas utilizando únicamente electricidad?**

Al finalizar el estudio de esta guía soy capaz de:
1. Convertir cualquier cantidad entre los sistemas **decimal, binario, octal y hexadecimal** y comprobar mis resultados mediante pesos posicionales.
2. Representar números negativos en el sistema de **complemento a dos** y realizar restas binarias utilizando únicamente sumas.
3. Identificar el propósito práctico de los códigos **BCD, Gray y ASCII** en dispositivos reales como pantallas digitales, sensores industriales y memorias.
4. Reconocer las **7 compuertas lógicas fundamentales**, construir sus tablas de verdad e identificar sus pines físicos dentro de un circuito integrado de la familia **74LS**.

---

## 💡 1. Activación de Conocimientos: Tres Preguntas Cotidianas

Al iniciar la clase nos planteamos tres interrogantes que conectan la electricidad básica con la computación diaria. Aquí presento mi análisis y respuesta razonada para cada una:

### Pregunta 1: ¿Qué hace un interruptor eléctrico?
Cuando recordamos los principios de electricidad básica, sabemos que un interruptor tiene solo dos estados posibles:
* **Interruptor abierto:** El camino está interrumpido, por lo que **no circula corriente** y medimos un voltaje bajo (cercano a $0\text{ V}$). En lógica digital, a este estado lo llamamos **`0`**.
* **Interruptor cerrado:** El circuito se cierra, **pasa la corriente** y medimos un voltaje alto (por ejemplo, $5\text{ V}$). En lógica digital, a este estado lo llamamos **`1`**.

### Pregunta 2: ¿Cómo guarda mi celular la letra «A» si la memoria solo entiende números?
La memoria de un teléfono o computador está formada por millones de diminutos interruptores que solo almacenan ceros y unos. Para guardar texto, los fabricantes establecieron un acuerdo internacional (como la tabla **ASCII**): a cada letra se le asigna un número fijo. En el caso de la letra **`A` mayúscula**, el acuerdo indica que equivale al número decimal **`65`**, el cual se guarda físicamente en memoria como la secuencia de 8 bits **`01000001`**.

### Pregunta 3: ¿Por qué las memorias vienen en capacidades de 8, 16, 32 o 64 GB y nunca de 10, 20 o 50 GB?
Porque internamente los circuitos de memoria se direccionan mediante líneas binarias (cables que solo pueden valer `0` o `1`). Cada vez que agregamos una línea de dirección adicional, la cantidad de posiciones disponibles se **duplica** siguiendo las potencias de base 2 ($2^1=2, 2^2=4, 2^3=8, 2^4=16, 2^5=32, 2^6=64\dots$). Fabricar una memoria de 50 GB desperdiciaría líneas de direccionamiento que ya permiten llegar hasta 64 GB.

---

## ⚡ 2. Representación Binaria y Niveles de Voltaje

### 2.1. ¿Por qué el computador usa únicamente 0 y 1?
En el interior de un procesador no existen interruptores mecánicos que se pulsen con el dedo, sino **transistores**, que funcionan como **interruptores controlados por voltaje**.

Si intentáramos construir un computador decimal donde $0,5\text{ V}$ represente al número 1, $1,0\text{ V}$ al número 2, $1,5\text{ V}$ al número 3 y así sucesivamente, cualquier pequeña variación eléctrica provocada por el calor o por cables largos alteraría los números constantemente. En cambio, al trabajar con **solo dos estados extremos (bajo y alto)**, resulta sumamente fácil distinguir uno del otro incluso cuando la señal eléctrica sufre interferencias en el camino.

### 2.2. Un «1» y un «0» no son voltajes exactos: son rangos
Uno de los conceptos más importantes que aprendí en esta sesión es que un circuito integrado no exige exactamente $5,000\text{ V}$ para reconocer un `1` ni exactamente $0,000\text{ V}$ para reconocer un `0`. En realidad, trabaja con **rangos de voltaje**.

Tomando como referencia la familia clásica **TTL 74LS** (alimentada con $5\text{ V}$), existe un compromiso entre la compuerta que envía la señal (salida A) y la compuerta que la recibe (entrada B):

```text
   SALIDA DE LA COMPUERTA A (Lo que promete entregar)      ENTRADA DE LA COMPUERTA B (Lo que exige para leer)
   
   5,0 V ┌──────────────────────────────────────┐          5,0 V ┌──────────────────────────────────────┐
         │                                      │                │                                      │
         │         Sale un 1 lógico             │                │            Lee un 1 lógico           │
         │           (≥ 2,7 V)                  │                │               (≥ 2,0 V)              │
   2,7 V ├──────────────────────────────────────┤                │                                      │
         │                                      │          2,0 V ├──────────────────────────────────────┤
         │       Zona de transición             │                │           ZONA INDEFINIDA            │
         │         (solo de paso)               │                │   (Entre 0,8 V y 2,0 V: Error/Azar)  │
         │                                      │          0,8 V ├──────────────────────────────────────┤
   0,5 V ├──────────────────────────────────────┤                │            Lee un 0 lógico           │
         │         Sale un 0 lógico (≤ 0,5 V)   │                │               (≤ 0,8 V)              │
   0,0 V └──────────────────────────────────────┘          0,0 V └──────────────────────────────────────┘
```

* **¿Qué ocurre en la Zona Indefinida ($0,8\text{ V}$ a $2,0\text{ V}$)?** Si a la entrada de un chip llega un voltaje dentro de esta franja, el fabricante **no garantiza el comportamiento**: el circuito puede interpretar un `0`, un `1` o cambiar erráticamente.
* **Consejo práctico de laboratorio:** La causa más frecuente de caer en la zona indefinida al armar un circuito en el *protoboard* es dejar **una patilla de entrada sin conectar (flotante)**. Toda entrada que no utilicemos debe conectarse firmemente a tierra ($0\text{ V}$) o a alimentación ($+5\text{ V}$) según lo requiera la lógica.

### 2.3. Margen de Ruido: El colchón de seguridad
Al observar el diagrama anterior, notamos que la compuerta emisora es más estricta que la receptora. Esa diferencia intencional se denomina **margen de ruido** y actúa como un colchón de protección frente a interferencias eléctricas:

| Tipo de Margen | Fórmula de Cálculo (Familia 74LS) | Valor | ¿Qué significa en la práctica? |
| :--- | :---: | :---: | :--- |
| **Margen de nivel alto ($NM_H$)** | $V_{OH(\text{mín})} - V_{IH(\text{mín})} = 2,7\text{ V} - 2,0\text{ V}$ | **$0,7\text{ V}$** | Un `1` lógico que sale a $2,7\text{ V}$ puede perder hasta $0,7\text{ V}$ por caída en el cable y seguirá leyéndose correctamente como `1`. |
| **Margen de nivel bajo ($NM_L$)** | $V_{IL(\text{máx})} - V_{OL(\text{máx})} = 0,8\text{ V} - 0,5\text{ V}$ | **$0,3\text{ V}$** | Un `0` lógico que sale a $0,5\text{ V}$ puede subir hasta $0,3\text{ V}$ por ruido externo y seguirá leyéndose correctamente como `0`. |

> **⚠️ Ejemplo real de falla por ruido:** Supongamos que una compuerta envía un `0` limpio a $0,5\text{ V}$. Si cerca del cable se enciende un motor eléctrico que induce $0,4\text{ V}$ de interferencia electromagnética, el voltaje que llega a la siguiente compuerta será $0,5\text{ V} + 0,4\text{ V} = 0,9\text{ V}$. Como $0,9\text{ V}$ supera el límite de $0,8\text{ V}$, la señal entra en la **zona indefinida** y el circuito falla.

---

## 🔢 3. Sistemas Numéricos Posicionales y Conversiones

### 3.1. ¿Qué es un sistema posicional?
En un sistema posicional, el valor que aporta cada dígito depende del **lugar que ocupa** dentro de la cifra, ya que cada posición multiplica al dígito por una potencia de la base:
* En el número decimal **$555$** (base 10), los tres dígitos son iguales, pero el primero vale $5 \times 10^2 = 500$, el segundo vale $5 \times 10^1 = 50$ y el tercero vale $5 \times 10^0 = 5$, sumando $500 + 50 + 5 = 555$.
* **Contraejemplo (Sistema no posicional):** En los números romanos, como **$\text{XV}$**, la letra $\text{V}$ siempre vale 5 sin importar su posición, razón por la cual realizar operaciones aritméticas con números romanos resulta tan complejo.

### 3.2. El Sistema Binario (Base 2)
En el sistema binario únicamente disponemos de dos símbolos (`0` y `1`), y las posiciones de derecha a izquierda valen potencias de 2: **$1, 2, 4, 8, 16, 32, 64, 128\dots$**

* **Bit (*binary digit*):** Es la unidad mínima de información; un solo dígito binario (`0` o `1`).
* **Nibble:** Conjunto de **4 bits** (fundamental para hexadecimal y BCD).
* **Byte:** Conjunto de **8 bits**.

Para memorizar y convertir rápidamente cualquier byte a decimal, utilizo la siguiente tabla de pesos de 8 bits:

| Posición ($2^n$) | $2^7$ | $2^6$ | $2^5$ | $2^4$ | $2^3$ | $2^2$ | $2^1$ | $2^0$ |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **Peso en Decimal** | **128** | **64** | **32** | **16** | **8** | **4** | **2** | **1** |

**Ejemplo de binario a decimal ($1011_2$):**  
Asigno los pesos de derecha a izquierda ($8, 4, 2, 1$) y sumo únicamente las posiciones donde hay un `1`:
$$1011_2 = (1 \times 8) + (0 \times 4) + (1 \times 2) + (1 \times 1) = 8 + 0 + 2 + 1 = 11_{10}$$

### 3.3. Conversión de Decimal a Binario: Divisiones Sucesivas entre 2
Cuando quiero pasar un número decimal a binario, divido sucesivamente entre 2 hasta que el cociente sea `0` y luego **leo los restos de abajo hacia arriba** (el último resto es el bit más significativo):

**Ejemplo paso a paso: Convertir $25_{10}$ a binario**

| Operación de División | Cociente Entero | Resto (Bit obtenido) | Posición en el resultado |
| :---: | :---: | :---: | :--- |
| $25 \div 2$ | $12$ | **`1`** | $\uparrow$ Bit menos significativo (derecha, peso 1) |
| $12 \div 2$ | $6$ | **`0`** | $\uparrow$ Segundo bit (peso 2) |
| $6 \div 2$ | $3$ | **`0`** | $\uparrow$ Tercer bit (peso 4) |
| $3 \div 2$ | $1$ | **`1`** | $\uparrow$ Cuarto bit (peso 8) |
| $1 \div 2$ | $0$ | **`1`** | $\uparrow$ Bit más significativo (izquierda, peso 16) |

* **Resultado leyendo de abajo hacia arriba:** $25_{10} = \mathbf{11001_2}$.
* **Comprobación con pesos:** $16 + 8 + 0 + 0 + 1 = 25$ ✓.

> **💡 Truco adicional (Método por resta de pesos):** Para números de hasta 8 bits, también puedo preguntar de izquierda a derecha qué potencias de 2 caben dentro del $25$: cabe el $16$ (resto $9$), cabe el $8$ (resto $1$), no caben el $4$ ni el $2$, y cabe el $1$ (resto $0$). Activo los bits de $16$, $8$ y $1$ y obtengo inmediatamente `11001` sin dividir.

### 3.4. Hexadecimal (Base 16) y Octal (Base 8): Abreviaturas del Binario
Leer cadenas largas de 32 o 64 bits es propenso a errores visuales para los humanos. Por eso usamos el sistema **hexadecimal** y el **octal** como simples abreviaturas del binario:

* **Hexadecimal (Base 16):** Agrupa el número binario en **bloques de 4 bits desde la derecha** (porque $2^4 = 16$). Usa los dígitos del `0` al `9` y las letras de la `A` (10) a la `F` (15).
* **Octal (Base 8):** Agrupa el número binario en **bloques de 3 bits desde la derecha** (porque $2^3 = 8$). Usa los dígitos del `0` al `7`. Si al extremo izquierdo faltan bits para completar el grupo, se rellena con ceros a la izquierda.

| Decimal | Binario (4 bits) | Hexadecimal | Octal | Decimal | Binario (4 bits) | Hexadecimal | Octal |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **0** | `0000` | `0` | `0` | **8** | `1000` | `8` | `10` |
| **1** | `0001` | `1` | `1` | **9** | `1001` | `9` | `11` |
| **2** | `0010` | `2` | `2` | **10** | `1010` | **`A`** | `12` |
| **3** | `0011` | `3` | `3` | **11** | `1011` | **`B`** | `13` |
| **4** | `0100` | `4` | `4` | **12** | `1100` | **`C`** | `14` |
| **5** | `0101` | `5` | `5` | **13** | `1101` | **`D`** | `15` |
| **6** | `0110` | `6` | `6` | **14** | `1110` | **`E`** | `16` |
| **7** | `0111` | `7` | `7` | **15** | `1111` | **`F`** | `17` |

**Ejemplo con el número $157_{10} = 10011101_2$:**
* **Paso a Hexadecimal (grupos de 4 bits):** Separo `1001` y `1101`. Como `1001` = $9$ y `1101` = $13$ ($\text{D}$), obtengo **$9\text{D}_{16}$**.
* **Paso a Octal (grupos de 3 bits desde la derecha):** Separo `(0)10`, `011` y `101`. Como `010` = $2$, `011` = $3$ y `101` = $5$, obtengo **$235_8$**.

---

## ✍️ 4. Resolución de la Actividad 1: Reto de Conversión

A continuación desarrollo paso a paso los tres ejercicios planteados en la Actividad 1 de clase, incluyendo la comprobación mediante pesos posicionales:

### Ejercicio 1: Convertir $45_{10}$ a binario y a hexadecimal
1. **Conversión a binario (divisiones entre 2):**
   * $45 \div 2 = 22$, resto **`1`**
   * $22 \div 2 = 11$, resto **`0`**
   * $11 \div 2 = 5$, resto **`1`**
   * $5 \div 2 = 2$, resto **`1`**
   * $2 \div 2 = 1$, resto **`0`**
   * $1 \div 2 = 0$, resto **`1`**
   * Leyendo de abajo hacia arriba: **$45_{10} = 101101_2$**.
2. **Comprobación por pesos:** $32 + 0 + 8 + 4 + 0 + 1 = 45$ ✓.
3. **Conversión a hexadecimal:** Agrupo `101101` de 4 en 4 desde la derecha rellenando con dos ceros a la izquierda: `0010` ($2$) y `1101` ($13 = \text{D}$). Por lo tanto, **$45_{10} = 2\text{D}_{16}$**.

### Ejercicio 2: Convertir $\text{A}7_{16}$ a decimal
1. **Método directo por potencias de 16:**
   * Sabemos que $\text{A} = 10$ (en la posición $16^1 = 16$) y $7 = 7$ (en la posición $16^0 = 1$).
   * $\text{A}7_{16} = (10 \times 16) + (7 \times 1) = 160 + 7 = \mathbf{167_{10}}$.
2. **Comprobación pasando por binario:** $\text{A} = 1010_2$ y $7 = 0111_2 \rightarrow 10100111_2 = 128 + 32 + 4 + 2 + 1 = 167_{10}$ ✓.

### Ejercicio 3: Convertir $11100101_2$ a hexadecimal y a octal
1. **A hexadecimal (grupos de 4 bits desde la derecha):**
   * Grupo izquierdo: `1110` $= 8 + 4 + 2 + 0 = 14 \rightarrow \mathbf{E}$
   * Grupo derecho: `0101` $= 0 + 4 + 0 + 1 = 5 \rightarrow \mathbf{5}$
   * **Resultado en hexadecimal:** **$\text{E}5_{16}$**.
2. **A octal (grupos de 3 bits desde la derecha):**
   * Separo de derecha a izquierda y completo con un cero al inicio: `(0)11` | `100` | `101`
   * `011` $= 3$, `100` $= 4$, `101` $= 5$.
   * **Resultado en octal:** **$345_8$**.
3. **Comprobación en decimal:** $128 + 64 + 32 + 4 + 1 = 229_{10}$. En hex: $(14 \times 16) + 5 = 229_{10}$. En octal: $(3 \times 64) + (4 \times 8) + 5 = 192 + 32 + 5 = 229_{10}$ ✓.

---

## ➕ 5. Aritmética Binaria y Números con Signo (Complemento a Dos)

### 5.1. Suma Binaria con Acarreo
La suma en binario funciona exactamente igual que la suma decimal que aprendimos en la escuela, pero el acarreo («lo que llevo») se produce cada vez que la suma alcanza `2` ($10_2$):

* `0 + 0 = 0`
* `0 + 1 = 1`
* `1 + 1 = 10` $\rightarrow$ **escribo `0` y llevo `1`** a la columna siguiente.
* `1 + 1 + 1 = 11` $\rightarrow$ **escribo `1` y llevo `1`** a la columna siguiente.

```text
  Acarreo:    1   1   1
              1   0   1   1   (11 en decimal)
            + 0   1   1   0   ( 6 en decimal)
            ───────────────
          1   0   0   0   1   (17 en decimal: 16 + 1 = 17 ✓)
```

### 5.2. ¿Cómo representamos números negativos si el circuito no tiene signo «$-$»?
En el hardware solo existen ceros y unos; no podemos guardar un símbolo «$-$» aparte sin complicar los circuitos sumadores. Para solucionarlo, utilizamos la misma lógica de un **cuentakilómetros mecánico**: si el marcador está en `000` y retrocedemos un kilómetro, pasa a mostrar `999`.

En binario de 4 bits ocurre exactamente igual: si a `0000` le restamos `1`, retrocedemos a `1111`. Así nace el acuerdo llamado **Complemento a Dos**:
* Si el **bit más a la izquierda (bit de signo) es `0`**, el número es **positivo** (o cero).
* Si el **bit más a la izquierda es `1`**, el número es **negativo**.

#### Tabla completa de Complemento a Dos en 4 bits (Rango: de $-8$ a $+7$)

| Positivos (`Bit izq = 0`) | Valor Decimal | Negativos (`Bit izq = 1`) | Valor Decimal |
| :---: | :---: | :---: | :---: |
| `0000` | **$0$** | `1000` | **$-8$** |
| `0001` | **$+1$** | `1001` | **$-7$** |
| `0010` | **$+2$** | `1010` | **$-6$** |
| `0011` | **$+3$** | `1011` | **$-5$** |
| `0100` | **$+4$** | `1100` | **$-4$** |
| `0101` | **$+5$** | `1101` | **$-3$** |
| `0110` | **$+6$** | `1110` | **$-2$** |
| `0111` | **$+7$** | `1111` | **$-1$** |

> **💡 Nota guía adicional (Método del peso negativo):** Una forma rapidísima de saber cuánto vale un número negativo en complemento a dos (por ejemplo, `1101`) sin invertir sus bits es asignarle **peso negativo al bit más a la izquierda** ($-8$ en 4 bits, o $-128$ en 8 bits) y pesos positivos normales al resto ($+4, +2, +1$). Así, `1101` vale $-8 + 4 + 0 + 1 = \mathbf{-3}$. ¡Funciona siempre!

### 5.3. Regla práctica para obtener el negativo: Invertir bits y sumar 1
Si quiero obtener la representación de **$-3$** en 4 bits:
1. Escribo el $+3$ positivo en binario de 4 bits: **`0011`**.
2. Invierto todos sus bits (cambio `0` por `1` y `1` por `0`, lo que se conoce como *complemento a uno*): **`1100`**.
3. Sumo `1` al resultado: `1100 + 1` = **`1101`** (que representa a $-3$).

**¿Cuál es la gran ventaja para el diseño de circuitos?**  
Que **no necesitamos diseñar ni comprar un circuito restador**. Para calcular $5 - 3$, el procesador simplemente calcula $5 + (-3)$ usando el mismo circuito sumador:
```text
     0101   (+5)
   + 1101   (-3 en complemento a dos)
   ──────
  (1)0010   → Se descarta el 1 que sobra a la izquierda y queda 0010 = +2 ✓
```

### 5.4. Desbordamiento (*Overflow*): Cuando el resultado no cabe
Con $n$ bits en complemento a dos, el rango representable es de $-2^{n-1}$ a $+(2^{n-1}-1)$:
* **Con 4 bits:** de **$-8$ a $+7$**.
* **Con 8 bits:** de **$-128$ a $+127$**.
* **Con 32 bits (`int` en Java/C):** de **$-2\,147\,483\,648$ a $+2\,147\,483\,647$**.

Si en un sistema de 4 bits intento sumar $7 + 1$:
```text
    0111  (+7)
  + 0001  (+1)
  ──────
    1000  → ¡Se lee como -8! ✗ (Error de desbordamiento)
```
Como el $+8$ no existe en 4 bits con signo, el resultado invade el bit de signo y dos números positivos producen un resultado negativo. Esto mismo ocurre en programación cuando a una variable `int x = 2147483647;` le sumamos `1` y de repente se vuelve `-2147483648`.

---

## 📟 6. Códigos Binarios Especiales: BCD, Gray y ASCII

No siempre usamos el binario natural para hacer cuentas; a veces codificamos la información para adaptarla a pantallas humanas, sensores mecánicos o texto:

| Código | ¿Cómo funciona? | ¿Para qué sirve en la vida real? | Detalle clave que debo recordar |
| :--- | :--- | :--- | :--- |
| **BCD** (*Binary-Coded Decimal*) | Codifica **cada dígito decimal por separado** usando 4 bits (`0000` a `1001`). Ejemplo: $157 \rightarrow$ `0001` `0101` `0111`. | Mostrar números en **displays de 7 segmentos** (relojes digitales, calculadoras, contadores). | No es igual al binario natural ($157_{10} = 10011101_2$). Las combinaciones `1010` (10) a `1111` (15) **no se usan** (serán condiciones indiferentes `X` en la Semana 3). |
| **Código Gray** | Ordena las combinaciones de modo que **entre un número y su vecino cambie un solo bit a la vez**. | **Encoders rotativos y sensores de posición** física; además define el orden `00, 01, 11, 10` de los **Mapas de Karnaugh**. | Evita lecturas falsas: en binario natural, al pasar de $3$ (`011`) a $4$ (`100`) cambian los 3 bits al mismo tiempo; si el sensor lee a mitad de giro, detecta un valor erróneo. |
| **ASCII** | Tabla estándar de **7 bits (128 caracteres)** que asigna un número a cada letra, dígito, signo y control. | Almacenar y transmitir texto en inglés (ej. `HOLA` = `72, 79, 76, 65` en decimal o `0x48, 0x4F, 0x4C, 0x41` en hex). | El ASCII original no incluye la `ñ` ni vocales con tilde (`á, é, í, ó, ú`); por ello hoy empleamos **UTF-8**, que es compatible con ASCII pero soporta todos los idiomas. |

### Cómo construir el Código Gray con el «Método del Espejo»
Si necesito escribir el código Gray de 2 o 3 bits sin memorizarlo, aplico este procedimiento:
1. Escribo la lista de 1 bit: `0` y `1`.
2. Copio la misma lista **al revés** debajo (como un espejo): `0, 1` arriba y `1, 0` abajo.
3. Agrego un **`0` delante** a la mitad superior y un **`1` delante** a la mitad inferior:
   * **2 bits:** `00`, `01`, `11`, `10`.
   * **3 bits:** `000`, `001`, `011`, `010`, `110`, `111`, `101`, `100`.

---

## 🔌 7. Las 7 Compuertas Lógicas y el Circuito Integrado 74LS08

Las compuertas lógicas son los bloques constructivos que toman decisiones a partir de entradas binarias. En el curso utilizamos la norma gráfica **ANSI/IEEE** (formas distintivas), aunque muchos simuladores también incluyen la norma europea **IEC** (cajas rectangulares con símbolos como `1`, `&`, `>=1`, `=1`).

### 7.1. Tabla Maestra de las 7 Compuertas Lógicas (2 Entradas: A y B)

| Compuerta | Expresión Booleana | Símbolo IEC | Regla rápida para recordarla | `00` | `01` | `10` | `11` | Chip TTL 74LS típico |
| :--- | :---: | :---: | :--- | :---: | :---: | :---: | :---: | :---: |
| **NOT** *(Inversor)* | $Y = A'$ | `[ 1 ]o` | Invierte la entrada ($0 \leftrightarrow 1$). | `1` *(si A=0)* | — | `0` *(si A=1)* | — | **`74LS04`** (6 inversores) |
| **AND** | $Y = A \cdot B$ | `[ & ]` | Da `1` **solo si TODAS** las entradas valen `1`. | `0` | `0` | `0` | **`1`** | **`74LS08`** (4 AND de 2 ent.) |
| **OR** | $Y = A + B$ | `[ ≥1 ]` | Da `1` **si AL MENOS UNA** entrada vale `1`. | `0` | **`1`** | **`1`** | **`1`** | **`74LS32`** (4 OR de 2 ent.) |
| **NAND** | $Y = (A \cdot B)'$ | `[ & ]o` | Da `0` **solo si todas** valen `1` (AND negada). | **`1`** | **`1`** | **`1`** | `0` | **`74LS00`** (4 NAND de 2 ent.) |
| **NOR** | $Y = (A + B)'$ | `[ ≥1 ]o` | Da `1` **solo si NINGUNA** vale `1` (OR negada). | **`1`** | `0` | `0` | `0` | **`74LS02`** (4 NOR de 2 ent.) |
| **XOR** *(OR Exclusiva)* | $Y = A \oplus B$ | `[ =1 ]` | Da `1` si las entradas son **DISTINTAS** (como el interruptor doble de una escalera). | `0` | **`1`** | **`1`** | `0` | **`74LS86`** (4 XOR de 2 ent.) |
| **XNOR** *(Equivalencia)* | $Y = (A \oplus B)'$ | `[ =1 ]o` | Da `1` si las entradas son **IGUALES** (comparador de igualdad). | **`1`** | `0` | `0` | **`1`** | **`74LS266`** (4 XNOR de 2 ent.) |

> **🔍 Detalle visual clave:** El pequeño **círculo (burbuja)** en la salida de una compuerta siempre significa **negación (inversión)**. Además, las compuertas **NAND** y **NOR** son llamadas *universales* porque con cualquiera de ellas se puede construir todo un computador (como veremos a fondo en la Semana 3).

### 7.2. Cómo leer y conectar un Circuito Integrado en la vida real (`SN74LS08N`)
En el laboratorio no compramos compuertas sueltas, sino **circuitos integrados (chips)** con encapsulado **DIP-14** (dos filas de 7 pines que encajan en el canal central del *protoboard*).

```text
                    ┌──────┴──────┐
         Entrada 1A ─┤ 1   ╰─╯  14 ├─ VCC (+5 V Alimentación)
         Entrada 1B ─┤ 2        13 ├─ Entrada 4B
          Salida 1Y ─┤ 3        12 ├─ Entrada 4A
         Entrada 2A ─┤ 4  74LS08 11 ├─ Salida 4Y
         Entrada 2B ─┤ 5        10 ├─ Entrada 3B
          Salida 2Y ─┤ 6         9 ├─ Entrada 3A
     GND (Tierra 0V)─┤ 7         8 ├─ Salida 3Y
                    └─────────────┘
```

1. **¿Cómo cuento los pines sin equivocarme?** Coloco el chip con la **muesca semicircular hacia arriba**. El **pin 1** siempre es el de arriba a la izquierda; bajo hasta el **pin 7** (abajo a la izquierda) y luego subo por la columna derecha desde el **pin 8** hasta el **pin 14** (en sentido antihorario).
2. **Alimentación obligatoria:** Para que las 4 compuertas internas funcionen, siempre debo conectar primero el **pin 14 a $+5\text{ V}$ ($\text{VCC}$)** y el **pin 7 a Tierra ($\text{GND}$)**.
3. **¿Qué significa cada parte del código impreso `SN74LS08N`?**
   * **`SN`**: Fabricante (*Texas Instruments*).
   * **`74`**: Rango de temperatura comercial ($0\text{ °C}$ a $70\text{ °C}$). El código `54` indica rango militar/industrial.
   * **`LS`**: Familia tecnológica TTL de bajo consumo (*Low-power Schottky*). Si en la tienda me venden un **`74HC08`**, tiene exactamente la misma función y los mismos pines, pero está fabricado con tecnología **CMOS** (*High-speed CMOS*).
   * **`08`**: Función lógica del integrado (cuatro compuertas AND de 2 entradas).
   * **`N`**: Tipo de encapsulado físico (plástico DIP para *protoboard*).

---

## 🛠️ 8. Resolución de la Actividad 2: Del Enunciado a la Compuerta

A continuación resuelvo de forma detallada el taller individual de cierre de la Semana 1:

### Parte 1: ¿Qué compuerta lógica describe cada frase?

* **a) «La alarma suena solo si las dos puertas están abiertas.»**  
  * **Respuesta:** Compuerta **AND** ($Y = A \cdot B$). Se requiere que la puerta A (`1`) **y** la puerta B (`1`) estén abiertas simultáneamente para activar la alarma (`1`).
* **b) «La luz del pasillo se enciende si se pulsa cualquiera de los dos botones.»**  
  * **Respuesta:** Compuerta **OR** ($Y = A + B$). Basta con que al menos uno de los dos botones se pulse (`1`) para que la salida encienda (`1`).
* **c) «El juego marca "diferentes" si los dos jugadores eligen distinto.»**  
  * **Respuesta:** Compuerta **XOR** ($Y = A \oplus B$). Su salida vale `1` únicamente cuando una entrada es `0` y la otra es `1` (entradas distintas).
* **d) «La bomba de agua se apaga cuando el sensor de nivel está activo.»**  
  * **Respuesta:** Compuerta **NOT** ($Y = A'$). Tiene una sola entrada y actúa al revés: cuando el sensor marca lleno (`1`), la bomba pasa a apagado (`0`); cuando el sensor marca bajo (`0`), la bomba se enciende (`1`).

### Parte 2: Tabla de verdad completa de $Y = A \cdot B + C'$
* **Análisis previo:** Como la función tiene **3 variables de entrada ($A, B, C$)**, la tabla de verdad tiene exactamente $2^3 = \mathbf{8\text{ filas}}$ (numeradas del `0` al `7` en decimal).
* Resuelvo primero las columnas intermedias respetando la jerarquía ($A \cdot B$ y $C'$) y finalmente las uno con una suma lógica OR ($+$):

| Fila Decimal (Bonus) | $A$ | $B$ | $C$ | Columna Intermedia $A \cdot B$ | Columna Intermedia $C'$ | Salida Final $Y = A \cdot B + C'$ | Explicación del resultado en esa fila |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :--- |
| **0** | 0 | 0 | 0 | 0 | 1 | **`1`** | Vale `1` porque $C=0 \rightarrow C'=1$. |
| **1** | 0 | 0 | 1 | 0 | 0 | **`0`** | Ambos términos valen `0`. |
| **2** | 0 | 1 | 0 | 0 | 1 | **`1`** | Vale `1` porque $C=0 \rightarrow C'=1$. |
| **3** | 0 | 1 | 1 | 0 | 0 | **`0`** | Ambos términos valen `0`. |
| **4** | 1 | 0 | 0 | 0 | 1 | **`1`** | Vale `1` porque $C=0 \rightarrow C'=1$. |
| **5** | 1 | 0 | 1 | 0 | 0 | **`0`** | Ambos términos valen `0`. |
| **6** | 1 | 1 | 0 | 1 | 1 | **`1`** | Vale `1` tanto por $A \cdot B=1$ como por $C'=1$. |
| **7** | 1 | 1 | 1 | 1 | 0 | **`1`** | Vale `1` porque $A=1$ y $B=1 \rightarrow A \cdot B=1$. |

---

## 📌 9. Síntesis Personal de la Semana 1

1. **Dos estados con margen de seguridad:** Los valores `0` y `1` representan rangos de voltaje separados por una zona prohibida; el margen de ruido ($0,7\text{ V}$ en alto y $0,3\text{ V}$ en bajo en 74LS) protege al sistema frente a interferencias.
2. **Sistemas numéricos conectados:** El binario funciona con potencias de 2, mientras que el hexadecimal (grupos de 4 bits) y el octal (grupos de 3 bits) nos sirven para abreviar cadenas binarias extensas sin hacer cálculos largos.
3. **Restar sin restador:** Gracias al complemento a dos (invertir bits y sumar 1), cualquier resta se transforma en una suma, siempre cuidando que el resultado no supere el rango de bits disponibles (*overflow*).
4. **Del papel al chip real:** Cada operación lógica se traduce en una de las 7 compuertas estándar que podemos simular en **Logisim-evolution** (en formato ANSI) y cablear físicamente alimentando los pines 14 ($\text{VCC}$) y 7 ($\text{GND}$) en los integrados DIP-14.

---

| 🏠 [Portada Principal](README.md) | [Semana 2 ➡️](semana-2.md) |
| :--- | ---: |
