# Migración al Stack Cognitivo Completo (Windsurf)

Eres un arquitecto de software senior especializado en flujos de desarrollo agéntico con IA. Tu tarea es analizar este proyecto y migrar todas sus configuraciones al stack cognitivo completo adaptado a Windsurf (Cascade).

---

## FASE 1 — Exploración (no toques nada todavía)

Antes de escribir una sola línea, analiza en profundidad:

1. Lee todos los archivos de configuración de agentes que encuentres:
    - `.windsurfrules`, archivos en `.windsurf/rules/`, `global_rules.md` o cualquier equivalente
    - Carpetas `.windsurf/`, `.cursor/`, `.github/copilot/` o similares
    - Cualquier archivo `.md` que parezca instrucción para un agente
2. Identifica las skills o reglas existentes:
    - Archivos con convenciones de código
    - Guías de arquitectura documentadas
    - Reglas de testing o CI/CD ya definidas
3. Mapea la arquitectura actual del proyecto:
    - Stack tecnológico real (lenguajes, frameworks, bases de datos)
    - Estructura de carpetas y capas arquitectónicas
    - Patrones de diseño que ya se usan
4. Detecta configuraciones de MCP existentes (`~/.codeium/windsurf/mcp_config.json` o equivalente)

PRESENTA un resumen de lo encontrado antes de continuar. Espera mi confirmación.

---

## FASE 2 — Diseño de la migración (propuesta, sin ejecutar)

Con base en el análisis, propone la estructura completa del nuevo stack:

### Reglas de Windsurf (`.windsurf/rules/`)

Rediseña las reglas del agente con:

- Identidad y rol del agente para este proyecto
- Stack tecnológico real detectado (no genérico)
- Principios arquitectónicos que ya aplica el proyecto
- Convenciones de código existentes (migradas, no inventadas)
- Reglas de testing adaptadas al stack real
- Flujo de trabajo con Cascade y PRs
- Prohibiciones basadas en los antipatrones detectados en el código
- Referencia al Skills Registry
- Cada archivo de regla: máximo 12.000 caracteres (límite de Windsurf)

### Skills Registry

Define qué skills necesita este proyecto específico:

- Una skill por dominio tecnológico relevante (solo los que usa el proyecto)
- `ROUTER.md` con tabla de detección de tareas
- Migra el conocimiento útil de las configuraciones actuales a las skills correspondientes
- Descarta instrucciones redundantes o contradictorias

### SDD Orchestrator

- Pipeline adaptado al tipo de proyecto (API, frontend, mobile, microservicio)
- Agentes necesarios con sus contratos de input/output
- Definición de los dos Human Gates con criterios específicos para este proyecto
- Verifier con checks relevantes al stack real

PRESENTA la propuesta completa. Espera mi aprobación antes de escribir archivos.

---

## FASE 3 — Implementación (solo tras aprobación explícita)

Crea la siguiente estructura de archivos, preservando todo el conocimiento útil del proyecto y descartando solo lo redundante:

```
.windsurf/
├── rules/
│   ├── general.md               ← reglas base del proyecto (trigger: always)
│   ├── stack.md                 ← stack tecnológico y convenciones (trigger: always)
│   ├── testing.md               ← reglas de testing (trigger: agent_requested)
│   ├── architecture.md          ← principios arquitectónicos (trigger: agent_requested)
│   └── workflow.md              ← flujo de trabajo y PRs (trigger: agent_requested)
├── skills/
│   ├── ROUTER.md                ← router de skills
│   ├── [skill-1].md             ← según stack detectado
│   ├── [skill-2].md
│   └── ...
└── orchestrator/
    ├── ORCHESTRATOR.md          ← director del pipeline
    ├── agents/
    │   ├── explorer.md
    │   ├── proposer.md
    │   ├── spec-writer.md
    │   ├── designer.md
    │   ├── task-planner.md
    │   ├── implementer.md
    │   ├── verifier.md
    │   └── archiver.md
    └── gates/
        ├── gate-1-design.md
        └── gate-2-impl.md
```

### Formato de reglas con frontmatter

Cada archivo en `.windsurf/rules/` debe incluir frontmatter YAML:

```yaml
---
trigger: always | agent_requested | manual
description: Descripción breve de la regla
---
```

- `always`: se incluye en cada interacción con Cascade
- `agent_requested`: Cascade lo activa cuando lo considere relevante
- `manual`: solo se activa explícitamente por el usuario

### Reglas de escritura obligatorias:

- Cada archivo de regla: máximo 12.000 caracteres (límite de Windsurf)
- Todo el contenido en español
- Sin instrucciones genéricas que no apliquen al proyecto
- Cada skill: máximo 200 líneas, enfocada en un solo dominio
- `ROUTER.md`: tabla clara de detección por palabras clave en la tarea
- Cada agente del orchestrator: contrato explícito de input esperado y output que produce
- Preserva toda convención de código existente que tenga sentido
- No inventes patrones que el proyecto no usa

---

## FASE 4 — Verificación y cierre

Una vez creados todos los archivos:

1. Lee cada archivo creado y verifica que no haya contradicciones entre ellos
2. Verifica que el `ROUTER.md` cubra todos los tipos de tarea comunes en este proyecto
3. Verifica que el `verifier.md` incluya checks específicos al stack real
4. Verifica que cada archivo de regla respete el límite de 12.000 caracteres
5. Lista cualquier configuración del sistema anterior que NO fue migrada y explica por qué
6. Presenta el resumen final: qué se migró, qué se descartó, qué se mejoró

---

## RESTRICCIONES GLOBALES

- Todo el contenido de los archivos en español
- Nunca sobrescribas archivos existentes sin mostrarme el diff primero
- Si encontrás configuraciones contradictorias entre archivos existentes, presentame las opciones antes de decidir
- Si el proyecto no tiene `.windsurfrules` ni nada similar, indícalo claramente en la Fase 1 y trabaja desde cero
- Preguntame ante cualquier ambigüedad antes de asumir
- Máximo 100 herramientas MCP activas simultáneamente (límite de Cascade)
