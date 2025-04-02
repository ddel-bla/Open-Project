# Bloques Funcionales: Self Hosted 101

## Introducción

Este documento describe los bloques funcionales que componen una infraestructura empresarial autogestionada, adaptada para implementación incremental mediante metodología Scrum. Cada bloque funcional representa un conjunto de capacidades que pueden desarrollarse como parte de un backlog de producto, priorizando según el valor para empresas emergentes.

## 1. Infraestructura Base

### Necesidades a cubrir:
- Plataforma unificada para despliegue y gestión de servicios
- Almacenamiento persistente y fiable para datos críticos
- Sistema de red interno seguro y eficiente
- Acceso controlado desde el exterior
- Escalabilidad para adaptarse al crecimiento de la organización

### Casos de uso:
- Despliegue rápido de nuevos servicios sin preocuparse por infraestructura subyacente
- Aprovisionamiento automático de recursos según demanda
- Aislamiento de entornos (desarrollo, pruebas, producción)
- Recuperación rápida ante fallos de hardware
- Gestión centralizada de toda la infraestructura tecnológica

### Criterios de aceptación (TDD):
- Un nuevo servicio puede desplegarse en menos de 10 minutos siguiendo un procedimiento documentado
- La pérdida de un nodo no causa pérdida de datos ni interrupción prolongada de servicios
- Nuevos volúmenes de almacenamiento pueden crearse y asignarse mediante interfaz web o CLI
- Las comunicaciones externas están protegidas mediante TLS y autenticación adecuada
- El sistema entero puede escalar horizontalmente añadiendo nuevos nodos sin reconfiguraciones manuales

## 2. Identidad y Seguridad

### Necesidades a cubrir:
- Gestión centralizada de identidades y accesos
- Sistema de autenticación único (SSO)
- Almacenamiento seguro de secretos y credenciales
- Protección del perímetro y segmentación interna
- Certificados y cifrado para comunicaciones seguras
- Auditoría completa de accesos y cambios

### Casos de uso:
- Incorporación y baja de empleados con provisión/desprovisión automática de accesos
- Autenticación multifactor para acceso a sistemas críticos
- Gestión diferenciada de accesos para contratistas y personal temporal
- Detección de intentos de acceso no autorizados
- Rotación automática de credenciales de servicios y APIs

### Criterios de aceptación (TDD):
- Un usuario nuevo tiene acceso a todos los servicios asignados a su rol en menos de 5 minutos tras su creación
- La revocación de acceso tras una baja es efectiva en menos de 15 minutos
- La autenticación MFA está habilitada para todos los accesos administrativos y funciona con apps estándar
- Se mantiene un registro completo de todos los intentos de acceso que se puede consultar y filtrar
- Los secretos almacenados no son accesibles en texto plano incluso para administradores del sistema

## 3. Comunicación y Colaboración

### Necesidades a cubrir:
- Comunicación por correo electrónico profesional
- Almacenamiento y compartición de archivos
- Comunicación en tiempo real (chat y videoconferencia)
- Documentación colaborativa y base de conocimiento

### Casos de uso:
- Comunicación segura con clientes y proveedores mediante correo corporativo
- Trabajo colaborativo en documentos compartidos en tiempo real
- Reuniones virtuales con participantes internos y externos
- Organización de equipos en canales temáticos para proyectos
- Acceso a información corporativa desde dispositivos móviles

### Criterios de aceptación (TDD):
- Los emails enviados desde el sistema tienen configuración correcta de SPF, DKIM y DMARC
- Los archivos compartidos mantienen control de versiones y registro de cambios
- Las videoconferencias soportan al menos 10 participantes simultáneos con calidad estable
- El sistema de documentación permite edición concurrente con resolución de conflictos
- Todos los servicios de comunicación son accesibles desde dispositivos móviles estándar

## 4. Observabilidad

### Necesidades a cubrir:
- Monitorización en tiempo real de servicios y sistemas
- Recopilación centralizada de logs
- Seguimiento de operaciones entre servicios
- Sistema de alertas y notificaciones
- Visualización de métricas técnicas y de negocio

