&lt;div align=&quot;center&quot;&gt;

# ⚡ Cuaderno Digital: Diseño de Circuitos
### Unidad 1 · Análisis de Lógica Combinacional con Diseño Clásico

&lt;kbd&gt;🏠 **Portada Principal**&lt;/kbd&gt; &nbsp; &lt;kbd&gt;[📘 Semana 1](https://www.google.com/search?q=./semana-1.md)&lt;/kbd&gt; &nbsp; &lt;kbd&gt;[📗 Semana 2](https://www.google.com/search?q=./semana-2.md)&lt;/kbd&gt; &nbsp; &lt;kbd&gt;[📙 Semana 3](https://www.google.com/search?q=./semana-3.md)&lt;/kbd&gt; &nbsp; &lt;kbd&gt;[📕 Semana 4](https://www.google.com/search?q=./semana-4.md)&lt;/kbd&gt; &nbsp; &lt;kbd&gt;[➡️ Empezar: Semana 1](https://www.google.com/search?q=./semana-1.md)&lt;/kbd&gt;

&lt;/div&gt;

---

## 🏛️ Portada Institucional

| Campo | Información Académica |
| :--- | :--- |
| **Institución** | Universidad Nacional de Loja (UNL) |
| **Facultad / Carrera** | Carrera de Computación |
| **Ciclo Académico** | Segundo Ciclo (2.º Ciclo «A») · Período Septiembre 2026 – Marzo 2027 |
| **Asignatura** | Diseño de Circuitos |
| **Docente** | Ingeniero Luis Darío Shinche Cueva, Mg. |
| **Estudiante / Autor** | Luis Anderson Carranza Quispe |
| **Resultado de Aprendizaje (R1)** | Diseña e implementa circuitos combinacionales básicos utilizando técnicas de diseño clásico. |

---

## 👋 Presentación del Cuaderno

¡Bienvenido a mi cuaderno digital de la asignatura **Diseño de Circuitos**! He creado este repositorio como una guía de estudio completa, estructurada y redactada desde mi propia perspectiva de aprendizaje a lo largo de las primeras cuatro semanas de clases en la **Universidad Nacional de Loja**.

Mi objetivo con este material no es únicamente recopilar los apuntes vistos en el aula, sino explicar cada concepto con un lenguaje claro, formal y sencillo de comprender. A lo largo de cada semana incluyo:

* **Explicaciones paso a paso en primera persona**, conectando la teoría electrónica con situaciones cotidianas para entender el *porqué* de cada regla antes de aplicar fórmulas.
* **Notas de guía y consejos prácticos**, donde destaco los errores más comunes que debemos evitar al diseñar o simular circuitos y trucos para comprobar resultados rápidamente.
* **Resolución íntegra de todos los retos, talleres y tickets de salida** planteados en clase, mostrando el procedimiento completo desde el enunciado hasta el circuito final.
* **Diagramas en texto, tablas de verdad detalladas y mapas de Karnaugh**, listos para visualizarse directamente en GitHub desde cualquier dispositivo.

---

## 🧭 Navegación Rápida por Semanas

Puedes utilizar los botones superiores o la siguiente tabla interactiva para desplazarte directamente a cualquier semana del cuaderno:

| Semana | Título de la Unidad / Sesión | Temas Principales y Contenido Guía | Acceso Directo |
| :---: | :--- | :--- | :---: |
| **Semana 1** | **Sistemas Numéricos, Códigos y Compuertas Lógicas** | Señales digitales y margen de ruido (familia 74LS), sistemas binario, octal y hexadecimal, aritmética binaria, complemento a dos, desbordamiento, códigos BCD, Gray y ASCII, las 7 compuertas lógicas (normas ANSI e IEC) y lectura del chip `74LS08`. | &lt;kbd&gt;[Ir a Semana 1 ➡️](https://www.google.com/search?q=./semana-1.md)&lt;/kbd&gt; |
| **Semana 2** | **Álgebra de Boole y Formas Canónicas** | Equivalencia entre circuito y expresión algebraica, postulados de Huntington, principio de dualidad, teoremas de consulta, Leyes de De Morgan, minitérminos ($m_i$) y maxitérminos ($M_i$), formas canónicas SOP ($\Sigma m$) y POS ($\Pi M$), simplificación algebraica y análisis de costo en circuitos integrados. | &lt;kbd&gt;[Ir a Semana 2 ➡️](https://www.google.com/search?q=./semana-2.md)&lt;/kbd&gt; |
| **Semana 3** | **Mapas de Karnaugh, Indiferentes y Compuertas NAND/NOR** *(Unificada: Sesiones 3 y 3.1)* | Adyacencia lógica y orden Gray en mapas de 3 y 4 variables, reglas de agrupación y topología toroidal, implicantes primos y esenciales, minimización SOP y POS, condiciones indiferentes (*don't care* `X`), universalidad de compuertas NAND y NOR, conversión a redes `NAND-NAND` / `NOR-NOR` y lanzamiento del proyecto integrador con FPGA Basys 3. | &lt;kbd&gt;[Ir a Semana 3 ➡️](https://www.google.com/search?q=./semana-3.md)&lt;/kbd&gt; |
| **Semana 4** | **Circuitos Aritméticos Combinacionales** | Suma binaria por columnas, diseño del semisumador (*Half Adder*) y del sumador completo (*Full Adder*), sumador en cascada de 4 bits (*Ripple Carry Adder*), cálculo de retardo de propagación, sumador/restador unificado con inversores controlados (XOR), detección de *overflow* ($V = C_3 \oplus C_4$), comparador de magnitud (`7485`) e integración en la ALU. | &lt;kbd&gt;[Ir a Semana 4 ➡️](https://www.google.com/search?q=./semana-4.md)&lt;/kbd&gt; |

---

## 🛠️ Herramientas y Estándares que Utilizamos en el Curso

Para aprovechar al máximo esta guía, resumo las convenciones y herramientas prácticas con las que trabajamos en los laboratorios (prácticas APE):

1. **Simulador Digital — Logisim-evolution:** Es nuestra herramienta principal para armar, probar y verificar esquemas combinacionales antes de pasar al hardware físico. Trabajamos con la simbología **ANSI/IEEE** (formas distintivas para cada compuerta).
2. **Familia Lógica Comercial — Serie 74LS / 74HC:** Analizamos el costo real de nuestros diseños contando no solo el número de compuertas, sino cuántos **circuitos integrados (CI)** físicos necesitamos (por ejemplo, `7404` para NOT, `7408` y `7411` para AND, `7432` para OR, `7400` y `7410` para NAND, `7483` para sumadores de 4 bits y `7485` para comparadores).
3. **Tarjeta de Desarrollo — Digilent Basys 3 (FPGA):** Para el proyecto integrador de la asignatura, proyectamos nuestros diseños hacia una tarjeta Basys 3 equipada con 16 interruptores, 5 pulsadores, 16 LEDs y 4 displays de 7 segmentos, evolucionando desde bloques combinacionales (Unidad 1) y secuenciales (Unidad 2) hasta descripción en lenguaje VHDL (Unidad 3).

---

## 📂 Estructura de Archivos del Repositorio

```text
📦 cuaderno-diseno-circuitos/
 ┣ 📄 README.md       # Portada institucional, presentación e índice general
 ┣ 📄 semana-1.md     # Guía completa de la Semana 1 + Retos y Actividad 2 resueltos
 ┣ 📄 semana-2.md     # Guía completa de la Semana 2 + Taller grupal (G1-G8) y Ticket resueltos
 ┣ 📄 semana-3.md     # Guía unificada de la Semana 3 (Sesiones 1 y 2) + Talleres T1-T4 resueltos
 ┗ 📄 semana-4.md     # Guía completa de la Semana 4 + Demostraciones y Taller dirigido resuelto
```

---

## ✒️ Créditos y Autoría

* **Autor del cuaderno y desarrollo de ejercicios:** Luis Anderson Carranza Quispe
* **Cátedra y material base de referencia:** Ingeniero Luis Darío Shinche Cueva, Mg. — Docente de la asignatura *Diseño de Circuitos*
* **Institución:** Universidad Nacional de Loja (UNL) · Carrera de Computación · Segundo Ciclo

---

&lt;div align=&quot;center&quot;&gt;

### 🚀 Comenzar el Recorrido del Cuaderno

&lt;kbd&gt;🏠 **Estás en la Portada**&lt;/kbd&gt; &nbsp; &lt;kbd&gt;[📘 Semana 1](https://www.google.com/search?q=./semana-1.md)&lt;/kbd&gt; &nbsp; &lt;kbd&gt;[📗 Semana 2](https://www.google.com/search?q=./semana-2.md)&lt;/kbd&gt; &nbsp; &lt;kbd&gt;[📙 Semana 3](https://www.google.com/search?q=./semana-3.md)&lt;/kbd&gt; &nbsp; &lt;kbd&gt;[📕 Semana 4](https://www.google.com/search?q=./semana-4.md)&lt;/kbd&gt; &nbsp; &lt;kbd&gt;[➡️ Ir a la Semana 1](https://www.google.com/search?q=./semana-1.md)&lt;/kbd&gt;

&lt;/div&gt;
