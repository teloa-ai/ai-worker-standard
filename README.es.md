<p align="center">
  <a href="https://www.teloa.ai/en/"><img src="assets/brand/teloa-reading.webp" width="120" height="120" alt="Teloa, la ardilla que lee"></a>
</p>

# AI Worker Standard 1.0

**Un estándar abierto de definición para AI Workers, AI Teams y AI Organizations.**

Por **Max Luo y Morgan Chen, cofundadores de Teloa** · 18 de octubre de 2026

[English](README.md) · [简体中文](README.zh-CN.md) · [繁體中文](README.zh-Hant.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Deutsch](README.de.md) · [Français](README.fr.md) · [Español](README.es.md)

[Website](https://www.teloa.ai/en/) · [Docs](https://docs.teloa.ai/en/) · [Marketplace](https://market.teloa.ai/en/) · [AI Security](https://security.teloa.ai/?lang=en) · [Blog](https://www.teloa.ai/en/blog/) · [Security reports](https://security.teloa.ai/report/?lang=en)

[![Version 1.0](assets/badges/version.svg)](https://github.com/teloa-ai/ai-worker-standard/releases/tag/v1.0) [![8 core rules](assets/badges/core-rules.svg)](spec/core.es.md) [![8 languages](assets/badges/languages.svg)](docs/languages.md) [![License Apache-2.0](assets/badges/license.svg)](LICENSE)

Un **AI Worker** es una entidad de trabajo de IA cuya identidad persiste entre tareas, con responsabilidades definidas, un límite de autorización y entregables verificables. En Teloa, el concepto de producto es **empleado de IA (AI employee)**; AI Worker es el concepto técnico subyacente del estándar. Grok Bot también puede describirse así, sujeto a las comprobaciones de correspondencia indicadas más abajo.

## 1. Conceptos principales

| Concepto | Qué describe | Para qué sirve |
| --- | --- | --- |
| AI Worker | Una entidad identificable con responsabilidades, contratos de trabajo y comprobaciones de entrega | Mantener la atribución del trabajo entre tareas, modelos y agentes temporales |
| AI Team | Una composición orientada a un resultado común con roles, traspasos y aceptación explícitos | Dotar a la colaboración de un contrato de entrega verificable |
| AI Organization | Una composición de Workers y equipos con responsabilidad organizativa, límites de recursos y gestión de cambios | Permitir que los equipos trabajen bajo una gobernanza explícita |

La identidad persistente no exige una máquina dedicada, un proceso permanente ni memoria a largo plazo. Aquí la responsabilidad significa atribución y trazabilidad del trabajo, no personalidad jurídica. Un chat de grupo o una lista de miembros no constituye por sí solo un contrato de equipo.

## 2. Alcance y formato

| Elemento | Versión 1.0 |
| --- | --- |
| Objetos del núcleo | `agent` y `organization` |
| Perfil de trabajo obligatorio | `urn:teloa:profile:ai-work`, versión `1.0` |
| Worker / Team / Organization | Worker: núcleo + perfil de trabajo; Team: añadir colaboración; Organization: añadir gobernanza |
| Intercambio | JSON; redacción opcional en YAML conforme a reglas de conversión restringidas |
| Comprobaciones del núcleo | Ocho reglas comunes R01–R08; seis casos de intercambio T01–T06 |
| Texto autorizado | Núcleo y perfil de trabajo en chino simplificado; traducciones íntegras de lectura en otros siete idiomas |

El estándar define identidades, revisiones inmutables, referencias, responsabilidades, interfaces de trabajo, colaboración y gobernanza. Los anfitriones implementan la ejecución de modelos, la planificación, las conexiones y la autorización. Importar una definición no instala herramientas, conecta cuentas, inicia trabajo ni activa rutinas. Un documento `agent` con solo los seis campos del núcleo no es una definición completa de AI Worker.

## 3. Leer el estándar

| Documento | Contenido |
| --- | --- |
| [Guía con diagramas y texto consultable](docs/guide.es.md) | Composición, arquitectura, flujo de entrega y ejemplo detallado |
| [Especificación completa del núcleo](spec/core.es.md) | Campos, revisiones fijas, referencias, reglas comunes y casos de intercambio |
| [Perfil completo de trabajo y colaboración](spec/work-profile.es.md) | Contratos, recursos, traspasos, rutinas, vinculaciones de instancias y gobernanza |
| [Ejemplos](examples/README.md) | Worker completo en JSON/YAML, ejemplo de composición y entradas/salidas |
| [Correspondencias de productos y especificaciones](docs/mappings.md) | Teloa, Grok Bot, OASF y Oracle Agent Spec |
| [Conformidad](CONFORMANCE.md) · [Gobernanza](GOVERNANCE.md) | Límites de las pruebas, contribución y mantenimiento |
| [Los ocho idiomas](docs/languages.md) | Ediciones íntegras de lectura y política de traducción |

Cada guía explica también los diagramas mediante texto. Las traducciones conservan los nombres de campos, identificadores de reglas y ejemplos JSON. Todavía no han recibido una revisión completa de especialistas del sector hablantes nativos. En caso de divergencia, prevalecen los dos documentos normativos en chino. Los documentos compartidos de correspondencias, ejemplos y gobernanza están en inglés y/o chino, según el índice de idiomas.

## 4. Empezar con un Worker completo

1. Leer la [guía de composición y entrega](docs/guide.es.md).
2. Examinar [document-worker.json](examples/document-worker.json) o su equivalente [YAML](examples/document-worker.yaml). Recibe `material` y devuelve `summary` y `openQuestions`.
3. Comprobar responsabilidades, esquemas de entrada/salida, recursos necesarios y contrato de aceptación; comparar la [entrada](examples/document-input.json) y la [salida](examples/document-output.json) de ejemplo.
4. Para ejecutarlo, vincular la definición a una instancia real, recursos compatibles y autorización efectiva en un anfitrión. Registrar por separado el resultado, la aceptación y la adopción por el negocio.

Los ejemplos ilustran contratos; no son un paquete de despliegue ni un validador de conformidad. El ejemplo de composición incluye vinculaciones sin resolver y no está listo para la ejecución real.

## 5. Nombres de productos y especificaciones externas

El concepto de producto que Teloa presenta a sus usuarios sigue siendo **empleado de IA**. La definición Worker describe el rol y el contrato de trabajo; una instancia Worker identifica a un empleado de IA concreto. Un equipo de IA organiza la colaboración y la entrega mediante un contrato Team.

El **Bot** de Grok puede corresponder a un AI Worker. Un **Team Bot** compartido entre personas se entiende primero como un Worker compartido, no automáticamente como un equipo de varios Workers. La conformidad depende de las definiciones y del comportamiento del anfitrión, no del nombre. Las fuentes y comprobaciones están en la [guía de correspondencias](docs/mappings.md).

OASF aporta ideas para metadatos de descubrimiento y taxonomías. Oracle Agent Spec aporta ideas para referencias de componentes, interfaces y composición. Esto no establece una compatibilidad completa de formato o de ejecución.

## 6. Estado, contribución y cita

Esta es una **especificación abierta mantenida por Teloa**, no un estándar internacional adoptado ni un programa de certificación. No se afirma el respaldo de xAI, AGNTCY, Oracle ni de organizaciones de normalización. La interoperabilidad independiente requiere pruebas reproducibles de implementaciones independientes.

Enviar ambigüedades concretas, contraejemplos e informes de implementación con sus versiones mediante [Issues](https://github.com/teloa-ai/ai-worker-standard/issues). Consultar [GOVERNANCE.md](GOVERNANCE.md) para los cambios y [CONFORMANCE.md](CONFORMANCE.md) para las declaraciones.

Cita sugerida: **Max Luo and Morgan Chen. AI Worker Standard 1.0. Teloa, 2026.** Cita legible por máquinas: [CITATION.cff](CITATION.cff).

## 7. Licencia

El material original se publica bajo [Apache-2.0](LICENSE). Los nombres de productos y las especificaciones de terceros conservan sus respectivos derechos. No se requiere cuenta ni servicio de Teloa para leer o implementar este estándar.

El nombre Teloa y la ilustración de la ardilla se rigen por [normas de uso de marca independientes](assets/brand/README.md).
