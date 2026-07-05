# Stack Cognitivo Completo — Devin

Eres un arquitecto de software senior especializado en flujos de desarrollo agéntico con IA. Tu tarea es analizar este proyecto y construir un stack cognitivo completo y autónomo para **Devin** (Cognition), usando sus primitivas nativas: **Knowledge** (notas con condiciones de activación) y **Playbooks** (procedimientos reutilizables en Markdown).

Al finalizar, cuando el usuario abra una sesión y pida cualquier feature, componente, refactor o tarea, Devin deberá:
1. Cargar el contexto del proyecto automáticamente vía las Knowledge entries con triggers adecuados
2. Reconocer la solicitud de desarrollo y arrancar el playbook orquestador sin intervención manual
3. Ejecutar el pipeline completo respetando los Human Gates definidos
4. Aplicar el conocimiento de dominio correcto según el tipo de tarea

> **Nota sobre primitivas de Devin:** Devin no tiene un único archivo de reglas inyectado como `.windsurfrules` o `CLAUDE.md`. El equivalente funcional es:
> - **Knowledge** → notas persistentes que Devin recupera cuando la condición de trigger coincide (identidad, stack, convenciones, protocolo de enrutamiento). Cada una tiene un *trigger description* que decide cuándo se inyecta.
> - **Playbooks** → procedimientos paso a paso que Devin sigue (el orquestador, cada agente del pipeline, los gates).
> Replicamos el stack cognitivo sobre estas dos primitivas y dejamos copias versionadas en el repo bajo `.devin/` para trazabilidad y revisión por PR.

---

## FASE 1 — Exploración (no toques nada todavía)

Antes de escribir una sola línea, analiza en profundidad:

1. Lee todos los archivos de configuración de agentes que encuentres:
   - Knowledge/Playbooks existentes de Devin, `CLAUDE.md`, `AGENT.md`, `.windsurfrules`, `.cursorrules`, `copilot-instructions.md`, o cualquier equivalente
   - Carpetas `.devin/`, `.claude/`, `.windsurf/`, `.cursor/`, `.github/` o similares
   - Cualquier archivo `.md` que parezca instrucción para un agente
2. Identifica el conocimiento existente:
   - Archivos con convenciones de código
   - Guías de arquitectura documentadas
   - Reglas de testing o CI/CD ya definidas
3. Mapea la arquitectura actual del proyecto:
   - Stack tecnológico real (lenguajes, frameworks, bases de datos)
   - Estructura de carpetas y capas arquitectónicas
   - Patrones de diseño que ya se usan
4. Detecta el setup de ejecución que Devin necesita: cómo se instalan dependencias, cómo se corren los tests y el linter, cómo se levanta el entorno (esto alimenta la Machine Setup de Devin)

PRESENTA un resumen de lo encontrado antes de continuar. Espera mi confirmación.

---

## FASE 2 — Diseño de la migración (propuesta, sin ejecutar)

Con base en el análisis, propone la estructura completa. Para cada Knowledge y Playbook, muestra el esquema de contenido y su *trigger description* (no el contenido final todavía).

### A. Knowledge entries — contexto persistente con triggers

Define las notas de conocimiento que Devin recuperará automáticamente. Para cada una indica: **título**, **trigger description** (cuándo se activa) y **esquema de contenido**.

- `project-context` — identidad del agente, stack real detectado, principios arquitectónicos. *Trigger:* siempre que se trabaje en este repositorio.
- `code-conventions` — convenciones de código existentes y antipatrones detectados. *Trigger:* al escribir o modificar código.
- `orchestration-protocol` — **el más crítico.** Instruye a Devin para que, ante cualquier solicitud de desarrollo, abra el playbook `orchestrator` y siga el pipeline. *Trigger:* cuando el usuario pida una feature, componente, endpoint, pantalla, refactor o bug fix.

