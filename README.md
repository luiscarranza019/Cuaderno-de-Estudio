
---
<div align="center">

# UNIVERSIDAD NACIONAL DE LOJA

</div>

### Facultad de la Energía, las Industrias y los Recursos Naturales No Renovables
#### Carrera de Computación · Segundo Ciclo «A»

---

## ⚡ Cuaderno Digital: Diseño de Circuitos
> **Unidad 1:** Análisis de lógica combinacional con diseño clásico  
> **Período Académico:** Septiembre 2026 – Marzo 2027

---

## 🏛️ Ficha Técnica Institucional

| Parámetro | Detalle Académico |
| :--- | :--- |
| **Institución** | Universidad Nacional de Loja (UNL) |
| **Facultad** | Facultad de la Energía, las Industrias y los Recursos Naturales No Renovables |
| **Carrera** | Carrera de Computación |
| **Ciclo y Paralelo** | Segundo Ciclo «A» |
| **Asignatura** | Diseño de Circuitos |
| **Docente de Cátedra** | Ing. Luis Darío Shinche Cueva, Mg. |
| **Estudiante / Autor** | **Luis Anderson Carranza Quispe** |
| **Resultado de Aprendizaje (R1)** | Diseña e implementa circuitos combinacionales básicos utilizando técnicas de diseño clásico. |

---

## 💡 Sobre esta Guía Digital

> Este cuaderno recopila y profundiza los aprendizajes de las primeras cuatro semanas de clase, redactado en **primera persona** con un enfoque deductivo: desde el comportamiento físico de los voltajes y el ruido eléctrico hasta la formulación matemática y el diseño de la **Unidad Aritmético-Lógica (ALU)**.

### 🎯 Enfoque Metodológico
* **Rigor y claridad:** Conceptos formales explicados de forma intuitiva, estructurada y aplicada.
* **Resolución íntegra:** Procedimientos detallados de todos los retos, talleres guiados y tickets de salida.
* **Proyección a hardware:** Conexión constante con simulación en **Logisim-evolution**, circuitos integrados **TTL 74LS** y la tarjeta FPGA **Digilent Basys 3**.

---

## 📚 Índice de Contenidos

| Semana | Eje Temático | Contenidos Principales | Acceso |
| :---: | :--- | :--- | :---: |
| **01** | **Sistemas Numéricos, Códigos y Compuertas** | Niveles de voltaje en 74LS, margen de ruido, conversiones (Bin/Oct/Hex), complemento a dos, desbordamiento, códigos BCD, Gray, ASCII, las 7 compuertas lógicas y chip 74LS08. | **[Semana 1 ➔](./semana-1.md)** |
| **02** | **Álgebra de Boole y Formas Canónicas** | Equivalencia circuito-expresión, postulados de Huntington, principio de dualidad, teoremas booleanos, leyes de De Morgan, minitérminos ($m_i$), maxitérminos ($M_i$), SOP, POS y costo en CI. | **[Semana 2 ➔](./semana-2.md)** |
| **03** | **Mapas de Karnaugh, Indiferentes y NAND/NOR** | Adyacencia y orden Gray, mapas de 3 y 4 variables, implicantes primos esenciales, condiciones indiferentes (*don't care* `X`), redes universales NAND/NOR y proyecto Basys 3. | **[Semana 3 ➔](./semana-3.md)** |
| **04** | **Circuitos Aritméticos y Arquitectura ALU** | Suma por columnas, diseño de semisumador y sumador completo, sumador en cascada de 4 bits (*ripple carry*), retardo de propagación, resta con XOR, detección de *overflow* y comparador. | **[Semana 4 ➔](./semana-4.md)** |

---

## 🛠️ Herramientas de Trabajo

```text
[ Logisim-evolution ] ──► Simulación lógica esquemática (Norma ANSI/IEEE)
          │
          ▼
[ Familia TTL 74LS  ] ──► Validación física en protoboard (7400, 7404, 7408, 7432, 7483, 7485)
          │
          ▼
[ FPGA Basys 3      ] ──► Implementación del Proyecto Integrador (16 SW, 5 BTN, 16 LED, 4 Display)
```

---

## 📂 Estructura de Archivos

```bash
📦 Cuaderno-de-Estudio
 ┣ 📜 README.md       # Portada institucional e índice general
 ┣ 📘 semana-1.md     # Sistemas numéricos, códigos y compuertas lógicas
 ┣ 📗 semana-2.md     # Álgebra de Boole, formas canónicas y simplificación
 ┣ 📙 semana-3.md     # Mapas de Karnaugh, condiciones indiferentes y NAND/NOR
 ┗ 📕 semana-4.md     # Circuitos aritméticos combinacionales y ALU
```

---

## ✒️ Autoría y Créditos

* **Autor:** Luis Anderson Carranza Quispe
* **Docente de Cátedra:** Ing. Luis Darío Shinche Cueva, Mg.
* **Institución:** Universidad Nacional de Loja · Carrera de Computación

---

| 🏠 Inicio | [Comenzar: Semana 1 ➡️](./semana-1.md) |
| :--- | ---: |