### Casos de uso:
- Detección proactiva de degradación de servicios antes de que afecten a usuarios
- Diagnóstico rápido de la causa raíz en incidencias complejas
- Análisis de tendencias de uso para planificación de capacidad
- Notificación automática al equipo adecuado según el tipo de incidencia
- Paneles personalizados para diferentes roles (técnicos, gestores, ejecutivos)

### Criterios de aceptación (TDD):
- Las alertas se disparan dentro de los 30 segundos siguientes a una degradación significativa del servicio
- El 100% de los logs de todos los servicios se centralizan y retienen según política configurable
- Las operaciones que involucran múltiples servicios pueden trazarse end-to-end con identificadores únicos
- Las notificaciones de alerta llegan al destinatario adecuado en menos de 1 minuto
- Se mantiene un histórico de métricas con granularidad configurable por al menos 30 días

## 5. Automatización y CI/CD

### Necesidades a cubrir:
- Automatización de despliegues de software
- Integración y entrega continua
- Automatización de procesos repetitivos
- Gestión de configuración centralizada
- Programación de tareas y mantenimiento

### Casos de uso:
- Flujo automatizado desde commit de código hasta despliegue en producción
- Ejecución automática de pruebas unitarias, integración y seguridad
- Generación automática de informes periódicos para distintos departamentos
- Sincronización de datos entre sistemas heterogéneos
- Procesos de onboarding de clientes completamente automatizados

### Criterios de aceptación (TDD):
- Un cambio de código que pasa todas las pruebas se despliega automáticamente en menos de 15 minutos
- Cualquier fallo en el pipeline bloquea el despliegue y notifica al equipo responsable inmediatamente
- Los workflows de automatización permiten integraciones con sistemas externos mediante webhooks y APIs
- Las tareas programadas se ejecutan con fiabilidad y tienen seguimiento de éxito/fracaso
- Los procesos automatizados mantienen registro detallado de cada paso y resultado

## 6. Resiliencia y Recuperación

### Necesidades a cubrir:
- Sistema de respaldos automatizados
- Replicación de datos críticos
- Procedimientos documentados de recuperación
- Pruebas de escenarios de fallos
- Alta disponibilidad para servicios críticos

### Casos de uso:
- Recuperación completa tras un desastre en el centro de datos principal
- Restauración puntual de datos específicos solicitados por usuarios
- Pruebas programadas de recuperación ante desastres sin impacto en producción
- Failover automático de servicios críticos en caso de fallo de hardware
- Simulación de escenarios de ataque para validar procedimientos de respuesta

### Criterios de aceptación (TDD):
- Los backups automáticos se realizan según la programación y son verificados tras su creación
- La recuperación completa del sistema es posible dentro del RTO establecido (4 horas máximo)
- Los datos se pueden restaurar a cualquier punto en el tiempo dentro del RPO establecido (15 minutos máximo)
- Los servicios críticos permanecen disponibles incluso si un nodo completo falla
- Los procedimientos de recuperación están documentados y se prueban regularmente

## 7. Gestión Documental y Contratos

### Necesidades a cubrir:
- Almacenamiento estructurado de documentos
- Control de versiones de documentos
- Flujos de aprobación y firma electrónica
- Indexación y búsqueda avanzada
- Gestión del ciclo de vida documental

### Casos de uso:
- Proceso completo de negociación y firma de contratos sin papel
- Búsqueda por contenido en miles de documentos en segundos
- Repositorio centralizado de documentación legal y normativa
- Flujo de aprobación de documentos con múltiples revisores y niveles
- Archivo automático con destrucción programada según política de retención

### Criterios de aceptación (TDD):
- Los documentos subidos son automáticamente OCR'eados e indexados para búsqueda en texto completo
- El control de versiones mantiene historial completo de cambios con posibilidad de comparación
- Los flujos de aprobación notifican automáticamente a los participantes según su rol
- Las búsquedas de documentos devuelven resultados en menos de 3 segundos para repositorios de hasta 10,000 documentos
- El ciclo de vida documental aplica políticas de retención configurables con alertas previas a destrucción

