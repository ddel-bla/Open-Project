# Propuesta Tecnológica: Self Hosted 101

## Introducción

Este documento detalla la implementación tecnológica para cada bloque funcional del proyecto Self Hosted 101, una infraestructura empresarial completa y autogestionada. La selección de tecnologías considera criterios como madurez del proyecto, comunidad activa, licenciamiento open source, documentación disponible y requisitos de recursos. Esta propuesta está diseñada para implementación incremental mediante metodología Scrum.

## Stack Tecnológico por Bloque

### 1. Infraestructura Base

#### Tecnologías seleccionadas:
- **Plataforma de orquestación**: K3s (Lightweight Kubernetes)
  - *Justificación*: Ofrece todas las funcionalidades de Kubernetes estándar con menor huella de recursos, ideal para entornos con hardware limitado.
  - *User Stories*:
    - Como administrador, quiero desplegar nuevos servicios mediante archivos YAML para automatizar la provisión
    - Como desarrollador, quiero acceso a logs centralizados para diagnosticar problemas
    - Como SRE, quiero monitorizar el estado de nodos y pods para garantizar disponibilidad

- **Sistema de almacenamiento persistente**: Longhorn
  - *Justificación*: Proporciona almacenamiento distribuido con replicación, snapshots y backups, específicamente optimizado para K3s.
  - *User Stories*:
    - Como administrador, quiero crear volúmenes persistentes con diferentes niveles de replicación según criticidad
    - Como usuario, quiero que mis datos persistan incluso si un nodo falla
    - Como operador, quiero programar backups automáticos de volúmenes críticos

- **Redes y DNS interno**: Cilium + CoreDNS
  - *Justificación*: Cilium ofrece networking avanzado con políticas de seguridad basadas en identidad. CoreDNS proporciona resolución de nombres flexible y extensible.

- **Balanceador de carga**: MetalLB
  - *Justificación*: Implementación de balanceador de carga para entornos bare-metal sin depender de proveedores cloud.

- **Gestión de configuración**: 
  - Terraform para provisión inicial
  - Flux/ArgoCD para GitOps

#### Criterios de aceptación (TDD):
- La plataforma permite desplegar nuevos servicios en menos de 5 minutos
- El sistema continúa operativo con pérdida de hasta 1/3 de los nodos
- Se pueden crear y asignar volúmenes persistentes a aplicaciones con un único comando
- Las aplicaciones son accesibles mediante nombres DNS internos y externos
- La configuración completa está versionada en Git y se despliega automáticamente

### 2. Identidad y Seguridad

#### Tecnologías seleccionadas:
- **Identity Provider**: Keycloak
  - *Justificación*: Solución madura de gestión de identidad y acceso con soporte para OAuth, SAML, LDAP y múltiples factores de autenticación.
  - *User Stories*:
    - Como usuario, quiero iniciar sesión una sola vez para acceder a todos los servicios
    - Como administrador, quiero configurar políticas de contraseñas y MFA para diferentes grupos de usuarios
    - Como auditor, quiero acceder a registros completos de actividad de autenticación

- **Gestión de secretos**: HashiCorp Vault
  - *Justificación*: Plataforma robusta para gestión de secretos con rotación automática, auditoría y control de acceso.

- **Gestión de certificados**: cert-manager
  - *Justificación*: Automatiza la emisión y renovación de certificados TLS en Kubernetes.

- **Políticas de red y seguridad**: 
  - Cilium para políticas en capa 3-7
  - Open Policy Agent (OPA) para autorización basada en políticas

#### Criterios de aceptación (TDD):
- Un usuario nuevo puede acceder a todos los servicios con un único login
- La autenticación MFA está disponible y funcional para todos los usuarios
- Los secretos están cifrados en reposo y en tránsito
- Los certificados se renuevan automáticamente antes de su expiración
- Existe registro completo de todos los accesos con capacidad de auditoría

#### Integraciones clave:
- LDAP/Active Directory existente (si aplica)
- Integración SSO con todos los servicios internos
- Configuración de auditoría centralizada

### 3. Comunicación y Colaboración

#### Tecnologías seleccionadas:
- **Servidor de correo**: 
  - Postfix (SMTP)
  - Dovecot (IMAP/POP3)
  - Rspamd (antispam)
  - Postfixadmin (gestión)
  - *User Stories*:
    - Como usuario, quiero enviar y recibir correos desde cualquier dispositivo
    - Como administrador, quiero crear/modificar/eliminar cuentas de correo fácilmente
    - Como organización, quiero que nuestros correos pasen las validaciones anti-spam

