# Perfil de trabajo y colaboración de AI Worker

[Languages / 语言](../docs/languages.md)

**Max Luo y Morgan Chen, cofundadores de Teloa**

Fecha: 2026-10-18. Versión: `1.0`. Anexo normativo del [estándar abierto de definición AI Worker, AI Team y AI Organization 1.0](core.es.md). Define la semántica del trabajo y los requisitos de adaptación, no un nuevo motor de ejecución.

Traducción completa del [texto chino simplificado de referencia](work-profile.zh-CN.md), que prevalece ante discrepancias. No ha recibido una revisión completa por especialistas del sector de habla nativa; se agradecen correcciones. Los nombres de campos y los valores de los ejemplos se conservan para mantener el mismo contrato entre idiomas.

## 1. Un contrato común para Worker, Team y Organization

Un empleado ofrece interfaces de trabajo. Un equipo ofrece sus propias interfaces y explica cómo sus miembros se reparten el trabajo. Una organización compone equipos. Los tres especifican entradas, salidas, dependencias, responsabilidad y aceptación. Una organización no expone automáticamente todos los trabajos de sus miembros, y pertenecer a ella no comparte automáticamente la memoria personal.

El perfil aparece como una extensión obligatoria: `id = urn:teloa:profile:ai-work`, `version = "1.0"`, `required = true`. Es un identificador fijo de esta especificación, no autenticación ni un servicio en línea. Cualquier anfitrión puede implementarlo sin cuenta Teloa. No se debe convertir en optional para eludir semántica crítica. Un archivo que solo contenga campos agent básicos puede declarar intercambio básico, pero no conformidad de una definición AI Worker completa.

Siguen aplicándose las reglas del núcleo sobre cadenas no vacías, LocalId, referencias fijas, importación sin efectos secundarios y extensiones desconocidas. Las estructuras siguientes solo admiten los campos enumerados; todos son obligatorios salvo indicación expresa. Cuando no haya contenido aplicable se usan arrays vacíos explícitos, nunca null. Cada id es único en su propio array; las referencias se interpretan según este anexo.

## 2. Estructura de data en la extensión

| Campo | Tipo | Uso y valor por defecto |
| --- | --- | --- |
| responsibility | Responsibility | Reglas de decisión del puesto o de la composición |
| resources | Resource[] | Catálogo de métodos, material, aplicaciones y políticas; puede estar vacío; incluir una entrada no la convierte en requisito global |
| works | Work[] no vacío | Interfaces de trabajo ofrecidas externamente |
| requires | LocalId[], opcional | Recursos necesarios para todos los trabajos; omitir equivale a vacío |
| routines | Routine[], opcional | Planificación de rutinas; omitir equivale a vacío; no se activan automáticamente |
| authorityRoles | AuthorityRole[], opcional | Roles externos de aprobación, aceptación o responsabilidad; omitir equivale a vacío |
| memoryPolicyRef | LocalId, opcional | Recurso purpose=policy; si existe, es dependencia global; omitir no declara memoria entre tareas |
| collaboration | Collaboration | Obligatorio en organization; prohibido en agent |
| governance | Governance, opcional | Solo organization; obligatorio para declarar una definición AI Org conforme a esta edición |

**Responsibility** contiene cinco arrays obligatorios de cadenas: `triggers`, `autonomousActions`, `confirmationPoints`, `escalationRules`, `deliveryChecks`. Describen cuándo asumir trabajo, acciones autónomas, confirmaciones, escalamiento y controles de entrega. triggers, autonomousActions y deliveryChecks deben ser no vacíos. Los otros dos pueden estar vacíos cuando realmente no haya confirmaciones de negocio o reglas de escalamiento adicionales. Un array vacío de confirmaciones no elimina la autorización del anfitrión; uno de escalamiento vacío no permite continuar sin entradas o permisos. La descripción no otorga permiso de herramientas. Las instructions básicas describen responsabilidades persistentes; Work y recursos aportan detalles. La sustancia del contenido requiere revisión, no basta con cadenas no vacías.

**Resource** contiene `id:LocalId`, `title:cadena no vacía`, `purpose`, `binding` y opcionalmente `requires:LocalId[]`. purpose es `method | knowledge | procedure | application | runtime | policy | trigger | template-reference`. Son etiquetas de uso, no tipos de recurso nuevos que todo producto deba implementar. binding contiene `kind` y `config`, reutilizando las cuatro vinculaciones Capability y sus reglas sin duplicar protocolos.