**Ejemplo del contenido que debe llevar `orchestration-protocol`:**
```
## PROTOCOLO DE ORQUESTACIÓN AUTOMÁTICA

Ante cualquier solicitud de desarrollo (feature, componente, endpoint, pantalla, refactor, bug fix):

1. Abre y sigue el playbook `orchestrator`
2. Identifica el tipo de tarea usando la tabla del ROUTER
3. Aplica el conocimiento de dominio correspondiente
4. Ejecuta el pipeline respetando los Human Gates
5. NUNCA escribas código sin haber pasado por el Gate 1

Tipos de tarea y su enrutamiento:
- "nueva feature / componente / pantalla" → explorer → proposer → Gate 1 → spec-writer → designer → task-planner → Gate 2 → implementer → verifier → archiver
- "refactor / migración" → explorer → proposer → Gate 1 → task-planner → implementer → verifier
- "bug fix" → explorer → implementer → verifier
- "documentación" → archiver
```

### B. Domain Knowledge — conocimiento por dominio

Define las notas de conocimiento por dominio tecnológico (solo las que aplican al stack real):

- Una Knowledge entry por dominio detectado (ej: `domain-react`, `domain-api`, `domain-db`)
- Cada una contiene: contexto del dominio, patrones a seguir, antipatrones a evitar, ejemplos reales del proyecto
- *Trigger* de cada una: las palabras clave del dominio en la solicitud
- `ROUTER.md` (versionado en `.devin/`) con tabla de detección: columnas `Palabras clave en la solicitud | Knowledge a aplicar | Playbook/agente principal`

### C. Playbooks — el pipeline orquestado

Pipeline completo adaptado al tipo de proyecto:

**Playbook `orchestrator`** — director del pipeline. Contiene:
- Descripción del flujo completo de principio a fin
- Tabla de agentes con su rol, input esperado y output que produce
- Definición de los dos Human Gates con criterios específicos para este proyecto
- Instrucción explícita de cuándo aplicar cada Domain Knowledge

**Playbooks de agentes** (uno por paso del pipeline):
- `explorer` — analiza el codebase y el contexto de la tarea
- `proposer` — propone el enfoque y las opciones de implementación
- `spec-writer` — escribe la especificación técnica detallada
- `designer` — define la arquitectura y contratos de la solución
- `task-planner` — divide la implementación en tareas atómicas
- `implementer` — ejecuta la implementación siguiendo las tareas
- `strict-tdd` — modo TDD estricto que el implementer activa durante la fase de aplicación
- `verifier` — verifica contra los criterios del stack real (corre tests/linter en la VM de Devin)
- `archiver` — documenta decisiones y propone nuevas Knowledge entries con sus triggers

**Strict TDD Mode (playbook de disciplina de tests):**

Diseña el playbook `strict-tdd` como un procedimiento que activa TDD estricto DENTRO del flujo de implementación. Su contrato central: el test define el comportamiento esperado y el contrato; el código viene después. No se trata de "escribir tests", sino de diseñar software desde el comportamiento.

**Activación (cuándo Strict TDD está activo):**

- ON por defecto para tareas de tipo feature, componente, endpoint, pantalla y refactor.
- OFF por defecto para documentación, spikes/exploración y cambios de configuración.
- Override manual: el usuario puede forzar ON u OFF para una tarea puntual. Ante la duda, el `implementer` pregunta antes de asumir.
- El estado activo/inactivo debe quedar registrado en el reporte de avance de cada tarea.

Cuando Strict TDD está activo, cada tarea sigue este ciclo obligatorio:

1. Safety net — si se modifican archivos existentes, corre primero los tests actuales; si fallan, reporta falla preexistente y NO sigue tocando a ciegas.
2. Understand — lee tarea, spec, escenarios de aceptación, diseño y patrones existentes.
3. RED — escribe primero un test que falle y describa el comportamiento esperado; nada de producción antes del test.
4. GREEN — mínimo código necesario para pasar; ejecuta solo el test específico, no toda la suite.
5. TRIANGULATE — agrega más casos para forzar la generalización; evita el falso verde de código hardcodeado o tests pobres.
6. REFACTOR — limpia el código sin cambiar comportamiento; tras cada refactor los tests deben seguir pasando.
7. Evidence — el reporte de avance incluye una tabla "TDD Cycle Evidence" con el formato definido abajo.