- **Almacenamiento y compartición**: Nextcloud
  - *Justificación*: Plataforma completa de productividad con almacenamiento, calendario, contactos y aplicaciones colaborativas.
  - *User Stories*:
    - Como usuario, quiero sincronizar archivos entre mi PC y la nube
    - Como equipo, queremos editar documentos colaborativamente en tiempo real
    - Como administrador, quiero establecer cuotas y permisos por usuario/grupo

- **Comunicación en tiempo real**: 
  - Mattermost (chat y colaboración)
  - Jitsi Meet (videoconferencia)
  - *User Stories*:
    - Como equipo, queremos comunicarnos en canales organizados por tema/proyecto
    - Como usuario, quiero realizar videoconferencias sin instalar software adicional
    - Como organización, queremos mantener el historial de comunicaciones para referencia

- **Wiki y documentación**: BookStack
  - *Justificación*: Sistema de documentación estructurado con jerarquía libros-capítulos-páginas, ideal para conocimiento organizacional.

#### Criterios de aceptación (TDD):
- Los correos enviados tienen configuración SPF/DKIM/DMARC correcta
- Los archivos sincronizados mantienen versiones y se pueden restaurar
- Las videoconferencias soportan al menos 10 participantes simultáneos
- El sistema de documentación permite edición colaborativa sin conflictos
- Todos los servicios son accesibles mediante SSO

#### Consideraciones específicas:
- Configuración de DKIM, SPF y DMARC para correo
- Optimización de transferencia de archivos grandes
- Integración de autenticación con Keycloak

### 4. Observabilidad

#### Tecnologías seleccionadas:
- **Monitorización y métricas**: 
  - Prometheus (recolección y almacenamiento)
  - Grafana (visualización)
  - Node Exporter (métricas del sistema)
  - kube-state-metrics (métricas de Kubernetes)
  - *User Stories*:
    - Como SRE, quiero visualizar métricas históricas de rendimiento
    - Como desarrollador, quiero acceder a dashboards de mi aplicación
    - Como gerente, quiero ver paneles de estado general del sistema

- **Gestión de logs**: 
  - Loki (almacenamiento de logs)
  - Promtail (recolección de logs)
  - *User Stories*:
    - Como operador, quiero una vista centralizada de todos los logs
    - Como desarrollador, quiero filtrar logs por servicio y nivel 
    - Como equipo de seguridad, quiero alertas sobre patrones sospechosos

- **Trazabilidad distribuida**:
  - Tempo (backend de trazas)
  - OpenTelemetry (instrumentación)

- **Alertas y notificaciones**:
  - Alertmanager
  - Integraciones con email, chat y tickets

#### Criterios de aceptación (TDD):
- Las alertas se disparan dentro de los 30 segundos siguientes a un evento crítico
- Los dashboards muestran métricas con latencia menor a 1 minuto
- Los logs de todos los servicios se centralizan y son accesibles a través de interfaz unificada
- Las trazas distribuidas permiten seguir operaciones que atraviesan múltiples servicios
- Las notificaciones llegan a los destinatarios por el canal configurado en menos de 2 minutos

#### Consideraciones de rendimiento:
- Retención configurable de métricas y logs
- Muestreo adaptativo para trazas
- Agregación para reducir volumen de datos

### 5. Automatización y CI/CD

#### Tecnologías seleccionadas:
- **Pipeline CI/CD**: Tekton
  - *Justificación*: Sistema nativo de Kubernetes para CI/CD, extensible y basado en recursos personalizados.
  - *User Stories*:
    - Como desarrollador, quiero que mis cambios se desplieguen automáticamente tras pasar pruebas
    - Como equipo de QA, queremos revisar entornos de staging antes de producción
    - Como operador, quiero poder revertir fácilmente a versiones anteriores

- **Automatización de flujos**: n8n
  - *Justificación*: Plataforma de automatización de flujos de trabajo con interfaz visual y amplia gama de integraciones.
  - *User Stories*:
    - Como usuario no técnico, quiero automatizar tareas repetitivas sin escribir código
    - Como administrador, quiero programar tareas de mantenimiento recurrentes
    - Como equipo, queremos automatizar notificaciones basadas en eventos del sistema

- **Gestión de configuración**: Ansible
  - *Justificación*: Herramienta madura para automatización de configuración y aprovisionamiento.

- **Tareas programadas**: Kubernetes CronJobs + Kron

#### Criterios de aceptación (TDD):
- Un cambio de código que pasa pruebas se despliega automáticamente en menos de 15 minutos
- Los workflows de automatización se pueden configurar mediante interfaz visual
- Las tareas programadas se ejecutan con precisión temporal de ±1 minuto
- Las automatizaciones entre sistemas mantienen registro de éxito/fracaso
- Las herramientas CI/CD proporcionan retroalimentación inmediata a desarrolladores

#### Capacidades a implementar:
- Entorno completo de pruebas automatizadas
- Promoción automática entre entornos
- Templates para flujos de trabajo comunes

