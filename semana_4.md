&lt;div align=&quot;center&quot;&gt;

&lt;kbd&gt;[🏠 Portada](https://www.google.com/search?q=./README.md)&lt;/kbd&gt; &nbsp; &lt;kbd&gt;[⬅️ Anterior: Semana 3](https://www.google.com/search?q=./semana-3.md)&lt;/kbd&gt; &nbsp; &lt;kbd&gt;[📘 Semana 1](https://www.google.com/search?q=./semana-1.md)&lt;/kbd&gt; &nbsp; &lt;kbd&gt;[📗 Semana 2](https://www.google.com/search?q=./semana-2.md)&lt;/kbd&gt; &nbsp; &lt;kbd&gt;[📙 Semana 3](https://www.google.com/search?q=./semana-3.md)&lt;/kbd&gt; &nbsp; &lt;kbd&gt;📍 **Semana 4**&lt;/kbd&gt; &nbsp; &lt;kbd&gt;[🏠 Volver a Portada](https://www.google.com/search?q=./README.md)&lt;/kbd&gt;

&lt;/div&gt;

---

# 📕 Semana 4: Circuitos Aritméticos Combinacionales (El Corazón de la ALU)

**Asignatura:** Diseño de Circuitos · **Unidad 1:** Lógica combinacional con diseño clásico  
**Autor:** Luis Anderson Carranza Quispe · **Docente:** Ing. Luis Darío Shinche Cueva, Mg.  
**Institución:** Universidad Nacional de Loja · Carrera de Computación · Segundo Ciclo

---

## 🎯 Mi Objetivo de Aprendizaje en esta Semana

En las primeras tres semanas aprendí a representar números en binario, escribir tablas de verdad, simplificar ecuaciones mediante Mapas de Karnaugh e implementarlas con compuertas lógicas. En esta cuarta semana reúno todas esas herramientas para responder una pregunta apasionante: **¿cómo logra un circuito eléctrico sumar, restar y comparar números binarios?**

Al finalizar el estudio de esta guía soy capaz de:
1. Diseñar desde cero un **Semisumador (*Half Adder*)** y un **Sumador Completo (*Full Adder*)** aplicando el procedimiento clásico (tabla de verdad $\rightarrow$ Mapa de Karnaugh $\rightarrow$ circuito).
2. Conectar varios sumadores en cascada para construir un **Sumador de 4 bits (*Ripple Carry Adder*)** y calcular cuántos nanosegundos tarda en entregar la respuesta final.
3. Transformar el sumador en un **Sumador/Restador unificado** utilizando compuertas **XOR como inversores controlados** junto con el complemento a dos.
4. Detectar por hardware cuándo ocurre un error de **desbordamiento (*overflow*)** con números con signo ($V = C_3 \oplus C_4$), diseñar un **comparador de magnitud** y comprender cómo todos estos bloques forman la **ALU (Unidad Aritmético-Lógica)** de un procesador.

---

## ➕ 1. De la Suma en Papel al Diseño por Columnas

### 1.1. Sumar un solo bit: Los 4 casos posibles
Cuando sumamos dos números en el colegio, como $7 + 5 = 12$, decimos mentalmente: **«escribo el `2` en mi columna y me llevo `1` a la columna siguiente»**.

En binario ocurre exactamente lo mismo, pero es mucho más sencillo porque al sumar dos bits ($A$ y $B$) solo existen **4 combinaciones posibles**, las cuales generan dos salidas:
* **$S$ (*Sum* / Suma):** Es el bit que **escribo** debajo de la columna actual.
* **$C$ (*Carry* / Acarreo):** Es el bit que **me llevo** hacia la columna de la izquierda.

| Operación Binaria | Resultado en Decimal y Binario | Escribo en la columna ($S$) | Me llevo a la siguiente ($C$) |
| :---: | :---: | :---: | :---: |
| `0 + 0` | $0 = 00_2$ | **`0`** | **`0`** |
| `0 + 1` | $1 = 01_2$ | **`1`** | **`0`** |
| `1 + 0` | $1 = 01_2$ | **`1`** | **`0`** |
| `1 + 1` | $2 = 10_2$ | **`0`** | **`1`** |

### 1.2. Ejemplo de suma de 4 bits columna por columna: `0110` ($6$) + `0111` ($7$)
Observemos con lupa qué sucede en cada una de las 4 columnas (numeradas de derecha a izquierda como Columna 0, Columna 1, Columna 2 y Columna 3):

```text
  Acarreo que llega (Cin):   1   1   0   0   ← En la Columna 0 nunca llega acarreo previo
                Número A:    0   1   1   0   ( 6 en decimal)
              + Número B:    0   1   1   1   ( 7 en decimal)
              ────────────────────────────
               Resultado:    1   1   0   1   (13 en decimal: 8 + 4 + 0 + 1 = 13 ✓)
              
              Posición:    Col3 Col2 Col1 Col0
```

| Columna | Operación realizada en esa columna | Escribo ($S$) | Llevo ($C_{out}$) | Observación clave para el hardware |
| :---: | :---: | :---: | :---: | :--- |
| **Columna 0** | `0 + 1 = 1` | `1` | `0` | Solo suma **2 bits** ($A_0$ y $B_0$); a su derecha no hay ninguna columna previa. |
| **Columna 1** | `1 + 1 + 0 = 2` ($10_2$) | `0` | `1` | Suma **3 bits** ($A_1$, $B_1$ y el acarreo `0` que llegó de la Columna 0). |
| **Columna 2** | `1 + 1 + 1 = 3` ($11_2$) | `1` | `1` | Suma **3 bits** ($A_2$, $B_2$ y el acarreo `1` que llegó de la Columna 1). |
| **Columna 3** | `0 + 0 + 1 = 1` ($01_2$) | `1` | `0` | Suma **3 bits** ($A_3$, $B_3$ y el acarreo `1` que llegó de la Columna 2). |

> **💡 El efecto dominó del acarreo:** Al sumar `0111` ($7$) + `0001` ($1$), el acarreo que nace en la Columna 0 provoca un nuevo acarreo en la Columna 1, este a su vez en la Columna 2 y llega hasta la Columna 3 para formar `1000` ($8$). Como los acarreos se encadenan, distinguimos **dos tipos de columnas**, que dan origen a **dos circuitos diferentes**: el **Semisumador** (para la Columna 0) y el **Sumador Completo** (para las Columnas 1, 2, 3…).

---

## 🔧 2. El Semisumador (*Half Adder*): Para la Columna 0

El **Semisumador** es el circuito encargado de sumar únicamente **2 bits de entrada ($A$ y $B$)** sin recibir ningún acarreo previo. Se llama *«semi»* precisamente porque no tiene entrada de acarreo ($C_{in}$) y solo sirve para la primera columna de la derecha (Columna 0).

### 2.1. Tabla de Verdad y Ecuaciones del Semisumador

| Entrada $A$ | Entrada $B$ | Acarreo $C$ (Me llevo) | Suma $S$ (Escribo) | Análisis visual de las columnas de salida |
| :---: | :---: | :---: | :---: | :--- |
| `0` | `0` | `0` | `0` | Ambos son `0`. |
| `0` | `1` | `0` | **`1`** | $A$ y $B$ son distintos $\rightarrow S = 1$. |
| `1` | `0` | `0` | **`1`** | $A$ y $B$ son distintos $\rightarrow S = 1$. |
| `1` | `1` | **`1`** | `0` | Ambos son `1` $\rightarrow C = 1$ y $S = 0$. |

Al comparar las dos columnas de salida con las 7 compuertas que estudiamos en la Semana 1, las reconocemos de inmediato sin siquiera hacer un mapa de Karnaugh:
* **La columna de Suma ($S$)** vale `1` cuando $A$ y $B$ son distintos $\rightarrow$ es una compuerta **XOR**:
  $$\mathbf{S = A \oplus B}$$
* **La columna de Acarreo ($C$)** vale `1` solo cuando $A$ y $B$ son ambos `1` $\rightarrow$ es una compuerta **AND**:
  $$\mathbf{C = A \cdot B}$$

```text
   A ───┬──────► [ XOR ] ───► S = A ⊕ B  (Suma)
        │      ┌►[     ]
   B ───┼──────┘
        └──────► [ AND ] ───► C = A · B  (Acarreo)
               └►[     ]
```

---

## ⚙️ 3. El Sumador Completo (*Full Adder*): Para las Columnas 1, 2, 3…

A partir de la Columna 1, el circuito necesita sumar **3 bits al mismo tiempo**: el bit $A$, el bit $B$ y el acarreo que llega de la columna anterior ($C_{in}$). A este bloque lo llamamos **Sumador Completo**.

### 3.1. El truco de «Contar Unos» para armar la Tabla de Verdad sin memorizar
Para llenar las 8 filas de la tabla de verdad del sumador completo sin equivocarme jamás, aplico este método práctico: **cuento cuántos unos hay en las tres entradas ($A, B, C_{in}$) de esa fila y escribo esa cantidad en binario de 2 bits ($C_{out}\,S$)**:
* Si hay **cero unos** $\rightarrow$ escribo **`00`** ($C_{out}=0, S=0$).
* Si hay **un solo uno** $\rightarrow$ escribo **`01`** ($C_{out}=0, S=1$).
* Si hay **dos unos** $\rightarrow$ escribo **`10`** ($C_{out}=1, S=0$).
* Si hay **tres unos** $\rightarrow$ escribo **`11`** ($C_{out}=1, S=1$).

| Fila | $A$ | $B$ | $C_{in}$ | Cantidad de `1` en la fila | En Binario ($C_{out}\,S$) | Acarreo de Salida ($C_{out}$) | Suma ($S$) |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **0** | 0 | 0 | 0 | 0 unos | `00` | `0` | `0` |
| **1** | 0 | 0 | 1 | 1 uno | `01` | `0` | **`1`** |
| **2** | 0 | 1 | 0 | 1 uno | `01` | `0` | **`1`** |
| **3** | 0 | 1 | 1 | 2 unos | `10` | **`1`** | `0` |
| **4** | 1 | 0 | 0 | 1 uno | `01` | `0` | **`1`** |
| **5** | 1 | 0 | 1 | 2 unos | `10` | **`1`** | `0` |
| **6** | 1 | 1 | 0 | 2 unos | `10` | **`1`** | `0` |
| **7** | 1 | 1 | 1 | 3 unos | `11` | **`1`** | **`1`** |

> **🧠 Regla verbal para recordar toda la vida:**  
> * **$S = 1$** cuando en la entrada hay un **número IMPAR de unos** (1 o 3 unos).  
> * **$C_{out} = 1$** cuando en la entrada hay **AL MENOS DOS unos** (es exactamente la misma función de mayoría que nuestra alarma de 3 sensores de la Semana 2).

### 3.2. Obtención de las Ecuaciones con Mapas de Karnaugh

#### A) Mapa de Karnaugh para la Suma ($S$): El patrón «Tablero de Ajedrez»

| $A \setminus BC_{in}$ | `00` | `01` | `11` | `10` |
| :---: | :---: | :---: | :---: | :---: |
| **`0`** | `0` | **`1`** | `0` | **`1`** |
| **`1`** | **`1`** | `0` | **`1`** | `0` |

* **Diagnóstico:** Los unos quedan alternados en diagonal como un tablero de ajedrez; ningún `1` tiene un vecino horizontal ni vertical. Siempre que en un K-map aparece el tablero de ajedrez, significa que la función es una **XOR encadenada de todas las variables**:
  $$\mathbf{S = A \oplus B \oplus C_{in}}$$

#### B) Mapa de Karnaugh para el Acarreo de Salida ($C_{out}$): Tres grupos de dos

| $A \setminus BC_{in}$ | `00` | `01` | `11` | `10` |
| :---: | :---: | :---: | :---: | :---: |
| **`0`** | `0` | `0` | **`1`** ($m_3$) | `0` |
| **`1`** | `0` | **`1`** ($m_5$) | **`1`** ($m_7$) | **`1`** ($m_6$) |

* **Agrupaciones:**
  1. Par vertical en la columna `11` ($m_3, m_7$) $\rightarrow$ **$B \cdot C_{in}$**
  2. Par horizontal derecho en la fila $A=1$ ($m_7, m_6$) $\rightarrow$ **$A \cdot B$**
  3. Par horizontal izquierdo en la fila $A=1$ ($m_5, m_7$) $\rightarrow$ **$A \cdot C_{in}$**
* **Ecuación mínima de acarreo:**
  $$\mathbf{C_{out} = AB + A \cdot C_{in} + B \cdot C_{in}}$$
  *(Nota guía adicional: Si construimos el sumador completo uniendo dos semisumadores y una compuerta OR, la ecuación equivalente factorizada también se expresa como $C_{out} = AB + C_{in}(A \oplus B)$, lo que ahorra compuertas al reutilizar la XOR de $A \oplus B$).*

---

## 🌊 4. Sumador de 4 Bits en Cadena (*Ripple Carry*) y Estimación del Retardo

### 4.1. Cómo conectar 4 sumadores completos en cascada
Para sumar dos números de 4 bits ($A_3 A_2 A_1 A_0 + B_3 B_2 B_1 B_0$), colocamos **cuatro sumadores en fila** (como los que vienen integrados dentro del chip comercial **`74LS83`** o **`74LS283`**) siguiendo una regla única:
> **Regla de conexión en cascada:** El acarreo que sale de una etapa (**$C_{out}$**) se conecta directamente al acarreo que entra en la etapa siguiente (**$C_{in}$**), poniendo $C_{in} = 0$ en el Sumador 0.

```text
          A3 B3              A2 B2              A1 B1              A0 B0
           │  │               │  │               │  │               │  │
           ▼  ▼               ▼  ▼               ▼  ▼               ▼  ▼
        ┌────────┐   C3    ┌────────┐   C2    ┌────────┐   C1    ┌────────┐
Cout ◄──┤Sumador │◄────────┤Sumador │◄────────┤Sumador │◄────────┤Sumador │◄── Cin = 0
 (C4)   │   3    │         │   2    │         │   1    │         │   0    │
        └───┬────┘         └───┬────┘         └───┬────┘         └───┬────┘
            ▼                  ▼                  ▼                  ▼
           S3                 S2                 S1                 S0
       (Listo en t4)      (Listo en t3)      (Listo en t2)      (Listo en t1)
```

### 4.2. ¿Por qué se llama *Ripple Carry* (Acarreo en Ola) y cuánto tarda?
En el mundo físico las compuertas no son instantáneas: los transistores internos tardan unos cuantos **nanosegundos ($\text{ns}$)** en cambiar de estado.
* El **Sumador 1** no puede dar su resultado definitivo hasta que el **Sumador 0** termine de calcular el acarreo $C_1$.
* El **Sumador 2** debe esperar a que el Sumador 1 calcule $C_2$.
* El **Sumador 3** debe esperar a que el acarreo recorra las tres etapas previas de derecha a izquierda **como una ola (*ripple*)**.

#### Fórmula práctica de estimación del retardo
En clase tomamos dos datos estándar de referencia:
1. Cada compuerta lógica tarda aproximadamente **$10\text{ ns}$** ($\text{nanosegundos} = 10^{-9}\text{ s}$).
2. En cada etapa del sumador, el camino del acarreo atraviesa **2 niveles de compuertas** (la compuerta AND y la compuerta OR de $C_{out}$), es decir, **$20\text{ ns}$ por cada bit**.

Por lo tanto, para un sumador de **$n$ bits**, el tiempo total para que la última salida sea 100 % confiable es:
$$\mathbf{\text{Retardo total} \approx 2 \cdot n \cdot 10\text{ ns}}$$

| Tamaño del Sumador ($n$ bits) | Compuertas que cruza el acarreo ($2 \cdot n$) | Tiempo Estimado de Respuesta |
| :---: | :---: | :---: |
| **4 bits** | $2 \times 4 = 8\text{ compuertas}$ | **$80\text{ ns}$** |
| **16 bits** | $2 \times 16 = 32\text{ compuertas}$ | **$320\text{ ns}$** |
| **32 bits** | $2 \times 32 = 64\text{ compuertas}$ | **$640\text{ ns}$** |
| **64 bits** *(Adicional)* | $2 \times 64 = 128\text{ compuertas}$ | **$1280\text{ ns}$ ($1,28\text{ }\mu\text{s}$)** |

> **💡 Nota guía adicional:** En procesadores modernos de 64 bits esperar $1280\text{ ns}$ sería demasiado lento; por eso en arquitectura de computadores avanzada este sumador en ola se mejora con un circuito llamado *Carry Look-Ahead* (acarreo anticipado), que predice los acarreos con ecuaciones booleanas sin esperar la ola.

---

## ➖ 5. Sumador/Restador Unificado y Detección de *Overflow*

### 5.1. La compuerta XOR como «Inversor Controlado»
En la Semana 1 aprendimos que restar es sumar el negativo en complemento a dos:
$$A - B = A + (-B) = A + B' + 1$$

¿Cómo logramos que **un mismo circuito** sume cuando una señal de control vale $M = 0$ y reste cuando vale $M = 1$? Colocando una compuerta **XOR en cada bit de $B$** controlada por el cable de modo $M$:

| Señal de Modo $M$ | Entrada $B$ | Salida de la XOR ($B \oplus M$) | ¿Qué le hace la XOR al número $B$? |
| :---: | :---: | :---: | :--- |
| **`0` *(Modo SUMA)*** | `0` | `0` | **Deja pasar a $B$ intacto** ($B \oplus 0 = B$). |
| **`0` *(Modo SUMA)*** | `1` | `1` | **Deja pasar a $B$ intacto** ($B \oplus 0 = B$). |
| **`1` *(Modo RESTA)*** | `0` | **`1`** | **Invierte los bits de $B$** ($B \oplus 1 = B'$). |
| **`1` *(Modo RESTA)*** | `1` | **`0`** | **Invierte los bits de $B$** ($B \oplus 1 = B'$). |

* **¿Y de dónde sacamos el `$+1$` que falta para completar el complemento a dos?**  
  ¡Aprovechamos la entrada **$C_{in}$ del Sumador 0** que antes teníamos conectada a `0`! Conectamos el mismo cable $M$ directamente a $C_{in}$:
  * Cuando **$M = 0$ (Suma):** las XOR dejan pasar $B$ normal y entra $C_{in} = 0 \rightarrow$ calcula **$A + B + 0 = A + B$**.
  * Cuando **$M = 1$ (Resta):** las XOR invierten todos los bits de $B$ ($B'$) y entra $C_{in} = 1 \rightarrow$ calcula **$A + B' + 1 = A - B$**.

### 5.2. Detección de *Overflow* (Desbordamiento) por Hardware
Recordemos que en 4 bits con signo (complemento a dos) solo caben números entre **$-8$ y $+7$**. Si sumamos $+5$ (`0101`) más $+4$ (`0100`), el resultado matemático $+9$ no cabe y el circuito entrega `1001` ($-7$), produciendo un error de **overflow**.

* **¡Cuidado con una confusión muy común!** Que el acarreo final valga **$C_{out} = 1$ NO significa que haya overflow** (por ejemplo, al restar $6 - 3 = 0110 + 1101 = (1)0011 = +3$, sale $C_{out}=1$ y el resultado es perfectamente correcto).
* **¿Cómo detecta el circuito el overflow real ($V$)?**  
  Observando el acarreo que **entra** a la última etapa del bit de signo (**$C_3$**) y el acarreo que **sale** de esa última etapa (**$C_4$ o $C_{out}$**). Si esos dos acarreos son **diferentes**, hubo desbordamiento. Y como la compuerta que detecta cuándo dos bits son diferentes es la **XOR**, el detector de overflow es una sola compuerta XOR:
  $$\mathbf{V = C_3 \oplus C_4}$$

### 5.3. Análisis de la Demostración en Clase: `0101` + `0011` en el Circuito
Revisemos etapa por etapa qué ocurre cuando el sumador de 4 bits calcula `0101` + `0011`:

| Etapa (Columna) | Bit $A$ | Bit $B$ | Acarreo que entra ($C_{in}$) | # de unos | Suma ($S$) | Acarreo que sale ($C_{out}$) |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **Etapa 0** | 1 | 1 | 0 | 2 unos | **`0`** | `1` ($C_1$) |
| **Etapa 1** | 0 | 1 | 1 ($C_1$) | 2 unos | **`0`** | `1` ($C_2$) |
| **Etapa 2** | 1 | 0 | 1 ($C_2$) | 2 unos | **`0`** | **`1` ($C_3$)** |
| **Etapa 3** | 0 | 0 | **`1` ($C_3$)** | 1 uno | **`1`** | **`0` ($C_4$)** |

* **Resultado binario en las salidas $S_3 S_2 S_1 S_0$:** **`1000`**.
* **Interpretación 1 (Como números SIN signo, de 0 a 15):** Sumamos $5 + 3 = \mathbf{8}$ (`1000`). El resultado sin signo es **totalmente correcto**.
* **Interpretación 2 (Como números CON signo, de $-8$ a $+7$):** Sumamos $(+5) + (+3)$, pero en complemento a dos el `1000` significa **$-8$**. Calculamos la bandera de overflow: como $C_3 = 1$ y $C_4 = 0$, entonces **$V = C_3 \oplus C_4 = 1 \oplus 0 = \mathbf{1}$ (¡Hay Overflow con signo!)**.

---

## ⚖️ 6. Comparador de Magnitud y Arquitectura de la ALU

### 6.1. ¿Cómo compara un circuito dos números binarios $A$ y $B$?
Además de sumar y restar, un procesador necesita saber si **$A = B$** o si **$A > B$** (función que realiza el circuito integrado comercial **`74LS85`** para 4 bits):

1. **Para saber si son IGUALES ($A = B$):**  
   En la Semana 1 aprendimos que la compuerta **XNOR** da `1` cuando sus dos entradas son iguales. Por lo tanto, colocamos una XNOR en cada pareja de bits ($A_i, B_i$) y unimos todas las salidas con una **compuerta AND**:
   * Para 2 bits ($A_1 A_0$ y $B_1 B_0$): $E_1 = (A_1 \oplus B_1)'$ y $E_0 = (A_0 \oplus B_0)' \rightarrow \mathbf{(A = B) = E_1 \cdot E_0}$.
2. **Para saber si $A$ es MAYOR que $B$ ($A > B$):**  
   Comparamos igual que los humanos: **miramos de izquierda a derecha (desde el bit de mayor peso)** y el primer bit donde desempaten decide quién gana:
   * En 2 bits, $A > B$ si $A$ gana directamente en el bit alto ($A_1 = 1 \text{ y } B_1 = 0 \rightarrow A_1 B_1'$), **o bien** si empatan en el bit alto ($E_1 = 1$) y $A$ gana en el bit bajo ($A_0 = 1 \text{ y } B_0 = 0 \rightarrow E_1 A_0 B_0'$):
     $$\mathbf{(A > B) = A_1 B_1' + (A_1 \oplus B_1)' A_0 B_0'}$$

### 6.2. De aquí nace la ALU (Unidad Aritmético-Lógica)
Al juntar todos los circuitos que hemos construido entre la Semana 1 y la Semana 4, descubrimos que ya tenemos el corazón de una CPU real: la **ALU**.

```text
   ┌──────────────────────────┐
   │    Sumador / Restador    ├──────┐
   └──────────────────────────┘      │      ┌─────────────────────┐
   ┌──────────────────────────┐      ├─────►│     Multiplexor     │
   │  Comparador de Magnitud  ├─────►│      │   (Selector de la   ├────► Resultado Final de
   └──────────────────────────┘      ├─────►│      Semana 5)      │       la Instrucción
   ┌──────────────────────────┐      │      └─────────────────────┘
   │ Compuertas AND / OR / XOR├──────┘
   └──────────────────────────┘
```
Todos los bloques calculan su operación en paralelo, y un circuito selector llamado **Multiplexor** (que estudiaremos en la Semana 5) elige cuál de esos resultados pasa a la salida según la instrucción del programa.

---

## 🛠️ 7. Resolución Completa del Taller Dirigido en Parejas (4 Ejercicios)

A continuación desarrollo paso a paso los cuatro ejercicios prácticos planteados en el taller de la Semana 4:

### Ejercicio 1: Sumar `1011` + `0110` escribiendo el acarreo de cada columna
* Primero identifico sus valores en decimal (sin signo): `1011` $= 8 + 2 + 1 = 11$, y `0110` $= 4 + 2 = 6$. La suma debe dar $11 + 6 = 17$ ($10001_2$).

```text
  Acarreo (Cin):    1   1   1   0   ← Acarreos que entran a cada columna
       Número A:    1   0   1   1   (11)
     + Número B:    0   1   1   0   ( 6)
     ────────────────────────────
      Resultado:  1 0   0   0   1   (17 en sin signo: 16 + 1 = 17 ✓)
                Cout S3  S2  S1  S0
```

| Columna (Etapa) | $A_i$ | $B_i$ | $C_{in}$ | Operación en la columna | Escribo ($S_i$) | Me llevo ($C_{out}$) |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **Columna 0** | 1 | 0 | 0 | `1 + 0 + 0 = 1` ($01_2$) | **`1`** ($S_0$) | **`0`** ($C_1$) |
| **Columna 1** | 1 | 1 | 0 ($C_1$) | `1 + 1 + 0 = 2` ($10_2$) | **`0`** ($S_1$) | **`1`** ($C_2$) |
| **Columna 2** | 0 | 1 | 1 ($C_2$) | `0 + 1 + 1 = 2` ($10_2$) | **`0`** ($S_2$) | **`1`** ($C_3$) |
| **Columna 3** | 1 | 0 | 1 ($C_3$) | `1 + 0 + 1 = 2` ($10_2$) | **`0`** ($S_3$) | **`1`** ($C_4 = C_{out}$) |

* **Respuesta final:** Las salidas de suma son **$S_3 S_2 S_1 S_0 = 0001$** con acarreo final **$C_{out} = 1$** (formando `10001` $= 17_{10}$). *(Y como $C_3 = 1$ y $C_4 = 1$, la bandera de overflow con signo vale $V = 1 \oplus 1 = 0$, porque en complemento a dos la operación equivale a $(-5) + (+6) = +1$, es decir `0001` ✓).*

---

### Ejercicio 2: Restar `0110` $-$ `0011` con el circuito sumador poniendo $M = 1$
* En decimal queremos calcular $6 - 3 = +3$ (`0011`).
* Al activar la línea de control en modo resta (**$M = 1$**):
  1. Las 4 compuertas XOR invierten los bits de $B = 0011 \rightarrow B' = \mathbf{1100}$.
  2. La señal $M = 1$ entra como acarreo inicial **$C_{in} = 1$** en la Columna 0.
  3. El circuito ejecuta la suma $A + B' + 1$:

```text
  Acarreo (Cin):    1   1   0   1   ← En la Columna 0 entra Cin = M = 1
       Número A:    0   1   1   0   (+6)
     + B inv (B'):  1   1   0   0   (Bits de 0011 invertidos por las XOR)
     ────────────────────────────
      Resultado: (1)0   0   1   1   → Se descarta Cout=1 y queda 0011 = +3 ✓
```

| Columna (Etapa) | $A_i$ | $B_i' = B_i \oplus 1$ | $C_{in}$ | Suma en la columna | Escribo ($S_i$) | Me llevo ($C_{out}$) |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **Columna 0** | 0 | 1 | **`1`** ($M$) | `0 + 1 + 1 = 2` ($10_2$) | **`0`** ($S_0$ temporal si sin +1; con $C_{in}=1$: $0+1+1=2 \rightarrow S_0=0$? ¡Ojo!) | — |

*(Verifiquemos con cuidado bit a bit la tabla de $A = 0110$ y $B = 0011$):*
* En $B = 0011$, sus bits son $B_3=0, B_2=0, B_1=1, B_0=1$.
* Al invertirlos con XOR ($M=1$): **$B_3'=1, B_2'=1, B_1'=0, B_0'=0$** (`1100`).
* Ahora sumamos columna por columna con **$C_0 = M = 1$**:
  * **Columna 0:** $A_0(0) + B_0'(0) + C_0(1) = 1 \rightarrow$ **Escribo $S_0 = 1$, llevo $C_1 = 0$**.
  * **Columna 1:** $A_1(1) + B_1'(0) + C_1(0) = 1 \rightarrow$ **Escribo $S_1 = 1$, llevo $C_2 = 0$**.
  * **Columna 2:** $A_2(1) + B_2'(1) + C_2(0) = 2$ ($10_2$) $\rightarrow$ **Escribo $S_2 = 0$, llevo $C_3 = 1$**.
  * **Columna 3:** $A_3(0) + B_3'(1) + C_3(1) = 2$ ($10_2$) $\rightarrow$ **Escribo $S_3 = 0$, llevo $C_4 = 1$**.
* **Resultado final:** $S_3 S_2 S_1 S_0 = \mathbf{0011}$ ($+3$ en decimal), $C_4 = 1$ (se descarta) y overflow $V = C_3 \oplus C_4 = 1 \oplus 1 = \mathbf{0}$ (sin error) ✓.

---

### Ejercicio 3: Estimar el retardo de propagación de un sumador en cascada de 16 bits
* **Datos del problema:**
  * Número de etapas (bits): $n = 16\text{ bits}$.
  * Compuertas que atraviesa el acarreo por cada etapa: $2\text{ compuertas/bit}$.
  * Retardo típico por compuerta: $t_p \approx 10\text{ ns}$.
* **Cálculo paso a paso:**
  1. Total de compuertas en el camino crítico del acarreo: $2 \times 16 = \mathbf{32\text{ compuertas}}$.
  2. Tiempo total estimado:
     $$\text{Retardo}_{16\text{ bits}} = 32\text{ compuertas} \times 10\text{ ns/compuerta} = \mathbf{320\text{ ns}}$$

---

### Ejercicio 4: Comparar $A = 10_2$ y $B = 01_2$ — ¿$A > B$? ¿$A = B$?
* Identificamos los bits de cada número de 2 bits:
  * Para $A = 10_2$: el bit alto es **$A_1 = 1$** y el bit bajo es **$A_0 = 0$** (vale $2_{10}$).
  * Para $B = 01_2$: el bit alto es **$B_1 = 0$** y el bit bajo es **$B_0 = 1$** (vale $1_{10}$).
1. **Prueba de igualdad ($\text{¿}A = B\text{?}$):**
   * Evaluamos las XNOR en cada posición:
     * Bit alto: $E_1 = (A_1 \oplus B_1)' = (1 \oplus 0)' = 0$ (son diferentes).
     * Bit bajo: $E_0 = (A_0 \oplus B_0)' = (0 \oplus 1)' = 0$ (son diferentes).
   * Salida de igualdad: $(A = B) = E_1 \cdot E_0 = 0 \cdot 0 = \mathbf{0}$ (**No son iguales**).
2. **Prueba de mayoría ($\text{¿}A > B\text{?}$):**
   * Miramos desde la izquierda (el bit más significativo, posición 1): como **$A_1 = 1$ y $B_1 = 0$**, $A$ gana inmediatamente en el bit alto ($A_1 \cdot B_1' = 1 \cdot 1 = 1$) sin que importe qué haya en el bit bajo.
   * Evaluando la fórmula completa:
     $$(A > B) = A_1 B_1' + E_1 A_0 B_0' = (1 \cdot 0') + (0 \cdot 0 \cdot 1') = 1 + 0 = \mathbf{1}\quad (\text{Sí, } \mathbf{A > B}\text{ ✓})$$

---

## 📌 8. Síntesis Personal de la Semana 4 y Cierre de la Unidad 1

1. **Dos bloques para sumar cualquier tamaño:** Con un **Semisumador** ($S = A \oplus B$, $C = AB$) para la columna 0 y **Sumadores Completos** ($S = \text{impar de unos}$, $C_{out} = \text{al menos dos unos}$) para el resto de columnas podemos construir sumadores de 4, 16, 32 o 64 bits.
2. **El precio de la simplicidad es el retardo (*Ripple*):** Encadenar el acarreo de derecha a izquierda es muy sencillo de cablear, pero hace que cada etapa espere a la anterior ($\text{Retardo} \approx 2 \cdot n \cdot 10\text{ ns}$).
3. **La compuerta XOR convierte al sumador en restador:** Al usar $M$ como inversor controlado ($B \oplus M$) y conectarlo simultáneamente a $C_{in}$, un solo circuito suma ($M=0$) y resta en complemento a dos ($M=1$).
4. **El overflow con signo no es $C_{out}$:** El desbordamiento en complemento a dos se detecta comparando el acarreo que entra y el que sale del bit de signo mediante **$V = C_3 \oplus C_4$**.

---

&lt;div align=&quot;center&quot;&gt;

&lt;kbd&gt;[🏠 Portada](https://www.google.com/search?q=./README.md)&lt;/kbd&gt; &nbsp; &lt;kbd&gt;[⬅️ Anterior: Semana 3](https://www.google.com/search?q=./semana-3.md)&lt;/kbd&gt; &nbsp; &lt;kbd&gt;[📘 Semana 1](https://www.google.com/search?q=./semana-1.md)&lt;/kbd&gt; &nbsp; &lt;kbd&gt;[📗 Semana 2](https://www.google.com/search?q=./semana-2.md)&lt;/kbd&gt; &nbsp; &lt;kbd&gt;[📙 Semana 3](https://www.google.com/search?q=./semana-3.md)&lt;/kbd&gt; &nbsp; &lt;kbd&gt;📍 **Semana 4**&lt;/kbd&gt; &nbsp; &lt;kbd&gt;[🏠 Volver a Portada](https://www.google.com/search?q=./README.md)&lt;/kbd&gt;

&lt;/div&gt;
