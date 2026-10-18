# AI Worker: cómo trabaja un empleado y cómo entrega un equipo

[Languages / 语言](languages.md)

**Max Luo y Morgan Chen, cofundadores de Teloa** · 18 de octubre de 2026

En Teloa, las personas crean **empleados de IA** y trabajan con ellos. **AI Worker es el concepto general del estándar que los describe**: identidad persistente, responsabilidades, contratos de trabajo y límites de colaboración. El estándar también explica cómo los empleados forman equipos y acuerdan sus entregas.

Un nombre y un prompt de puesto permiten empezar a experimentar. Una segunda asignación plantea preguntas concretas: ¿es el mismo empleado de antes?, ¿qué material utilizó?, ¿qué recibió el siguiente miembro?, ¿quién decide adoptar el informe revisado?

El estándar hace explícitos esos acuerdos para comprobar qué asume y qué entrega un empleado, usando conceptos técnicos compartidos entre productos.

Esta guía presenta **AI Worker Standard 1.0**, incluido el núcleo y el perfil de trabajo. Los ejemplos son didácticos, no resultados de clientes ni pruebas de ejecución. Las [especificaciones chinas](../spec/core.zh-CN.md) son la referencia normativa; existe una [traducción completa al español](../spec/core.es.md). Esta traducción no ha recibido revisión completa por especialistas de habla nativa. Los diagramas se muestran en inglés, con explicación completa en español. Los ejemplos JSON conservan los valores originales para facilitar su comprobación entre idiomas.

Los tres conceptos principales se distinguen así:

| Concepto | Definición | Pregunta que responde |
| --- | --- | --- |
| **AI Worker** | Entidad de trabajo de IA con identidad persistente que asume responsabilidades, acepta trabajo y entrega resultados verificables dentro de un límite explícito de autorización | ¿Quién es este empleado, qué hace y cómo se comprueba la finalización? |
| **AI Team** | Composición organizada para una entrega común, con responsabilidades, traspasos y requisitos de aceptación explícitos | ¿Cómo completan varios empleados una tarea juntos? |
| **AI Organization (AI Org)** | Composición de empleados y equipos que añade responsabilidad organizativa, límites de recursos e información y gobernanza de cambios | ¿Quién responde entre equipos y cómo se gestionan recursos y cambios? |

La identidad persistente permite identificar al mismo trabajador entre tareas. No exige funcionamiento continuo, máquina exclusiva ni memoria a largo plazo. La responsabilidad expresa atribución y trazabilidad, no personalidad jurídica. Texto, tablas y pasos explican las relaciones; las imágenes ofrecen una visión visual del mismo contenido.

## 01 Qué compone un empleado de IA

Considere un empleado encargado de resumir documentos. Resume el material recibido y enumera preguntas pendientes. Puede repetir ese trabajo con modelos distintos. Cada resultado sigue perteneciendo al mismo empleado, representado por una instancia Worker concreta, y registra la revisión de definición utilizada.

![Correspondencia de los empleados de IA de Teloa y Grok Bot con AI Worker y sus cinco grupos de componentes](../diagrams/en/worker.png)

*Relación entre nombres de producto, AI Worker y sus cinco grupos de componentes.*

Las flechas expresan correspondencia conceptual: Teloa conserva empleado de IA y Grok conserva Bot. Ambos pueden describirse mediante AI Worker; el diagrama no demuestra una adaptación terminada ni certificación de proveedores.

**Los cinco grupos de componentes de un empleado de IA** describen qué debe acordar su puesto. Los recursos dependen del trabajo; no necesita todas las herramientas disponibles.

| Grupo | Significado general | Configuración del empleado de resúmenes |
| --- | --- | --- |
| **Puesto y responsabilidades** | Identidad persistente, misión, límites, decisiones autónomas, confirmación y escalamiento | Resumir fuentes, señalar contradicciones sin inventar hechos |
| **Métodos y fundamentos** | Skills aportan métodos; conocimiento aporta fuentes; SOP define pasos, controles y excepciones, con versiones y procedencia separadas | Usar el material actual; añadir Skills, conocimiento o SOP si hace falta |
| **Trabajo y planificación** | Contratos con objetivos, I/O, dependencias, responsabilidad y aceptación; rutinas que indican cuándo invocar un trabajo | Interfaz `summarize`, ejecutada por asignación, sin rutina programada |
| **Condiciones de ejecución** | Aplicaciones, herramientas, ámbito de datos, autorización efectiva, modelos, presupuesto y espacio de trabajo | Leer material autorizado; pedir confirmación separada para publicar |
| **Experiencia y resultados** | Reglas de conservación, uso y retirada de memoria; resultados y pruebas con versiones fijas y aceptación | Entregar resumen y preguntas; el responsable los contrasta con la fuente |