## 8. ERP y Gestión Financiera

### Necesidades a cubrir:
- Gestión contable básica
- Facturación y seguimiento de pagos
- Control de gastos e ingresos
- Informes financieros
- Gestión de inventario (si aplica)

### Casos de uso:
- Ciclo completo de facturación desde presupuesto hasta cobro
- Conciliación automática de movimientos bancarios
- Generación de informes financieros para dirección y accionistas
- Control de gastos con procesos de aprobación configurables
- Previsión de tesorería basada en facturas pendientes y recurrentes

### Criterios de aceptación (TDD):
- Las facturas generadas cumplen con requisitos legales actualizados
- Los movimientos contables mantienen trazabilidad completa (audit trail)
- Los informes financieros se generan con datos actualizados en tiempo real
- Los procesos de aprobación escalan automáticamente tras tiempo de inactividad
- El sistema detecta y alerta sobre discrepancias en conciliaciones bancarias

## 9. Helpdesk y Soporte

### Necesidades a cubrir:
- Gestión de incidencias y solicitudes
- Base de conocimiento para soluciones frecuentes
- Seguimiento de SLAs internos o externos
- Portal de autoservicio para usuarios
- Análisis de tendencias en problemas recurrentes

### Casos de uso:
- Soporte multicanal (email, chat, teléfono) con seguimiento unificado
- Gestión de permisos y accesos a través de tickets de solicitud
- Resolución guiada de problemas para usuarios finales
- Asignación inteligente de tickets según experticia y carga de trabajo
- Escalado automático de incidencias que se acercan a incumplimiento de SLA

### Criterios de aceptación (TDD):
- Las solicitudes recibidas por cualquier canal se registran en el sistema en menos de 2 minutos
- Los SLAs se monitorizan en tiempo real con alertas antes de incumplimiento
- La base de conocimiento sugiere artículos relevantes según el contenido del ticket
- La asignación automática distribuye la carga de trabajo equitativamente entre agentes disponibles
- Los informes de satisfacción se generan automáticamente tras resolución de tickets

## 10. Análisis de Datos y BI

### Necesidades a cubrir:
- Consolidación de datos de múltiples fuentes
- Creación de informes personalizados
- Dashboards para diferentes departamentos
- Distribución automatizada de informes
- Análisis ad-hoc para decisiones puntuales

### Casos de uso:
- Análisis de embudo de ventas con identificación de puntos de mejora
- Seguimiento de KPIs de negocio con alertas de desviación
- Segmentación de clientes basada en patrones de comportamiento
- Previsión de demanda para optimización de inventario
- Dashboard ejecutivo con visión 360° del estado del negocio

### Criterios de aceptación (TDD):
- Los datos de todas las fuentes configuradas se sincronizan al menos cada hora
- Los dashboards se actualizan en tiempo real con latencia máxima de 5 minutos
- Los informes programados se distribuyen puntualmente a todos los destinatarios
- Los usuarios pueden crear análisis ad-hoc sin conocimientos técnicos avanzados
- Las alertas de desviación de KPIs se disparan dentro de umbrales configurables

## 11. Gestión de Proyectos y Tareas

### Necesidades a cubrir:
- Planificación y seguimiento de proyectos
- Asignación y seguimiento de tareas
- Visualización del flujo de trabajo
- Gestión de recursos y capacidad
- Seguimiento del tiempo y reporte

### Casos de uso:
- Gestión integral de proyectos desde propuesta hasta entrega
- Planificación de capacidad para optimizar asignación de recursos
- Tableros Kanban para gestión visual del trabajo en curso
- Seguimiento automático del tiempo dedicado a tareas específicas
- Informes de avance de proyecto para clientes y stakeholders

### Criterios de aceptación (TDD):
- Los proyectos pueden configurarse con metodologías predefinidas (Kanban, Scrum, Cascada)
- Las tareas asignadas generan notificaciones automáticas a los responsables
- El seguimiento de tiempo permite registro manual y automático con precisión de minutos
- Los informes de avance muestran progreso real vs. planificado con indicadores visuales
- La asignación de recursos detecta y alerta sobre sobreasignaciones