Resource.requires solo referencia resources de la misma definición; omitir equivale a vacío. Es preferible que el material fijo incluya digest; una fuente mutable debe fijar su versión al usarla. Un método puede referenciar un paquete de habilidades completo y un SOP puede referenciar métodos o procedimientos. Una descripción legible de aplicación, configuración de modelo o política no demuestra conexión, entorno preparado ni política aplicada.

Para runtime, policy, trigger o application expresada como recurso, el informe de adaptación debe registrar formato nativo, versión exacta y responsable de tratamiento explícito. Puede ser una función del anfitrión que soporte el formato o una intervención humana expresamente asignada. Una explicación textual sin responsable de ejecución no satisface una condición necesaria. Se conservan formatos nativos; no se crea otro lenguaje universal de permisos o planificación.

**AuthorityRole** contiene `id:LocalId`, `name:cadena no vacía`, `kind:human | service`. Es una posición de responsabilidad, no una cuenta. El anfitrión debe vincularla a un principal fiable. service es un servicio externo reconocido por el anfitrión, no permiso para que el modelo se nombre aprobador. Cuando se requiere aprobación humana se debe declarar human. La función del rol depende del contrato de aceptación, escalamiento o gobernanza que lo referencia.

## 3. Entradas, salidas y entrega de cada trabajo

| Campo Work | Tipo | Significado |
| --- | --- | --- |
| id, title, instructions | LocalId, cadena no vacía, cadena no vacía | Interfaz, nombre y requisitos de objetivo o método |
| inputs, outputs | Objetos JSON Schema | Describen el objeto completo de entrada y de salida |
| requires | LocalId[] | Recursos necesarios para este trabajo; puede estar vacío |
| responsible | ParticipantRef | Una posición responsable principal |
| acceptance | Acceptance | Controles de finalización y responsabilidad de aceptación |

`ParticipantRef` es `self`, `member/<LocalId>` o `authority/<LocalId>`. self indica la instancia de la definición actual. member solo indica una posición Member directa de la organization actual, incluido un consultant por tarea. authority referencia data.authorityRoles actual. agent no puede usar member. Todas las referencias deben existir; no se adivinan a partir de nombres. responsible debe ser self o member. Las personas externas pueden aceptar o recibir escalamiento, sin hacerse pasar por ejecutores de IA. En organization, self expresa responsabilidad externa de la composición; el anfitrión aún registra ejecutores reales y responsables de repartir entradas y reunir salidas, sin sustituir contribuyentes reales por el nombre de la organización.

**Acceptance** contiene `criteria:array no vacío de cadenas`, `reviewer:ParticipantRef`, `independent:boolean`. criteria son requisitos verificables de finalización; reviewer es la posición responsable de aceptar. Con independent=true, el anfitrión debe demostrar que el revisor real es distinto del ejecutor real del resultado. En resultados compuestos se comprueban todos los contribuyentes reales, no solo el responsable principal o el nombre del equipo. Dos posiciones, sesiones o nombres no demuestran independencia. La independencia tampoco demuestra por sí sola calidad de juicio. El registro de aceptación fija la versión del resultado utilizada en la evaluación; validación estructural, finalización del modelo y adopción de negocio se registran por separado.

