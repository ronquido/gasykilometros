---
title: "Inteligencia y precio en OpenRouter: la brecha EE.UU.-China y el ahorro real de un usuario de Hermes Agent"
subtitle: "Analisis de 445 modelos, 20 fichas de precio y un modelo de coste mensual para tareas cotidianas"
lang: es
date: 2026-09-13
updated: 2026-09-13
layout: articulo
type: analisis
tags: [openrouter, llm, precios, hermes-agent, china, ee-uu, ahorro]
summary: "Los modelos mas inteligentes de OpenRouter, cuanto cuestan, por que los chinos salen hasta 14 veces mas baratos y que ahorro puede esperar un usuario medio de Hermes Agent que automatiza correo, apuntes, web y programacion."
fuente_precios: "API publica de OpenRouter (/api/v1/models), consultada el 13-sep-2026 (445 modelos con precio)"
volumen_modelado: "5,28 M tokens de entrada y 0,75 M de salida al mes (ratio 7:1)"
---

# Inteligencia y precio en OpenRouter: la brecha EE.UU.-China y el ahorro real de un usuario de Hermes Agent

## 1. Resumen ejecutivo

1. **OpenRouter publica 445 modelos con precio** (13-sep-2026): 202 de laboratorios
   estadounidenses, 130 chinos y 113 del resto del mundo. El catalogo ya no es una
   tienda de modelos americanos: China es un tercio del escaparate.
2. **El techo de inteligencia sigue siendo estadounidense.** Claude Fable 5.1
   (Anthropic) es el numero uno del *Artificial Analysis Intelligence Index*, con
   GPT-6 Astra (OpenAI) y Claude Opus 5 detras. El primer modelo de pesos abiertos
   es chino: **GLM-5.3 (Z.ai)**, seguido de **Kimi K3** (Moonshot).
3. **La brecha de precio es de un orden de magnitud, no de un porcentaje.** El
   frontier chino cuesta entre **8 y 14 veces menos** que el frontier americano
   equivalente: GLM-5.3 (II 45) a $1,09/$3,43 por millon de tokens frente a
   Claude Fable 5.1 (II 53) a $10/$50. En coste mensual modelado: **$8,33 frente a
   $90,20** para el mismo trabajo.
4. **Un usuario medio de Hermes Agent no necesita el frontier para casi nada.** Con
   el perfil de uso tipico (correo, apuntes, mantenimiento web y algo de
   programacion = 5,28 M tokens de entrada y 0,75 M de salida al mes), hacer todo
   con un modelo de frontera cuesta entre **$18 y $90 al mes**; hacerlo con una
   cascada de dos niveles cuesta **$5,09** ($5,37 con la comision de OpenRouter),
   y con cache de prompt, **$3,84**.
5. **El ahorro del que nadie habla no es elegir modelo chino, es enrutar.** Pasar
   de "todo con el modelo mas caro" a una cascada razonada ahorra **94 %**
   ($1.021 al ano). Pasar de GPT-5.6 Sol a GLM-5.3 manteniendo el resto igual
   ahorra un 54 %. Lo segundo es una decision de modelo; lo primero es una
   decision de arquitectura, y es la que se sostiene cuando los precios cambien.
6. **Contra la suscripcion, la API por uso gana en este perfil.** ChatGPT Plus o
   Claude Pro cuestan $20/mes y Claude Max 20x, $200/mes. La cascada modelada
   cuesta $5,37/mes y no tiene limite de mensajes: solo de tokens, que es lo que
   de verdad se consume.

---

## 2. Metodologia

**Precios.** Todos los precios de este analisis salen de la **API publica de
OpenRouter** (`/api/v1/models`), consultada el **13 de septiembre de 2026**. Son
tarifas de lista en dolares por millon de tokens ($/M), separando entrada y
salida. La API expone ademas tres precios que casi nadie mira y que cambian el
resultado: lectura de cache (`input_cache_read`), escritura de cache
(`input_cache_write`) y precios por tramos (`overrides`, que encarecen la
entrada al superar cierto tamano de contexto).

**Inteligencia.** Las puntuaciones provienen de fuentes publicas de referencia —
*Artificial Analysis Intelligence Index*, el *BenchAlign* de BenchLM y los
resumenes de Fello AI y Shakudo de septiembre de 2026. **Aviso importante:** el
indice de Artificial Analysis se ha versionado varias veces durante el ano
(v4.1, v4.2, v4.3) y las puntuaciones **no son comparables entre versiones**.
La propia FAQ del leaderboard daba 53 puntos al numero uno, mientras que un
resumen de la misma semana citaba 66 para el mismo modelo en una version
distinta del indice. En este documento los valores se citan con su fuente y se
evita comparar cifras de versiones diferentes.

