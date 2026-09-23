# Presentación ejecutiva: producción odontológica APS + percápita FONASA

## Diapositiva 1: Portada
- **Análisis de producción odontológica y cobertura poblacional**
- CESFAM Tierras Blancas y CESFAM Lila Cortés
- Período: 1 de enero – 31 de agosto de 2026
- Fecha de presentación: [fecha]
- [Logo institucional]

***

## Diapositiva 2: En 1 minuto

- 🔴 **1 de cada 5 citas se pierde**: inasistencia 18,3 % (27,9 % en la tarde; 26–28 % entre 10 y 19 años).
- 🔴 **La hora registrada no es confiable**: 50 % de la atención presencial figura a las 12:00 o 19:00 exactas.
- 🟡 **Los adultos casi no llegan**: 7 de cada 100 inscritos de 20–59 años atendidos en Tierras Blancas, frente a 54 de cada 100 menores de 5.
- 🟢 **La producción crece**: agosto fue el mejor mes (1.701 encuentros efectivos); 12,1 % de los inscritos de Tierras Blancas atendidos en 8 meses.
- ⚪ **Lila Cortés no es evaluable con esta base**: sus pacientes se atienden en su propio centro.

> Nota del analista: la plantilla original pedía "hora pico crítica", "capacidad ociosa 14–16 h" y "% cumplimiento GES". Ninguno es calculable de forma válida con estas fuentes; se reemplazaron por hallazgos verificables.

***

## Diapositiva 3: Metodología (amigable)

### ¿Qué hicimos?
1. **Cargamos** la base de producción (61.046 filas) y el percápita de ambos centros.
2. **Agrupamos** las filas en 13.697 encuentros: paciente + profesional + fecha + hora. Cada fila era un diagnóstico o una prestación, no una atención.
3. **Clasificamos** cada encuentro: presencial, telesalud, inasistencia o Sembrando Sonrisas.
4. **Revisamos la calidad** del registro antes de interpretar (horas por defecto, duración, cantidades).
5. **Cruzamos** pacientes atendidos con población inscrita por edad.
6. **Priorizamos** propuestas por impacto, viabilidad y costo.

### Herramientas
Python (pandas, plotly), Streamlit, HTML autocontenido con Chart.js.

### Fuentes
- Base de monitoreo diario AVIS LATAM (anonimizada)
- Percápita FONASA validado 2026, DATA SALUD Coquimbo: Tierras Blancas (43.234) y Lila Cortés (20.539)

***

## Diapositiva 4: Horas pico — primero hay que medirlas

### Gráfico
[Distribución horaria con horas por defecto excluidas — pestaña "Demanda y horas pico"]

### Qué vimos
- 3.878 encuentros a las 12:00:00 y 2.022 a las 19:00:00 exactas: son horas por defecto del sistema.
- 632 registros entre 20:00 y 23:59 y 300 en domingo: digitación diferida.
- Sin esas horas: 50 % mañana, 37 % tarde; 10:00, 8:00 y 14:00 concentran 39 % (provisional).
- La duración no es medible: la "hora de cierre" es la del registro (mediana 97 min, valores negativos).

### Propuesta
1. Registrar la hora real (capacitación + bloqueo de hora automática en AVIS).
2. Re-medir en 90 días antes de mover cupos.

***

## Diapositiva 5: Producción por centro del paciente

| Indicador | Tierras Blancas | Lila Cortés* |
|-----------|-----------------|--------------|
| Población inscrita | 43.234 | 20.539 |
| Encuentros efectivos | 9.188 | 550 |
| Pacientes distintos | 5.212 | 413 |
| Inasistencia | 19,8 % | 0,7 % |
| Cobertura | 12,1 % | 2,0 % |
| Encuentros por 1.000 inscritos | 213 | 27 |

\* Solo pacientes de Lila Cortés atendidos en este servicio; no refleja su propio equipo dental.

***