La raíz de inputs y outputs debe ser `type:object` y declarar `$schema = https://json-schema.org/draft/2020-12/schema`. Se reutiliza [JSON Schema 2020-12](https://json-schema.org/draft/2020-12), sin otro lenguaje de tipos. Schema puede contener descripciones y `$defs`; todas sus referencias, incluidas `$ref` y `$dynamicRef`, deben apuntar al mismo documento Schema. Un `$id` anidado no puede cambiar ese límite. La importación no resuelve referencias por red. La implementación debe informar vocabularios o restricciones no soportados; no puede ignorarlos y declarar validación satisfactoria. Si format es crítico para la aceptación, su capacidad de comprobación debe declararse y verificarse explícitamente.

Las entradas incompletas se informan como pendientes. Las salidas ausentes o con tipo incorrecto no cumplen el contrato. No se convierten cadenas a números implícitamente, no se truncan decimales y no se transforma false en 0. Schema default es descriptivo, no prueba que el usuario aportara o autorizara ese valor. Los resultados de texto libre pueden usar campos string; acceptance sigue comprobando evidencia y calidad de juicio.

**Comprobación de dependencias:** primero se satisfacen capabilities del núcleo y R05/R06. Después se une data.requires, memoryPolicyRef y Work.requires elegido, expandiendo Resource.requires recursivamente. Una referencia ausente o ciclo de recursos es error de definición. Falta de conexión, permisos o intérprete deja ese trabajo no preparado. Un recurso exclusivo de otro trabajo no bloquea el actual. Una dependencia global ausente sigue bloqueando toda ejecución. Se informa por separado soporte de interpretación, configuración preparada y autorización efectiva; disponibilidad parcial no significa que todo pueda ejecutarse.

## 4. Rutinas y experiencia

**Routine** contiene `id:LocalId`, `work:LocalId`, `policyRef:LocalId` y opcionalmente `triggerRef:LocalId`. work referencia works actual; policyRef apunta a purpose=policy y triggerRef a purpose=trigger. El puesto ejecutor pertenece a la instancia vinculada a la definición. El anfitrión vincula por separado creador, propietario de planificación y principal autorizador real; no tienen que coincidir con Worker. Importar no debe crear otro empleado, ampliar visibilidad de la rutina ni transferirla a otro miembro.

Sin triggerRef, la rutina queda pendiente de planificación. Incluso si existe, solo describe el disparador y la instancia está deshabilitada por defecto. El anfitrión interpreta el formato. En Teloa, daily/weekly o eventos locales solo declaran soporte realmente disponible; no se aproximan silenciosamente cron arbitrarios o eventos externos. Primero se verifican trabajo y disparador reales; después el responsable del despliegue la habilita.

La policy de rutina debe especificar ventana o cursor de entrada, el comportamiento cuando no hay entradas o cuando estas han dejado de ser válidas, deduplicación del disparador y registro del procesamiento de negocio, reintentos, concurrencia y recuperación de ejecuciones omitidas, presupuesto, notificaciones y pausa. Si falta algo, se conserva la planificación pero no se habilita. Un disparo satisfactorio no equivale a procesamiento satisfactorio; el estado de procesamiento solo avanza al alcanzar el resultado acordado. Un identificador de disparo nuevo no puede eludir deduplicación de negocio en reintentos. No existe un número universal de reintentos ni una regla sectorial fija de «dos rondas sin progreso».

memoryPolicyRef solo define conservación, procedencia, alcance, revisión y retirada de experiencia; no transporta la memoria privada del autor. template-reference es material de plantilla con procedencia; un registro histórico no puede presentarse como experiencia vivida por la nueva instancia. Los contextos personal, de negocio, proyecto y grupo siguen separados conforme a autorización. Una instancia compartida por varios equipos no hace que sus historiales sean mutuamente legibles.

## 5. Colaboración y gobernanza organizativa

| Campo Collaboration | Tipo | Significado |
| --- | --- | --- |
| roles | RoleAssignment[] | Cubre cada Member directo exactamente una vez |
| coordinator | ParticipantRef, opcional | Si existe, solo referencia un Member directo actual; omitir no selecciona a nadie |
| handoffs | Handoff[] | Relaciones explícitas de transferencia de material y responsabilidad; puede estar vacío |
| sharing | Sharing[] | Destinatarios esperados de recursos compartidos; puede estar vacío |
| stop | StopPolicy | Colaboración acotada, falta de progreso y escalamiento |

**RoleAssignment** contiene `member:LocalId`, `duty:cadena no vacía`, `participation:member | consultant`. duty describe el reparto en esta composición, sin reescribir responsabilidades originales. Ante conflicto se detiene el trabajo afectado. consultant participa por tarea y no entra automáticamente al grupo ni recibe su historial. Ambas clases de posición resuelven referencias fijas según el núcleo. El ámbito de participación se aplica en la vinculación y autorización de la tarea.

coordinator y Work.responsible pueden apuntar a un consultant, con asignación explícita y confirmación de participación en el trabajo actual. Sigue siendo participación por tarea, sin control permanente del equipo ni acceso a todo su historial. `member/` es sintaxis de referencia a una posición Member; no significa lo mismo que el valor member de participation.

**Handoff** contiene `id:LocalId`, `from:WorkRef`, `to:WorkRef`, `when:cadena no vacía`, `inputs:objeto`. WorkRef solo admite `member/<memberId>/<workId>` y referencia trabajo declarado de un miembro directo de la composición; la versión viene de Member.ref. Describe el traspaso desde trabajo de un miembro que produjo resultados hacia el trabajo receptor de otro. No admite `self/<workId>` ni mezcla entradas y salidas de la composición en esa semántica. Un subequipo expone sus propios works; no se accede directamente a sus miembros internos.

Las claves de Handoff.inputs son campos superiores de la entrada receptora; sus valores son campos superiores de la salida emisora. Se interpretan literalmente como cadenas, sin expresiones, scripts ni rutas con puntos. Los campos deben estar declarados en Schema.properties correspondiente; el valor copiado debe superar la validación real del Schema receptor. Deben aportarse explícitamente las demás entradas necesarias. Si faltan, se informa la carencia sin fingir recepción satisfactoria. when es una condición de negocio: solo una asignación explícita o un mecanismo de disparo soportado puede confirmarla. Un texto descriptivo no demuestra que exista un monitor de eventos.

La distribución de entradas y la reunión de salidas del Work compuesto corresponden a la orquestación nativa del anfitrión o a un coordinador expresamente asignado. Esta edición no crea otro lenguaje de mapeo de entrada y salida. El informe de adaptación debe indicar qué entrada compuesta recibe cada trabajo miembro, quién aporta cada salida obligatoria y desde qué resultado fijo, y quién comprueba la integridad. La validación estática no prueba que los mapeos se ejecutaran. Si faltan mapeos o responsables de tratamiento, el trabajo compuesto no puede declararse preparado. El resultado debe cumplir outputs Schema y acceptance; no se agregan salidas arbitrarias de miembros automáticamente como entrega del equipo.

Cada traspaso real fija trabajo/Run de origen, versión de resultado, emisor y receptor, material mapeado, ámbito permitido y aceptación o rechazo de recepción. Solo termina cuando el receptor confirma y se establece la pertenencia del trabajo. Solo se transmite material necesario para el contrato y autorizado, sin transferir implícitamente toda la sesión, memoria privada ni credenciales. Enviar un mensaje al grupo y completar un traspaso se registran por separado.

**Sharing** contiene `resource:LocalId`, `recipients:array ParticipantRef no vacío`. resource referencia resources actual; recipients solo puede señalar member actual. El anfitrión sigue autorizando visibilidad y versión concreta. sharing no concede permisos ni convierte el recurso automáticamente en dependencia obligatoria de todos los miembros. Si un trabajo lo necesita, debe recibirlo explícitamente como entrada o dependencia.

**StopPolicy** contiene `maxHops:entero positivo`, `maxNoProgress:entero positivo`, `escalateTo:ParticipantRef`. Los contadores pertenecen al mismo trabajo y a su cadena derivada de traspasos; no se reinician al cambiar empleado o reintentar. maxHops limita traspasos; maxNoProgress limita rondas consecutivas sin nueva evidencia verificable o sin mejora en un criterio de aceptación declarado. Alcanzar cualquiera detiene nuevas asignaciones y activa escalamiento. Reanudar requiere intervención explícita de un principal fiable y un nuevo registro. Los valores dependen del caso, no son puntuaciones de capacidad del empleado.

**Governance** contiene `accountable:ParticipantRef`, `changePolicyRef:LocalId`. accountable es la posición responsable del ámbito organizativo. changePolicyRef señala un recurso purpose=policy y se incluye automáticamente como dependencia global. La política debe definir nombramiento y retirada de miembros, cambios de definiciones y restricciones, suspensión y tratamiento del trabajo en curso. Los límites organizativos de datos, presupuesto y ejecución se expresan como recursos policy/runtime en data.requires. Una restricción heredada debe indicar miembros y ámbito aplicables, y el anfitrión debe comprobarla. No amplía permisos automáticamente; un conflicto no se resuelve tomando la opción más permisiva.

AI Team utiliza colaboración; una definición AI Organization de esta edición requiere también governance. Ambos usan organization y no se distinguen por tamaño. El estándar no crea automáticamente nuevos permisos organizativos, administración real de miembros ni sistemas de identidad entre organizaciones.

Para declarar definición completa Team u Organization, todos los agent del cierre de referencias deben cumplir el contrato de definición AI Worker y todos los organization deben tener el contrato de colaboración de este anexo. No todos los subequipos necesitan governance. Un Worker puede invocar agentes temporales con solo campos básicos mediante su mecanismo autorizado, pero no presentarlos como miembros persistentes conformes. Las posiciones externas humanas o de servicio que aceptan resultados se expresan mediante authorityRoles.

## 6. Vinculación del anfitrión y ejecución real

Los registros de vinculación permanecen en el entorno de despliegue. No se exige una misma base de datos o estructura JSON, pero debe poder comprobarse lo siguiente:

| Registro | Distinciones obligatorias |
| --- | --- |
| Posición de definición | id/revision raíz, ruta de posiciones formada por Member.id sucesivos y definición fija terminal |
| Selección de instancia | Nueva o reutilizada, identidad y versión reales, principal fiable propietario; no se crea durante importación |
| Identidad continua | Trazable entre tareas; identidades de modelo, sesión, agente temporal y Run separadas; habilitación, pausa, retirada y autorización de recuperación verificables |
| Tarea y autorización | Principal de gestión, iniciador, propietario de rutina y autorizador del recurso separados; compartir Worker no comparte todas las autorizaciones de una persona |
| Reutilización | Si varias posiciones usan la misma instancia; compartir en esta edición requiere la misma definición fija, no solo igual nombre |
| Roles de autoridad | Principal fiable vinculado a authorityRoles y tipo human/service; el aprobador no puede provenir de una identidad autodeclarada por el modelo |
| Condiciones de ejecución | Trabajo seleccionado, recursos y formato/versión nativos, conexiones reales, autorización efectiva, modelo y límites |
| Versiones y cambios | Versión de vinculación, reparto adoptado, versiones de restricciones y material, tratamiento de pendientes tras cambios |

Crear dos instancias de una plantilla y compartir una instancia entre equipos deben tener registros diferentes. Visibilidad de grupos y autorización se comprueban por trabajo; el presupuesto se carga a la misma cuota aplicable, sin duplicar una cuota mediante vinculaciones repetidas.

Una ejecución debe permitir rastrear definición raíz, trabajo elegido, ejecutor real, versiones de material y recursos, autorización efectiva y versiones de los resultados. Sustituir miembros o actualizar plantillas no toma control silenciosamente de Runs anteriores. Reanudación, terminación o tarea nueva siguen la política explícita de cambios. Si el anfitrión no puede cumplir una restricción crítica, conserva la definición e informa del bloqueo, sin degradarla a una nota y continuar.

Entradas, material recuperado, mensajes históricos y resultados de herramientas son material con procedencia, no instrucciones fiables para modificar definiciones o ampliar permisos. El anfitrión debe distinguir material de fuentes de autorización. Un Worker puede llamar a un agente temporal, pero conserva la atribución de la entrega; el ejecutor temporal no se convierte automáticamente en miembro Team. La revisión independiente comprueba contribuyentes y relaciones de delegación, sin fabricar independencia para la propia entrega usando otra sesión o ayudante temporal.

Los registros distinguen al menos éxito, fallo, finalización parcial, resultado desconocido y pausa; cada anfitrión puede usar sus nombres de estado. Una salida parcial o respuesta agotada por tiempo no es automáticamente éxito. Si hay efectos externos y resultado desconocido, primero se verifica el estado original antes de reintentar. Requisitos, aceptación, resultados y limitaciones delimitan la evaluación de calidad. No se impone una puntuación universal ni una regla de «superar una prueba basta para ocupar el puesto».

## 7. Límites de adopción de OASF y Agent Spec

OASF discovery se incluye como extensión independiente: id es el URI de su Schema nativo de versión fija, version coincide con schema_version y data conserva el payload sin cambios. Si solo aporta descubrimiento, required=false; si determina una condición de selección de ejecución no ignorable, required=true y debe validarse su semántica. La clasificación por sí sola no autoriza. Este wrapper es un mapeo del estándar, no un cuerpo A2A nativo.

Por ejemplo, `schema_version`, `skills`, `domains` conservan nombre y significado. skills son categorías de capacidad y domains son ámbitos. Solo entradas reales del catálogo fijo pueden declararse clasificadas conforme a OASF. El anfitrión puede usar sin red un Schema y catálogo fijos fiables; importar un URI no invoca un servicio. Si falta una categoría, se informa que no hay mapeo sin inventar un identificador oficial. [OASF discovery](https://docs.agntcy.org/oasf/discovery-extension/)

Metadatos record y locators de OASF pueden ser vistas exportadas; procedencia, versión y campos no mapeados figuran en el informe. No sustituyen revisión fija ni identidad real de instancia. La definición origen sigue siendo la fuente única; la vista derivada no es otra configuración editable independientemente. [Record Guide](https://docs.agntcy.org/oasf/agent-record-guide/)

Work y Handoff adoptan ideas de referencias, I/O y composición de Agent Spec. `$component_ref` referencia elementos dentro de una configuración. Al exportar se establece su correspondencia explícita con definiciones y recursos fijos de este estándar. Sus listas de propiedades I/O requieren conversión a los objetos Schema de esta edición; no basta cambiar nombres. Solo se declara mapeo de una versión/subconjunto al conservar por completo interfaces, restricciones y semántica de ejecución. [Agent Spec 26.1.2](https://oracle.github.io/agent-spec/26.1.2/agentspec/language_spec_26_1_2.html)

Swarm y ManagerWorkers pueden ser destinos de adaptación de equipos compatibles. Deben comprobarse transferencia de sesión, posibilidad de atención directa al usuario y flujo de mensajes. El contrato no obliga a Teloa a cambiar de motor ni a convertir todos los equipos al patrón gestor–trabajadores.

## 8. Cobertura de diseños existentes

| Elemento existente | Representación en este estándar |
| --- | --- |
| A01 Identidad y misión | id/revision/name/instructions; avatares y presentación en extensión no crítica |
| A02 Responsabilidad y juicio | Cinco campos responsibility; autorización efectiva en vinculación del anfitrión |
| A03 Skills | resources purpose=method, con paquetes completos o recursos de método nativos |
| A04 Conocimiento | purpose=knowledge; procedencia, versión o digest; referencia por trabajo |
| A05 SOP | purpose=procedure y dependencias; collaboration para traspasos entre roles |
| A06 Plantillas de trabajo | Objetivo, I/O, dependencias, responsable y aceptación de works |
| A07 Rutinas | routines, disparadores y políticas nativas; anfitrión registra ausencia de configuración o habilitación |
| A08 Conexiones a aplicaciones | purpose=application, vinculación nativa y conexión efectiva; catálogo no demuestra instalación |
| A09 Permisos | Recursos policy necesarios, ámbito del trabajo y autorización fiable del anfitrión; sin exportar credenciales |
| A10 Ejecución y espacio | Recursos runtime y vinculación de modelo, presupuesto y ámbito de archivos |
| A11 Experiencia | memoryPolicyRef y referencias de procedencia; experiencia privada real en la instancia |
| A12 Resultados y calidad | outputs, acceptance y pruebas; estructura y calidad de negocio por separado |
| T01 Objetivo común | organization.instructions y works externos |
| T02 Responsabilidades | Member, collaboration.roles, Work.responsible y authorityRoles |
| T03 Contexto compartido | sharing, entradas de trabajo y autorización por ámbito |
| T04 Traspasos | handoffs, mapeo de entradas, confirmación de recepción y stop |
| T05 Planificación | routines, procedure y coordinación de miembros o composición |
| T06 Permisos y recursos | requires globales y por trabajo, permisos propios y cuotas reales del anfitrión |
| T07 Aceptación del equipo | outputs/acceptance de composición, contribuciones y versiones fijas |

RoleDefinition, GroupDefinition, RoleWorkDelegation, RunRoleSnapshot y paquetes de recursos del repositorio comunitario son objetos a adaptar, no se sustituyen por nuevos objetos de producto. El empleado de IA de Teloa es la forma de producto Worker. RoleDefinition, versión fija del puesto e instancia Role participan juntos en el mapeo; renombrar no demuestra conformidad. role-coordination solo describe coordinación estática; crear grupos, asignar y automatizar traspasos necesita soporte y autorización reales por separado.

`kind: employee | twin` de Teloa es un tipo de rol de producto. Ambos pueden mapearse a Worker si cumplen el contrato; twin no otorga derecho a aprobar en nombre del usuario. El kind original permanece en el informe o una extensión de presentación no crítica, nunca se copia al kind básico. `scopes`, dataScope y executionScope se mapean a políticas de ámbito necesarias y autorización del anfitrión. Si no puede preservarse el ámbito de negocio entre anfitriones, el trabajo afectado queda no preparado. Son límites críticos, no datos de presentación ignorables. Se contrastaron esos campos con `packages/contract/src/roles.ts`; ello no prueba que exista un conversor.

En una recipe pública Grok, profile se mapea al puesto, skills a métodos, routines a trabajos y rutinas pendientes, plugins a aplicaciones por configurar y memory a material de plantilla con procedencia. Se conservan separadamente pautas profile y diarios log del autor. gettingStarted solo orienta el inicio. Responsabilidades, horarios o aceptación ausentes se informan; no se inventan a partir del nombre o experiencia del autor. El informe enlaza el original completo y su digest; la semántica crítica no se oculta en extensiones ignorables. Grok Team Bot se mapea a un Worker compartido, sin generar un Team de varios Workers por su nombre.

La documentación de Grok Team Bot atribuye las rutinas personales a su creador y permite conexiones con credenciales del usuario actual o del Bot. La adaptación debe conservar propietario de rutina, identidad de ejecución y visibilidad, sin combinar todas las rutinas en trabajo compartido de todos. Es un requisito de preservación de la fuente, no una obligación de copiar sus permisos por defecto. [Team Bots](https://docs.x.ai/grok-bot/team-bots)

**Ubicación de rutinas privadas:** permanecen como planificación privada del anfitrión, separadas por propietario y visibilidad, referenciando Work declarado. No entran en routines de la definición compartida ni en exportaciones compartidas. El informe indica «conservada como planificación privada de instancia, no incluida en la definición» y respeta también su visibilidad. Solo una plantilla publicada explícitamente por su autor y sin contenido privado puede entrar en routines; al importar sigue deshabilitada por defecto. Si no pueden preservarse propietario, visibilidad o referencia de trabajo, se informa la imposibilidad de mapeo; no se fusionan ni se pierden silenciosamente.

El informe incluye al menos formato/versión/digest de origen, revisión destino, correspondencia de campos, elementos conservados/convertidos/no soportados y trabajos afectados. Conservar originales no demuestra adaptación semántica. Si no se pueden cumplir permisos, traspasos o aceptación, el trabajo afectado debe bloquearse.

## 9. Casos positivos, negativos y declaraciones de finalización

El [ejemplo completo de composición](../examples/composition-fixture.json) usa un conjunto local fijo: dos empleados, dos equipos que reutilizan ambas definiciones, una organización anidada y expectativas de vinculación y preparación. Su contenedor y explicaciones son un formato de fixture de prueba, no un nuevo protocolo ni un paquete importable directamente en Teloa.

| Comprobación | Conclusión obligatoria |
| --- | --- |
| Puesto simple sin habilidades, memoria ni rutinas | Puede definirse; siguen siendo obligatorios trabajo, I/O, responsabilidad y aceptación |
| Solo nombre, prompt y kind=agent | Puede cumplir intercambio básico, no una definición AI Worker completa |
| triggers, acciones autónomas o controles de entrega vacíos | No cumple el perfil; el nombre del producto no subsana la ausencia |
| Un Worker inicia dos tareas y cambia de modelo | Deben rastrearse la misma instancia y dos Runs distintos; dos sesiones no prueban continuidad de identidad |
| Misma definición de empleado en dos equipos | Solo demuestra reutilización de definición; compartir instancia depende de las vinculaciones |
| Un trabajo sin aplicación exclusiva, otro con entradas completas | El primero no está preparado; el segundo comprueba dependencias globales y propias sin quedar bloqueado automáticamente por el primero |
| Versión de suborganización ausente, ciclo o perfil obligatorio desconocido | Detener ensamblaje e informar la referencia; no sustituir versión ni ignorarla |
| Campo receptor erróneo, entrada necesaria ausente o tipo de salida incorrecto | No declarar traspaso satisfactorio ni conformidad del resultado; no rellenar ni convertir implícitamente |
| Dos posiciones de revisión con el mismo ejecutor real | No satisface independent=true |
| Consultor que solo participa en una revisión | No entra a todos los grupos, no lee todo el historial ni se duplica como nuevo empleado |
| Plan sin disparador o política completa | Conservar como no preparado, sin habilitar |
| Clasificación OASF desconocida o patrón Agent Spec no equivalente | Informar falta de soporte o diferencias, sin declarar compatibilidad total |

Esta especificación establece contratos estructurales y de comportamiento. Revisión del texto, comprobación de ejemplos, conexiones y modelos reales, interoperabilidad independiente y adopción sectorial se informan por separado. Véanse los límites y requisitos del informe en [conformidad](../CONFORMANCE.md).
