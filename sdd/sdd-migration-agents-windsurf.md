# Stack Cognitivo Completo — Windsurf

Eres un arquitecto de software senior especializado en flujos de desarrollo agéntico con IA. Tu tarea es analizar este proyecto y construir un stack cognitivo completo y autónomo en Windsurf/Cascade.

Al finalizar, cuando el usuario pida cualquier feature, componente, refactor o tarea, Cascade deberá:
1. Leer el contexto automáticamente desde `.windsurfrules`
2. Enrutar la solicitud al agente correcto sin intervención manual
3. Ejecutar el pipeline completo con los Human Gates definidos
4. Invocar la skill correcta según el dominio de la tarea

---

## FASE 1 — Exploración (no toques nada todavía)

Antes de escribir una sola línea, analiza en profundidad:

1. Lee todos los archivos de configuración de agentes que encuentres:
   - `.windsurfrules`, `CLAUDE.md`, `AGENT.md`, `.cursorrules`, `copilot-instructions.md`, o cualquier equivalente
   - Carpetas `.windsurf/`, `.claude/`, `.cursor/`, `.github/` o similares
   - Cualquier archivo `.md` que parezca instrucción para un agente
2. Identifica el conocimiento existente:
   - Archivos con convenciones de código
   - Guías de arquitectura documentadas
   - Reglas de testing o CI/CD ya definidas
3. Mapea la arquitectura actual del proyecto:
   - Stack tecnológico real (lenguajes, frameworks, bases de datos)
   - Estructura de carpetas y capas arquitectónicas
   - Patrones de diseño que ya se usan
4. Detecta configuraciones de MCP existentes

PRESENTA un resumen de lo encontrado antes de continuar. Espera mi confirmación.

---

## FASE 2 — Diseño de la migración (propuesta, sin ejecutar)

Con base en el análisis, propone la estructura completa. Para cada archivo, muestra el esquema de contenido (no el contenido final todavía).

### A. `.windsurfrules` — cerebro central del sistema

Este es el archivo más crítico. Windsurf lo inyecta automáticamente en cada conversación de Cascade. Debe contener:

- Identidad y rol del agente para este proyecto
- Stack tecnológico real detectado
- Principios arquitectónicos del proyecto
- Convenciones de código existentes
- **Protocolo de enrutamiento automático**: instrucciones explícitas para que Cascade, al recibir cualquier solicitud, identifique el tipo de tarea y lea el agente correspondiente de `.windsurf/orchestrator/agents/` antes de actuar
- **Trigger de orquestación**: cuando el usuario pida una feature, componente, endpoint, pantalla, refactor, bug fix o cualquier tarea de desarrollo, Cascade DEBE iniciar el pipeline leyendo `ORCHESTRATOR.md` y siguiendo sus instrucciones
- Reglas de testing adaptadas al stack real
- Prohibiciones basadas en antipatrones detectados
- Máximo 500 líneas

**Ejemplo del bloque de enrutamiento que debe incluir `.windsurfrules`:**
```
## PROTOCOLO DE ORQUESTACIÓN AUTOMÁTICA

Ante cualquier solicitud de desarrollo (feature, componente, endpoint, pantalla, refactor, bug fix):

1. Lee `.windsurf/orchestrator/ORCHESTRATOR.md` para entender el pipeline completo
2. Identifica el tipo de tarea usando la tabla del ROUTER
3. Lee el agente correspondiente en `.windsurf/orchestrator/agents/`
4. Lee la skill correspondiente en `.windsurf/skills/`
5. Ejecuta el pipeline respetando los Human Gates
6. NUNCA escribas código sin haber pasado por el Gate 1

Tipos de tarea y su enrutamiento:
- "nueva feature / componente / pantalla" → explorer → proposer → Gate 1 → spec-writer → designer → task-planner → Gate 2 → implementer → verifier → archiver
- "refactor / migración" → explorer → proposer → Gate 1 → task-planner → implementer → verifier
- "bug fix" → explorer → implementer → verifier
- "documentación" → archiver
```

### B. Skills Registry — `.windsurf/skills/`

Define qué skills necesita este proyecto (solo las que aplican al stack real):

- Una skill por dominio tecnológico detectado
- Cada skill contiene: contexto del dominio, patrones a seguir, antipatrones a evitar, ejemplos reales del proyecto
- `ROUTER.md` con tabla de detección: columnas `Palabras clave en la solicitud | Skill a cargar | Agente principal`
- Las skills NO son workflows invocables con `/comando` — son archivos de conocimiento que el orchestrator lee e inyecta en contexto

### C. Orchestrator — `.windsurf/orchestrator/`

Pipeline completo adaptado al tipo de proyecto:

**`ORCHESTRATOR.md`** — director del pipeline. Contiene:
- Descripción del flujo completo de principio a fin
- Tabla de agentes con su rol, input esperado y output que produce
- Definición de los dos Human Gates con criterios específicos para este proyecto
- Instrucción explícita de cuándo leer cada skill

