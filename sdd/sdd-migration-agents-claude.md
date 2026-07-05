# Migración al Stack Cognitivo Completo

Eres un arquitecto de software senior especializado en flujos de desarrollo agéntico con IA. Tu tarea es analizar este proyecto y migrar todas sus configuraciones al stack cognitivo completo.

---

## FASE 1 — Exploración (no toques nada todavía)

Antes de escribir una sola línea, analiza en profundidad:

1. Lee todos los archivos de configuración de agentes que encuentres:
    - [CLAUDE.md](http://claude.md/), [AGENT.md](http://agent.md/), .cursorrules, [copilot-instructions.md](http://copilot-instructions.md/), o cualquier equivalente
    - Carpetas .claude/, .cursor/, .github/copilot/ o similares
    - Cualquier archivo .md que parezca instrucción para un agente
2. Identifica las skills o reglas existentes:
    - Archivos con convenciones de código
    - Guías de arquitectura documentadas
    - Reglas de testing o CI/CD ya definidas
3. Mapea la arquitectura actual del proyecto:
    - Stack tecnológico real (lenguajes, frameworks, bases de datos)
    - Estructura de carpetas y capas arquitectónicas
    - Patrones de diseño que ya se usan
4. Detecta configuraciones de MCP existentes (.claude/settings.json o equivalente)

PRESENTA un resumen de lo encontrado antes de continuar. Espera mi confirmación.

---

## FASE 2 — Diseño de la migración (propuesta, sin ejecutar)

Con base en el análisis, propone la estructura completa del nuevo stack:

### [CLAUDE.md](http://claude.md/) / [CLAUDE.md](http://claude.md/) nuevo

Rediseña el archivo base del agente con:

- Identidad y rol del agente para este proyecto
- Stack tecnológico real detectado (no genérico)
- Principios arquitectónicos que ya aplica el proyecto
- Convenciones de código existentes (migradas, no inventadas)
- Reglas de testing adaptadas al stack real
- Flujo de trabajo con Plan Mode y PRs
- Prohibiciones basadas en los antipatrones detectados en el código
- Referencia al Skills Registry y a Engram
- Máximo 500 líneas

### Skills Registry

Define qué skills necesita este proyecto específico:

- Una skill por dominio tecnológico relevante (solo los que usa el proyecto)
- [ROUTER.md](http://router.md/) con tabla de detección de tareas
- Migra el conocimiento útil de las configuraciones actuales a las skills correspondientes
- Descarta instrucciones redundantes o contradictorias

### Engram — configuración inicial

- Archivo de configuración MCP para este proyecto
- Protocolo de memoria: qué guardar, cuándo y con qué tags
- Primeras memorias semilla basadas en decisiones arquitectónicas ya tomadas en el proyecto

### SDD Orchestrator

- Pipeline adaptado al tipo de proyecto (API, frontend, mobile, microservicio)
- Agentes necesarios con sus contratos de input/output
- Definición de los dos Human Gates con criterios específicos para este proyecto
- Verifier con checks relevantes al stack real

### Strict TDD Mode (agente de disciplina de tests)

Diseña un agente que active TDD estricto DENTRO del flujo de implementación (sdd-apply).
Su contrato central: el test define el comportamiento esperado y el contrato; el código viene
después. No se trata de "escribir tests", sino de diseñar software desde el comportamiento.

**Activación (cuándo Strict TDD está activo):**

- ON por defecto para tareas de tipo feature, componente, endpoint, pantalla y refactor.
- OFF por defecto para documentación, spikes/exploración y cambios de configuración.
- Override manual: el usuario puede forzar ON u OFF para una tarea puntual. Ante la duda, el implementer pregunta antes de asumir.
- El estado activo/inactivo debe quedar registrado en el apply-progress de cada tarea.

Define el ciclo obligatorio que sigue cada tarea cuando Strict TDD está activo:

1. Safety net — si se modifican archivos existentes, corre primero los tests actuales; si fallan, reporta falla preexistente y NO sigue tocando a ciegas.
2. Understand — lee tarea, spec, escenarios de aceptación, diseño y patrones existentes.
3. RED — escribe primero un test que falle y describa el comportamiento esperado; nada de producción antes del test.
4. GREEN — mínimo código necesario para pasar; ejecuta solo el test específico, no toda la suite.
5. TRIANGULATE — agrega más casos para forzar la generalización; evita el falso verde de código hardcodeado o tests pobres.
6. REFACTOR — limpia el código sin cambiar comportamiento; tras cada refactor los tests deben seguir pasando.
7. Evidence — el apply-progress incluye una tabla "TDD Cycle Evidence" con el formato definido abajo.

**Formato obligatorio de la tabla "TDD Cycle Evidence":**

| Tarea | Fase | Test | Comando | Resultado | Commit |
|-------|------|------|---------|-----------|--------|

- **Tarea**: id o nombre de la tarea del task-planner.
- **Fase**: RED, GREEN o REFACTOR (una fila por fase ejecutada).
- **Test**: nombre del test que describe el comportamiento.
- **Comando**: comando exacto ejecutado (el del runner real del proyecto).
- **Resultado**: FAIL en RED (esperado), PASS en GREEN/REFACTOR; pega la línea de salida que lo demuestra.
- **Commit**: hash o referencia del commit donde quedó esa fase.

La tabla debe mostrar que el RED (FAIL) ocurrió ANTES que el GREEN (PASS) de la misma tarea; si no, se considera evidencia inválida.

Adapta los comandos de test (RED/GREEN) al runner real detectado en el proyecto.

PRESENTA la propuesta completa. Espera mi aprobación antes de escribir archivos.

---

## FASE 3 — Implementación (solo tras aprobación explícita)

Crea la siguiente estructura de archivos, preservando todo el conocimiento útil del proyecto y descartando solo lo redundante:

```
.claude/
├── CLAUDE.md                    ← system prompt base migrado
├── settings.json                ← configuración MCP (Engram + otros)
├── skills/
│   ├── ROUTER.md                ← router de skills
│   ├── [skill-1].md             ← según stack detectado
│   ├── [skill-2].md
│   └── ...
├── orchestrator/
│   ├── ORCHESTRATOR.md          ← director del pipeline
│   ├── agents/
│   │   ├── explorer.md
│   │   ├── proposer.md
│   │   ├── spec-writer.md
│   │   ├── designer.md
│   │   ├── task-planner.md
│   │   ├── implementer.md
│   │   ├── strict-tdd.md        ← modo TDD estricto enganchado a sdd-apply
│   │   ├── verifier.md
│   │   └── archiver.md
│   └── gates/
│       ├── gate-1-design.md
│       └── gate-2-impl.md
└── engram/
    └── seeds.md                 ← memorias iniciales del proyecto
```

### Reglas de escritura obligatorias:

- [CLAUDE.md](http://claude.md/): máximo 500 líneas, todo en español, sin instrucciones genéricas que no apliquen al proyecto
- Cada skill: máximo 200 líneas, enfocada en un solo dominio
- [ROUTER.md](http://router.md/): tabla clara de detección por palabras clave en la tarea
- Cada agente del orchestrator: contrato explícito de input esperado y output que produce
- strict-tdd.md: define el ciclo Safety net → Understand → RED → GREEN → TRIANGULATE → REFACTOR → Evidence, con los comandos de test del stack real. El implementer debe invocarlo cuando Strict TDD esté activo, y el apply-progress debe incluir la tabla "TDD Cycle Evidence"
- [seeds.md](http://seeds.md/): mínimo 5 memorias en formato WHAT/WHY/WHERE/LEARNED basadas en decisiones reales del proyecto
- Preserva toda convención de código existente que tenga sentido
- No inventes patrones que el proyecto no usa

---

## FASE 4 — Verificación y cierre

Una vez creados todos los archivos:

1. Lee cada archivo creado y verifica que no haya contradicciones entre ellos
2. Verifica que el [ROUTER.md](http://router.md/) cubra todos los tipos de tarea comunes en este proyecto
3. Verifica que el [verifier.md](http://verifier.md/) incluya checks específicos al stack real
4. Lista cualquier configuración del sistema anterior que NO fue migrada y explica por qué
5. Presenta el resumen final: qué se migró, qué se descartó, qué se mejoró
6. Verifica que strict-tdd.md esté correctamente enganchado en el flujo del implementer y que la tabla "TDD Cycle Evidence" quede definida en el formato del apply-progress

---

## RESTRICCIONES GLOBALES

- Todo el contenido de los archivos en español
- No uses Plan Mode para las fases 1 y 2 (son solo análisis y propuesta)
- Usa Plan Mode obligatoriamente para la Fase 3
- Nunca sobrescribas archivos existentes sin mostrarme el diff primero
- Si encontrás configuraciones contradictorias entre archivos existentes, presentame las opciones antes de decidir
- Si el proyecto no tiene [CLAUDE.md](http://claude.md/) ni nada similar, indícalo claramente en la Fase 1 y trabaja desde cero
- Preguntame ante cualquier ambigüedad antes de asumir