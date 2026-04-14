# Simulador CPU RISC-V — Pipeline de 5 Etapas

Simulador interactivo de una CPU RISC-V con pipeline de 5 etapas implementado en HTML/CSS/JS puro, sin dependencias externas.

## Demo

Abre `riscv_pipeline_simulator.html` directamente en tu navegador (no requiere servidor).

---

## Características

- Pipeline de 5 etapas: **IF → ID → EX → MEM → WB**
- Registros de pipeline entre etapas (IF/ID, ID/EX, EX/MEM, MEM/WB)
- Detección y resolución de **data hazards** mediante **forwarding**
- Detección de **load-use hazard** con inserción de **stall (burbuja)**
- Detección de **branch hazard** con **flush** del pipeline
- Diagrama de tiempo acumulado ciclo a ciclo
- Register File visible (x0–x15) con resaltado al escribir
- Estadísticas en tiempo real: ciclos, instrucciones completadas, stalls, CPI
- Log de eventos con clasificación por tipo
- Soporte para modo oscuro automático
- 4 programas de demostración seleccionables

---

## Programas incluidos

| # | Programa | Concepto demostrado |
|---|----------|---------------------|
| 1 | Suma secuencial | Pipeline ideal, CPI = 1 |
| 2 | Data hazard + forwarding | Forwarding EX→EX y MEM→EX |
| 3 | Load-use hazard (stall) | Burbuja obligatoria, CPI > 1 |
| 4 | Branch hazard (flush) | Flush de instrucciones inválidas |

---

## Cómo usar

```
1. Abrir riscv_pipeline_simulator.html en el navegador
2. Seleccionar un programa del menú desplegable
3. Presionar "Siguiente ciclo" para avanzar paso a paso
   o "Ejecutar todo" para ver el resultado final
4. Observar el diagrama de tiempo, el Register File y el log
5. Presionar "Reiniciar" para volver al ciclo 0
```

---

## Estructura del proyecto

```
riscv-pipeline-simulator/
├── riscv_pipeline_simulator.html   # Simulador completo (todo en un archivo)
└── README.md
```

---

## Conceptos implementados

### Etapas del pipeline

| Etapa | Función |
|-------|---------|
| IF | Busca la instrucción en memoria usando el PC |
| ID | Decodifica la instrucción y lee el Register File |
| EX | Ejecuta la operación en la ALU |
| MEM | Accede a memoria de datos (solo LW/SW) |
| WB | Escribe el resultado de vuelta en el Register File |

### Tipos de hazards

**Data hazard** — una instrucción necesita un valor que aún no llegó a WB.  
Solución: Forwarding unit redirige el dato directamente desde EX/MEM o MEM/WB hacia la entrada de la ALU.

**Load-use hazard** — un `lw` no puede hacer forwarding porque el dato no está listo hasta el final de MEM.  
Solución: Se inserta una burbuja (NOP) en la etapa EX y se congela IF e ID un ciclo.

**Branch hazard** — en un `beq` tomado, las instrucciones que entraron al pipeline después son incorrectas.  
Solución: Se hace flush (descarte) de las instrucciones en IF e ID.

---

## Tecnologías

- HTML5 / CSS3 / JavaScript (ES6+)
- Sin frameworks ni dependencias externas
- Compatible con todos los navegadores modernos
- Soporte automático para modo oscuro (`prefers-color-scheme`)

---

## Materia

Arquitectura de Computadoras — Proyecto: Diseño de CPU Pipelined RISC-V
