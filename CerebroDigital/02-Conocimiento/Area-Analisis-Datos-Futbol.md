---
fecha: 2026-05-12
hora: 19:30
fuente: usuario
tags: [area-de-interes, futbol, datos, ml, ia-no-generativa, pasion]
estado: area-de-interes-fuerte
---

# Área de Interés — Análisis de Datos Futbolísticos

**Pasión declarada.** No es hobby, no es proyecto aún — es área donde **identidad personal + identidad técnica convergen** y donde a futuro va a haber producto.

## Qué te atrae específicamente

> "Que una IA **no generativa** los analice y genere informes y resultados para equipos de fútbol."

Subrayado importante: **IA NO generativa**. No LLMs, no ChatGPT-para-fútbol. ML clásico, estadística, modelos predictivos, análisis cuantitativo.

## Por qué encaja con vos perfecto

| Pieza de vos | Cómo conecta |
|--------------|--------------|
| [[Hincha-de-River]] | Análisis táctico profundo ya lo hacés mentalmente. Sistematizarlo es el paso siguiente. |
| [[Lifestyle-e-Intereses\|Football Manager]] | Estás entrenado en pensar fútbol como sistema de variables. FM es básicamente un simulador ML. |
| [[Perfil-Profesional\|Project Orchestrator]] | Pipeline de datos → modelos → informes = orquestación pura |
| [[Stack-Tecnologico-Lenguajes\|Python]] | Ecosistema ML/stats (scikit-learn, pandas, statsmodels, XGBoost) |
| [[Stack-Tecnologico-Lenguajes\|Go]] | Ingesta de datos en tiempo real si entrás a partidos en vivo |
| [[Expertise-Tecnica\|Clean / Hexagonal]] | Separar dominio (fútbol) de infra (ingesta/modelos) es justo tu fortaleza |

**Esto no es "otra idea más". Es un proyecto que tu identidad reclama.**

## Vertical elegida primero ⭐

> **¿Qué formación haría jugar mejor a River?**

Pregunta concreta definida. Ver [[Pregunta-Tactica-Optima-River]] con el modelo completo, datos, stack y MVP de 4 semanas.

## Espacio de problemas posibles

Cada uno es un producto potencial:

### Scouting / valoración de jugadores
Modelos que detectan talento subvaluado a partir de stats. Como Moneyball pero para fútbol. Mercado: clubes con presupuesto limitado.

### Análisis táctico post-partido
Procesar datos de un partido → informe automatizado al cuerpo técnico. Movimientos, espacios, presión, transiciones.

### Predicción de resultado / desempeño
Modelos probabilísticos para apuestas (B2C) o para fantasy (B2C/B2B).

### Optimización de plantel
Dado un presupuesto, ¿cuál es el roster óptimo para tu sistema de juego?

### Detección de lesiones
A partir de carga física + datos de wearables, predecir riesgo de lesión.

### xG (Expected Goals) y métricas avanzadas
Calcular y vender métricas avanzadas a clubes / medios.

## Qué hace "IA no generativa" interesante acá

- **Modelos tienen explicación**: un cuerpo técnico puede confiar en "este modelo dice X **porque** estas variables". LLMs no dan eso.
- **Reproducibles**: mismo input → mismo output. Crítico cuando hay dinero o decisiones de carrera en juego.
- **Más barato a escala**: una inferencia de XGBoost cuesta milicentavos vs un prompt de Claude.
- **Compliance más simple**: no estás generando contenido, estás analizando — menos zona gris regulatoria.

## Stack probable

- **Ingesta**: Go (alto throughput de datos de partidos en vivo)
- **Procesamiento / modelos**: Python (pandas, scikit-learn, XGBoost, LightGBM, posibles redes neuronales para visión si entrás a video)
- **Storage**: PostgreSQL + TimescaleDB para series temporales, posible Parquet para datasets grandes
- **Visualización / informes**: ¿Streamlit para prototipos? ¿Custom dashboards después?
- **Fuente de datos**: ⚠️ **decisión grande** — propia (scraping, partners) vs proveedor pago (StatsBomb, Wyscout, Opta)

## Notas abiertas

- ¿B2B (vendés a clubes) o B2C (vendés a hinchas/aficionados que aman analizar)?
- ¿Empezás por scouting? ¿Por análisis post-partido? Una vertical clara primero.
- Mercado argentino primero (más accesible, vínculos con River?) vs internacional (más plata, más competencia)
- ¿Hay overlap con [[BlendPadel]] que está pausado? Mismo músculo de "deporte + matemática" pero distinto deporte.
- Costo de datos profesionales: StatsBomb, Wyscout cobran caro. ¿Hay vía gratuita? (StatsBomb tiene dataset open para empezar)

## Próximos movimientos sugeridos

> No te apures a codear. Aplicá tu propia regla [[Filosofia-de-Trabajo|conceptos > código]].

1. Definir **una pregunta** que un cuerpo técnico pagaría por responder
2. Buscar si esa pregunta ya tiene producto en el mercado (validación)
3. Buscar el dataset mínimo viable que la responda
4. Recién después → modelo + producto
