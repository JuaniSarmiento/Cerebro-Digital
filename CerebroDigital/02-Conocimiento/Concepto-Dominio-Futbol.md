---
fecha: 2026-05-12
hora: 20:30
fuente: usuario + conceptualizacion claude-code
tags: [futbol, tactica, concepto, dominio, analytics, filosofia-juego]
area: Area-Analisis-Datos-Futbol
---

# Concepto — Dominio en Fútbol Moderno

Pilar conceptual para [[Area-Analisis-Datos-Futbol]] y específicamente para [[Pregunta-Tactica-Optima-River]].

## El malentendido común

> "Dominio = posesión alta."

**Falso.** Eso es lo que mira el público. El análisis serio lo mide distinto.

Equipos como el Barça de Guardiola, el Bayern de Heynckes y el Liverpool de Klopp dominaban — pero con **mecanismos diferentes**. La posesión es un **síntoma**, no el dominio mismo.

## Las 4 dimensiones del dominio

### 1. Territorio

¿Dónde se juega el partido? Si la pelota vive en mitad de cancha rival, dominás aunque tengas 45% de posesión.

**Métrica**: **Field Tilt** = pases en último tercio rival / (pases en último tercio rival + pases en último tercio propio).

> Field tilt alto = territorio dominado, independiente de la posesión.

### 2. Presión

¿Cuánto tarda tu equipo en recuperar tras perder la pelota? Si la recuperás rápido, no le das al rival respiro para organizarse.

**Métrica**: **PPDA** (Passes Per Defensive Action) = pases que permitís al rival antes de hacer una acción defensiva.

> Bajo PPDA = presión alta = no dejás respirar.

Klopp llamaba a esto **gegenpressing**. Bielsa lo llamaba "la presión es la mejor forma de tener la pelota".

### 3. Volumen ofensivo de calidad

No alcanza con llegar — hay que llegar al lugar peligroso.

**Métricas**:
- **xG creado por 90'** — peligro generado
- **Touches in opposition box** — toques en el área rival
- **Box entries** — cuántas veces entrás al área

> Estos miden si convertís el dominio territorial en peligro real.

### 4. Privación

Mientras dominás, ¿cuánto le das al rival?

**Métricas**:
- **xG concedido** — peligro permitido
- **Tiros recibidos** — exposición
- **Field tilt invertido** — ¿el rival ataca o solo aguanta?

> Dominar implica que el rival **no sale**, no solo que vos llegás.

## Filosofías clásicas asociadas

| Entrenador | Cómo se materializa el dominio | Trade-off |
|------------|-------------------------------|-----------|
| **Guardiola** | Posesión posicional + juego entre líneas | Vulnerable al contragolpe rápido |
| **Bielsa** | Presión asfixiante + verticalidad | Costo físico alto, lesiones |
| **Klopp** | Gegenpressing + transiciones rápidas | Necesita perfiles físicos específicos |
| **Sarri** | Construcción metódica + posesión | Lento ante equipos que cierran |

Cada uno **domina diferente**. Identificar **cuál estilo de dominio** es relevante para River dado su plantel.

## Aplicación al modelo de [[Pregunta-Tactica-Optima-River]]

La función objetivo combina **xG creado + 3 métricas de dominio** porque sin esa combinación, el modelo podría recomendar formaciones de contragolpe que crean mucho xG pero sin dominio (lo opuesto a la filosofía declarada).

## Datos requeridos para medir dominio

- **FBref** tiene Field Tilt, PPDA, xG, touches in box agregados por equipo y por partido. Suficiente para MVP.
- **StatsBomb Open Data** tiene datos de eventos para algunas competiciones — permite calcular las métricas más finas (no necesariamente para River específicamente).
- **Para River en torneos locales**: FBref + Understat es probablemente suficiente.

## Conexión con identidad

Esta concepción del fútbol **es coherente con vos**:
- [[Filosofia-de-Trabajo|Contra el inmediatismo]] → no contragolpe pragmático, construcción
- [[Filosofia-de-Trabajo|Fundaciones sólidas]] → estructura clara antes que improvisación
- [[Hincha-de-River|Identidad River histórica]] → tradición Marcelo Gallardo de juego dominante

El "dominio absoluto" no es solo gusto estético — es **filosofía coherente con cómo pensás todo lo demás**.