Memoria, aplicaciones externas y horarios son opcionales. Las dependencias siguen el compromiso de trabajo. Investigar fuera requiere conexión y autorización; resumir texto aportado no necesita una aplicación solo para completar una lista.

Una definición Worker completa requiere identidad y revisión, responsabilidades y límites de decisión, y al menos un contrato con I/O, responsable y aceptación. El uso real añade vinculación de instancia, condiciones y autorización fiable. Los cinco grupos cubren identidad, responsabilidad, Skills, conocimiento, SOP, plantillas de trabajo, rutinas, aplicaciones, permisos, condiciones de ejecución, experiencia, resultados y calidad. Los campos se definen en el [perfil de trabajo](../spec/work-profile.es.md).

## 02 Un equipo necesita traspasos trazables

Para revisar material, un empleado analista lee las pruebas y otro comprueba las conclusiones. El equipo entrega informe y revisión. El chat facilita comunicación; el contrato también especifica entrega común, responsabilidades, condiciones de traspaso y parada.

**Un AI Team combina objetivo común, miembros y funciones, límites de contexto compartido, traspasos, planificación, restricciones de recursos y aceptación.**

| Elemento | Acuerdo necesario |
| --- | --- |
| Objetivo común | Qué trabajo acepta el equipo y qué entrega externamente |
| Miembros y funciones | Responsabilidades y participación permanente o como consultor por tarea |
| Contexto compartido | Qué material y versión puede recibir cada miembro; sin compartir por defecto toda la conversación o memoria privada |
| Traspasos | Quién envía qué resultado fijo, a quién, con qué mapeo y confirmación o rechazo |
| Planificación | Quién distribuye entradas y reúne salidas: el anfitrión o un coordinador expresamente asignado |
| Recursos | Autorización, aplicaciones, presupuesto, parada y escalamiento por falta de progreso |
| Aceptación | Salidas que se comprueban, responsable, versión y necesidad de independencia |

El coordinador es opcional; no todo equipo necesita un gerente. Lista de miembros, chat y organigrama no sustituyen los acuerdos.

La composición tiene su propia interfaz. Por ejemplo, `deliver` recibe `evidence` y devuelve `report`, `evidence` y `review`. Coordinador o anfitrión identifica quién recibe cada entrada, qué resultado fijo aporta cada salida y quién comprueba que todo esté completo. Sin esos mapeos, el trabajo no está preparado.

![Empleados de IA de Teloa colaboran conforme al contrato Team; las personas aprueban y aceptan](../diagrams/en/delivery.png)

*Colaboración del equipo y responsabilidad humana de aprobación y aceptación.*

El ejemplo tiene seis pasos:

1. **El responsable humano asigna el trabajo.** Especifica objetivo, entradas, autorización, interfaz de equipo y requisitos de entrega y aceptación.
2. **Anfitrión o coordinador valida y distribuye.** Comprueba contratos, dependencias y vínculos; reparte solo material permitido y registra pertenencia de tareas.
3. **El analista entrega el resultado.** Ejecuta lo asignado y fija versiones del informe, pruebas y contribuyentes reales.
4. **El revisor confirma la recepción y revisa.** Comprueba entradas, las acepta y fija su opinión. Si rechaza, registra motivos y devuelve para coordinación.
5. **Anfitrión o coordinador comprueba integridad.** Reúne fuentes fijas según el contrato, comprueba salidas y conserva contribuyentes y traspasos.
6. **El responsable humano acepta y decide adoptar.** Acepta una versión concreta y registra por separado su uso de negocio; la aceptación no autoriza acciones externas.

El ejemplo separa análisis, revisión y aceptación humana; cada equipo sigue su contrato. Se conserva la cadena **entrada autorizada → informe fijo → revisión → paquete final**.

Este fragmento procede del [ejemplo completo de composición](../examples/composition-fixture.json):

```json
{
  "id": "review-report",
  "from": "member/analyst/inspect",
  "to": "member/quality/review",
  "when": "已有固定报告与证据且获得本项交接授权",
  "inputs": {
    "report": "report",
    "evidence": "evidence"
  }
}
```

Las claves de inputs son campos receptores; los valores son campos emisores. La condición china exige informe y pruebas fijos y autorización para ese traspaso. La definición no asigna tareas. Un traspaso real registra actores, Run origen, versión, material permitido y aceptación o rechazo. Un mensaje en un grupo no demuestra que el receptor obtuviera entradas utilizables.

El revisor del diagrama aporta una opinión; el responsable designado acepta el conjunto. La independencia se comprueba frente a contribuyentes reales. Si el revisor edita el informe, contribuye a esa revisión y no puede aceptarla de forma independiente.

## 03 Qué añade una AI Organization

