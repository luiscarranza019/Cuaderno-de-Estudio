---

# 📕 Semana 4: Circuitos Aritméticos Combinacionales (El Corazón de la ALU)

---

## 🎯 Mi Objetivo de Aprendizaje en esta Semana

En las primeras tres semanas aprendí a representar números en binario, escribir tablas de verdad, simplificar ecuaciones mediante Mapas de Karnaugh e implementarlas con compuertas lógicas. En esta cuarta semana reúno todas esas herramientas para responder una pregunta apasionante: **¿cómo logra un circuito eléctrico sumar, restar y comparar números binarios?**

Al finalizar el estudio de esta guía soy capaz de:
1. Diseñar desde cero un **Semisumador (*Half Adder*)** y un **Sumador Completo (*Full Adder*)** aplicando el procedimiento clásico (tabla de verdad → Mapa de Karnaugh → circuito).
2. Conectar varios sumadores en cascada para construir un **Sumador de 4 bits (*Ripple Carry Adder*)** y calcular cuántos nanosegundos tarda en entregar la respuesta final.
3. Transformar el sumador en un **Sumador/Restador unificado** utilizando compuertas **XOR como inversores controlados** junto con el complemento a dos.
4. Detectar por hardware cuándo ocurre un error de **desbordamiento (*overflow*)** con números con signo (V = C_3 ⊕ C_4), diseñar un **comparador de magnitud** y comprender cómo todos estos bloques forman la **ALU (Unidad Aritmético-Lógica)** de un procesador.

---

## ➕ 1. De la Suma en Papel al Diseño por Columnas

### 1.1. Sumar un solo bit: Los 4 casos posibles
Cuando sumamos dos números en el colegio, como 7 + 5 = 12, decimos mentalmente: **«escribo el `2` en mi columna y me llevo `1` a la columna siguiente»**.

En binario ocurre exactamente lo mismo, pero es mucho más sencillo porque al sumar dos bits (A y B) solo existen **4 combinaciones posibles**, las cuales generan dos salidas:
* **S (*Sum* / Suma):** Es el bit que **escribo** debajo de la columna actual.
* **C (*Carry* / Acarreo):** Es el bit que **me llevo** hacia la columna de la izquierda.

| Operación Binaria | Resultado en Decimal y Binario | Escribo en la columna (S) | Me llevo a la siguiente (C) |
| :---: | :---: | :---: | :---: |
| `0 + 0` | 0 = 00_2 | **`0`** | **`0`** |
| `0 + 1` | 1 = 01_2 | **`1`** | **`0`** |
| `1 + 0` | 1 = 01_2 | **`1`** | **`0`** |
| `1 + 1` | 2 = 10_2 | **`0`** | **`1`** |

### 1.2. Ejemplo de suma de 4 bits columna por columna: `0110` (6) + `0111` (7)
Observemos con lupa qué sucede en cada una de las 4 columnas (numeradas de derecha a izquierda como Columna 0, Columna 1, Columna 2 y Columna 3):

```text
  Acarreo que llega (Cin):   1   1   0   0   ← En la Columna 0 nunca llega acarreo previo
                Número A:    0   1   1   0   ( 6 en decimal)
              + Número B:    0   1   1   1   ( 7 en decimal)
              ────────────────────────────
               Resultado:    1   1   0   1   (13 en decimal: 8 + 4 + 0 + 1 = 13 ✓)
              
              Posición:    Col3 Col2 Col1 Col0
```

| Columna | Operación realizada en esa columna | Escribo (S) | Llevo (Cout) | Observación clave para el hardware |
| :---: | :---: | :---: | :---: | :--- |
| **Columna 0** | `0 + 1 = 1` | `1` | `0` | Solo suma **2 bits** (A_0 y B_0); a su derecha no hay ninguna columna previa. |
| **Columna 1** | `1 + 1 + 0 = 2` (10_2) | `0` | `1` | Suma **3 bits** (A_1, B_1 y el acarreo `0` que llegó de la Columna 0). |
| **Columna 2** | `1 + 1 + 1 = 3` (11_2) | `1` | `1` | Suma **3 bits** (A_2, B_2 y el acarreo `1` que llegó de la Columna 1). |
| **Columna 3** | `0 + 0 + 1 = 1` (01_2) | `1` | `0` | Suma **3 bits** (A_3, B_3 y el acarreo `1` que llegó de la Columna 2). |