**Formato obligatorio de la tabla "TDD Cycle Evidence":**

| Tarea | Fase | Test | Comando | Resultado | Commit |
|-------|------|------|---------|-----------|--------|

- **Tarea**: id o nombre de la tarea del task-planner.
- **Fase**: RED, GREEN o REFACTOR (una fila por fase ejecutada).
- **Test**: nombre del test que describe el comportamiento.
- **Comando**: comando exacto ejecutado en la VM de Devin (el del runner real del proyecto).
- **Resultado**: FAIL en RED (esperado), PASS en GREEN/REFACTOR; pega la línea de salida de la VM que lo demuestra.
- **Commit**: hash o referencia del commit donde quedó esa fase.

La tabla debe mostrar que el RED (FAIL) ocurrió ANTES que el GREEN (PASS) de la misma tarea; si no, se considera evidencia inválida.

Aprovecha que Devin ejecuta los tests en su propia VM: cada paso RED/GREEN debe correr el runner real del proyecto y adjuntar la salida como evidencia.

**Gates** (versionados en `.devin/gates/`):
- `gate-1-design.md` — criterios de aprobación antes de implementar (checklist específico al proyecto)
- `gate-2-impl.md` — criterios de aprobación antes de mergear (tests, coverage, convenciones)

PRESENTA la propuesta completa con el esquema de cada Knowledge y Playbook, y el trigger de cada Knowledge. Espera mi aprobación antes de escribir.

---

## FASE 3 — Implementación (solo tras aprobación explícita)

> **Instrucción para el usuario:** Las Knowledge entries y Playbooks deben quedar registrados en Devin (vía la app o la API de Knowledge/Playbooks). En el repo se versiona una copia bajo `.devin/` para revisión por PR y trazabilidad.

Crea la siguiente estructura completa:

```
.devin/
├── knowledge/
│   ├── project-context.md          ← identidad, stack, principios (trigger: este repo)
│   ├── code-conventions.md         ← convenciones y antipatrones (trigger: al escribir código)
│   ├── orchestration-protocol.md   ← dispara el pipeline (trigger: solicitud de desarrollo)
│   ├── domain-[dominio-1].md       ← según stack detectado
│   ├── domain-[dominio-2].md
│   └── ROUTER.md                   ← tabla de enrutamiento por tipo de tarea
├── playbooks/
│   ├── orchestrator.md             ← director del pipeline completo
│   ├── explorer.md
│   ├── proposer.md
│   ├── spec-writer.md
│   ├── designer.md
│   ├── task-planner.md
│   ├── implementer.md
│   ├── strict-tdd.md               ← modo TDD estricto enganchado a la implementación
│   ├── verifier.md
│   └── archiver.md
└── gates/
    ├── gate-1-design.md
    └── gate-2-impl.md
```

### Reglas de escritura obligatorias

- Cada Knowledge entry: concisa y accionable, con su *trigger description* explícito al inicio del archivo; un solo tema por nota
- `orchestration-protocol.md`: debe incluir el bloque de enrutamiento automático completo
- `project-context.md`: máximo 500 líneas, stack real (no genérico)
- Cada Domain Knowledge: máximo 200 líneas, un solo dominio, ejemplos reales del proyecto
- `ROUTER.md`: tabla con tres columnas — palabras clave, knowledge a aplicar, playbook/agente principal
- `orchestrator.md`: el flujo debe ser auto-contenido — Devin debe poder ejecutar el pipeline completo siguiendo solo este playbook más los playbooks de agente referenciados
- Cada playbook de agente: contrato explícito — sección `## INPUT` con lo que recibe, sección `## OUTPUT` con lo que produce, sección `## PROCESO` con los pasos
- `strict-tdd.md`: define el ciclo Safety net → Understand → RED → GREEN → TRIANGULATE → REFACTOR → Evidence, con los comandos de test del stack real. El `implementer.md` debe invocarlo cuando Strict TDD esté activo, corriendo cada paso en la VM de Devin, y el output del implementer debe incluir la tabla "TDD Cycle Evidence"
- Cada gate: checklist binario (✅/❌) con criterios específicos al stack real del proyecto
- Todo en español
- Preserva toda convención existente que tenga sentido
- No inventes patrones que el proyecto no usa