## Casos de Uso Clave para Integración

Esta sección explica cómo los diferentes bloques funcionales trabajan juntos en situaciones reales del día a día, demostrando el valor de la plataforma completa.

### 1. Onboarding Completo de Empleado

**¿Qué es?** El proceso de dar de alta a un nuevo empleado en todos los sistemas necesarios para que pueda trabajar productivamente desde el primer día.

**Cómo funciona:**
- **Día 1 - Mañana**: El administrador introduce los datos básicos del empleado en el sistema de identidad (nombre, puesto, departamento)
- **Día 1 - Automáticamente**: El sistema crea todas las cuentas necesarias según su rol (email, chat, almacenamiento, proyectos)
- **Día 1 - Tarde**: El empleado recibe un email con sus credenciales iniciales y accede por primera vez
- **Día 1 - Automáticamente**: El sistema registra el primer acceso y notifica al supervisor
- **Día 2**: El empleado ya tiene acceso a todos sus documentos, proyectos y herramientas sin intervención adicional

**Para operaciones:** Reduce el tiempo de configuración de 2-3 días a menos de 4 horas. Elimina errores manuales que frecuentemente causan tickets de soporte en la primera semana de los empleados. Garantiza cumplimiento al asignar exactamente los permisos correctos.

**Ejemplo real:** María, nueva diseñadora gráfica, recibe acceso automático a la biblioteca de recursos visuales, herramientas de diseño, y es añadida a los canales de comunicación relevantes sin que el equipo de IT tenga que realizar 7-8 tareas manuales diferentes.

### 2. Ciclo de Vida de Proyecto para Cliente

**¿Qué es?** La gestión completa de un proyecto desde su aprobación inicial hasta el soporte posterior a la entrega.

**Cómo funciona:**
- **Inicio**: El gerente crea un nuevo proyecto en el sistema, lo que automáticamente:
  - Configura un espacio de trabajo colaborativo
  - Crea repositorios para documentación y código
  - Asigna plantilla de tareas según tipo de proyecto
- **Durante el proyecto**: 
  - Los miembros registran tiempo en tareas que se vincula automáticamente con facturación
  - Los documentos entregables se gestionan con control de versiones y aprobaciones
  - El cliente tiene acceso a un portal para ver avances y aprobar entregas
- **Finalización y soporte**:
  - Se genera automáticamente facturación final basada en hitos completados
  - El proyecto pasa a fase de soporte con SLAs predefinidos
  - El conocimiento del proyecto alimenta base de datos para futuros trabajos similares

**Para operaciones:** Elimina la necesidad de configurar manualmente múltiples herramientas para cada proyecto. Garantiza que toda la información está centralizada y accesible. Facilita la transición entre fases del proyecto sin pérdida de información.

**Ejemplo real:** Un proyecto de desarrollo web para una tienda online pasa automáticamente del equipo de ventas al de implementación y finalmente al de soporte, manteniendo toda la documentación, comunicaciones y código en un único sistema integrado.

### 3. Gestión de Incidente Crítico

**¿Qué es?** El proceso completo de detección, respuesta y resolución de problemas técnicos críticos que afectan a los servicios.

**Cómo funciona:**
- **Detección**: Los sistemas de monitorización detectan un problema (por ejemplo, lentitud en la base de datos)
  - Se registran automáticamente logs, métricas y estado del sistema en el momento del fallo
- **Notificación**: 
  - Se crea automáticamente un ticket de alta prioridad
  - Se notifica al equipo responsable por múltiples canales (email, SMS, chat)
  - Se actualiza el estado en el panel de control visible para todos los equipos
- **Respuesta y resolución**:
  - El equipo trabaja en la solución con todos los datos necesarios ya recopilados
  - El sistema hace seguimiento del tiempo de resolución según SLAs establecidos
  - Todas las acciones quedan registradas automáticamente
