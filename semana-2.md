
---

# 📗 Semana 2: Álgebra de Boole y Formas Canónicas

**Asignatura:** Diseño de Circuitos · **Unidad 1:** Análisis de lógica combinacional con diseño clásico  
**Autor:** Luis Anderson Carranza Quispe · **Docente:** Ing. Luis Darío Shinche Cueva, Mg.  
**Institución:** Universidad Nacional de Loja · Carrera de Computación · Segundo Ciclo

---

## 🎯 Mi Objetivo de Aprendizaje en esta Semana

Durante la segunda semana dimos un salto fundamental: aprendimos un método ordenado y matemático para pasar desde un **problema escrito en palabras (enunciado)** hasta una **tabla de verdad**, extraer su **expresión algebraica exacta (forma canónica)** y finalmente **simplificarla** para construir un circuito que utilice la menor cantidad posible de chips físicos.

Al finalizar esta guía puedo:
1. Traducir cualquier circuito combinacional a una expresión booleana y viceversa respetando la precedencia de operadores.
2. Evitar ambigüedades al interpretar los requisitos de un cliente y volcarlos en una tabla de verdad.
3. Aplicar los **postulados de Huntington**, los **teoremas del Álgebra de Boole** y las **Leyes de De Morgan** para transformar y reducir expresiones.
4. Escribir una función booleana en sus dos formas canónicas únicas: **Suma de Productos (SOP, $\Sigma m$)** y **Producto de Sumas (POS, $\Pi M$)**, eligiendo la alternativa más económica en circuitos integrados.

---

## 🔄 1. Un Circuito y una Expresión Algebraica son lo Mismo

Al comenzar la semana retomamos el circuito que simulamos en la práctica **APE 01**:

```text
   A ───┐
        │ AND (·) ───┐
   B ───┘            ├─── OR (+) ───► Salida Y = A·B + C'
                     │
   C ───► NOT (') ───┘
```

### 1.1. Análisis de las preguntas de activación
* **¿Qué compuertas reconozco en el esquema?** Una compuerta **AND** de dos entradas (arriba), un inversor **NOT** en la entrada $C$ (abajo) y una compuerta **OR** que une ambos resultados en la salida $Y$.
* **Si $A = 1, B = 0, C = 0$, ¿cuánto vale $Y$?**  
  Sustituyo en la expresión: $A \cdot B = 1 \cdot 0 = 0$, mientras que $C' = 0' = 1$. Luego en la compuerta OR: $Y = 0 + 1 = \mathbf{1}$.
* **¿Y si $A = 1, B = 1, C = 1$?**  
  Sustituyo: $A \cdot B = 1 \cdot 1 = 1$, mientras que $C' = 1' = 0$. Al sumarlos en la OR: $Y = 1 + 0 = \mathbf{1}$.
* **¿Cómo lo diría con palabras claras?**  
  *«La salida $Y$ se activa cuando las entradas $A$ y $B$ están activas al mismo tiempo, o bien cuando la entrada $C$ está apagada»*.

### 1.2. Tabla de Equivalencia y Orden de Precedencia
Cada elemento físico del circuito tiene una traducción directa al lenguaje matemático del Álgebra de Boole:

