---
fecha: 2026-05-12
hora: 20:10
fuente: usuario + conceptualizacion claude-code
tags: [futbol, river, tactica, formaciones, ml, problema-definido, mvp]
area: Area-Analisis-Datos-Futbol
estado: pregunta-definida
---

# Pregunta — ¿Qué formación haría jugar mejor a River?

**Pregunta concreta declarada como vertical inicial de [[Area-Analisis-Datos-Futbol]]:**

> Dado el plantel actual de River Plate, **¿qué formación maximiza su rendimiento?**

Esta es **excelente pregunta de entrada** porque:

1. Es **definida** (no abierta tipo "analicemos fútbol")
2. Tiene **output medible** (un ranking de formaciones)
3. Tenés **dominio profundo** (hincha + Football Manager 24 + análisis táctico mental constante)
4. **Hay dataset accesible** (FBref, StatsBomb open data, Transfermarkt)
5. **No requiere clientes** para validar — vos sos el primer evaluador

## Función objetivo — DEFINIDA ✅

> **Maximizar xG creado vía dominio absoluto.**

No es solo "crear chances". Es **crear chances dominando** — fútbol propositivo, no reactivo. Esto es **filosofía Guardiola/Bielsa/Klopp**, no contraataque pragmático.

Ver [[Concepto-Dominio-Futbol]] para el modelo completo de qué significa "dominio" en analytics modernos.

### Traducción a métricas

| Componente | Métrica | Cómo se calcula | Peso inicial |
|------------|---------|------------------|--------------|
| Crear peligro | **xG creado** | Suma de xG generado por el equipo | 0.40 |
| Dominar territorio | **Field Tilt** | % pases en último tercio rival vs propio | 0.20 |
| Presionar alto | **PPDA** (Passes Per Defensive Action) — invertido | Cuanto más bajo, mejor | 0.20 |
| Estar en el área | **Touches in opp box** | Toques en el área rival por 90' | 0.20 |

**Función objetivo combinada (MVP):**

```
score(formación) = 0.40·norm(xG_creado)
                 + 0.20·norm(field_tilt)
                 + 0.20·norm(1/PPDA)
                 + 0.20·norm(touches_in_box)
```

Cada métrica normalizada 0-1 para que los pesos tengan sentido.

### Restricción del espacio de formaciones

Esta función objetivo **excluye** sistemas reactivos. No tiene sentido probar **5-4-1** (contragolpe puro) ni **4-4-2 directo** (juego vertical sin construcción).

Formaciones que sí se prueban (compatibles con dominio):
- **4-3-3** (Guardiola, falso 9 incluido como variante)
- **4-2-3-1** (con doble pivote constructor)
- **3-4-3** / **3-2-4-1** (build-up con línea de 3)
- **4-1-4-1** (pivote único defensivo, dos pivotes ofensivos)
- **3-1-4-2** (versión Bielsa)

Esto **reduce el espacio de búsqueda** y respeta la identidad táctica del producto.

## Modelo conceptual

```
INPUT
├── Roster River (jugador → atributos: stats temporada, perfil físico, edad)
├── Formación candidata (ej: 4-3-3, 4-2-3-1, 3-5-2)
└── Función "mejor" elegida

PROCESO
├── 1. Para cada formación, definir slots posicionales (ej: 4-3-3 = GK + 2CB + 2FB + 3CM + 2W + 1ST)
├── 2. Calcular fit jugador-slot (rating posicional)
├── 3. Asignación óptima (problema clásico: Hungarian algorithm)
├── 4. Calcular métrica de "mejor" para el equipo resultante
└── 5. Ranking de formaciones

OUTPUT
└── Top-N formaciones para el roster, con detalle de lineup óptimo
```

## Datos necesarios (MVP)

| Fuente | Qué da | Costo | Cómo obtenerlo |
|--------|--------|-------|----------------|
| **FBref** | Stats agregadas por jugador, por temporada | Free | Scraping con `soccerdata` (Python) |
| **StatsBomb Open Data** | Datos de eventos a nivel partido (limitado a competiciones específicas) | Free | API pública |
| **Transfermarkt** | Roster, valor de mercado, posiciones | Free (scraping) | Scraping cuidadoso |
| **Understat** | xG por partido y jugador | Free | API pública |

**Para River específicamente**: campeonato argentino + Libertadores. FBref + Understat lo cubren razonable.

## Stack propuesto

- **Ingesta**: Python (`soccerdata`, `requests-html`, `beautifulsoup`)
- **Storage**: PostgreSQL con tabla por entidad (jugador, partido, evento) + Parquet para datasets grandes
- **Modelado**:
  - Rating jugador-posición: empezar con **heurística explicable** (peso de stats por posición), no ML al toque
  - Optimización: `scipy.optimize.linear_sum_assignment` (Hungarian)
  - Métrica de equipo: combinar ratings + ajuste por sinergia
- **Visualización**: Streamlit para prototipo, después dashboard custom si vale la pena
- **Lenguaje**: 100% Python en MVP. [[Stack-Tecnologico-Lenguajes|Go]] aparece si entrás a ingesta en vivo

## MVP — 4 semanas

| Semana | Entregable |
|--------|------------|
| 1 | Pipeline de ingesta: roster actual de River + stats temporada FBref + Understat |
| 2 | Modelo de rating jugador-posición (heurística con pesos transparentes) |
| 3 | Optimización: dada una formación, lineup óptimo. Comparar 3-4 formaciones manualmente |
| 4 | Loop: probar las 8-10 formaciones más comunes, ranking automático |

**Criterio de éxito MVP**: que el output **sorprenda a vos**. Si el modelo dice "River juega mejor con 3-5-2" y eso te llama la atención + podés justificarlo viendo los números → ganaste.

## Riesgos a vigilar

- **Sesgo del dato**: stats temporada esconden contexto (lesiones, rivales, sistema previo)
- **Falsa precisión**: el modelo puede ser preciso sin ser válido. Validar con sentido común de hincha
- **Sobre-ingeniería**: no metas ML hasta que la heurística falle
- **Scope creep**: NO trates de modelar al rival aún. Solo River vs sí mismo

## Por qué esto es **MVP comercial** sin pretenderlo

Si esto funciona razonable, ya tenés:
- Un servicio que un **cuerpo técnico aficionado / cancha 5 / categorías formativas** podría usar
- Un **producto de contenido** para Twitter/YouTube (análisis táctico de River con datos)
- Una **demo viva** para mostrar a clubes B2B

No empieces pensando en producto. Empezá respondiéndote a vos. La pregunta concreta que te morís por responder ES el MVP.

## Conexión con el resto del cerebro

- [[Area-Analisis-Datos-Futbol]] — la nota padre
- [[Hincha-de-River]] — el motivo emocional
- [[Lifestyle-e-Intereses]] — Football Manager 24 es la inspiración del modelo
- [[Stack-Tecnologico-Lenguajes]] — Python ML para MVP
- [[Filosofia-de-Trabajo]] — empezar por la heurística simple antes que ML complejo
- [[Workflows-SDD-OPSX]] — antes de codear, escribir spec técnica + roadmap, como siempre
