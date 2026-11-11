# ⚡ Cuaderno Digital: Diseño de Circuitos
> **Universidad Nacional de Loja** · Facultad de la Energía, las Industrias y los Recursos Naturales No Renovables  
> **Carrera de Computación** · Segundo Ciclo «A» · Período Septiembre 2026 – Marzo 2027

---

### 🧭 Menú de Navegación Rápida

| 🏠 Inicio | 📘 Unidad 1.1 | 📗 Unidad 1.2 | 📙 Unidad 1.3 | 📕 Unidad 1.4 | 🚀 Primer Paso |
| :---: | :---: | :---: | :---: | :---: | :---: |
| **[Portada](https://www.google.com/search?q=./README.md)** | **[Semana 1](https://www.google.com/search?q=./semana-1.md)** | **[Semana 2](https://www.google.com/search?q=./semana-2.md)** | **[Semana 3](https://www.google.com/search?q=./semana-3.md)** | **[Semana 4](https://www.google.com/search?q=./semana-4.md)** | **[Iniciar Lectura ➡️](https://www.google.com/search?q=./semana-1.md)** |

---

## 🏛️ Ficha Técnica y Portada Académica

```text
========================================================================================
                     UNIVERSIDAD NACIONAL DE LOJA
                     CARRERA DE COMPUTACIÓN · 2.º CICLO
========================================================================================
  Asignatura:     Diseño de Circuitos
  Unidad 1:       Análisis de lógica combinacional con diseño clásico
  Docente Cátedra: Ing. Luis Darío Shinche Cueva, Mg.
  Estudiante:     Luis Anderson Carranza Quispe
  Ciclo:          Segundo Ciclo «A»
  Período:        Septiembre 2026 – Marzo 2027
  Resultado (R1): Diseña e implementa circuitos combinacionales básicos utilizando
                  técnicas de diseño clásico.
========================================================================================
```

---

## 🎯 Presentación y Propósito de la Guía

> 💡 **Nota del autor:**  
> He diseñado este repositorio como una bitácora técnica de aprendizaje en primera persona. En lugar de limitarme a copiar diapositivas, he estructurado cada tema con un enfoque deductivo: partiendo del problema físico del hardware (voltajes, ruido e interferencias) hasta llegar a las ecuaciones booleanas y su optimización en circuitos integrados reales y tarjetas FPGA.

### 🌟 Pilares Pedagógicos de este Cuaderno
1. **Claridad conceptual sin rodeos:** Explicaciones sencillas que no pierden rigor técnico ni formalidad académica.
2. **Puente directo teoría-laboratorio:** Vinculación permanente con el simulador **Logisim-evolution**, circuitos integrados de la familia **TTL 74LS** y la tarjeta **Basys 3**.
3. **Resolución íntegra de talleres:** Cada reto, cuestionario y taller grupal planteado en clase cuenta con su desarrollo paso a paso y comprobación por múltiples métodos (álgebra, tablas y mapas).

---

## 📚 Mapa de Contenidos por Semanas

| Semana | Eje Temático Principal | Conceptos Clave Desarrollados | Entregables y Prácticas | Enlace Directo |
| :---: | :--- | :--- | :--- | :---: |
| **01** | **Sistemas Numéricos, Códigos y Compuertas** | • Señales binarias y margen de ruido (74LS)<br>• Conversión Binario, Octal y Hexadecimal<br>• Complemento a dos y desbordamiento<br>• Códigos BCD, Gray y ASCII<br>• Las 7 compuertas lógicas y chip `74LS08` | • Reto de conversión resuelto<br>• Actividad 2 resuelta (Alarma, luces, bomba)<br>• Preparación práctica APE 01 | **[Abrir Semana 1 ➡️](https://www.google.com/search?q=./semana-1.md)** |
| **02** | **Álgebra de Boole y Formas Canónicas** | • Equivalencia circuito = expresión booleana<br>• Postulados de Huntington y Principio de Dualidad<br>• Teoremas de absorción, adyacencia y consenso<br>• Leyes de De Morgan demostradas<br>• Minitérminos ($m_i$), Maxitérminos ($M_i$), SOP y POS | • Caso Alarma de 3 sensores (ahorro del 50% de CI)<br>• Ticket de salida resuelto<br>• Taller grupal resuelto: G1-G8 (Luz, Primos, Bomba, Cinturón) | **[Abrir Semana 2 ➡️](https://www.google.com/search?q=./semana-2.md)** |
| **03** | **Mapas de Karnaugh, Indiferentes y NAND/NOR**<br>*(Sesiones 1 y 2 Unificadas)* | • Adyacencia lógica y topología toroidal en 3 y 4 variables<br>• Implicantes primos y primos esenciales<br>• Minimización en SOP y POS<br>• Condiciones indiferentes (*Don't Care* `X`)<br>• Redes universales `NAND-NAND` y `NOR-NOR`<br>• Lanzamiento del Proyecto Integrador | • Talleres T1 a T4 resueltos paso a paso<br>• Diagnóstico y corrección de errores en K-maps<br>• Detector BCD $\ge 5$ con 5 literales<br>• Especificación para FPGA Digilent Basys 3 | **[Abrir Semana 3 ➡️](https://www.google.com/search?q=./semana-3.md)** |
| **04** | **Circuitos Aritméticos y Arquitectura ALU** | • Suma binaria columna por columna<br>• Diseño del Semisumador y Sumador Completo<br>• Sumador en cascada de 4 bits (*Ripple Carry*)<br>• Estimación analítica del retardo de propagación<br>• Sumador/Restador unificado con inversor XOR<br>• Detección de *overflow* con signo ($V = C_3 \oplus C_4$)<br>• Comparador de magnitud e integración en ALU | • Demostración de `0101 + 0011` y análisis de signo<br>• Taller de 4 ejercicios resuelto (Suma, resta con $M=1$, retardo de 16 bits y comparador)<br>• Cierre conceptual de la Unidad 1 | **[Abrir Semana 4 ➡️](https://www.google.com/search?q=./semana-4.md)** |

---

## 🛠️ Entorno de Laboratorio y Tecnologías

```text
  [ Logisim-evolution ] ──► Simulación y verificación esquemática funcional (Simbología ANSI)
            │
            ▼
  [ Familia TTL 74LS  ] ──► Montaje en protoboard (Chips 7400, 7404, 7408, 7432, 7483, 7485)
            │
            ▼
  [ FPGA Digilent Basys 3 ] ─► Proyecto Integrador: 16 switches, 5 pulsadores, 16 LEDs, 4 displays
```

---

## 🗂️ Organización del Repositorio

```bash
📦 Cuaderno-de-Estudio
 ┣ 📜 README.md       # Portada institucional, presentación pedagógica e índice general
 ┣ 📘 semana-1.md     # Guía Semana 1: Sistemas numéricos, códigos y compuertas lógicas
 ┣ 📗 semana-2.md     # Guía Semana 2: Álgebra de Boole, formas canónicas y simplificación
 ┣ 📙 semana-3.md     # Guía Semana 3: Mapas de Karnaugh, indiferentes y compuertas NAND/NOR
 ┗ 📕 semana-4.md     # Guía Semana 4: Circuitos aritméticos combinacionales y ALU
```

---

## ✍️ Autoría y Créditos Académicos

* **Elaboración y desarrollo de ejercicios:** [Luis Anderson Carranza Quispe](https://www.google.com/search?q=https://github.com/luiscarranza019)  
* **Asignatura y cátedra docente:** Ing. Luis Darío Shinche Cueva, Mg.  
* **Institución:** Universidad Nacional de Loja (UNL) · Carrera de Computación

---

### 🚀 ¿Listo para explorar el contenido?

| 🏠 Portada | 📘 Semana 1 | 📗 Semana 2 | 📙 Semana 3 | 📕 Semana 4 |
| :---: | :---: | :---: | :---: | :---: |
| **[Volver arriba](#-cuaderno-digital-diseño-de-circuitos)** | **[Ir a Semana 1](https://www.google.com/search?q=./semana-1.md)** | **[Ir a Semana 2](https://www.google.com/search?q=./semana-2.md)** | **[Ir a Semana 3](https://www.google.com/search?q=./semana-3.md)** | **[Ir a Semana 4](https://www.google.com/search?q=./semana-4.md)** |