### 6. Resiliencia y Recuperación

#### Tecnologías seleccionadas:
- **Sistema de backups**: Velero
  - *Justificación*: Herramienta nativa de Kubernetes para backups y restauración de clúster completo.
  - *User Stories*:
    - Como administrador, quiero programar backups automáticos con diferentes retenciones
    - Como operador, quiero restaurar rápidamente desde backup en caso de fallo
    - Como usuario, quiero recuperar versiones anteriores de mis datos

- **Replicación de datos**: 
  - Longhorn (para datos en cluster)
  - Syncthing (para replicación externa)

- **Chaos Engineering**: Litmus Chaos
  - *Justificación*: Framework para pruebas de resiliencia en entornos Kubernetes.

- **Gestión de incidentes**: Incident Response System

#### Criterios de aceptación (TDD):
- Los backups se realizan automáticamente según calendario configurado
- La restauración completa del sistema es posible en menos de 4 horas (RTO)
- La pérdida máxima de datos es de 15 minutos en caso de desastre (RPO)
- El sistema supera pruebas de caos controlado sin pérdida de servicio
- Existe documentación detallada de procedimientos de recuperación

#### Métricas a monitorizar:
- RPO (Punto Objetivo de Recuperación)
- RTO (Tiempo Objetivo de Recuperación)
- MTTR (Tiempo Medio de Recuperación)

### 7. Gestión Documental y Contratos

#### Tecnologías seleccionadas:
- **Sistema DMS**: Paperless-ngx
  - *Justificación*: Sistema moderno de gestión documental con OCR, etiquetado automático y búsqueda avanzada.
  - *User Stories*:
    - Como usuario, quiero subir documentos que se OCR automáticamente
    - Como equipo legal, queremos encontrar rápidamente documentos por contenido
    - Como administrador, quiero clasificar automáticamente documentos según patrones

- **Firma electrónica**: 
  - jSign (integración con Paperless-ngx)

- **Gestión de contratos**: OpenKM
  - *Justificación*: Sistema especializado en gestión documental con flujos de trabajo y manejo de metadatos.

#### Criterios de aceptación (TDD):
- Los documentos subidos son procesados con OCR en menos de 5 minutos
- Las búsquedas por contenido devuelven resultados en menos de 3 segundos
- Los flujos de aprobación notifican a los participantes en tiempo real
- Los documentos mantienen historial completo de versiones y cambios
- El sistema aplica políticas de retención configurables

#### Capacidades específicas:
- Indexación de texto completo
- Clasificación automática de documentos
- Extracción de metadatos
- Control de versiones

### 8. ERP y Gestión Financiera

#### Tecnologías seleccionadas:
- **Sistema ERP**: ERPNext
  - *Justificación*: ERP completo, modular y open source con funcionalidades para diferentes sectores.
  - *User Stories*:
    - Como contable, quiero registrar facturas y seguir pagos pendientes
    - Como gerente, quiero ver informes financieros actualizados
    - Como vendedor, quiero crear presupuestos y convertirlos en pedidos

- **Contabilidad**: ERPNext (módulo financiero)
  - Funcionalidades: libro mayor, cuentas por pagar/cobrar, conciliación bancaria

- **Facturación electrónica**: 
  - Módulo integrado en ERPNext con adaptadores para requisitos locales

- **Informes financieros**:
  - ERPNext + Metabase para informes personalizados

#### Criterios de aceptación (TDD):
- Las facturas generadas cumplen requisitos legales vigentes
- La conciliación bancaria identifica automáticamente transacciones coincidentes
- Los informes financieros reflejan datos actualizados en tiempo real
- Los procesos de aprobación respetan jerarquías organizativas
- La exportación contable es compatible con software de asesorías externas

#### Consideraciones de implementación:
- Configuración según plan contable local
- Integración con sistema bancario si es viable
- Exportación compatible con software de asesorías

### 9. Helpdesk y Soporte

#### Tecnologías seleccionadas:
- **Sistema de tickets**: Zammad
  - *Justificación*: Plataforma moderna de helpdesk con múltiples canales de comunicación y base de conocimiento integrada.
  - *User Stories*:
    - Como usuario, quiero crear tickets por email, web o chat
    - Como agente, quiero ver toda la información relevante en una pantalla
    - Como supervisor, quiero monitorizar SLAs y carga de trabajo
    - Como equipo, queremos construir una base de conocimiento colaborativa

- **Base de conocimiento**: 
  - Integrada en Zammad
  - Sincronización con BookStack para documentación más compleja

- **Portal de autoservicio**:
  - Frontend personalizado de Zammad

