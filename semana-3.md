---

# 📙 Semana 3: Mapas de Karnaugh, Condiciones Indiferentes y Compuertas Universales NAND/NOR


---

## 🎯 Mi Objetivo de Aprendizaje en esta Semana

En esta tercera semana unifico el estudio de las dos sesiones dedicadas a la optimización visual e implementación industrial de circuitos combinacionales. Al terminar la Semana 2 nos quedó una duda clave: *cuando simplificamos una ecuación con álgebra de Boole, ¿cómo sabemos con certeza que ya llegamos a la expresión más corta posible?*

Para resolver ese problema, en esta guía aprendo a:
1. Utilizar los **Mapas de Karnaugh (K-maps)** de 3 y 4 variables para encontrar visualmente la expresión mínima tanto en **SOP** (agrupando los `1`) como en **POS** (agrupando los `0`).
2. Distinguir entre **implicantes**, **implicantes primos** e **implicantes primos esenciales** para no incluir términos redundantes.
3. Aprovechar las **condiciones indiferentes (`X` o *don't care*)** —aquellas combinaciones de entrada que nunca ocurren en la realidad— como comodines para formar grupos más grandes y económicos.
4. Implementar cualquier circuito utilizando **una sola familia de compuertas universales (solo NAND o solo NOR)**, reduciendo el inventario de circuitos integrados y preparándonos para el **Proyecto Integrador con la tarjeta FPGA Basys 3**.

---

# PARTE I · Mapas de Karnaugh: Minimización Visual en SOP y POS

## 🧩 1. Del Álgebra al Mapa: La Clave está en la Adyacencia y el Código Gray

### 1.1. El reto inicial y el teorema de adyacencia
Al empezar la primera sesión analizamos la función F(A,B,C) = Σm(0, 2, 4, 5, 6). Si intentamos reducirla con álgebra de Boole a ciegas, corremos el riesgo de dejar términos de más.

La pieza secreta que hace funcionar al Mapa de Karnaugh es el **teorema de adyacencia** que vimos en la Semana 2:
X · Y + X · Y' = X

Observemos qué ocurre al sumar los minitérminos m_4 (`100`) y m_5 (`101`):
* En m_4 = AB'C', las variables valen A=1, B=0, C=0.
* En m_5 = AB'C, las variables valen A=1, B=0, C=1.
* Como ambos términos **difieren en una sola variable (C cambia de `0` a `1`)**, esa variable que cambia desaparece y solo se conservan las que permanecen iguales: **AB'**.

### 1.2. ¿Por qué las cabeceras del mapa llevan el orden `00, 01, 11, 10`?
Si ordenáramos las columnas del mapa en binario normal (`00, 01, 10, 11`), al pasar de la segunda columna (`01`) a la tercera (`10`) cambiarían **dos bits al mismo tiempo**, rompiendo la vecindad lógica.

Por esa razón Maurice Karnaugh organizó las filas y columnas siguiendo el **Código Gray (`00, 01, 11, 10`)** que estudiamos en la Semana 1:
* De `00` a `01` \rightarrow cambia 1 solo bit.
* De `01` a `11` \rightarrow cambia 1 solo bit.
* De `11` a `10` \rightarrow cambia 1 solo bit.
* Y del extremo final `10` al extremo inicial `00` \rightarrow ¡también cambia 1 solo bit!

Gracias al código Gray, **dos celdas que se tocan horizontal o verticalmente (incluidos los bordes opuestos) siempre difieren en una sola variable**.

---

## 🗺️ 2. Estructura de los Mapas de 3 y 4 Variables

### 2.1. Mapa de Karnaugh de 3 Variables (2^3 = 8\text{ celdas)
Colocamos la variable A en las filas (`0` y `1`) y las variables BC en las columnas siguiendo el código Gray (`00, 01, 11, 10`):

| A \setminus BC | **`00`** | **`01`** | **`11`** *(¡Atención!)* | **`10`** |
| :---: | :---: | :---: | :---: | :---: |
| **`0`** | m_0 (`000`) | m_1 (`001`) | **m_3 (`011`)** | **m_2 (`010`)** |
| **`1`** | m_4 (`100`) | m_5 (`101`) | **m_7 (`111`)** | **m_6 (`110`)** |

> **⚠️ Error clásico que debo evitar:** Debido al orden Gray de las columnas, la celda **m_3 va en la tercera columna y m_2 va en la cuarta** (igual ocurre abajo con **m_7 y m_6**). Olvidar este cruce al vaciar la tabla de verdad altera todo el diseño.

### 2.2. Mapa de Karnaugh de 4 Variables (2^4 = 16\text{ celdas)
Tanto las filas (AB) como las columnas (CD) siguen el orden Gray (`00, 01, 11, 10`), por lo que el salto de numeración ocurre tanto en las dos últimas columnas como en las dos últimas filas:

| AB \setminus CD | **`00`** | **`01`** | **`11`** | **`10`** |
| :---: | :---: | :---: | :---: | :---: |
| **`00`** | **m_0`** | **m_1`** | **m_3`** | **m_2`** |
| **`01`** | **m_4`** | **m_5`** | **m_7`** | **m_6`** |
| **`11`** *(Fila 3)* | **m_{12**** | **m_{13**** | **m_{15**** | **m_{14**** |
| **`10`** *(Fila 4)* | **m_8`** | **m_9`** | **m_{11**** | **m_{10**** |

---

## 📏 3. Las 4 Reglas para Agrupar y los Implicantes Primos

### 3.1. Reglas de Agrupación en el Mapa
1. **Tamaño en potencias de 2 (2^k):** Un grupo válido solo puede contener **1, 2, 4, 8\text{ o ** 16\text{ celdas**. Nunca se permiten grupos de 3, 5 o 6 celdas.
   * **¿Cuántas variables elimina cada grupo?** Un grupo de 2^k celdas elimina exactamente **k variables** (y deja n - k literales en el término):
     * Grupo de 1 (2^0) \rightarrow elimina 0 variables.
     * Grupo de 2 (2^1) \rightarrow elimina 1 variable.
     * Grupo de 4 (2^2) \rightarrow elimina 2 variables.
     * Grupo de 8 (2^3) \rightarrow elimina 3 variables.
2. **Forma estrictamente rectangular o cuadrada:** Los grupos solo pueden formarse en horizontal, vertical o bloques rectangulares. **No valen diagonales ni figuras en forma de «L»**.
3. **Lo más grande posible:** Cuanto más grande sea el grupo, más variables cambian dentro de él y **más corto queda el término resultante**. Se permite (y es deseable) que dos grupos compartan algún `1` si eso ayuda a agrandar un grupo vecino.
4. **El mapa se enrolla como un cilindro o toroide (dona):**
   * El **borde izquierdo (`00`) es vecino del borde derecho (`10`)**.
   * El **borde superior (`00`) es vecino del borde inferior (`10`)**.
   * En un mapa de 4 variables, **las 4 esquinas (m_0, m_2, m_8, m_{10**) son vecinas entre sí** y forman un grupo válido de 4 celdas cuyo término siempre es **B'D'**.

### 3.2. Resolución del Reto Inicial: F(A,B,C) = Σm(0, 2, 4, 5, 6)

| A \setminus BC | `00` | `01` | `11` | `10` |
| :---: | :---: | :---: | :---: | :---: |
| **`0`** | **`1`** (m_0) | `0` (m_1) | `0` (m_3) | **`1`** (m_2) |
| **`1`** | **`1`** (m_4) | **`1`** (m_5) | `0` (m_7) | **`1`** (m_6) |

* **Grupo 1 (4 celdas enrollando los bordes izquierdo y derecho: m_0, m_2, m_4, m_6):** En estas cuatro celdas A cambia (`0` y `1`) y B cambia (`0` y `1`), pero **C vale siempre `0`**. Como estamos en SOP, C=0 se lee como **C'**.
* **Grupo 2 (2 celdas horizontales: m_4, m_5):** Para cubrir el `1` de m_5, lo uno con su vecino m_4. Allí A=1 y B=0 se mantienen fijos mientras C cambia \rightarrow se lee como **AB'**.
* **Resultado mínimo garantizado:** **F = C' + AB'**.

### 3.3. Implicantes, Implicantes Primos y Primos Esenciales
Para asegurarnos de no agregar grupos de sobra, clasificamos los grupos con tres conceptos clave:

| Concepto | Definición clara | ¿Cómo lo identifico en el mapa? |
| :--- | :--- | :--- |
| **Implicante** | Cualquier grupo rectangular válido de potencias de 2 formado por unos (o `X`). | Cualquier celda individual, par o cuarteto válido dentro del mapa. |
| **Implicante Primo** | Un grupo válido que **ya no se puede doblar ni agrandar más** sin tocar un cero. | Un grupo que creció hasta su tamaño máximo posible. |
| **Implicante Primo Esencial** | Aquel implicante primo que es **el único capaz de cubrir algún `1` específico** del mapa. | Busco los `1` que solo tienen una manera de agruparse; esos grupos van **obligatoriamente** en la respuesta final. |

**Procedimiento de 5 pasos para obtener la SOP mínima:**
1. **Colocar** los `1`, `0` y `X` en sus celdas correspondientes cuidando el orden Gray.
2. **Agrupar** formando los implicantes primos más grandes posibles.
3. **Elegir primero todos los primos esenciales** (aquellos que cubren algún `1` exclusivo).
4. **Cubrir los `1` restantes** eligiendo la menor cantidad posible de grupos adicionales.
5. **Leer un término por cada grupo elegido:** si una variable vale siempre `1` se escribe normal (A); si vale siempre `0` se escribe negada (A'); si cambia dentro del grupo, desaparece.

---

## ⚖️ 4. Ejemplo Guiado de 4 Variables: Comparación SOP vs. POS

Analicemos la función vista en clase: **F(A,B,C,D) = Σm(0, 1, 2, 5, 8, 9, 10)**.

| AB \setminus CD | `00` | `01` | `11` | `10` |
| :---: | :---: | :---: | :---: | :---: |
| **`00`** | **`1`** (m_0) | **`1`** (m_1) | `0` (m_3) | **`1`** (m_2) |
| **`01`** | `0` (m_4) | **`1`** (m_5) | `0` (m_7) | `0` (m_6) |
| **`11`** | `0` (m_{12**) | `0` (m_{13**) | `0` (m_{15**) | `0` (m_{14**) |
| **`10`** | **`1`** (m_8) | **`1`** (m_9) | `0` (m_{11**) | **`1`** (m_{10**) |

### 4.1. Solución en SOP (Agrupando los `1` — Lectura normal: `1`\rightarrow A, `0`\rightarrow A')
1. **Grupo verde (m_0, m_1, m_8, m_9 — bordes superior e inferior):** Se mantienen constantes B=0 y C=0 \rightarrow **B'C'**.
2. **Grupo naranja (m_0, m_2, m_8, m_{10** — las 4 esquinas):** Se mantienen constantes B=0 y D=0 \rightarrow **B'D'**.
3. **Grupo azul (m_1, m_5 — par vertical):** Se mantienen constantes A=0, C=0, D=1 \rightarrow **A'C'D**.
* **Expresión SOP mínima:** **F = B'C' + B'D' + A'C'D** \rightarrow **3 términos y 7 literales**.

### 4.2. Solución en POS (Agrupando los `0` — Lectura invertida: `0`\rightarrow A, `1`\rightarrow A')
Usando el mismo mapa, ahora agrupamos las celdas que contienen **`0`** para formar sumas (maxitérminos simplificados):
1. **Grupo rosado (m_{12**, m_{13**, m_{15**, m_{14** — toda la fila AB = 11):** Como A=1 y B=1, con lectura invertida se escribe **(A' + B')**.
2. **Grupo celeste (m_3, m_7, m_{15**, m_{11** — toda la columna CD = 11):** Como C=1 y D=1, con lectura invertida se escribe **(C' + D')**.
3. **Grupo amarillo (m_4, m_6, m_{12**, m_{14** — bordes izquierdo y derecho en filas `01` y `11`):** Se mantienen fijos B=1 y D=0; con lectura invertida se escribe **(B' + D)**.
* **Expresión POS mínima:** **F = (A' + B')(B' + D)(C' + D')** \rightarrow **3 factores y 6 literales**.

> **💡 Conclusión de ingeniería:** Al contar los literales, descubrimos que para esta función la forma **POS (6 literales)** es más compacta que la forma **SOP (7 literales)**. Siempre conviene revisar ambas antes de construir el circuito.

---

## 🛠️ 5. Resolución Completa del Taller en Parejas (T1 a T4) y Ticket de Salida (Sesión 1)

### Ejercicio T1: F(A,B,C) = Σm(1, 3, 4, 6) (Mapa de 3 variables)

| A \setminus BC | `00` | `01` | `11` | `10` |
| :---: | :---: | :---: | :---: | :---: |
| **`0`** | `0` (m_0) | **`1`** (m_1) | **`1`** (m_3) | `0` (m_2) |
| **`1`** | **`1`** (m_4) | `0` (m_5) | `0` (m_7) | **`1`** (m_6) |

* **Grupo 1 (m_1, m_3 en la fila A=0):** A=0 y C=1 son constantes (B cambia) \rightarrow **A'C**.
* **Grupo 2 (m_4, m_6 enrollando los extremos de la fila A=1):** A=1 y C=0 son constantes (B cambia) \rightarrow **AC'**.
* **Resultado SOP mínima:** **F = A'C + AC'** *(que equivale exactamente a una sola compuerta **XOR** entre A y C: F = A ⊕ C)*.

---

### Ejercicio T2: F(A,B,C,D) = Σm(0, 1, 4, 5, 6, 7, 14, 15) — ¿Algún primo sobra?

| AB \setminus CD | `00` | `01` | `11` | `10` |
| :---: | :---: | :---: | :---: | :---: |
| **`00`** | **`1`** (m_0) | **`1`** (m_1) | `0` (m_3) | `0` (m_2) |
| **`01`** | **`1`** (m_4) | **`1`** (m_5) | **`1`** (m_7) | **`1`** (m_6) |
| **`11`** | `0` (m_{12**) | `0` (m_{13**) | **`1`** (m_{15**) | **`1`** (m_{14**) |
| **`10`** | `0` (m_8) | `0` (m_9) | `0` (m_{11**) | `0` (m_{10**) |

* **Análisis de Implicantes Primos:**
  1. **Cuarteto izquierdo (m_0, m_1, m_4, m_5):** Es **primo esencial** porque es el único que cubre a m_0 y m_1. Se lee como **A'C'**.
  2. **Cuarteto derecho (m_6, m_7, m_{14**, m_{15**):** Es **primo esencial** porque es el único que cubre a m_{14** y m_{15**. Se lee como **BC**.
  3. **Cuarteto horizontal de la fila `01` (m_4, m_5, m_7, m_6):** Vale A'B, pero **¡SOBRA (es redundante)!** ¿Por qué? Porque sus cuatro unos (m_4, m_5, m_6, m_7) ya quedaron 100 % cubiertos por los dos primos esenciales anteriores.
* **Resultado SOP mínima:** **F = A'C' + BC**.

---

### Ejercicio T3: F(A,B,C,D) = Σm(0, 2, 5, 8, 10, 13) — Atención a las esquinas

| AB \setminus CD | `00` | `01` | `11` | `10` |
| :---: | :---: | :---: | :---: | :---: |
| **`00`** | **`1`** (m_0) | `0` (m_1) | `0` (m_3) | **`1`** (m_2) |
| **`01`** | `0` (m_4) | **`1`** (m_5) | `0` (m_7) | `0` (m_6) |
| **`11`** | `0` (m_{12**) | **`1`** (m_{13**) | `0` (m_{15**) | `0` (m_{14**) |
| **`10`** | **`1`** (m_8) | `0` (m_9) | `0` (m_{11**) | **`1`** (m_{10**) |

* **Grupo 1 (Las 4 esquinas: m_0, m_2, m_8, m_{10**):** En las cuatro esquinas B=0 y D=0 permanecen constantes \rightarrow **B'D'**.
* **Grupo 2 (Par vertical interior: m_5, m_{13**):** En ambas celdas B=1, C=0, D=1 permanecen constantes (A cambia de `0` a `1`) \rightarrow **BC'D**.
* **Resultado SOP mínima:** **F = B'D' + BC'D**.

---

### Ejercicio T4: Diagnóstico de Errores — ¿Qué está mal en el mapa entregado por el estudiante?
En la diapositiva 17 se muestra el mapa que un estudiante entregó para F = Σm(0, 1, 4, 5, 6, 7, 14, 15) obteniendo la expresión incorrecta F = A'C' + BC + A'B + A'B'C'. Aquí identifico y explico los **3 errores cometidos**:

1. **Error 1 (Orden incorrecto en el encabezado de las columnas `CD`):** El estudiante rotuló las columnas como `00, 01, 10, 11` (binario natural) en lugar del **Código Gray obligatorio (`00, 01, 11, 10`)**, provocando una contradicción entre las cabeceras y los minitérminos internos.
2. **Error 2 (Grupo de 3 celdas e inclusión de un término redundante):** En la fila `01` trazó un grupo morado de **3 celdas** (violando la regla de que todo grupo debe tener tamaño 2^k: 1, 2, 4, 8...) y a partir de esa fila agregó el término **A'B**, el cual es un implicante redundante porque todas sus celdas ya están cubiertas por los grupos verde (A'C') y azul (BC).
3. **Error 3 (Subgrupo redundante dentro de un grupo mayor):** Trazó un grupo amarillo de 2 celdas (m_0, m_1 \rightarrow A'B'C') que ya estaba **totalmente contenido dentro del grupo verde de 4 celdas (m_0, m_1, m_4, m_5 \rightarrow A'C')**, violando la regla de tomar únicamente los grupos más grandes posibles.
* **Expresión correcta una vez corregidos los 3 errores:** **F = A'C' + BC**.

---

### Resolución del Ticket de Salida (Sesión 1): Simplificar F(A,B,C,D) = Σm(0, 2, 5, 7, 8, 10, 13, 15)

| AB \setminus CD | `00` | `01` | `11` | `10` |
| :---: | :---: | :---: | :---: | :---: |
| **`00`** | **`1`** (m_0) | `0` (m_1) | `0` (m_3) | **`1`** (m_2) |
| **`01`** | `0` (m_4) | **`1`** (m_5) | **`1`** (m_7) | `0` (m_6) |
| **`11`** | `0` (m_{12**) | **`1`** (m_{13**) | **`1`** (m_{15**) | `0` (m_{14**) |
| **`10`** | **`1`** (m_8) | `0` (m_9) | `0` (m_{11**) | **`1`** (m_{10**) |

* **Grupo 1 (Las 4 esquinas: m_0, m_2, m_8, m_{10**):** Se mantienen fijos B=0 y D=0 \rightarrow **B'D'**.
* **Grupo 2 (El bloque central de 4 celdas: m_5, m_7, m_{13**, m_{15**):** Se mantienen fijos B=1 y D=1 \rightarrow **BD**.
* **Expresión SOP mínima:** **F = B'D' + BD**.
* **Respuesta a la pregunta de conexión con la Semana 1:** *¿Qué compuerta de la Semana 1 es esta?*  
  Vale `1` cuando B y D son iguales (ambos `0` o ambos `1`), por lo tanto es exactamente una compuerta **XNOR** entre B y D: F = (B ⊕ D)'.

---

# PARTE II · Condiciones Indiferentes (`X`) e Implementación con NAND/NOR

## 🃏 6. Condiciones Indiferentes (*Don't Care* `X`): Comodines de Diseño

### 6.1. ¿Por qué existen combinaciones que nunca ocurren?
Recordemos el código **BCD** de la Semana 1: utiliza 4 bits para representar un dígito decimal del `0` (`0000`) al `9` (`1001`). ¿Qué sucede con las 6 combinaciones restantes desde el `1010` (10) hasta el `1111` (15)? **Nunca van a entrar al circuito** porque un dígito BCD jamás toma esos valores.

Como esas entradas jamás ocurrirán en el funcionamiento normal, no nos importa si para ellas el circuito saca un `0` o un `1`. Las marcamos en la tabla y en el mapa con una **`X` (condición indiferente)** y las expresamos con la notación:
F = Σm(\text{minitérminos**) + d(\text{indiferentes**)

### 6.2. Las 3 Reglas para Usar las `X` en el Mapa de Karnaugh
1. **Usarla como `1`:** Si incluir una `X` me permite **duplicar o agrandar un grupo de unos**, la incluyo sin dudarlo (el término resultante será más corto).
2. **Usarla como `0`:** Si una `X` no me ayuda a agrandar ningún grupo de unos, simplemente **la ignoro** (ninguna `X` está obligada a quedar cubierta).
3. **Nunca agrupar solo `X`:** Jamás debo formar un grupo compuesto exclusivamente por letras `X` sin ningún `1` adentro, ya que eso agregaría compuertas inútiles al circuito.

### 6.3. Ejemplo Guiado: Detector de Dígito \text{BCD** ≥ 5
Queremos que la salida F valga `1` cuando el dígito BCD esté entre `5` y `9`:
F(A,B,C,D) = Σm(5, 6, 7, 8, 9) + d(10, 11, 12, 13, 14, 15)

| AB \setminus CD | `00` | `01` | `11` | `10` |
| :---: | :---: | :---: | :---: | :---: |
| **`00`** | `0` (m_0) | `0` (m_1) | `0` (m_3) | `0` (m_2) |
| **`01`** | `0` (m_4) | **`1`** (m_5) | **`1`** (m_7) | **`1`** (m_6) |
| **`11`** | **`X`** (d_{12**) | **`X`** (d_{13**) | **`X`** (d_{15**) | **`X`** (d_{14**) |
| **`10`** | **`1`** (m_8) | **`1`** (m_9) | **`X`** (d_{11**) | **`X`** (d_{10**) |

* **Si NO usamos las `X` (tratándolas como `0`):** Solo podemos hacer tres pares pequeños: (m_8, m_9) \rightarrow AB'C', (m_5, m_7) \rightarrow A'BD y (m_6, m_7) \rightarrow A'BC. Obtenemos **F = AB'C' + A'BD + A'BC (9 literales y 3 circuitos integrados: `7404 + 7411 + 7432`)**.
* **Usando las `X` inteligentemente como comodines:**
  1. Con las seis `X` de las filas inferiores formo un **octeto gigante (m_8, m_9, d_{10**, d_{11**, d_{12**, d_{13**, d_{14**, d_{15**)** que se lee simplemente como **A**.
  2. Agrando (m_5, m_7) con (d_{13**, d_{15**) formando un **cuarteto** que se lee como **BD**.
  3. Agrando (m_6, m_7) con (d_{14**, d_{15**) formando otro **cuarteto** que se lee como **BC**.
  * **Resultado optimizado:** **F = A + BD + BC** (**5 literales y solo 2 circuitos integrados**, sin ningún inversor).

---

## 🔄 7. Universalidad de las Compuertas NAND y NOR

En la industria y en el laboratorio resulta mucho más económico comprar un solo tipo de circuito integrado (por ejemplo, puros chips `7400` de compuertas NAND) y aprovechar todas sus compuertas internas que comprar tres modelos distintos (`7404` NOT + `7408` AND + `7432` OR) y dejar la mitad de cada chip sin usar.

### 7.1. Tabla de Construcción Universal (Cómo hacer NOT, AND y OR con una sola familia)

| Función Base | Construcción usando **Solo NAND (`7400`)** | Construcción usando **Solo NOR (`7402`)** | Explicación práctica |
| :--- | :---: | :---: | :--- |
| **NOT (A')** | (A · A)' = A' | (A + A)' = A' | Uno las dos entradas de la compuerta al mismo cable A (o en NAND conecto la otra entrada a `1` fijo). |
| **AND (A · B)** | ((A · B)')' = AB | (A' + B')' = AB | Con NAND es una NAND seguida de otra NAND como inversor; con NOR se invierten primero las entradas (De Morgan). |
| **OR (A + B)** | (A' · B')' = A + B | ((A + B)')' = A + B | Con NAND se invierten las entradas y entran a una NAND (De Morgan); con NOR es una NOR seguida de un inversor NOR. |

### 7.2. De SOP a Red `NAND-NAND` en 4 Pasos
Cualquier expresión mínima en **SOP** se transforma directamente en un circuito de dos niveles compuesto **únicamente por compuertas NAND** (donde las antiguas AND del nivel 1 y la antigua OR del nivel 2 se reemplazan por NANDs):

Tomemos nuestra ecuación optimizada del detector BCD:
1. **Paso 1 (Partir de la SOP mínima):** F = A + BD + BC
2. **Paso 2 (Aplicar doble negación, que no altera la función):** F = ((A + BD + BC)')'
3. **Paso 3 (Aplicar De Morgan sobre la barra interior):** F = (A' · (BD)' · (BC)')'
4. **Paso 4 (Identificar cada paréntesis negado como una NAND):**  
   F = \text{NAND**\big(  A',\; \text{NAND**(B,D),\; \text{NAND**(B,C)  \big)

```text
   Nivel 1 (Antes AND, ahora NAND)          Nivel 2 (Antes OR, ahora NAND de 3 entradas)
   
   A ──┬──[ NAND ]o──── A' ─────────────────┐
       └──┘                                 │
   B ─────[      ]                          ├───[        ]
          [ NAND ]o──── (BD)' ──────────────┼───[  NAND  ]o───► Salida F = A + BD + BC
   D ─────[      ]                          │   [ (7410) ]
                                            │
   B ─────[      ]                          │
          [ NAND ]o──── (BC)' ──────────────┘
   C ─────[      ]
```

> **💡 Caso dual (De POS a `NOR-NOR`):** Si nuestra expresión mínima está en **POS** —por ejemplo, F = (A'+B')(B'+D)(C'+D')—, aplicamos el mismo truco y reemplazamos todas las compuertas por **NOR** (`7402` / `7427`), formando una red `NOR-NOR` de dos niveles.

### 7.3. Comparación Final de Costo del Detector BCD (≥ 5)

| Diseño | Chips Necesarios | Cantidad Total de CI | Ventaja Principal |
| :--- | :--- | :---: | :--- |
| **1. Sin usar las `X` (AND-OR)** | `7404` (NOT) + `7411` (AND 3 ent.) + `7432` (OR) | **3 CI** | Ninguna (desperdicia comodines). |
| **2. Usando las `X` (AND-OR)** | `7408` (AND 2 ent.) + `7432` (OR) | **2 CI** | Las `X` eliminaron un chip completo y todos los inversores. |
| **3. Usando las `X` (Solo NAND)** | `7400` (NAND 2 ent.) + `7410` (NAND 3 ent.) | **2 CI** | Mismo número de chips (2 CI) y el mismo retardo (2 niveles), pero **toda la red pertenece a una sola familia lógica (NAND)**. |

---

## 💬 8. Resolución de la Actividad «Piensa, Pareja, Comparte» (Sesión 2)

### Pregunta 1: ¿Por qué la variable A entra complementada (A') en el circuito NAND-NAND de F = A + BD + BC, mientras que B, C y D entran sin complementar?
* **Mi respuesta fundamentada:**  
  En una red `NAND-NAND` de dos niveles, las señales que pasan por el **Nivel 1** (BD y BC) atraviesan primero una compuerta NAND (que las invierte a (BD)' y (BC)') y luego atraviesan la compuerta NAND del **Nivel 2** (que aplica De Morgan y vuelve a invertirlas, recuperando BD + BC).  
  En cambio, como la variable **A es un literal solitario** (no está multiplicada por ninguna otra variable), no pasa por una compuerta de producto en el Nivel 1, sino que entra directamente a la compuerta NAND de salida (Nivel 2). Si entrara como A normal, la NAND de salida la invertiría y obtendríamos A' + BD + BC. Por eso **todo literal solitario debe entrar ya complementado (A') a la NAND final**, para que al invertirse por segunda vez salga como A positivo: (A')' = A.

### Pregunta 2: Convierta F = A'C' + BC (del taller T2) a una red NAND-NAND. ¿Cuántos chips `7400` necesita?
1. **Conversión algebraica en 4 pasos:**
   * **Paso 1 (SOP mínima):** F = A'C' + BC
   * **Paso 2 (Doble negación):** F = ((A'C' + BC)')'
   * **Paso 3 (De Morgan interno):** F = ((A'C')' · (BC)')'
   * **Paso 4 (Expresión en bloques NAND de 2 entradas):**
     F = \text{NAND**\big(  \text{NAND**(A', C'),\; \text{NAND**(B, C)  \big)
2. **Recuento exacto de compuertas NAND de 2 entradas y chips `7400`:**
   * Para generar A' y C' usamos **2 compuertas NAND** conectadas como inversores: \text{NAND**(A,A) y \text{NAND**(C,C).
   * Para el Nivel 1 usamos **2 compuertas NAND**: una para (A'C')' y otra para (BC)'.
   * Para el Nivel 2 (salida F) usamos **1 compuerta NAND** que une los dos resultados anteriores.
   * **Total de compuertas:** 2 + 2 + 1 = **5\text{ compuertas NAND de 2 entradas****.
   * **Cantidad de chips `7400`:** Como cada circuito integrado `7400` contiene **4 compuertas NAND**, para alojar 5 compuertas necesitamos exactamente **2 chips `7400`** (quedando 3 compuertas libres en el segundo chip).

---

## 🚀 9. Lanzamiento del Proyecto Integrador: Un Sistema, Tres Versiones en Basys 3

Al cierre de la Semana 3 se presentó la hoja de ruta del **Proyecto Integrador** de la asignatura, en el cual trabajamos organizados en **8 equipos con 4 roles rotativos** y una tarjeta FPGA **Digilent Basys 3** por equipo.

### 9.1. Evolución del Proyecto a lo largo del Semestre

| Etapa / Entrega | Semana y Unidad | Enfoque Técnico y Entregables |
| :--- | :---: | :--- |
| **Versión 1: Bloque Combinacional** | **Semana 6 · Unidad 1** | Diseño clásico: tabla de verdad, expresión mínima por Karnaugh y esquema simulado en **Logisim-evolution**. |
| **Versión 2: Se Agrega la Memoria** | **Semana 11 · Unidad 2** | Diseño secuencial: diagrama de estados, incorporación de *flip-flops* y esquema completo simulado. |
| **Versión 3: Descrito en VHDL** | **Semana 16 · Unidad 3** | Diseño moderno: descripción en hardware con **VHDL**, simulación funcional, implementación física en la tarjeta **Basys 3** y sustentación individual contrastando el método clásico con el moderno. |

### 9.2. Los 4 Roles Rotativos del Equipo y Recursos de la Tarjeta Basys 3
* **Rol 1 — Especificación:** Traduce el problema a entradas, salidas definidas y tabla de casos de prueba.
* **Rol 2 — Diseño:** Construye la tabla de verdad, los mapas de Karnaugh, las ecuaciones mínimas y el esquema lógico.
* **Rol 3 — Verificación:** Ejecuta la simulación en Logisim, contrasta cada fila con lo predicho y documenta correcciones.
* **Rol 4 — Documentación:** Redacta el informe técnico del equipo e incluye la declaración transparente de uso de herramientas de IA.
* **Límites físicos de nuestra tarjeta Basys 3:** Nuestro diseño debe ajustarse a un máximo de **16 interruptores (*switches*)**, **5 pulsadores (*buttons*)**, **16 LEDs individuales** y **4 displays de 7 segmentos**.

---

## 📌 10. Síntesis Personal de la Semana 3

1. **El orden Gray hace visible la adyacencia:** Gracias a la secuencia `00, 01, 11, 10`, simplificar deja de ser un juego de azar algebraico y se convierte en agrupar rectángulos de tamaño 1, 2, 4, 8\text{ o ** 16 recordando que los bordes opuestos y las 4 esquinas se tocan.
2. **Primos esenciales primero:** Elegir primero los grupos que cubren unos exclusivos evita añadir términos redundantes (como comprobamos en el ejercicio T2 y en el diagnóstico de errores T4).
3. **Las condiciones indiferentes (`X`) ahorran chips reales:** Aprovechar como `1` las combinaciones que jamás ocurren (como los códigos `1010` a `1111` en BCD) reduce drásticamente el número de literales.
4. **Una sola familia lo construye todo:** Cualquier expresión SOP se convierte en una red `NAND-NAND` y cualquier POS en `NOR-NOR`, cuidando siempre complementar los literales que entran solos al segundo nivel.

---

| [⬅️ Semana 2](semana-2.md) | [Semana 4 ➡️](semana-4.md) |
| :--- | ---: |