## Diapositiva 6: Cobertura poblacional (Tierras Blancas)

### Gráfico
[Pacientes atendidos por 100 inscritos, por grupo etario — pestaña "Cobertura y percápita"]

| Edad | 0–4 | 5–9 | 10–14 | 15–19 | 20–39 | 40–59 | 60+ |
|------|-----|-----|-------|-------|-------|-------|-----|
| Atendidos por 100 inscritos | 53,9 | 28,2 | 19,2 | 13,2 | 7,5 | 7,0 | 12,1 |

- **Brecha principal**: adultos de 20–59 años (57 % de los inscritos, ~7 %).

***

## Diapositiva 7: Equipo

### Gráfico
[Top 10 profesionales por encuentros efectivos — pestaña "Producción y equipo"]

- 23 códigos de profesional; 17 con 100 o más encuentros.
- Profesional típico: 9,2 encuentros por día activo (rango 4,1–10,6).
- Los 5 con más volumen concentran 61 % de la producción.
- 7 registran la mitad o más de su atención a una hora por defecto → foco de capacitación, no de sanción.

***

## Diapositiva 8: Programas prioritarios

| Programa | Volumen | Meta | Estado |
|----------|---------|------|--------|
| Urgencia GES | 498 consultas | Garantía de acceso | ⚪ Sin denominador |
| Gestantes | 39 atendidas | Según norma | ⚪ Falta gestantes bajo control (REM-P) |
| 60 años / adulto mayor | — | Según norma | ⚪ Falta denominador |
| Sembrando Sonrisas | 1.162 niños | Según convenio | 🟢 Activo |
| Altas integrales enseñanza media | 201 | Según convenio | ⚪ Falta denominador |

***

## Diapositiva 9: Propuestas de mejora

| # | Propuesta | Impacto | Viabilidad | Costo |
|---|-----------|---------|------------|-------|
| 1 | Registro de hora real | Alto | Alta | Bajo |
| 2 | Confirmación de citas 24–48 h (tarde y < 20 años primero) | Alto (~420 encuentros / 8 meses) | Alta | Bajo–medio |
| 3 | Sobreagenda acotada (~5 %) en bloques con inasistencia > 25 % | Medio | Alta | Bajo |
| 4 | Estrategia adultos 20–59 (cupos vespertinos protegidos) | Alto | Media | Medio |
| 5 | Base de Lila Cortés + denominadores REM-P/SIGGES | Alto | Alta | Bajo |
| 6 | Tablero mensual con el pipeline | Medio | Alta | Bajo |

***

## Diapositiva 10: Plan de 90 días

- **Semanas 1–2**: validación con el equipo; solicitud a proveedor AVIS.
- **Semanas 3–4**: capacitación en registro; piloto de confirmación en la tarde.
- **Semanas 5–8**: confirmación en todos los bloques; sobreagenda acotada; solicitud de bases faltantes.
- **Semanas 9–12**: re-medición de horas pico y cobertura; decisión sobre cupos.

### Indicadores de seguimiento
| Indicador | Línea base | Meta 90 días |
|-----------|-----------|--------------|
| Atención presencial con hora por defecto | 50,4 % | < 20 % |
| Inasistencia | 18,3 % | < 15 % |
| Concentración 3 horas pico | 39 % (provisional) | < 40 % con hora real |
| Cobertura 20–59 años (Tierras Blancas) | 7,3 % en 8 meses | Definir con el equipo |
| Satisfacción usuaria | Sin medición | Instalar medición |

***

## Diapositiva 11: Próximos pasos

### Esta semana
1. [ ] Reunión con equipo de odontología
2. [ ] Enviar solicitud al proveedor AVIS sobre hora por defecto
3. [ ] Definir responsable del piloto de confirmación de citas
4. [ ] Programar seguimiento a 30 días

### Contacto
- Analista: [nombre]
- Correo: [correo]
- Teléfono: [teléfono]
