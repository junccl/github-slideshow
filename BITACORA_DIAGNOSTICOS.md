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

<!-- Agregar nuevas entradas arriba de esta línea, con el formato: fecha, herramienta, código(s), interpretación, acción y estado -->