Una organización compone equipos y trabajadores con responsabilidad, límites y reglas de cambio entre ellos. Más miembros u otra capa de chats no crea esos acuerdos.

**Una AI Org combina individuos y equipos, responsabilidad entre equipos, límites de recursos e información y reglas de escalamiento y cambio.**

| Elemento | Qué añade a los equipos |
| --- | --- |
| Composición | Miembros y revisiones fijas; el subequipo participa mediante sus interfaces sin exponer todas sus capacidades |
| Responsabilidad | Principal fiable responsable del ámbito y destinos de escalamiento entre equipos |
| Recursos e información | Qué restricciones de datos, presupuesto y ejecución aplica el anfitrión a cada miembro |
| Escalamiento y cambios | Nombramiento, retirada, cambios, suspensión, recuperación y trabajo en curso |

![Composición Worker, AI Team y AI Org con empleados de IA de Teloa](../diagrams/en/composition.png)

*Individuos, equipos, gobernanza e instancia de calidad compartida.*

Una organización SOC puede combinar investigación e ingeniería de detección y compartir un empleado de calidad. Compartir requiere vincular explícitamente la misma instancia. Referenciar dos veces una plantilla no demuestra que sea el mismo empleado.

La organización determina quién responde, quién nombra o retira miembros, cómo se trata el trabajo pausado y qué límites de datos o presupuesto afectan a cada miembro. Un empleado en dos equipos no gana automáticamente ambos historiales ni duplica el presupuesto compartido.

Es una estructura expresable; operar con ella exige pruebas de vinculación, autorización y ejecución. Se distingue del fixture descargable: sus equipos A y B reutilizan analista y revisor para ilustrar vinculaciones, sin configurar un puesto real de ingeniería de detección.

## 04 Definición, instancia y Run

La definición Worker describe puesto y contratos. La instancia Worker corresponde a un empleado concreto en Teloa. Un Run es una ejecución de ese empleado. Actualizar una plantilla conserva las versiones de Runs anteriores; cambiar de modelo no exige transferir la historia a otra identidad.

![Ejemplo Teloa: definición Worker, instancia del empleado, Run y aceptación](../diagrams/en/architecture.png)

*Separación de definición, vinculación, ejecución y entrega.*

**El recorrido técnico es definición → validación del contrato → vinculación del anfitrión → Run → resultados fijos → aceptación → adopción de negocio.**

| Capa | Objeto o etapa | Qué se registra y comprueba |
| --- | --- | --- |
| Definición | Núcleo y perfil | id, revision, kind, referencias fijas, responsabilidades, I/O, dependencias, colaboración y aceptación |
| Configuración | Contratos y capacidades | Existencia de referencias, contratos completos y soporte; una carencia deja el trabajo afectado no preparado |
| Configuración | Instancia Worker | Identidad persistente, material, modelos, conexiones y autorización para ese trabajo |
| Ejecución | Work, tarea y Run | Work es contrato reutilizable; tarea es asignación concreta; Run fija entradas, versiones y ejecutor de una ejecución |
| Entrega | Resultados fijos | Contenido inspeccionable, versiones, pruebas y contribuyentes, además del estado «finalizado» |
| Entrega | Aceptación | Quién comprobó qué versión, contra qué requisitos y con qué conclusión; independencia frente a contribuyentes cuando se exige |
| Decisión | Adopción | Si el responsable usará el resultado; separada de aceptación, sin permiso automático para publicar o contener incidentes |

La revisión 2 del puesto de resúmenes es una definición. Un asistente documental creado con ella es una instancia. Los resúmenes de esta semana y la siguiente son tareas distintas con sus Runs y resultados. Actualizar la plantilla no reescribe Runs ni transfiere resultados a otro empleado.

El núcleo conserva agent y organization. Worker añade el perfil obligatorio a agent; Team añade colaboración a organization; Org añade gobernanza. Las tres formas usan las mismas reglas de referencias y revisiones.

El anfitrión —por ejemplo Teloa— vincula definiciones con instancias, material, conexiones, modelos y principales autorizadores; comprueba entradas y dependencias y registra resultados. Importar no conecta cuentas, concede permisos, inicia ejecución ni habilita rutinas. Validación estructural, preparación, éxito de ejecución, aceptación y adopción necesitan pruebas diferentes.

## 05 Un ejemplo comprobable campo por campo

El [Worker de resúmenes](../examples/document-worker.json) es una definición completa. summarize recibe material como cadena no vacía y devuelve summary no vacío y openQuestions como array de cadenas. La entrada y salida siguientes son ejemplos didácticos escritos a mano; sus valores originales chinos se conservan:

```json
{
  "material": "示例周报：甲项目本周完成三份资料摘要。第二份缺少来源日期，负责人要求补充。材料没有提供工时或客户反馈。"
}
```