> **💡 El efecto dominó del acarreo:** Al sumar `0111` (7) + `0001` (1), el acarreo que nace en la Columna 0 provoca un nuevo acarreo en la Columna 1, este a su vez en la Columna 2 y llega hasta la Columna 3 para formar `1000` (8). Como los acarreos se encadenan, distinguimos **dos tipos de columnas**, que dan origen a **dos circuitos diferentes**: el **Semisumador** (para la Columna 0) y el **Sumador Completo** (para las Columnas 1, 2, 3…).

---

## 🔧 2. El Semisumador (*Half Adder*): Para la Columna 0

El **Semisumador** es el circuito encargado de sumar únicamente **2 bits de entrada (A y B)** sin recibir ningún acarreo previo. Se llama *«semi»* precisamente porque no tiene entrada de acarreo (Cin) y solo sirve para la primera columna de la derecha (Columna 0).

### 2.1. Tabla de Verdad y Ecuaciones del Semisumador

| Entrada A | Entrada B | Acarreo C (Me llevo) | Suma S (Escribo) | Análisis visual de las columnas de salida |
| :---: | :---: | :---: | :---: | :--- |
| `0` | `0` | `0` | `0` | Ambos son `0`. |
| `0` | `1` | `0` | **`1`** | A y B son distintos → S = 1. |
| `1` | `0` | `0` | **`1`** | A y B son distintos → S = 1. |
| `1` | `1` | **`1`** | `0` | Ambos son `1` → C = 1 y S = 0. |

Al comparar las dos columnas de salida con las 7 compuertas que estudiamos en la Semana 1, las reconocemos de inmediato sin siquiera hacer un mapa de Karnaugh:
* **La columna de Suma (S)** vale `1` cuando A y B son distintos → es una compuerta **XOR**:
  **S = A ⊕ B**
* **La columna de Acarreo (C)** vale `1` solo cuando A y B son ambos `1` → es una compuerta **AND**:
  **C = A · B**

```text
   A ───┬──────► [ XOR ] ───► S = A ⊕ B  (Suma)
        │      ┌►[     ]
   B ───┼──────┘
        └──────► [ AND ] ───► C = A · B  (Acarreo)
               └►[     ]
```

---

## ⚙️ 3. El Sumador Completo (*Full Adder*): Para las Columnas 1, 2, 3…

A partir de la Columna 1, el circuito necesita sumar **3 bits al mismo tiempo**: el bit A, el bit B y el acarreo que llega de la columna anterior (Cin). A este bloque lo llamamos **Sumador Completo**.

### 3.1. El truco de «Contar Unos» para armar la Tabla de Verdad sin memorizar
Para llenar las 8 filas de la tabla de verdad del sumador completo sin equivocarme jamás, aplico este método práctico: **cuento cuántos unos hay en las tres entradas (A, B, Cin) de esa fila y escribo esa cantidad en binario de 2 bits (Cout S)**:
* Si hay **cero unos** → escribo **`00`** (Cout = 0, S = 0).
* Si hay **un solo uno** → escribo **`01`** (Cout = 0, S = 1).
* Si hay **dos unos** → escribo **`10`** (Cout = 1, S = 0).
* Si hay **tres unos** → escribo **`11`** (Cout = 1, S = 1).

| Fila | A | B | Cin | Cantidad de `1` en la fila | En Binario (Cout S) | Acarreo de Salida (Cout) | Suma (S) |
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
> * **S = 1** cuando en la entrada hay un **número IMPAR de unos** (1 o 3 unos).  
> * **Cout = 1** cuando en la entrada hay **AL MENOS DOS unos** (es exactamente la misma función de mayoría que nuestra alarma de 3 sensores de la Semana 2).

### 3.2. Obtención de las Ecuaciones con Mapas de Karnaugh

#### A) Mapa de Karnaugh para la Suma (S): El patrón «Tablero de Ajedrez»

| A \ BCin | `00` | `01` | `11` | `10` |
| :---: | :---: | :---: | :---: | :---: |
| **`0`** | `0` | **`1`** | `0` | **`1`** |
| **`1`** | **`1`** | `0` | **`1`** | `0` |

* **Diagnóstico:** Los unos quedan alternados en diagonal como un tablero de ajedrez; ningún `1` tiene un vecino horizontal ni vertical. Siempre que en un K-map aparece el tablero de ajedrez, significa que la función es una **XOR encadenada de todas las variables**:
  **S = A ⊕ B ⊕ Cin**