#### Criterios de aceptación (TDD):
- Los tickets creados por cualquier canal se registran en menos de 1 minuto
- La asignación automática distribuye equitativamente la carga entre agentes
- Los SLAs se monitorizan con alertas tempranas de posibles incumplimientos
- La base de conocimiento sugiere artículos relevantes según el contenido del ticket
- Los clientes pueden consultar el estado de sus tickets en tiempo real

#### Funcionalidades clave:
- Seguimiento de SLAs
- Encuestas de satisfacción
- Enrutamiento inteligente de tickets
- Informes de rendimiento

### 10. Análisis de Datos y BI

#### Tecnologías seleccionadas:
- **Plataforma BI**: Metabase
  - *Justificación*: Herramienta intuitiva de BI que permite crear dashboards y reportes sin conocimientos técnicos avanzados.
  - *User Stories*:
    - Como gerente, quiero ver KPIs actualizados en un dashboard personalizado
    - Como analista, quiero crear consultas ad-hoc sin conocer SQL
    - Como equipo, queremos compartir informes periódicos automatizados
    - Como departamento, queremos analizar tendencias históricas de datos

- **Data warehouse**: 
  - ClickHouse para análisis de grandes volúmenes

- **ETL y sincronización**: Airbyte
  - *Justificación*: Plataforma open source para extraer, transformar y cargar datos desde múltiples fuentes.

- **Programación de reportes**: 
  - Funcionalidad nativa de Metabase
  - n8n para flujos más complejos

#### Criterios de aceptación (TDD):
- Los dashboards se actualizan con datos en tiempo real (máximo 5 min de latencia)
- Los usuarios pueden crear visualizaciones sin asistencia técnica
- Los informes programados se envían puntualmente a destinatarios
- Los datos de diferentes sistemas se integran coherentemente
- Las consultas complejas se ejecutan en menos de 10 segundos

#### Consideraciones de implementación:
- Políticas de acceso a datos sensibles
- Optimización de consultas frecuentes
- Estrategia de archivado para datos históricos

### 11. Gestión de Proyectos y Tareas

#### Tecnologías seleccionadas:
- **Sistema de gestión de proyectos**: Taiga
  - *Justificación*: Plataforma completa que soporta metodologías ágiles y tradicionales con interfaz intuitiva.
  - *User Stories*:
    - Como project manager, quiero seguir progreso de tareas en múltiples proyectos
    - Como equipo, queremos utilizar metodología Kanban/Scrum
    - Como miembro, quiero reportar tiempo dedicado a tareas
    - Como cliente, quiero ver avance de proyecto sin acceder a detalles internos

- **Seguimiento de tiempo**: Kimai (integrado con Taiga)
  - *Justificación*: Sistema especializado en seguimiento de tiempo con informes detallados.

- **Gestión de recursos**: 
  - Módulos de Taiga
  - Integraciones con calendarios (Nextcloud Calendar)

#### Criterios de aceptación (TDD):
- Los proyectos pueden configurarse con diferentes metodologías (Kanban, Scrum)
- Las tareas asignadas generan notificaciones automáticas
- El seguimiento de tiempo se integra con facturación
- Los informes de progreso muestran desviación respecto a planificación
- La visualización de carga de recursos detecta sobreasignaciones

#### Funcionalidades específicas:
- Plantillas para tipos de proyectos comunes
- Automatización de tareas repetitivas
- Notificaciones inteligentes
- Informes de progreso personalizados

## Arquitectura de Integración entre Bloques

Para asegurar una integración efectiva entre todos los bloques funcionales, se implementará:

1. **Capa de API Gateway**:
   - Kong como punto central de gestión de APIs
   - Autenticación centralizada vía Keycloak
   - Monitoring de tráfico entre servicios

2. **Bus de eventos**:
   - NATS para comunicación asíncrona entre servicios
   - Esquemas de eventos normalizados
   - Persistencia para eventos críticos

3. **Sincronización de datos**:
   - Procesos ETL programados para consolidación
   - Webhooks para actualizaciones en tiempo real
   - Cache distribuido para datos frecuentemente accedidos

4. **Portal unificado**:
   - Dashboard de administración centralizado
   - Single Sign-On para todos los servicios
   - Búsqueda federada entre sistemas



## Conclusión

Esta propuesta tecnológica representa una solución completa y viable para implementar una infraestructura empresarial autogestionada, adecuada para empresas emergentes que buscan mantener control total sobre sus datos y servicios. La selección de tecnologías open source maduras y la arquitectura modular permiten una implementación incremental mediante metodología Scrum, priorizando los componentes que aportan mayor valor inmediato.

El proyecto Self Hosted 101 no solo proporciona independencia tecnológica, sino que también sirve como plataforma de aprendizaje sobre infraestructuras modernas basadas en contenedores y microservicios, permitiendo a las organizaciones evolucionar su stack tecnológico de manera sostenible.