**Agentes necesarios** (en `.windsurf/orchestrator/agents/`):
- `explorer.md` — analiza el codebase y el contexto de la tarea
- `proposer.md` — propone el enfoque y las opciones de implementación
- `spec-writer.md` — escribe la especificación técnica detallada
- `designer.md` — define la arquitectura y contratos de la solución
- `task-planner.md` — divide la implementación en tareas atómicas
- `implementer.md` — ejecuta la implementación siguiendo las tareas
- `verifier.md` — verifica contra los criterios del stack real
- `archiver.md` — documenta decisiones y actualiza el contexto del proyecto

**Gates** (en `.windsurf/orchestrator/gates/`):
- `gate-1-design.md` — criterios de aprobación antes de implementar (checklist específico al proyecto)
- `gate-2-impl.md` — criterios de aprobación antes de mergear (tests, coverage, convenciones)

PRESENTA la propuesta completa con el esquema de cada archivo. Espera mi aprobación antes de escribir.

---

## FASE 3 — Implementación (solo tras aprobación explícita)

> **Instrucción para el usuario:** Antes de dar la aprobación, activa Plan Mode en Cascade. Cascade usará Plan Mode durante toda esta fase.

Crea la siguiente estructura completa:

```
.windsurfrules                           ← cargado automáticamente por Windsurf en cada sesión

.windsurf/
├── skills/
│   ├── ROUTER.md                        ← tabla de enrutamiento skill por tipo de tarea
│   ├── [skill-dominio-1].md             ← según stack detectado
│   ├── [skill-dominio-2].md
│   └── ...
└── orchestrator/
    ├── ORCHESTRATOR.md                  ← director del pipeline completo
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

### Reglas de escritura obligatorias

- `.windsurfrules`: máximo 500 líneas, todo en español, debe incluir el bloque de enrutamiento automático completo
- Cada skill: máximo 200 líneas, un solo dominio, ejemplos reales del proyecto
- `ROUTER.md`: tabla con tres columnas — palabras clave, skill a cargar, agente principal
- `ORCHESTRATOR.md`: el flujo debe ser auto-contenido — Cascade debe poder ejecutar el pipeline completo leyendo solo este archivo más los agentes referenciados
- Cada agente: contrato explícito — sección `## INPUT` con lo que recibe, sección `## OUTPUT` con lo que produce, sección `## PROCESO` con los pasos
- Cada gate: checklist binario (✅/❌) con criterios específicos al stack real del proyecto
- Todo en español
- Preserva toda convención existente que tenga sentido
- No inventes patrones que el proyecto no usa

### Cómo debe funcionar al terminar

Cuando el usuario escriba en Cascade:

> "Quiero agregar autenticación con JWT"

Cascade debe automáticamente:
1. Detectar que es una nueva feature (por el protocolo en `.windsurfrules`)
2. Leer `ORCHESTRATOR.md`
3. Invocar `explorer.md` → leer contexto del proyecto
4. Invocar `proposer.md` → proponer enfoque
5. **Pausar en Gate 1** → presentar propuesta y esperar aprobación del usuario
6. Invocar `spec-writer.md` + `designer.md` → con la skill de autenticación si existe
7. Invocar `task-planner.md` → lista de tareas atómicas
8. **Pausar en Gate 2** → presentar plan y esperar aprobación
9. Invocar `implementer.md` → escribir código
10. Invocar `verifier.md` → verificar contra criterios del proyecto
11. Invocar `archiver.md` → documentar decisión

Sin que el usuario haya escrito ningún `/comando`.

---

## FASE 4 — Verificación y cierre

Una vez creados todos los archivos:

1. Lee `.windsurfrules` completo y verifica que el bloque de enrutamiento esté presente y sea correcto
2. Simula mentalmente el flujo: "el usuario pide una nueva feature" → traza el camino completo y verifica que cada archivo referenciado existe
3. Verifica que `ROUTER.md` cubra todos los tipos de tarea comunes del proyecto
4. Verifica que `verifier.md` incluya checks específicos al stack real (no genéricos)
5. Verifica que cada gate tenga criterios binarios y medibles
6. Lista cualquier configuración anterior que NO fue migrada y explica por qué
7. Presenta el resumen final: qué se construyó, qué enruta a qué, cómo probarlo

---

## RESTRICCIONES GLOBALES

- Todo el contenido de los archivos en español
- No uses Plan Mode en las fases 1 y 2
- La Fase 3 requiere que el usuario active Plan Mode manualmente antes de aprobar
- Nunca sobrescribas archivos existentes sin mostrarme el diff primero
- Si encontrás configuraciones contradictorias entre archivos existentes, presentame las opciones antes de decidir
- Si el proyecto no tiene configuración de agente previa, indícalo en la Fase 1 y construye desde cero
- El sistema debe ser autónomo: el usuario nunca debería tener que escribir un slash command para iniciar el pipeline de una feature
- Preguntame ante cualquier ambigüedad antes de asumir