**Volumen modelado.** El "usuario medio" de este analisis usa Hermes Agent para
cinco bloques de trabajo. Las cifras son estimaciones declaradas, no medidas:

| Bloque | Ejecuciones/mes | Tokens entrada | Tokens salida | Entrada total | Salida total |
|---|---|---|---|---|---|
| Correo: triage y redaccion | 22 (1/dia laborable) | 45.000 | 4.000 | 0,99 M | 0,09 M |
| Apuntes y documentos: resumenes | 60 (3/dia) | 12.000 | 3.000 | 0,72 M | 0,18 M |
| Mantenimiento web | 8 (2/semana) | 120.000 | 15.000 | 0,96 M | 0,12 M |
| Programacion | 12 (3/semana) | 180.000 | 25.000 | 2,16 M | 0,30 M |
| Automatizaciones y consultas | 30 | 15.000 | 2.000 | 0,45 M | 0,06 M |
| **Total** | **132** | — | — | **5,28 M** | **0,75 M** |

El ratio resultante es de **7,1 tokens de entrada por cada token de salida**. No
es casual: un agente como Hermes reenvia contexto (system prompt, historial,
resultado de herramientas) en cada vuelta del bucle, mientras que la respuesta
del modelo es corta. Cualquier calculo de coste que use un ratio 1:1
sobreestimara el peso de la salida y subestimara el de la entrada. Se incluye
una sensibilidad a ratio 3:1 al final, porque un usuario que genere mucho texto
(redaccion larga, documentacion) se acercara mas a ese perfil.

**Comision.** OpenRouter no aplica margen sobre el token, pero **cobra un 5,5 %
(un minimo de $0,80) al comprar creditos** (5 % si se paga en cripto). Todos los
importes "de caja" de este analisis van marcados con el +5,5 %. Los creditos
caducan a los 12 meses. El servicio BYOK (traer tus propias claves) tiene una
franquicia de $25.000/mes a precio de lista y un 5 % sobre lo que exceda.

---

## 3. Los modelos mas inteligentes y lo que cuesta cada uno

Veinte modelos, agrupados por pais y por nivel, con la puntuacion de inteligencia
publicada y su tarifa real en OpenRouter. El "coste/mes" es el del perfil de la
tabla anterior.

