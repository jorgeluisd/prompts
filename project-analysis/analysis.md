# PROMPT DE PLANIFICACIÓN EN DOS FASES (reutilizable)

## Parámetros (edítalos)
- PROYECTO: <<nombre + una línea de qué es>>
- FUENTES A LEER PRIMERO: <<IDs de Notion / rutas del repo / docs; o "solo esta conversación">>
- VISIÓN/ALCANCE ACTUAL: <<pega lo que refinamos, o "usa lo que ya conversamos en esta sesión">>
- CONVENCIÓN DE IDIOMA: <<default: documentación en español neutro/venezolano (nunca jerga argentina), naming de código en inglés, comentarios de código en español. Ajusta si el mercado es otro (p. ej. UI/marketing en rioplatense para Argentina)>>
- DEFAULTS DE ARQUITECTURA: <<default: onion/hexagonal, screaming, SOLID, CQRS selectivo, dominio en el centro, detalles externos detrás de puertos, ADRs para toda decisión no trivial. Ajusta si aplica>>
- DEFAULTS DE INFRA (a ratificar): <<p. ej. NestJS + Fly.io gru + Supabase detrás de puertos; o lo que aplique>>
- CONDICIONES EXTERNAS A VERIFICAR EN VIVO: <<regulación, APIs, pricing, requisitos legales que NO debo dar por firmes>>
- FLUJO DE MODELOS: Fable = planificación/ADRs; Opus = daily driver de construcción; Sonnet = CRUD/boilerplate.

## Reglas de la sesión (no negociables)
- Documentación primero: NO se escribe código de aplicación en esta sesión.
- Artefactos de planificación LOCALES en el repo (docs/adr, docs/decisiones, docs/roadmap, docs/build). Los ADRs son locales. Notion (u otra fuente) es solo CONTEXTO de lectura: no crees páginas nuevas y no la modifiques sin permiso explícito; si te lo pido, actualiza sobre la página existente, nunca crees para actualizar.
- Tú PROPONES; yo RATIFICO. Nada se decide sin mi "acepto".
- Todo hecho externo no verificado (regulación, API, pricing, requisito legal) va como CONDICIÓN BLOQUEANTE con su árbol de fallbacks; no lo conviertas en decisión.
- Usa la convención de idioma y los defaults de arquitectura de los parámetros.
- Trabaja en DOS FASES y NO te saltes a la Fase 2 sin cerrar la Fase 1 conmigo.
- Si ya respondí algo en la conversación, úsalo; no me lo vuelvas a preguntar.

## FASE 1 — Entender y conversar
1. Lee las FUENTES A LEER y la conversación previa. Dime al final qué leíste.
2. Devuélveme tu entendimiento del proyecto en tus palabras (máx. 20 líneas), integrando la visión/alcance actual.
3. Evalúa honestamente el estado de validación (mercado, regulación, pricing según aplique) y pregúntame si quiero (a) planificar en paralelo a la validación o (b) esperar. No asumas por mí.
4. Sepárame con claridad: qué está CERRADO, qué sigue ABIERTO, y qué CONDICIONES BLOQUEANTES detectas (con fallback). Señala lo ya investigado que no hay que rehacer.
5. Verifica en vivo (tu conocimiento puede estar desactualizado) las CONDICIONES EXTERNAS A VERIFICAR; márcalas pendientes si no puedes confirmarlas.
6. Hazme una tanda de preguntas de intake AFILADAS (las que de verdad cambian la arquitectura), con TU RECOMENDACIÓN en cada una para que yo ratifique o ajuste. Cubre al menos: orden/alcance del MVP, bounded contexts y límites de agregado, multi-tenancy e identidad, contextos especiales del dominio, reglas/cálculo sensible, integraciones externas, infraestructura y su gate, y modelo de cobro. Termina esperando MIS respuestas. No avances sin ellas.

## FASE 2 — Entregables (SOLO tras mis respuestas), en este orden
1. Un CLAUDE.md canónico: flujo Fable/Opus/Sonnet, sistema de ADRs LOCAL, arquitectura por defecto, bounded contexts, restricciones fijas ratificadas, condiciones bloqueantes con fallback, idioma y convención de código.
2. Una secuencia de prompts para pasar a FABLE en Claude Code, uno por workstream (intake → dominio y bounded contexts → multi-tenancy e identidad → contextos especiales del dominio → módulos núcleo del MVP → reglas/cálculo sensible → integraciones externas → infraestructura → notificaciones → tracks diferidos → roadmap/backlog). Cada prompt cierra con un STATE-OF-PLAY y un GATE DE RATIFICACIÓN (tabla de ADRs propuestos; me pides veredicto uno por uno; nada ratificado sin mi "acepto").
3. Una guía breve de construcción Opus (daily driver) + Sonnet (CRUD/boilerplate): Definition of Ready para pasar una story a construcción y qué debe estar firmado antes de escribir código.

Marca todo hallazgo dependiente de verificación externa (regulatoria, integración, pricing) como CONDICIÓN BLOQUEANTE con su fallback. Cierra la Fase 2 con un STATE-OF-PLAY. Entrega los artefactos como archivos de repositorio listos para pegar.