#### B) Mapa de Karnaugh para el Acarreo de Salida (Cout): Tres grupos de dos

| A \ BCin | `00` | `01` | `11` | `10` |
| :---: | :---: | :---: | :---: | :---: |
| **`0`** | `0` | `0` | **`1`** (m_3) | `0` |
| **`1`** | `0` | **`1`** (m_5) | **`1`** (m_7) | **`1`** (m_6) |

* **Agrupaciones:**
  1. Par vertical en la columna `11` (m_3, m_7) → **B · Cin**
  2. Par horizontal derecho en la fila A=1 (m_7, m_6) → **A · B**
  3. Par horizontal izquierdo en la fila A=1 (m_5, m_7) → **A · Cin**
* **Ecuación mínima de acarreo:**
  **Cout = AB + A · Cin + B · Cin**

---

## 🌊 4. Sumador de 4 Bits en Cadena (*Ripple Carry*) y Estimación del Retardo

### 4.1. Cómo conectar 4 sumadores completos en cascada
Para sumar dos números de 4 bits (A_3 A_2 A_1 A_0 + B_3 B_2 B_1 B_0), colocamos **cuatro sumadores en fila** siguiendo una regla única:
> **Regla de conexión en cascada:** El acarreo que sale de una etapa (**Cout**) se conecta directamente al acarreo que entra en la etapa siguiente (**Cin**), poniendo Cin = 0 en el Sumador 0.

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
En el mundo físico las compuertas tardan unos cuantos nanosegundos (ns) en conmutar:
* Cada compuerta tarda ≈ 10 ns.
* El acarreo cruza ≈ 2 compuertas por etapa (20 ns/bit).
* Para n bits: **Retardo ≈ 2 · n · 10 ns**.
  * **4 bits:** 80 ns
  * **16 bits:** 320 ns
  * **32 bits:** 640 ns

---

## ➖ 5. Sumador/Restador Unificado y Detección de *Overflow*

### 5.1. La compuerta XOR como «Inversor Controlado»
Restar es sumar el negativo en complemento a dos (A - B = A + B' + 1).
Usamos una compuerta **XOR en cada bit de B** controlada por el modo M:
* **M = 0 (Suma):** B ⊕ 0 = B y Cin = 0 → **A + B**.
* **M = 1 (Resta):** B ⊕ 1 = B' y Cin = 1 → **A + B' + 1 = A - B**.

### 5.2. Detección de *Overflow* por Hardware
En 4 bits con signo el rango es de -8 a +7. Ocurre overflow si el acarreo que entra a la última etapa (C_3) difiere del que sale (C_4):
**V = C_3 ⊕ C_4**
*(Recordatorio: Cout = 1 NO significa error).*

---

## ⚖️ 6. Comparador de Magnitud y Arquitectura de la ALU

* **Igualdad (A = B):** Compuertas **XNOR** en cada bit (dan `1` si son iguales) y una compuerta **AND** final.
* **Mayor que (A > B):** Se evalúa de izquierda a derecha. En 2 bits: **A_1 B_1' + (A_1 ⊕ B_1)' A_0 B_0'**.
* **La ALU (Unidad Aritmético-Lógica):** Sumador/Restador, Comparador y compuertas lógicas operan en paralelo; un **Multiplexor** selecciona el resultado final.

---

## 🛠️ 7. Resolución del Taller Dirigido en Parejas

1. **Sumar `1011` + `0110`:**  
   A = 11 (decimal), B = 6 (decimal). Acarreos: C_3 = 1, C_2 = 1, C_1 = 1, C_0 = 0.  
   Salida: S = **0001** con Cout = **1** (resultado 10001_2 = 17 en decimal ✓).
2. **Restar `0110` - `0011` con M=1:**  
   B = 0011 → B' = 1100, con Cin = 1.  
   Suma columna por columna: S = **0011** (+3 en decimal ✓), Cout = 1 (se descarta) y V = 1 ⊕ 1 = 0.
3. **Retardo de un sumador de 16 bits:**  
   2 × 16 = 32 compuertas × 10 ns = **320 ns**.
4. **Comparar A = 10_2 y B = 01_2:**  
   Bit alto: A_1 = 1, B_1 = 0. Como A_1 > B_1, **A > B = 1** y **A = B = 0**.

---

| [⬅️ Semana 3](semana-3.md) | 🏠 [ Inicio](README.md) |
| :--- | ---: |