- **Cierre y mejora**:
  - Se documenta la solución y causa raíz
  - El conocimiento se añade a la base de datos para futuros casos similares
  - Se crean automáticamente tareas de mejora para prevenir recurrencia

**Para operaciones:** Reduce significativamente el tiempo medio de resolución (MTTR) al eliminar pasos manuales en momentos críticos. Proporciona toda la información necesaria de forma inmediata. Facilita el aprendizaje continuo y la mejora de la respuesta a incidentes.

**Ejemplo real:** Una caída del sistema de procesamiento de pagos es detectada a las 3 AM. En lugar de esperar a que los clientes reporten el problema, el sistema alerta automáticamente al personal de guardia con toda la información contextual necesaria, permitiendo una resolución en minutos en lugar de horas.

### 4. Cumplimiento Normativo y Auditoría

**¿Qué es?** El proceso de verificar, documentar y demostrar que todos los sistemas cumplen con los requisitos legales y normativos aplicables.

**Cómo funciona:**
- **Preparación continua**:
  - El sistema registra automáticamente todas las acciones relevantes (accesos, cambios, aprobaciones)
  - Los datos sensibles se identifican y gestionan según políticas configuradas
  - Se ejecutan verificaciones periódicas automatizadas de cumplimiento
- **Durante una auditoría**:
  - Se generan informes predefinidos con evidencias específicas solicitadas
  - Se puede demostrar trazabilidad completa de datos y procesos
  - Los auditores pueden acceder a un portal seguro con información relevante
- **Mejora post-auditoría**:
  - Los hallazgos se registran y convierten en tareas de remediación
  - Se actualizan automáticamente políticas y configuraciones
  - Se programan verificaciones para confirmar implementación de mejoras

**Para operaciones:** Transforma las auditorías de eventos estresantes que interrumpen el trabajo normal a procesos rutinarios con mínima intervención manual. Reduce el riesgo de sanciones por incumplimiento.

**Ejemplo real:** Una auditoría de seguridad de datos que tradicionalmente requeriría semanas de preparación manual se completa en días, con informes generados automáticamente que muestran todos los controles de acceso, cifrado y políticas de retención de datos.

### 5. Aprovisionamiento y Gestión de Infraestructura

**¿Qué es?** El proceso de crear, configurar y mantener la infraestructura tecnológica necesaria para soportar servicios y aplicaciones.

**Cómo funciona:**
- **Solicitud y aprobación**:
  - Los equipos solicitan recursos mediante formularios estandarizados
  - El sistema verifica automáticamente disponibilidad y políticas
  - Los aprobadores reciben notificaciones con toda la información relevante
- **Aprovisionamiento**:
  - Los recursos se crean automáticamente según especificaciones
  - Se aplican configuraciones de seguridad y políticas predefinidas
  - Los solicitantes reciben credenciales y documentación de acceso
- **Monitorización y mantenimiento**:
  - El sistema realiza verificaciones continuas de rendimiento y seguridad
  - Se ejecutan rutinas de mantenimiento programadas automáticamente
  - Los recursos se escalan según necesidades detectadas
- **Desmantelamiento**:
  - Los recursos no utilizados se identifican automáticamente
  - Se gestiona respaldo de datos antes de eliminación
  - Se liberan recursos y licencias para reutilización

**Para operaciones:** Reduce el tiempo de aprovisionamiento de días a minutos. Asegura consistencia en todas las configuraciones. Optimiza el uso de recursos al identificar y reclamar capacidad no utilizada.

**Ejemplo real:** Un equipo de desarrollo necesita un entorno de pruebas. En lugar de abrir tickets y esperar configuración manual, el entorno se crea automáticamente en minutos con todos los servicios necesarios, políticas de seguridad y conexiones a otros sistemas ya configuradas correctamente.

La integración entre estos casos de uso demuestra cómo la plataforma Self Hosted 101 elimina silos de información, automatiza tareas repetitivas y proporciona una experiencia coherente tanto para usuarios técnicos como no técnicos.