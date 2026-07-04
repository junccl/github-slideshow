# Bitácora de Diagnósticos OBD-II — Toyota Camry 2006 (2.4L Automático)

Registro de lecturas de escáner (DTCs) del vehículo, con interpretación y acciones recomendadas/realizadas.

---

## 2026-07-04

- **Herramienta:** Car Scanner ELM OBD2 v2.1.46 (iOS)
- **Perfil de conexión:** Toyota OBD-II / EOBD
- **Código:** P1135 [0x1135]
- **Descripción (Toyota):** A/F Sensor Heater Circuit Response Malfunction (Bank 1, Sensor 1)
- **Estatus:** Confirmada
- **ECU:** ECU ID 10, ECU Address $6A

### Interpretación

Código específico de Toyota (no genérico SAE). Indica que la ECU detectó una respuesta anómala en el circuito calentador del sensor A/F (sensor de oxígeno de banda ancha) del banco 1, sensor 1 — el ubicado antes del catalizador, cerca del múltiple de escape.

> Nota: la app también muestra una definición genérica alterna ("Pedal Position Sensor A Circuit Intermittent") que **no aplica** a este vehículo; con perfil Toyota, la definición correcta es la del calentador del sensor A/F.

### Causas probables

- Conector/arnés del sensor A/F con corrosión o falso contacto (zona de mucho calor)
- Fusible del calentador de sensores O2 en mal estado
- Elemento calentador del sensor A/F degradado
- Sensor A/F próximo a falla total

### Acción recomendada

1. Inspeccionar visualmente conector y cableado del sensor A/F (banco 1, sensor 1)
2. Si el cableado está en buen estado, reemplazar el sensor A/F
3. Borrar el código y verificar que no reaparezca tras un ciclo de manejo

### Estado

- [ ] Pendiente de revisión/reparación

---

## 2026-07-04 (síntoma observado, sin código asociado)

- **Síntoma:** la aguja del medidor de temperatura del tablero apenas sube, no llega a su posición normal (~mitad del rango)
- **Código relacionado:** ninguno reportado aún (posible P0128 no confirmado por escáner)

### Interpretación

Síntoma clásico de **termostato pegado en posición abierta**: el refrigerante circula constantemente por el radiador y el motor no alcanza su temperatura normal de operación (~90°C).

### Consecuencias esperadas

- Mayor consumo de combustible (ECU permanece más tiempo en "motor frío"/loop abierto)
- Calefacción sopla tibio en vez de caliente
- Combustión menos eficiente, mayor desgaste en frío
- Posible aparición futura de P0128 (Coolant Thermostat - Below Regulating Temperature)

### Acción recomendada

1. Manejar 15-20 min y observar si la aguja se estabiliza o permanece baja
2. Revisar con el escáner si aparece P0128
3. Con motor tibio, verificar temperatura de manguera superior del radiador (si está caliente con aguja baja, el termostato no cierra bien)
4. Reemplazar termostato si se confirma el diagnóstico

### Relación con P1135

No es causa directa, pero un motor que no alcanza temperatura normal puede prolongar el loop abierto de la ECU, lo que a veces retrasa la confirmación de fallas en el sensor A/F. Se atienden como problemas independientes.

### Estado

- [ ] Pendiente de confirmación (falta verificar P0128 y temperatura de manguera)

---

## 2026-07-04 (análisis cruzado: informes del vehículo + diagnósticos)

Cruce entre el Informe Autofact / Registro Civil (`FICHA_VEHICULO.md`) y las lecturas de escáner registradas arriba.

### Kilometraje estimado actual

- Último registro oficial: 125.572 km (04-12-2024, revisión técnica)
- Uso histórico promedio: ~6.200 km/año ("bajo")
- **Estimado a julio 2026: ~135.000-138.000 km**

### Relevancia para el P1135 y el termostato

A este kilometraje, tanto el sensor A/F como el termostato son piezas originales del vehículo (fabricado en 2006, sin registro de reemplazo en los informes). Es el rango típico donde ambos componentes empiezan a fallar por desgaste — coincide con lo diagnosticado, no es casualidad que aparezcan cerca uno del otro.

### ⚠️ Plazo importante: Revisión Técnica

- **Mes de renovación: Noviembre** (según Informe Autofact)
- El ítem **GA (emisión de gases)** de la revisión técnica puede verse afectado por una mezcla aire-combustible mal calibrada — justamente lo que controla el sensor A/F fallando (P1135)
- Historial de revisión técnica del vehículo es mayormente limpio (rechazos previos solo por IV/luces, y frenos una vez en 2018, ya resueltos) — nunca ha fallado por emisiones, por lo que conviene resolver el P1135 **antes de noviembre** para no interrumpir ese historial

### Situación legal/administrativa (sin relación con lo mecánico, para descartar sorpresas)

Sin choques ni remates registrados, sin encargo por robo, sin multas, sin limitaciones al dominio. El presupuesto puede enfocarse 100% en mecánica.

### Estado

- [ ] Pendiente: reparar sensor A/F y confirmar termostato antes de noviembre 2026 (revisión técnica)

---

<!-- Agregar nuevas entradas arriba de esta línea, con el formato: fecha, herramienta, código(s), interpretación, acción y estado -->