| Modelo | Pais | Inteligencia (fuente) | $/M entrada | $/M salida | Contexto | Coste/mes | Con 5,5 % |
|---|---|---|---|---|---|---|---|
| Claude Fable 5.1 | EE.UU. (Anthropic) | II #1 (53 en la version consultada; 66 en otra version) · BenchAlign 84,61 | 10,00 | 50,00 | 1,0 M | $90,20 | $95,16 |
| GPT-6 Astra | EE.UU. (OpenAI) | II 53 (max) · BenchAlign 84,08 · lider LiveBench Reasoning | 10,00 | 50,00 | 1,05 M | $90,20 | $95,16 |
| Claude Opus 5 | EE.UU. (Anthropic) | II 63 (segunda fuente) · BenchAlign 81,97 | 5,00 | 25,00 | 1,0 M | $45,10 | $47,58 |
| GPT-5.6 Terra | EE.UU. (OpenAI) | II 42 (max) · 108 tokens/s de salida | 2,00 | 12,00 | 1,05 M | $19,54 | $20,61 |
| Gemini 3.1 Pro | EE.UU. (Google) | 98 % ARC-AGI-1 a $0,52/tarea · 94,1 % GPQA | 2,00 | 12,00 | 1,05 M | $19,54 | $20,61 |
| GPT-5.6 Sol | EE.UU. (OpenAI) | II 42 (high) · BenchAlign 80,63 | 2,00 | 10,00 | 1,05 M | $18,04 | $19,03 |
| Grok 4.6 | EE.UU. (xAI) | II 44 (high/xhigh) | 2,00 | 6,00 | 500 k | $15,05 | $15,88 |
| Gemini 3.8 Flash | EE.UU. (Google) | BenchAlign 75,61 | 0,75 | 3,75 | 1,05 M | $6,77 | $7,14 |
| GPT-5.5 Pro | EE.UU. (OpenAI) | Generacion anterior, el mas caro vigente | 30,00 | 180,00 | 1,05 M | $293,04 | $309,16 |
| Kimi K3 | China (Moonshot) | II 44 (#2 en pesos abiertos) · BenchAlign 74,82 | 2,65 | 13,28 | 1,05 M | $23,92 | $25,23 |
| Qwen3.8 Max | China (Alibaba) | Mejor peso abierto segun BenchLM (71,63) | 2,00 | 6,00 | 1,0 M | $15,05 | $15,88 |
| DeepSeek V4 Pro | China (DeepSeek) | Pesos MIT · II no publicada por AA | 1,60 | 3,20 | 1,05 M | $10,84 | $11,44 |
| GLM-5.3 | China (Z.ai) | II 45: **#1 en pesos abiertos** | 1,09 | 3,43 | 1,31 M | $8,33 | $8,79 |
| HY4 preview | China (Tencent) | Previa, sin auditoria independiente | 0,83 | 2,50 | 1,05 M | $6,27 | $6,62 |
| MiniMax M3 | China (MiniMax) | Peso abierto multimodal | 0,30 | 1,20 | 1,05 M | $2,48 | $2,62 |
| GLM-5.3-Flash | China (Z.ai) | II 42 · 107 tokens/s | 0,08 | 0,25 | 1,31 M | $0,58 | $0,62 |
| Qwen3.8 Flash | China (Alibaba) | Gama flash | 0,15 | 0,47 | 1,0 M | $1,14 | $1,21 |
| DeepSeek V4.1 Flash | China (DeepSeek) | Gama flash (**modelo configurado hoy en este Hermes**) | 0,15 | 0,60 | 1,05 M | $1,24 | $1,31 |
| GPT-5.6 Luna | EE.UU. (OpenAI) | Gama flash | 0,20 | 1,20 | 1,05 M | $1,95 | $2,06 |
| Gemini 3.5 Flash-Lite | EE.UU. (Google) | 348 tokens/s (el mas rapido medido) | 0,30 | 2,50 | 1,05 M | $3,45 | $3,64 |

Tres detalles de la tabla que no se ven a primera vista:

- **El precio de salida manda.** En un ratio 7:1 la entrada pesa mas en volumen,
  pero la salida pesa mas en dinero. Claude Fable 5.1 cobra su salida 10 veces
  mas cara que su entrada; GLM-5.3, 3,1 veces. Un modelo con entrada barata y
  salida carisima (Kimi K3: $2,65/$13,28) sale mas caro de lo que sugiere su
  tarifa de entrada.
- **Los tramos de contexto existen.** GPT-6 Astra pasa de $10/$50 a **$20/$75**
  a partir de 272.000 tokens de prompt. En un agente que acumula contexto, una
  sesion larga puede costar el doble sin que el usuario haya cambiado nada.
- **La cache no es un detalle.** Leer cache cuesta, en Fable 5.1, $0,25/M frente
  a $10/M de entrada normal: **40 veces menos**. En DeepSeek V4 Pro, $0,135 frente
  a $1,60 (11,9x). Para un agente que reenvia el mismo contexto en cada vuelta,
  esto es la palanca de ahorro mas grande que existe, y es invisible si solo se
  miran las dos cifras de cabecera.

---

## 4. La brecha de precio entre el frontier americano y el chino

### 4.1 Comparacion por mediana

Tomando solo modelos de frontera (los que la industria reconoce como punteros de
cada laboratorio) y excluyendo la gama flash:

| Grupo | $/M entrada (mediana) | $/M salida (mediana) | Coste/mes (mediana) |
|---|---|---|---|
| Frontier EE.UU. (Anthropic, OpenAI, Google, xAI) | 2,00 | 12,00 | $19,54 |
| Frontier China (Z.ai, Moonshot, Alibaba, DeepSeek, Tencent, MiniMax) | 1,35 | 3,32 | $9,59 |
| **Brecha** | **1,5x** | **3,6x** | **2,0x** |

La mediana da una brecha de 2x, pero **esconde lo importante**: en la mediana
americana estan entrando modelos de gama media (Sol, Terra, Gemini 3.8 Flash)
que en China no tienen equivalente, y en la china entra Kimi K3, que es el
modelo chino mas caro. La brecha real aparece cuando se comparan modelos de
**nivel equivalente**.

### 4.2 Comparacion por nivel

| Nivel | EE.UU. | $/M in / out | China | $/M in / out | Veces mas caro EE.UU. |
|---|---|---|---|---|---|
| Techo absoluto | Claude Fable 5.1 (II 53) | 10,00 / 50,00 | GLM-5.3 (II 45) | 1,09 / 3,43 | **9,2x / 14,6x** |
| Segundo escalon | GPT-6 Astra (II 53) | 10,00 / 50,00 | Kimi K3 (II 44) | 2,65 / 13,28 | 3,8x / 3,8x |
| Frontier de trabajo | GPT-5.6 Sol (II 42) | 2,00 / 10,00 | DeepSeek V4 Pro | 1,60 / 3,20 | 1,25x / 3,1x |
| Razonamiento rapido | Grok 4.6 (II 44) | 2,00 / 6,00 | Qwen3.8 Max | 2,00 / 6,00 | **1,0x / 1,0x** |
| Gama flash | GPT-5.6 Luna | 0,20 / 1,20 | GLM-5.3-Flash | 0,08 / 0,25 | 2,6x / 4,8x |

La conclusion honesta es que **la brecha no es un fenomeno unico, son tres**:

1. **En el techo hay un abismo (9-15x).** Nadie compite con GLM-5.3 en relacion
   inteligencia/precio: es el unico caso en que un modelo de pesos abiertos,
   descargable y autoalojable, se sienta a la misma mesa que el frontier
   propietario por un decimo del precio.
2. **En la zona media la brecha se estrecha a 1,25-3x.** GPT-5.6 Sol ($2/$10) y
   Grok 4.6 ($2/$6, ambos estadounidenses) compiten de tu a tu con Qwen3.8 Max
   ($2/$6). Aqui la eleccion ya no es de precio sino de ecosistema, latencia y
   politica de datos.
3. **En la gama flash la brecha vuelve (2,6-4,8x)** porque el coste dominante es
   la salida y los laboratorios chinos han optimizado modelos de razonamiento
   corto con salidas muy baratas.

### 4.3 Por que la brecha existe (y por que se esta cerrando)

- **Arquitectura de pesos abiertos.** GLM-5.2 era un MoE de 744.000 millones de
  parametros con ~40.000 millones activos por token, publicado bajo licencia MIT;
  GLM-5.3 se construyo **sin nuevo preentrenamiento**: toda la mejora vino de
  post-entrenamiento sobre la misma base. Eso reduce el coste amortizado por
  token a casi nada y explica que puedan vender a $1,09/$3,43. DeepSeek mantiene
  sus pesos en MIT. La consecuencia incomoda para el frontier propietario es que
  ya no compra el mejor modelo, compra el mejor modelo *cerrado*.
- **Presion de precios interna.** Los planes de suscripcion chinos para
  programacion (Kimi, GLM Coding Plan, MiniMax Token Plan, Bailian) se mueven en
  el rango de 49 a 699 CNY/mes —**de $7,28 a $103,85**— con niveles intermedios
  muy agresivos: 99 CNY = $14,71/mes, 199 CNY = $29,57/mes. Un precio de
  suscripcion bajo presiona a la baja el precio por token de la API.
- **Especializacion en salida corta.** Los modelos chinos de gama flash (GLM-5.3-
  Flash a $0,25/M de salida, Qwen3.8 Flash a $0,47) estan disenados para responder
  sin cadenas de razonamiento largas, que es exactamente lo que necesita un
  agente que hace clasificacion, resumen y extraccion de datos.
- **Lo que no se ve en el precio.** El ahorro tiene contrapartidas reales:
  residencia de datos en China y marco legal distinto (RGPD, secreto
  profesional), menor calidad en tareas de agente largo, y dependencia de
  proveedores con menos garantias contractuales. Para correo de empresa o datos
  personales, la diferencia de precio no siempre es el criterio decisivo.

---

## 5. Cuanto cuesta de verdad el perfil de un usuario de Hermes Agent

Hermes Agent tiene dos tipos de "hueco" de modelo: **el modelo principal**, que
piensa y ejecuta el bucle de herramientas, y **once tareas auxiliares**
(compresion de contexto, vision, extraccion web, puntuacion de aprobaciones,
generacion de titulos, curador de skills...). La documentacion oficial de Hermes
es explicita con las auxiliares: *"un modelo flash hace el trabajo por 1/50 del
coste"* y *"un modelo caro aqui es un desperdicio"*. Ese es el punto de partida
del ahorro: la mayor parte de las llamadas de un agente no requieren el modelo
mas listo del mundo.

### 5.1 El mismo trabajo, veinte modelos

| Escenario (todo con un solo modelo) | $/mes | Con 5,5 % | $/ano |
|---|---|---|---|
| GLM-5.3-Flash | 0,58 | 0,62 | 7,38 |
| DeepSeek V4.1 Flash *(modelo configurado hoy)* | 1,24 | 1,31 | 15,71 |
| GPT-5.6 Luna | 1,95 | 2,06 | 24,70 |
| MiniMax M3 | 2,48 | 2,62 | 31,41 |
| Gemini 3.5 Flash-Lite | 3,45 | 3,64 | 43,64 |
| Gemini 3.8 Flash | 6,77 | 7,14 | 85,70 |
| GLM-5.3 | 8,33 | 8,79 | 105,49 |
| DeepSeek V4 Pro | 10,84 | 11,44 | 137,25 |
| Grok 4.6 / Qwen3.8 Max | 15,05 | 15,88 | 190,52 |
| GPT-5.6 Sol | 18,04 | 19,03 | 228,39 |
| Kimi K3 | 23,92 | 25,23 | 302,80 |
| Claude Opus 5 | 45,10 | 47,58 | 570,94 |
| Claude Fable 5.1 | 90,20 | 95,16 | 1.141,93 |
| GPT-6 Astra | 90,20 | 95,16 | 1.141,93 |
| GPT-5.5 Pro | 293,04 | 309,16 | 3.709,89 |

### 5.2 Arquitecturas de enrutado y su ahorro

La cascada consiste en repartir los cinco bloques de trabajo entre dos niveles:
un modelo flash para lo repetitivo (correo, resumenes, automatizaciones) y un
frontier barato para lo que exige razonamiento (mantenimiento web y
programacion).

| Escenario | $/mes | Con 5,5 % | $/ano | Ahorro vs. frontier top EE.UU. |
|---|---|---|---|---|
| Todo con Claude Fable 5.1 | 90,20 | 95,16 | 1.141,93 | — |
| Todo con GPT-6 Astra | 90,20 | 95,16 | 1.141,93 | 0 % |
| Todo con GPT-5.6 Sol | 18,04 | 19,03 | 228,39 | 80 % |
| Todo con Kimi K3 | 23,92 | 25,23 | 302,80 | 73 % |
| Todo con GLM-5.3 | 8,33 | 8,79 | 105,49 | 91 % |
| Cascada: flash + GPT-5.6 Sol (solo codigo) | 9,02 | 9,52 | 114,25 | 90 % |
| Cascada: flash + GPT-5.6 Sol (web y codigo) | 10,68 | 11,27 | 135,26 | 88 % |
| **Cascada: flash + GLM-5.3 (web y codigo)** | **5,09** | **5,37** | **64,47** | **94 %** |
| **Cascada optima con 50 % de lecturas de cache** | **3,64** | **3,84** | **46,09** | **96 %** |

Lectura de la tabla: elegir un modelo chino en vez de uno americano ahorra,
pero **es el enrutado lo que produce el ahorro grande**. Entre "todo con GLM-5.3"
(8,79 $/mes) y "cascada con GLM-5.3 y su flash" (5,37 $/mes) hay otro 39 % de
ahorro sin bajar de nivel en las tareas que importan. Y el extremo opuesto —todo
con GPT-5.5 Pro, el modelo mas caro vigente— cuesta **309 $/mes**, 57 veces mas
que la cascada optima.

### 5.3 Sensibilidad

El modelo de volumen es una estimacion. Como el coste es lineal en tokens, la
escala es directa:

| Perfil | Tokens/mes | Cascada ($/mes, con 5,5 %) | Todo con Fable 5.1 |
|---|---|---|---|
| Ligero (1/3 del modelado) | 2,01 M | 1,79 | 31,72 |
| **Medio (el modelado)** | **6,03 M** | **5,37** | **95,16** |
| Intenso (3x) | 18,09 M | 16,11 | 285,48 |

Y el efecto del ratio entrada/salida, con el mismo volumen total:

| Ratio in:out | Cascada flash + GLM-5.3 | Todo con Fable 5.1 | Brecha |
|---|---|---|---|
| 3:1 (perfil de redaccion) | 7,79 | 129,94 | 17x |
| 7:1 (perfil de agente, el modelado) | 5,09 | 90,20 | 18x |
| 15:1 (perfil de agente con contexto enorme) | 3,96 | 68,36 | 17x |

La brecha entre ambos enfoques se mantiene en torno a 17-18x sea cual sea el
ratio: **el ahorro no depende de acertar con el perfil de uso, depende de la
decision de arquitectura**.

---

## 6. Suscripcion o API por uso

| Opcion | Precio | Limite | Encaja si... |
|---|---|---|---|
| ChatGPT Plus | $20/mes | Cuotas de uso, sin API | Quieres chat, no agente |
| ChatGPT Pro | $200/mes | Mas cuota | Trabajas horas al dia con el modelo top |
| Claude Pro | $20/mes | Cuotas (Cowork incluido desde ene-2026) | Quieres Claude en interfaz propia |
| Claude Max 5x / 20x | $100 / $200/mes | 5x / 20x el uso de Pro | Programas con Claude Code a diario |
| Planes chinos de programacion (Kimi, GLM, MiniMax) | 49-699 CNY/mes = $7,28-$103,85 | Cuotas por plan | Coste bajo, ecosistema fuera de la UE |
| **Cascada via OpenRouter** | **$5,37/mes** (perfil modelado) | Solo pagas tokens | Automatizas con Hermes y controlas que modelo hace que |

Tres conclusiones:

1. **Para automatizacion con agente, la API por uso es mas barata que cualquier
   suscripcion** en el perfil modelado: $5,37 frente a los $20 del plan mas
   barato de pago. La razon es simple: una suscripcion se paga entera aunque el
   mes se use poco, mientras que la API escala con el uso real.
2. **La suscripcion solo gana si el uso es humano e intensivo.** Un programador
   que pasa cinco horas al dia dentro de Claude Code o ChatGPT no esta en este
   perfil: su volumen de tokens seria de decenas de millones al mes y la
   suscripcion le saldria mas barata.
3. **El punto de cruce esta en torno a los $20/mes de consumo de API**, que con
   el perfil modelado corresponde a unos 22 M tokens al mes de cascada o a unos
   2 M tokens de frontier top. A partir de ahi, conviene comparar.

### 6.1 Palancas adicionales de ahorro en Hermes

- **Modelos gratuitos.** El catalogo tiene 19 modelos con sufijo `:free`. Con al
  menos 10 creditos comprados, el limite es de 1.000 peticiones al dia. Para
  tareas triviales (titulos de sesion, clasificacion) pueden llevar el coste a
  cero, con la advertencia de que la politica de datos y la disponibilidad no
  son las de un proveedor de pago.
- **Modelos locales.** Hermes soporta proveedores locales (Ollama): la config de
  este equipo tiene alias para `gemma4-e4b-fixed`, `mistral` y `qwen3.6_custom`.
  Un modelo local tiene coste marginal cero y, para resumenes y clasificacion, es
  competitivo con la gama flash. Su limite es la calidad en tareas de agente
  largo y la velocidad del hardware.
- **Cache de prompt.** Ya cuantificado: 40x mas barata la lectura en el frontier
  top. Ojo con la letra pequena de Hermes: **cambiar de modelo a mitad de sesion
  invalida la cache**, porque el system prompt contiene contenido especifico del
  modelo. Los cambios frecuentes de modelo salen caros por esta via.
- **Modo `--once`.** Hermes permite `/model X --once`: usar un modelo caro para
  un unico turno y volver al barato. Es la forma mas limpia de tener el frontier
  disponible sin pagarlo de forma continua.
- **Cadena de fallback.** Configurar `fallback_providers` con modelos chinos
  baratos evita que un fallo de capacidad del proveedor caro acabe en una
  factura inesperada.

---

## 7. Pros y contras

### Del frontier estadounidense

**Pros:** techo de inteligencia y de fiabilidad superiores en tareas de agente
largo (uso de herramientas, cadenas de razonamiento, calidad de codigo);
contratos, residencia de datos y cumplimiento adaptados al RGPD; ecosistema de
documentacion, integraciones y soporte; mayor estabilidad de proveedor.

**Contras:** 9-15x mas caro en el techo; precios por tramos que castigan el
contexto largo; cuotas de suscripcion que no escalan con el uso real; cambios de
modelo frecuentes invalidan cache.

### Del frontier chino

**Pros:** relacion inteligencia/precio sin rival en la franja alta (GLM-5.3:
II 45 a $1,09/$3,43); pesos abiertos con licencia MIT en DeepSeek y la base de
GLM, lo que permite autoalojar y auditar; contextos de 1 M de tokens en casi
toda la gama; gama flash extraordinariamente barata ($0,08/$0,25) y muy
adecuada para tareas de agente repetitivas; planes de suscripcion desde
$7,28/mes.

**Contras:** residencia de datos y marco legal fuera de la UE; menor calidad en
tareas de agente muy largas y en matices de idioma; documentacion y soporte mas
irregulares; riesgo de cambio de licencia o de precio sin aviso previo (GLM-5.3
se publico sin liberar pesos nuevos, lo que demuestra que la politica de
apertura puede cambiar); no apto por defecto para datos personales o
profesionales sensibles.

### Perspectivas de futuro

- **La brecha del techo se estrechara primero en precio, no en calidad.** El
  patron de 2026 —el mejor modelo abierto va 8 puntos de indice por detras del
  mejor cerrado, y cuesta una decima parte— sugiere que el frontier abierto
  llegara antes a precio competitivo que a paridad de capacidad.
- **El precio por token seguira bajando en la gama flash.** Los tramos de
  contexto y las tarifas de cache seran las palancas de diferenciacion: el
  precio de cabecera importara cada vez menos.
- **El enrutado se vuelve producto.** La ventaja competitiva se desplaza del
  modelo al orquestador: quien decide que modelo hace cada tarea. Hermes lo hace
  explicitamente con su modelo principal y sus once huecos auxiliares.
- **Riesgo de dependencia de un solo agregador.** Centralizar 445 modelos en
  OpenRouter simplifica la factura pero anade un punto de fallo y una comision
  del 5,5 %. La salida BYOK (con franquicia de $25.000/mes) mitiga el coste,
  no la dependencia.
- **La ventaja de la gama flash china puede erosionarse** si la regulacion
  europea o las exigencias de las empresas empujan a usar modelos con residencia
  en la UE. El coste de cumplimiento se acabara pagando en el precio del token.

---

## 8. Recomendacion concreta para un usuario de Hermes Agent

1. **Modelo principal: un frontier barato, no el techo.** GLM-5.3 ($1,09/$3,43)
   o, si hay requisitos de datos, GPT-5.6 Sol ($2/$10). Coste modelado: $8,79/
   mes frente a los $95,16 de usar Fable 5.1 para todo.
2. **Once tareas auxiliares: un flash, siempre.** GLM-5.3-Flash ($0,08/$0,25) o
   DeepSeek V4.1 Flash ($0,15/$0,60). La documentacion de Hermes ya lo dice:
   compresion, aprobaciones, titulos y vision no necesitan el modelo caro.
3. **Frontier top, solo con `--once`.** Para el problema dificil, un turno con
   el mejor modelo y vuelta al barato: se paga lo que se usa y no se rompe la
   cache del resto de la sesion.
4. **Cache de prompt activada y cambios de modelo minimos.** Es el mayor
   ahorro invisible (hasta 40x en lecturas) y se pierde al cambiar de modelo
   dentro de una sesion.
5. **Cadena de fallback con modelos baratos** y `fallback_providers` bien
   configurado, para que un fallo de capacidad no acabe en factura sorpresa.
6. **Revisar la factura cada mes, no cada ano.** Los precios de esta gama se han
   movido varias veces en nueve meses; el analisis de hoy es una foto, no una
   tarifa.

**Resultado esperado del perfil modelado:** $3,84-$5,37 al mes, es decir
**$46-$64 al ano**, con trabajo de frontier disponible cuando hace falta. Frente
a los $1.141,93 al ano de hacer lo mismo siempre con el modelo mas inteligente,
el ahorro es del **94-96 %**; frente a una suscripcion de $20/mes, del **73-78 %**.

---

## 9. Referencias consultadas

Todas las fuentes se consultaron el **13 de septiembre de 2026**.

1. OpenRouter — **API publica de modelos y precios**: `https://openrouter.ai/api/v1/models` (445 modelos, tarifas por millon de tokens, precios de cache y tramos por contexto). Datos descargados y procesados para este analisis.
2. OpenRouter — *FAQ: Pricing and fees* (comision del 5,5 % con minimo de $0,80 al comprar creditos; 5 % en cripto; franquicia BYOK de $25.000/mes y 5 % por encima; caducidad de creditos a 12 meses; 1.000 peticiones/dia en modelos gratuitos con 10 creditos comprados). `https://openrouter.ai/docs/faq`
3. OpenRouter — *Pricing*. `https://openrouter.ai/pricing`
4. Artificial Analysis — *LLM Leaderboard* (Intelligence Index, precios, velocidad; FAQ con el ranking: Fable 5.1 primero con II 53 sobre 149 modelos y GLM-5.3 como mejor modelo de pesos abiertos con II 45). `https://artificialanalysis.ai/leaderboards/models`
5. Artificial Analysis — *Intelligence Index v4.2* (composicion del indice y cambios de version, 4-sep-2026). `https://artificialanalysis.ai/articles/artificial-analysis-intelligence-index-v4-2`
6. BenchLM — *LLM Leaderboard & AI Model Benchmarks, septiembre de 2026* (BenchAlign: Fable 5.1 84,61; GPT-6 Astra 84,08; Claude Opus 5 81,97; GPT-5.6 Sol 80,63; Gemini 3.8 Flash 75,61; Kimi K3 74,82; Qwen3.8 Max como mejor peso abierto con 71,63). `https://benchlm.ai/`
7. Fello AI — *Best AI Models in 2026*, 5-sep-2026 (Fable 5.1 en cabeza con 66 en una version del indice; Agentic Index 61; HLE 59,1 %; AA-Omniscience 67 %; GLM-5.3 publicado el 14-ago-2026 sin nuevos pesos abiertos; pesos de DeepSeek V4 en MIT). `https://felloai.com/best-ai-models/`
8. Shakudo — *Top 9 Large Language Models as of September 2026*, 1-sep-2026 (GLM-5.2 como MoE de 744.000 M de parametros y ~40.000 M activos bajo licencia MIT; SWE-bench Pro 62,1 % frente a 58,6 % de GPT-5.5; MiniMax M3 y su salida a bolsa en Hong Kong en enero de 2026). `https://www.shakudo.io/blog/top-9-large-language-models`
9. Hermes Agent — *Configuring Models* (dos tipos de hueco: modelo principal y once tareas auxiliares; recomendacion de modelos flash para las auxiliares; la cache de prompt se invalida al cambiar de modelo; `/model --once`). `https://hermes-agent.nousresearch.com/docs/user-guide/configuring-models`
10. Hermes Agent — *Tool Gateway / Nous Portal* (mas de 300 modelos bajo una suscripcion y 10 % de descuento en proveedores facturados por token). `https://hermes-agent.nousresearch.com/docs/user-guide/features/tool-gateway`
11. CloudZero — *Claude pricing in 2026* (planes de suscripcion de Anthropic de $0 a $200/mes). `https://www.cloudzero.com/blog/claude-pricing/`
12. CodeAgentSwarm — *Claude Code Pricing 2026* (Pro $20, Max 5x $100, Max 20x $200, segun el catalogo de Anthropic de agosto de 2026). `https://www.codeagentswarm.com/en/guides/claude-code-plans-and-pricing`
13. GuruSup — *ChatGPT Plus vs Pro*, 18-may-2026 (Plus a $20/mes, Pro a $200/mes). `https://gurusup.com/es/blog/chatgpt-plus-vs-pro`
14. CodePick — *5 China-Native AI Coding Subscriptions Compared*, agosto de 2026 (planes de programacion de Volcengine Ark, Alibaba Bailian, MiniMax, GLM y Kimi). `https://codepick.dev/en/compare/coding-plan-comparison-2026/`
15. Jiegec's Knowledge Base — *AI Coding Plan* (niveles de suscripcion de Kimi: Andante 49 CNY, Moderato 99 CNY, Allegretto 199 CNY, Allegro 699 CNY al mes). `https://jia.je/kb/en/software/coding_plan.html`
16. OptimNow — *DeepSeek, Qwen, Kimi & GLM Pricing (2026): Open-Weight Model Pricing*, 23-ago-2026 (tarifas de los modelos chinos de pesos abiertos y economia del autoalojamiento). `https://www.optimnow.io/post/open-weight-model-pricing-deepseek-qwen-kimi-glm`
17. Layer3Labs — *Best Chinese AI Models 2026*, 17-jul-2026 (comparativa DeepSeek, Qwen, GLM y Kimi, con la advertencia de residencia de datos). `https://www.layer3labs.io/comparisons/best-chinese-ai-models`
18. Ofox AI — *OpenRouter Pricing: Token Costs, Credit Fees and BYOK*, 23-may-2026 (desglose de la comision del 5,5 %, minimo de $0,80 y el cambio de medicion del BYOK). `https://ofox.ai/blog/openrouter-pricing-hidden-markup-breakdown-2026/`
19. ExchangeRate API — tipo de cambio CNY/USD utilizado para convertir los planes chinos (1 CNY = 0,1486 USD). `https://open.er-api.com/v6/latest/CNY`
20. Configuracion local de Hermes Agent en este equipo (`~/.hermes/config.yaml`): modelo principal `deepseek/deepseek-v4.1-flash` via OpenRouter y proveedor local Ollama con alias `gemma4-e4b-fixed`, `mistral` y `qwen3.6_custom`. Usada como caso real para los ejemplos de coste.

### Nota metodologica y limitaciones

- Los precios son **tarifas de lista de OpenRouter** en la fecha de consulta y no
  incluyen promociones por tiempo limitado ni descuentos de proveedor. Cambian:
  el analisis es una foto, no una tarifa.
- Las puntuaciones de inteligencia **no son comparables entre versiones del
  indice** de Artificial Analysis (v4.1/v4.2/v4.3). Cuando dos fuentes dan cifras
  distintas para el mismo modelo, se han recogido ambas.
- El modelo de volumen mensual es una **estimacion declarada** con cinco bloques
  de trabajo; no son mediciones de un usuario real. La seccion de sensibilidad
  cubre los ordenes de magnitud razonables (1/3x, 1x, 3x) y dos ratios
  entrada/salida alternativos.
- La comparacion de inteligencia entre modelos chinos y americanos se apoya en
  indices agregados de terceros. **Ninguna de estas cifras ha sido medida en
  este documento**: no se ha ejecutado ningun benchmark propio.
- Los importes con la comision aplicada corresponden a la compra de creditos en
  OpenRouter con tarjeta (+5,5 %). Un usuario que pague en cripto pagaria un
  5 %; uno que use BYOK dentro de la franquicia, nada.