```json
{
  "summary": "甲项目本周完成三份资料摘要，其中第二份仍需补充来源日期。",
  "openQuestions": [
    "第二份资料的来源日期是什么？",
    "现有材料未提供工时或客户反馈，无法据此评价效率或满意度。"
  ]
}
```

El material indica tres resúmenes completados, uno sin fecha de fuente, y ausencia de datos de horas o comentarios de clientes. La salida resume esos hechos y pide la fecha faltante; no infiere eficiencia ni satisfacción.

La validación estructural detecta campos ausentes y tipos incorrectos. El responsable comprueba también fidelidad al material. Sin datos de tiempo no hay fundamento para afirmar aumento de eficiencia. Se acepta una versión fija; cambiarla requiere los controles acordados de nuevo.

La misma relación sirve en SOC. Si el material didáctico solo contiene un registro WAF HTTP 403, el informe debe explicar qué demuestra y qué evidencia de backend o terminal falta. No demuestra compromiso ni seguridad total. Investigar más necesita esas fuentes; contener necesita herramientas y autorización. El ejemplo no conecta un entorno real ni ejecuta ataques.

## 06 Correspondencia entre Grok Bot y Teloa

El empleado de IA de Teloa expresa AI Worker como producto; Grok Bot también puede describirse como AI Worker. Se conservan ambos nombres comerciales. El estándar proporciona una definición técnica común, cuya adaptación comprueba responsabilidades, contratos y comportamiento del anfitrión. El diseño público de Grok describe Bots persistentes, Skills y Routines. Permite comparar conceptos; no demuestra conversor existente ni certificación de compatibilidad. [Diseño de Grok](https://x.ai/news/designing-grok-bot)

| Concepto de producto | Correspondencia | Comprobación |
| --- | --- | --- |
| Empleado Teloa; Grok Bot | Definición e instancia Worker | Identidad, responsabilidades, I/O y aceptación |
| Prompt de puesto; Bot profile | instructions y responsibility | Límites concretos sin inferir contratos ausentes del nombre |
| Skills, conocimiento, SOP | resources según finalidad | Versiones, procedencia y dependencias |
| Herramientas o plugins | Capacidades, aplicaciones y conexiones | Autorización efectiva; credenciales en el anfitrión |
| Grupo y proyecto Teloa | Comunicación y contexto | Aún requieren funciones, traspasos y contrato de equipo |
| Grok Team Bot | Worker compartido | Autoridad, conversaciones y rutinas personales separadas |

Team Bot es un Bot utilizado por varias personas, distinto de varios Workers que colaboran. Las rutinas personales permanecen privadas en el anfitrión, fuera de la definición compartida. Solo las plantillas publicadas deliberadamente y sin material privado pueden compartirse; importarlas no las habilita. [Team Bots](https://docs.x.ai/grok-bot/team-bots)

Véase el [documento de mapeo](mappings.md) para correspondencias de campos y casos sin soporte.

## 07 Reutilizar especificaciones existentes

OASF aporta registros de descubrimiento, dominios y taxonomías de habilidades. El estándar conserva los metadatos nativos y versiones; Work define I/O y aceptación. Una categoría taxonómica y un paquete Skill ejecutable son objetos distintos. [OASF Record Guide](https://docs.agntcy.org/oasf/agent-record-guide/)

Oracle Agent Spec aporta referencias de componentes, interfaces y composición. Su adopción conceptual exige convertir referencias, interfaces y semántica para una versión concreta. Renombrar campos no basta para interoperabilidad. [Oracle Agent Spec 26.1.2](https://oracle.github.io/agent-spec/26.1.2/agentspec/language_spec_26_1_2.html)

JSON es el intercambio normativo. YAML es autoría opcional, con conversión restringida: rechaza claves duplicadas, anclas, alias, etiquetas explícitas, números no finitos y múltiples documentos; conserva significado de cadenas y números. revision con valor "1" es cadena; valor 1 numérico falla el tipo. Ambos archivos no deben ser fuentes editables independientes de la misma revisión.

## 08 Empezar con un trabajo real

Elija un trabajo existente y defina entradas, salidas, dependencias, responsabilidad y aceptación. Tras vincular instancias reales, compruebe material efectivamente recibido y resultados fijos antes de ampliar a un equipo. Si el anfitrión no aplica una restricción crítica, marque el trabajo afectado como no preparado.

La adopción sectorial necesita pruebas de interoperabilidad independientes, casos ajenos a SOC y debate público de cambios. El repositorio utiliza Apache-2.0. Una ambigüedad o contraejemplo concreto es una buena contribución. [Repositorio](https://github.com/teloa-ai/ai-worker-standard) · [Issues](https://github.com/teloa-ai/ai-worker-standard/issues)