| Elemento en el Circuito Físico | Equivalente en la Expresión Booleana | Prioridad de Cálculo (Precedencia) |
| :--- | :--- | :---: |
| **Cable de entrada** | Variable lógica ($A, B, C\dots$) | — |
| **Compuerta NOT (Inversor)** | Complemento o apóstrofo ($A'$) | **1.º (Se resuelve primero)** |
| **Compuerta AND** | Producto lógico ($A \cdot B$ o simplemente $AB$) | **2.º (Se resuelve después del NOT)** |
| **Compuerta OR** | Suma lógica ($A + B$) | **3.º (Se resuelve al final)** |
| **Cable de salida** | Resultado de evaluar la expresión ($Y$ o $F$) | — |

> **💡 Regla de oro:** Si necesito que una suma OR se ejecute antes que una multiplicación AND (o que un inversor afecte a toda una operación completa), debo encerrarla obligatoriamente entre **paréntesis**, tal como en el álgebra tradicional: $(A + B) \cdot C$ o $(A \cdot B)'$.

---

## 📋 2. Del Enunciado a la Especificación: El Peligro de la Ambigüedad

Cuando un cliente nos pide diseñar un sistema digital, suele explicarlo en lenguaje cotidiano. Nuestra primera tarea como ingenieros es **eliminar cualquier ambigüedad** antes de tocar un solo cable.

Analicemos la frase vista en clase:  
> *«La sirena suena cuando se activan dos sensores.»* (Suponiendo tres sensores: $A, B, C$).

Existen dos maneras de interpretar esa misma oración:

| Criterio | Lectura 1: «Exactamente dos sensores» | Lectura 2: «Al menos dos sensores» *(La correcta)* |
| :--- | :--- | :--- |
| **¿Qué combinaciones activan la sirena?** | Solo cuando hay dos `1` y un `0`: filas `011` (3), `101` (5) y `110` (6). | Cuando hay dos o tres `1`: filas `011` (3), `101` (5), `110` (6) y `111` (7). |
| **Notación compacta** | $F = \Sigma m(3, 5, 6)$ | $F = \Sigma m(3, 5, 6, 7)$ |
| **¿Qué pasa si un ladrón activa los 3 sensores a la vez (`111`)?** | **¡La sirena se calla!** (Falla grave de seguridad). | **La sirena suena** como espera el cliente en una emergencia real. |

**Lección aprendida:** El lenguaje humano suele omitir la frase *«al menos»*. La tabla de verdad nos obliga a revisar las $2^n$ situaciones posibles sin dejar casos sueltos.

---

## 📐 3. Álgebra de Boole: Postulados, Dualidad y Teoremas

En 1854 George Boole propuso un álgebra para la lógica, y en 1904 Edward Huntington formalizó sus reglas básicas sobre el conjunto $\{0, 1\}$ con las operaciones $+$ (OR), $\cdot$ (AND) y $'$ (NOT).

### 3.1. Los Postulados de Huntington y el Principio de Dualidad

| Postulado | Forma OR (Suma) | Forma AND (Producto / Dual) | Explicación intuitiva con interruptores |
| :--- | :---: | :---: | :--- |
| **1. Identidad** | $X + 0 = X$ | $X \cdot 1 = X$ | Sumar `0` o multiplicar por `1` deja la señal intacta. |
| **2. Complemento** | $X + X' = 1$ | $X \cdot X' = 0$ | Una señal o su opuesta siempre da `1`; ambas a la vez es imposible (`0`). |
| **3. Conmutativa** | $X + Y = Y + X$ | $X \cdot Y = Y \cdot X$ | El orden en que conecto los cables a una compuerta no altera la salida. |
| **4. Distributiva** | $X(Y + Z) = XY + XZ$ | **$X + YZ = (X + Y)(X + Z)$** | La segunda forma no existe en el álgebra común, pero en Boole es totalmente válida. |

* **¿Qué es el Principio de Dualidad?**  
  Si tenemos cualquier identidad verdadera en el Álgebra de Boole e intercambiamos todos los operadores **`+` por `·`** (y viceversa) y todos los números **`0` por `1`** (y viceversa), **sin negar ni tocar las variables**, obtenemos otra ley booleana igualmente verdadera. Gracias a la dualidad, por cada fórmula que aprendo tengo automáticamente dos.

### 3.2. Tabla de Consulta de Teoremas para Simplificar
En el curso no memorizamos esta tabla de forma mecánica, sino que la consultamos estratégicamente para reducir circuitos:

| Teorema | Forma OR | Forma AND (Dual) | ¿Para qué me sirve al simplificar? |
| :--- | :---: | :---: | :--- |
| **Elemento nulo** | $X + 1 = 1$ | $X \cdot 0 = 0$ | Si una entrada de una OR vale `1`, toda la expresión se vuelve `1` (anula el resto). |
| **Idempotencia** | $X + X = X$ | $X \cdot X = X$ | Me permite eliminar términos repetidos o **duplicar un término** para factorizarlo con varios vecinos. |
| **Involución** | $(X')' = X$ | $(X')' = X$ | Dos inversores seguidos en el mismo cable se cancelan entre sí. |
| **Absorción** | $X + XY = X$ | $X(X + Y) = X$ | Si una variable aparece sola y también multiplicada por otra, el término largo desaparece por completo. |
| **Absorción del complemento** | $X + X'Y = X + Y$ | $X(X' + Y) = XY$ | Si una variable se suma con su propia negación acompañada de otra ($X'Y$), la negada $X'$ se elimina. |
| **Adyacencia** *(Combinación)* | $XY + XY' = X$ | $(X+Y)(X+Y') = X$ | **El teorema más usado:** si dos términos son idénticos salvo por una variable que cambia de normal a negada, esa variable desaparece. |
| **Consenso** | $XY + X'Z + YZ = XY + X'Z$ | $(X+Y)(X'+Z)(Y+Z) = (X+Y)(X'+Z)$ | Si aparece $X$ con $Y$, $X'$ con $Z$, y además la combinación $YZ$, el término $YZ$ es redundante y se elimina. |

### 3.3. Demostración paso a paso de «La Distributiva que Sorprende»
En el álgebra del colegio, $(A + B)(A + C)$ nunca es igual a $A + BC$. Sin embargo, en el Álgebra de Boole sí se cumple. Aquí desarrollo su demostración justificando cada paso:

1. **Punto de partida:** $(A + B)(A + C)$
2. **Propiedad distributiva normal:** $= A \cdot A + A \cdot C + A \cdot B + B \cdot C$
3. **Teorema de idempotencia ($A \cdot A = A$):** $= A + AC + AB + BC$
4. **Factor común $A$ en los tres primeros términos:** $= A(1 + C + B) + BC$
5. **Teorema del elemento nulo ($1 + C + B = 1$):** $= A(1) + BC$
6. **Postulado de identidad ($A \cdot 1 = A$):** $= \mathbf{A + BC}$ ✓

---

## 🔀 4. Leyes de De Morgan y Resolución de la Práctica en Parejas

Augustus De Morgan demostró cómo se comporta una negación cuando cubre a todo un producto o a toda una suma:

* **Primera Ley (NAND equivale a OR con entradas negadas):** $(A \cdot B)' = A' + B'$
* **Segunda Ley (NOR equivale a AND con entradas negadas):** $(A + B)' = A' \cdot B'$

> **🧠 Regla mnemotécnica infalible:** **«Rompe la barra (el paréntesis negado) y cambia el operador»**. Donde se parte la negación general, el producto `·` se transforma en suma `+` (o la suma `+` en producto `·`), y cada pedazo conserva su propio signo de negación. Esto se generaliza para cualquier número de variables: $(A + B + C)' = A' \cdot B' \cdot C'$.

### Resolución de la Actividad en Parejas (5 minutos): Apliquen De Morgan

#### Ejercicio 1: $(A \cdot B \cdot C)'$
* **Paso único:** Rompo el paréntesis negado general y cambio los productos `·` por sumas `+`:
  $$(A \cdot B \cdot C)' = \mathbf{A' + B' + C'}$$
* **Verificación con una fila de la tabla ($A=1, B=1, C=0$):**
  * Original: $(1 \cdot 1 \cdot 0)' = (0)' = 1$.
  * Resultado: $1' + 1' + 0' = 0 + 0 + 1 = 1$ ✓.

#### Ejercicio 2: $(A + B'C)'$
* **Paso 1:** Trato a $(B'C)$ como un solo bloque. Rompo el paréntesis exterior cambiando el `+` principal por un `·`:
  $$(A + B'C)' = A' \cdot (B'C)'$$
* **Paso 2:** Aplico De Morgan nuevamente al paréntesis $(B'C)'$, cambiando su `·` interno por `+`:
  $$= A' \cdot ((B')' + C')$$
* **Paso 3:** Aplico involución ($(B')' = B$):
  $$= \mathbf{A'(B + C')} \quad \text{(o distribuido: } \mathbf{A'B + A'C'}\text{)}$$
* **Verificación con la fila $A=0, B=0, C=1$:**
  * Original: $(0 + 0'\cdot 1)' = (0 + 1\cdot 1)' = (1)' = 0$.
  * Resultado: $0' \cdot (0 + 1') = 1 \cdot (0 + 0) = 0$ ✓.

#### Ejercicio 3: $(A'B + C)'$
* **Paso 1:** Rompo la negación exterior cambiando la suma `+` por producto `·`:
  $$(A'B + C)' = (A'B)' \cdot C'$$
* **Paso 2:** Aplico De Morgan sobre $(A'B)'$ y luego involución sobre $(A')'$:
  $$= ((A')' + B') \cdot C' = \mathbf{(A + B')C'} \quad \text{(o distribuido: } \mathbf{AC' + B'C'}\text{)}$$
* **Verificación con la fila $A=0, B=1, C=0$:**
  * Original: $(0'\cdot 1 + 0)' = (1 + 0)' = (1)' = 0$.
  * Resultado: $(0 + 1') \cdot 0' = (0 + 0) \cdot 1 = 0$ ✓.

---

## 🧩 5. Minitérminos ($m_i$), Maxitérminos ($M_i$) y Formas Canónicas (SOP y POS)

Para convertir cualquier tabla de verdad en una ecuación matemática sin adivinar, disponemos de dos tipos de «piezas estándar» para cada fila $i$:

1. **Minitérmino ($m_i$):** Es un **producto (AND)** de todas las variables que vale `1` únicamente en esa fila.
   * **Regla de escritura:** Si la variable vale **`1`** se escribe **normal ($A$)**; si vale **`0`** se escribe **negada ($A'$)**.
2. **Maxitérmino ($M_i$):** Es una **suma (OR)** de todas las variables que vale `0` únicamente en esa fila (es el complemento exacto del minitérmino: $M_i = (m_i)'$).
   * **Regla de escritura (invertida):** Si la variable vale **`0`** se escribe **normal ($A$)**; si vale **`1`** se escribe **negada ($A' $)**.

### 5.1. Tabla de las 8 Piezas Posibles para 3 Variables ($A, B, C$)

| Índice $i$ | Combinación $ABC$ | Minitérmino $m_i$ (Producto que vale 1) | Maxitérmino $M_i$ (Suma que vale 0) | Ejemplo con la Alarma ($F$: al menos 2 sensores) |
| :---: | :---: | :---: | :---: | :---: |
| **0** | `0 0 0` | $m_0 = A'B'C'$ | $M_0 = A + B + C$ | $F = 0$ *(Se usa en POS)* |
| **1** | `0 0 1` | $m_1 = A'B'C$ | $M_1 = A + B + C'$ | $F = 0$ *(Se usa en POS)* |
| **2** | `0 1 0` | $m_2 = A'BC'$ | $M_2 = A + B' + C$ | $F = 0$ *(Se usa en POS)* |
| **3** | `0 1 1` | $m_3 = A'BC$ | $M_3 = A + B' + C'$ | **$F = 1$ *(Se usa en SOP)*** |
| **4** | `1 0 0` | $m_4 = AB'C'$ | $M_4 = A' + B + C$ | $F = 0$ *(Se usa en POS)* |
| **5** | `1 0 1` | $m_5 = AB'C$ | $M_5 = A' + B + C'$ | **$F = 1$ *(Se usa en SOP)*** |
| **6** | `1 1 0` | $m_6 = ABC'$ | $M_6 = A' + B' + C$ | **$F = 1$ *(Se usa en SOP)*** |
| **7** | `1 1 1` | $m_7 = ABC$ | $M_7 = A' + B' + C'$ | **$F = 1$ *(Se usa en SOP)*** |

### 5.2. Las Dos Formas Canónicas de una Misma Función
* **SOP Canónica (Suma de Productos / Suma de Minitérminos):** Miro únicamente las filas donde **$F = 1$**, tomo sus minitérminos y los uno con **`+` (OR)**:
  $$F = \Sigma m(3, 5, 6, 7) = A'BC + AB'C + ABC' + ABC$$
* **POS Canónica (Producto de Sumas / Producto de Maxitérminos):** Miro únicamente las filas donde **$F = 0$**, tomo sus maxitérminos y los uno con **`·` (AND)**:
  $$F = \Pi M(0, 1, 2, 4) = (A+B+C)(A+B+C')(A+B'+C)(A'+B+C)$$

> **🔑 Propiedad clave:** Los índices de $\Sigma m$ y los índices de $\Pi M$ son mutuamente complementarios: juntos completan todas las filas de la tabla (del $0$ al $7$). Si conozco $\Sigma m(3, 5, 6, 7)$, sé de inmediato y sin mirar la tabla que $\Pi M$ contiene los números faltantes: $\Pi M(0, 1, 2, 4)$.

### 5.3. ¿Cuándo me conviene elegir SOP y cuándo POS?
1. **Cantidad de unos frente a ceros:** Si la tabla tiene **pocos `1`**, elijo **SOP** (tendrá menos términos). Si tiene **pocos `0`**, elijo **POS**.
2. **Disponibilidad de chips en el inventario:** Si solo dispongo de compuertas **NAND (`7400`)**, parto de **SOP** porque se convierte directamente a una red `NAND-NAND`. Si solo tengo compuertas **NOR (`7402`)**, parto de **POS** para pasar a `NOR-NOR`.

---

## 🚨 6. Caso Guiado: Alarma de 3 Sensores y Ahorro de Hardware

En una bodega tenemos tres sensores: **puerta ($A$)**, **ventana ($B$)** y **movimiento ($C$)**. La sirena $F$ debe sonar si **al menos dos sensores** se activan.

### 6.1. Simplificación Algebraica de la Alarma
Partimos de la SOP canónica obtenida en la sección anterior:
$$F = A'BC + AB'C + ABC' + ABC$$

Aquí aplicamos un truco fundamental: gracias al **teorema de idempotencia ($X = X + X + X$)**, podemos **triplicar el término $ABC$** para que ayude a simplificar a los otros tres términos al mismo tiempo:
1. Primer par: $A'BC + ABC = BC(A' + A) = BC(1) = \mathbf{BC}$
2. Segundo par: $AB'C + ABC = AC(B' + B) = AC(1) = \mathbf{AC}$
3. Tercer par: $ABC' + ABC = AB(C' + C) = AB(1) = \mathbf{AB}$

Uniendo los tres resultados obtenemos la **SOP simplificada**:
$$\mathbf{F = AB + AC + BC}$$
*(Y por dualidad o agrupando ceros, su versión POS simplificada es $F = (A+B)(A+C)(B+C)$).*

### 6.2. Comparación Real de Costo en Circuitos Integrados (CI)

| Característica | Versión Canónica ($A'BC + AB'C + ABC' + ABC$) | Versión Simplificada ($AB + AC + BC$) |
| :--- | :--- | :--- |
| **Inversores necesarios** | 3 inversores $\rightarrow$ **1 chip `7404`** (trae 6 NOT) | **0 inversores** (ninguna variable va negada) |
| **Compuertas AND** | 4 AND de 3 entradas $\rightarrow$ **2 chips `7411`** (cada `7411` solo trae 3 AND de 3 entradas) | 3 AND de 2 entradas $\rightarrow$ **1 chip `7408`** (trae 4 AND de 2 entradas) |
| **Compuertas OR** | 1 OR de 4 entradas (o 3 OR de 2 entradas) $\rightarrow$ **1 chip `7432`** | 1 OR de 3 entradas (o 2 OR de 2 entradas) $\rightarrow$ **1 chip `7432`** |
| **Costo Total Físico** | **4 Circuitos Integrados (CI)** | **2 Circuitos Integrados (CI)** *(¡50 % de ahorro!)* |

---

## 🎫 7. Resolución del Ticket de Salida y Ejemplo de Simplificación

### 7.1. Ticket de Salida (Sesión 1)
1. **Aplique De Morgan a $(A' + BC)'$:**
   * Rompo la barra principal cambiando `+` por `·`: $(A')' \cdot (BC)'$.
   * Aplico involución en $(A')' = A$ y De Morgan en $(BC)' = B' + C'$:
     $$(A' + BC)' = \mathbf{A(B' + C')} = \mathbf{AB' + AC'}$$
2. **Escriba el minitérmino $m_4$ y el maxitérmino $M_4$ (para 3 variables $A, B, C$):**
   * El índice $4$ en binario de 3 bits es **`100`** ($A=1, B=0, C=0$).
   * **Minitérmino $m_4$** (1=normal, 0=negado): **$m_4 = AB'C'$**.
   * **Maxitérmino $M_4$** (0=normal, 1=negado): **$M_4 = A' + B + C$**.

### 7.2. Procedimiento General de Simplificación Algebraica y Ejemplo de Clase
Para simplificar con álgebra sigo siempre este orden de 5 herramientas:
1. Buscar **pares adyacentes** ($XY + XY' = X$).
2. **Duplicar términos** si hace falta ($X = X + X$).
3. Aplicar **absorción** ($X + XY = X$).
4. Aplicar **absorción del complemento** ($X + X'Y = X + Y$).
5. Verificar el **teorema de consenso** ($XY + X'Z + YZ = XY + X'Z$).

**Ejemplo resuelto de la Sesión 2:** Simplificar $F = A + A'B + AB'C$
* **Paso 1 (Absorción del complemento en $A + A'B = A + B$):** $F = (A + B) + AB'C$
* **Paso 2 (Propiedad conmutativa para juntar los términos con $A$):** $F = (A + AB'C) + B$
* **Paso 3 (Absorción directa en $A + A(B'C) = A$):** $F = \mathbf{A + B}$
* **Comprobación con la fila $A=0, B=0, C=1$:**
  * Expresión original: $0 + (1 \cdot 0) + (0 \cdot 1 \cdot 1) = 0 + 0 + 0 = 0$.
  * Expresión simplificada: $0 + 0 = 0$ ✓. ¡Pasamos de 3 términos (5 literales) a una sola compuerta OR de 2 entradas!

---

## 🛠️ 8. Resolución Completa del Taller Dirigido por Grupos (4 Casos Reales)

Como parte del contenido de guía de este cuaderno, resuelvo a continuación los **cuatro proyectos del taller grupal** de la Semana 2, entregando para cada uno su tabla de verdad, $\Sigma m$, $\Pi M$, SOP canónica, versión simplificada y recuento de compuertas y circuitos integrados (CI).

---

### Caso 1 (Grupos G1 y G5): Luz de Pasillo con Interruptor Maestro
> **Enunciado:** *«Dos interruptores $A$ y $B$ conmutan la luz; el interruptor maestro $M$ ($=1$) la apaga siempre.»*

1. **Definición de entradas y comportamiento:**  
   Ordeno las 3 variables como $(A, B, M)$. Cuando el maestro está apagado ($M=0$), los interruptores $A$ y $B$ funcionan como una escalera conmutada (compuerta XOR: la luz enciende si $A$ y $B$ son distintos). Cuando el maestro se activa ($M=1$), la luz $F$ vale `0` sin importar $A$ ni $B$.

| Fila $i$ | $A$ | $B$ | $M$ | Salida $F$ | Minitérmino / Maxitérmino Relevante |
| :---: | :---: | :---: | :---: | :---: | :--- |
| **0** | 0 | 0 | 0 | `0` | $M_0 = A + B + M$ |
| **1** | 0 | 0 | 1 | `0` | $M_1 = A + B + M'$ |
| **2** | 0 | 1 | 0 | **`1`** | **$m_2 = A'BM'$** |
| **3** | 0 | 1 | 1 | `0` | $M_3 = A + B' + M'$ |
| **4** | 1 | 0 | 0 | **`1`** | **$m_4 = AB'M'$** |
| **5** | 1 | 0 | 1 | `0` | $M_5 = A' + B + M'$ |
| **6** | 1 | 1 | 0 | `0` | $M_6 = A' + B' + M$ |
| **7** | 1 | 1 | 1 | `0` | $M_7 = A' + B' + M'$ |

2. **Notación compacta y SOP canónica:**
   * $\Sigma m(2, 4)$ y $\Pi M(0, 1, 3, 5, 6, 7)$.
   * **SOP canónica:** $F = A'BM' + AB'M'$ (6 literales).
3. **Simplificación algebraica:**
   * En forma de suma de productos de dos niveles, $A'BM' + AB'M'$ no tiene pares adyacentes directos, pero al sacar factor común $M'$ obtenemos:
     $$F = M'(A'B + AB') = \mathbf{M' \cdot (A \oplus B)}$$
4. **Recuento de compuertas y CI:**
   * **En SOP de dos niveles ($A'BM' + AB'M'$):** 3 inversores (`7404`), 2 AND de 3 entradas (`7411`) y 1 OR de 2 entradas (`7432`) = **6 compuertas y 3 CI**.
   * **Factorizada con XOR ($M' \cdot (A \oplus B)$):** 1 inversor (`7404`), 1 XOR (`7486`) y 1 AND (`7408`) = **3 compuertas** (o solo **2 CI** si usamos una segunda XOR con entrada fija en `1` para invertir $M$).

---

### Caso 2 (Grupos G2 y G6): Detector de Números Primos de 3 Bits
> **Enunciado:** *«Entrada de 3 bits $ABC$ ($0$–$7$). $F = 1$ si el número es primo.»*

1. **Definición matemática:** Entre el $0$ y el $7$, los números primos son únicamente el **$2, 3, 5$ y $7$** (recordando que ni el $0$ ni el $1$ son primos).

| Fila $i$ | $A$ | $B$ | $C$ | ¿Es primo? ($F$) | Minitérmino / Maxitérmino |
| :---: | :---: | :---: | :---: | :---: | :--- |
| **0** | 0 | 0 | 0 | `0` | $M_0 = A + B + C$ |
| **1** | 0 | 0 | 1 | `0` | $M_1 = A + B + C'$ |
| **2** | 0 | 1 | 0 | **`1`** | **$m_2 = A'BC'$** |
| **3** | 0 | 1 | 1 | **`1`** | **$m_3 = A'BC$** |
| **4** | 1 | 0 | 0 | `0` | $M_4 = A' + B + C$ |
| **5** | 1 | 0 | 1 | **`1`** | **$m_5 = AB'C$** |
| **6** | 1 | 1 | 0 | `0` | $M_6 = A' + B' + C$ |
| **7** | 1 | 1 | 1 | **`1`** | **$m_7 = ABC$** |

2. **Notación compacta y SOP canónica:**
   * $\Sigma m(2, 3, 5, 7)$ y $\Pi M(0, 1, 4, 6)$.
   * **SOP canónica:** $F = A'BC' + A'BC + AB'C + ABC$ (12 literales).
3. **Simplificación algebraica paso a paso:**
   * Agrupo los dos primeros términos ($m_2 + m_3$) por adyacencia: $A'B(C' + C) = A'B(1) = \mathbf{A'B}$.
   * Agrupo los dos últimos términos ($m_5 + m_7$) por adyacencia: $AC(B' + B) = AC(1) = \mathbf{AC}$.
   * **SOP simplificada:** $\mathbf{F = A'B + AC}$ (4 literales).
4. **Recuento de compuertas y CI:**
   * **Versión canónica:** 3 NOT (`7404`), 4 AND de 3 entradas (`2 x 7411`) y 1 OR (`7432`) = **4 CI**.
   * **Versión simplificada ($A'B + AC$):** 1 NOT (`7404`), 2 AND de 2 entradas (`7408`) y 1 OR de 2 entradas (`7432`) = **4 compuertas y 3 CI** (o **1 solo CI `7400`** si lo pasamos a NAND-NAND en la Semana 3).

---

### Caso 3 (Grupos G3 y G7): Control Automático y Manual de Bomba de Agua
> **Enunciado:** *«Enciende si hay agua en la cisterna ($B$) y el tanque está bajo ($A$) o se pide en manual ($C$).»*

1. **Análisis de especificación:**  
   Siguiendo la precedencia directa del enunciado ($B \text{ AND } A \text{ OR } C$, donde el modo manual $C$ fuerza el encendido directo, o bien protegiendo la bomba para que solo encienda si hay agua en la cisterna $B$ tanto en automático como en manual), analizo la forma booleana directa **$F = AB + C$** (y dejo indicada la variante con protección de cisterna $F = B(A+C) = AB + BC$):

| Fila $i$ | $A$ (Tanque bajo) | $B$ (Cisterna con agua) | $C$ (Manual) | Salida $F = AB + C$ | Minitérminos ($F=1$) |
| :---: | :---: | :---: | :---: | :---: | :--- |
| **0** | 0 | 0 | 0 | `0` | — |
| **1** | 0 | 0 | 1 | **`1`** | $m_1 = A'B'C$ |
| **2** | 0 | 1 | 0 | `0` | — |
| **3** | 0 | 1 | 1 | **`1`** | $m_3 = A'BC$ |
| **4** | 1 | 0 | 0 | `0` | — |
| **5** | 1 | 0 | 1 | **`1`** | $m_5 = AB'C$ |
| **6** | 1 | 1 | 0 | **`1`** | $m_6 = ABC'$ |
| **7** | 1 | 1 | 1 | **`1`** | $m_7 = ABC$ |

2. **Notación compacta y SOP canónica:**
   * $\Sigma m(1, 3, 5, 6, 7)$ y $\Pi M(0, 2, 4)$.
   * **SOP canónica:** $F = A'B'C + A'BC + AB'C + ABC' + ABC$.
3. **Simplificación algebraica:**
   * Sumando los cuatro términos que contienen $C$ ($m_1 + m_3 + m_5 + m_7$):
     $$C(A'B' + A'B + AB' + AB) = C(A'(B'+B) + A(B'+B)) = C(A' + A) = \mathbf{C}$$
   * Queda entonces $F = C + ABC'$. Aplicando absorción del complemento ($C + C'(AB) = C + AB$):
     $$\mathbf{F = AB + C}$$
   * *(Nota: Si el cliente exige que ni siquiera en modo manual funcione sin agua en la cisterna, entonces $F = \Sigma m(3, 6, 7) = A'BC + ABC' + ABC = \mathbf{AB + BC} = \mathbf{B(A + C)}$).*
4. **Recuento de compuertas y CI:**
   * **Versión simplificada ($AB + C$):** 1 compuerta AND de 2 entradas (`7408`) y 1 compuerta OR de 2 entradas (`7432`) = **2 compuertas y 2 CI** (frente a los 4 CI de la versión canónica).

---

### Caso 4 (Grupos G4 y G8): Aviso de Cinturón y Puerta en un Vehículo
> **Enunciado:** *«Suena si el motor está encendido ($E$) y el cinturón no está puesto ($C=0$, es decir $C'$) o la puerta está abierta ($P$).»*

1. **Análisis de variables ($E, C, P$):**  
   Definimos $E = 1$ (motor encendido), $C = 1$ (cinturón abrochado $\rightarrow C' = 1$ cuando **no** está puesto) y $P = 1$ (puerta abierta). La alarma de seguridad suena solo con el motor en marcha ($E=1$) cuando ocurre cualquiera de las dos faltas de seguridad ($C'=1$ o $P=1$), es decir, $F = E(C' + P) = EC' + EP$:

| Fila $i$ | $E$ (Motor) | $C$ (Cinturón puesto) | $P$ (Puerta abierta) | Salida $F$ | Minitérmino / Maxitérmino |
| :---: | :---: | :---: | :---: | :---: | :--- |
| **0** | 0 | 0 | 0 | `0` | $M_0 = E + C + P$ |
| **1** | 0 | 0 | 1 | `0` | $M_1 = E + C + P'$ |
| **2** | 0 | 1 | 0 | `0` | $M_2 = E + C' + P$ |
| **3** | 0 | 1 | 1 | `0` | $M_3 = E + C' + P'$ |
| **4** | 1 | 0 | 0 | **`1`** | **$m_4 = EC'P'$** *(Motor encendido y sin cinturón)* |
| **5** | 1 | 0 | 1 | **`1`** | **$m_5 = EC'P$** *(Motor encendido, sin cinturón y puerta abierta)* |
| **6** | 1 | 1 | 0 | `0` | $M_6 = E' + C' + P$ *(Todo en orden: no suena)* |
| **7** | 1 | 1 | 1 | **`1`** | **$m_7 = ECP$** *(Motor encendido y puerta abierta)* |

2. **Notación compacta y SOP canónica:**
   * $\Sigma m(4, 5, 7)$ y $\Pi M(0, 1, 2, 3, 6)$.
   * **SOP canónica:** $F = EC'P' + EC'P + ECP$ (9 literales).
3. **Simplificación algebraica:**
   * Duplico el término $EC'P$ por idempotencia:
     $$F = (EC'P' + EC'P) + (EC'P + ECP) = EC'(P' + P) + EP(C' + C) = \mathbf{EC' + EP} = \mathbf{E(C' + P)}$$
4. **Recuento de compuertas y CI:**
   * **En SOP simplificada ($EC' + EP$):** 1 NOT (`7404`), 2 AND (`7408`) y 1 OR (`7432`) = **4 compuertas y 3 CI**.
   * **Factorizada ($E \cdot (C' + P)$):** 1 NOT (`7404`), 1 OR (`7432`) y 1 AND (`7408`) = **3 compuertas**.

---

## 📌 9. Síntesis Personal de la Semana 2

1. **Simplificar en papel ahorra dinero en hardware:** Reducir una ecuación mediante los teoremas de Boole elimina compuertas, cables y circuitos integrados completos en el *protoboard*.
2. **La tabla de verdad es la autoridad absoluta:** Dos circuitos o ecuaciones distintas son lógicamente equivalentes **si y solo si** producen exactamente la misma tabla de verdad.
3. **La forma canónica siempre existe y es única:** A partir de cualquier tabla podemos escribir directamente $\Sigma m$ (leyendo los `1`) y $\Pi M$ (leyendo los `0`).
4. **¿Por qué necesitamos los Mapas de Karnaugh en la Semana 3?** Porque al simplificar con álgebra dependemos de nuestra habilidad visual para elegir qué teorema aplicar primero, sin una garantía automática de haber llegado al mínimo absoluto.

---

| [⬅️ Semana 1](semana-1.md) | [Semana 3 ➡️](semana-3.md) |
| :--- | ---: |
