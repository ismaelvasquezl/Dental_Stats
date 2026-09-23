# Análisis de producción odontológica APS — DATA SALUD Coquimbo

## Descripción

Análisis de la producción odontológica de enero a agosto de 2026 cruzada con la población inscrita (percápita FONASA validado) de CESFAM Tierras Blancas y CESFAM Lila Cortés. Incluye un dashboard interactivo (Streamlit y respaldo HTML), un informe ejecutivo, una presentación y el código reproducible.

## Objetivos

- Medir producción, inasistencia y telesalud por centro, profesional y período.
- Describir la demanda horaria, separando la hora real de las horas por defecto del registro.
- Calcular cobertura y uso por grupo etario con el percápita.
- Detectar problemas de calidad del registro que limitan la gestión.
- Entregar propuestas priorizadas y un plan a 90 días.

## Fuentes de datos

1. **AVIS LATAM**: base de monitoreo diario de producción odontológica, anonimizada (una fila por diagnóstico o prestación).
2. **Percápita FONASA validado 2026** (DATA SALUD Coquimbo): población por edad simple y sexo, Tierras Blancas (43.234) y Lila Cortés (20.539).

## Estructura del proyecto

```
├── datos/
│   ├── BD_ODONTOLOGIA_PRODUCCION_2026_ANONIMIZADO.xlsx
│   ├── Percapita_Lila_Cortes_2026.xlsx
│   └── Percapita_Tierras_Blancas_2026.xlsx
├── src/
│   ├── procesamiento.py              # carga, limpieza, encuentros, KPIs (módulo compartido)
│   ├── app_dashboard_odontologia.py  # dashboard Streamlit (9 pestañas)
│   ├── analisis_completo.py          # genera tablas CSV y el dashboard HTML de respaldo
│   └── plantilla_dashboard.html      # plantilla del respaldo HTML
├── output/
│   ├── informe_ejecutivo.html
│   ├── dashboard_odontologia_interactivo.html
│   ├── presentacion_ejecutiva.md
│   └── tablas_*.csv
├── requirements.txt
└── README.md
```

Los archivos de `datos/` se buscan por nombre: el de producción debe contener `ODONTO`, y los percápita `Tierras` y `Lila`.

## Instalación y ejecución

Requiere Python 3.10 o superior.

```bash
python -m venv .venv
# Windows
.venv\Scripts\activate
# macOS / Linux
source .venv/bin/activate

pip install -r requirements.txt

# 1) Dashboard interactivo (abre http://localhost:8501)
streamlit run src/app_dashboard_odontologia.py

# 2) Regenerar tablas y dashboard HTML de respaldo en output/
python src/analisis_completo.py
```

El respaldo `output/dashboard_odontologia_interactivo.html` se abre con doble clic, sin servidor. Necesita internet para cargar Chart.js y las fuentes.

## Actualización mensual

1. Reemplazar la base de producción en `datos/` por la exportación acumulada del año.
2. Actualizar los percápita si cambió el corte.
3. Ejecutar `python src/analisis_completo.py` y revisar la consola (encuentros, inasistencia, hora por defecto).
4. Abrir el dashboard y revisar la pestaña **Calidad y alertas** antes de interpretar.

El informe ejecutivo y la presentación son un corte a agosto de 2026: sus cifras están escritas en el documento y no se regeneran solas.

## Reglas de negocio

| Regla | Definición | Motivo |
|-------|-----------|--------|
| Encuentro | Profesional + paciente + fecha + hora de atención únicos | Cada fila es un diagnóstico o una prestación; contar filas infla ~4,5 veces |
| Tipo de encuentro | Sembrando Sonrisas > Inasistencia > Telesalud > Presencial | Un encuentro con fila “Inasistencia a consulta” no es producción |
| Encuentro efectivo | Presencial o telesalud | Base de producción y cobertura |
| Telesalud | Comentario de agenda contiene “TELESALUD” | Única marca disponible; puede subregistrar |
| Hora por defecto | 12:00:00 o 19:00:00 exactas | Concentran 43 % de los encuentros; no son horas reales |
| Inasistencia % | Inasistencias / (inasistencias + efectivos) | Sembrando Sonrisas se excluye del denominador |
| Cobertura | Pacientes distintos efectivos / inscritos percápita | Por centro de inscripción del paciente |
| Grupos etarios | 0–4, 5–9, 10–14, 15–19, 20–39, 40–59, 60+ | Edad en años al momento del encuentro |
| Franja | Mañana 8–13 h, tarde 14–18 h, otro | Según hora registrada |

## Limitaciones conocidas

- **Centro del paciente ≠ centro de atención.** La base parece corresponder a un solo servicio dental (86 % de las filas son pacientes de Tierras Blancas). La cobertura de Lila Cortés está subestimada; se necesita su propia base de producción.
- **Duración no válida.** “Hora Cierre Atención” es el cierre del registro. La duración derivada solo se usa como alerta de calidad.
- **“Cantidad” en Sembrando Sonrisas** trae totales de establecimiento repetidos por niño; no se suma.
- **Sin denominadores de programas** (gestantes bajo control, 6 años, 60 años): no se calcula cumplimiento GES ni de metas. Fuente sugerida: REM Serie P y SIGGES.
- **“Programa asociado”** está vacío en 94 % de las filas y “Género social” es mayoritariamente “no revelado”.
- **Proyección** en el simulador: tendencia lineal con 8 meses, sin estacionalidad; es un orden de magnitud.

## Ética y privacidad

Los datos llegan anonimizados (profesionales y pacientes codificados). El análisis es agregado y no intenta re-identificar a nadie (Ley 19.628). Los indicadores por profesional están pensados para planificar agenda y focalizar capacitación, no para sancionar. No publique el dashboard con la base de pacientes a personas fuera del equipo autorizado.

## Validación

- Los totales del respaldo HTML se verificaron contra el pipeline Python (mismos encuentros, prestaciones y percápita).
- El dashboard Streamlit se probó con `streamlit.testing` (carga y combinaciones de filtros sin errores).