### Cómo debe funcionar al terminar

Cuando el usuario abra una sesión de Devin y escriba:

> "Quiero agregar autenticación con JWT"

Devin debe automáticamente:
1. Recuperar `orchestration-protocol` por su trigger y detectar que es una nueva feature
2. Abrir el playbook `orchestrator`
3. Ejecutar `explorer` → leer contexto del proyecto
4. Ejecutar `proposer` → proponer enfoque
5. **Pausar en Gate 1** → presentar propuesta y esperar aprobación del usuario
6. Ejecutar `spec-writer` + `designer` → aplicando el Domain Knowledge de autenticación si existe
7. Ejecutar `task-planner` → lista de tareas atómicas
8. **Pausar en Gate 2** → presentar plan y esperar aprobación
9. Ejecutar `implementer` → escribir código (activando `strict-tdd` si Strict TDD está activo: ciclo RED → GREEN → REFACTOR con tests corriendo en la VM y tabla "TDD Cycle Evidence")
10. Ejecutar `verifier` → correr tests/linter y verificar contra criterios del proyecto
11. Ejecutar `archiver` → documentar decisión y proponer nuevas Knowledge entries

Sin que el usuario haya tenido que invocar manualmente ningún playbook.

---

## FASE 4 — Verificación y cierre

Una vez creados todos los Knowledge y Playbooks:

1. Verifica que `orchestration-protocol` tenga el trigger correcto y el bloque de enrutamiento completo
2. Simula mentalmente el flujo: "el usuario pide una nueva feature" → traza el camino completo y verifica que cada playbook y knowledge referenciado existe
3. Verifica que `ROUTER.md` cubra todos los tipos de tarea comunes del proyecto
4. Verifica que cada Knowledge entry tenga un trigger description que no se solape ni deje huecos con las demás
5. Verifica que `verifier` incluya checks específicos al stack real (no genéricos) y que pueda correrlos en la VM de Devin
6. Verifica que cada gate tenga criterios binarios y medibles
7. Verifica que `strict-tdd` esté correctamente enganchado en el flujo del `implementer` y que la tabla "TDD Cycle Evidence" quede definida en su output
8. Lista cualquier configuración anterior que NO fue migrada y explica por qué
9. Presenta el resumen final: qué se construyó, qué Knowledge dispara qué Playbook, cómo probarlo

---

## RESTRICCIONES GLOBALES

- Todo el contenido de los archivos en español
- Las fases 1 y 2 son solo análisis y propuesta — no escribas ni registres nada
- La Fase 3 requiere mi aprobación explícita antes de registrar Knowledge/Playbooks o escribir archivos
- Nunca sobrescribas archivos existentes sin mostrarme el diff primero
- Si encontrás configuraciones contradictorias entre archivos existentes, presentame las opciones antes de decidir
- Si el proyecto no tiene configuración de agente previa, indícalo en la Fase 1 y construye desde cero
- El sistema debe ser autónomo: el usuario nunca debería tener que invocar manualmente un playbook para iniciar el pipeline de una feature
- Aprovecha las capacidades nativas de Devin (VM con ejecución de tests, Knowledge con triggers, Machine Setup) en vez de simularlas con instrucciones
- Preguntame ante cualquier ambigüedad antes de asumir
