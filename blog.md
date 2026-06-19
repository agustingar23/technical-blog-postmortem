Initial technical blog
# Post-Mortem Técnico: Resolución de un incidente durante el despliegue de una API REST

**Autor:** Agustín García  
**Fecha:** Junio 2026

---

# Contexto

Durante el desarrollo de una API REST para un sistema de gestión de clientes, se planificó una actualización cuyo objetivo era incorporar un nuevo sistema de autenticación mediante JSON Web Tokens (JWT) y reorganizar la documentación técnica del proyecto.

Para gestionar el desarrollo se utilizó **GitHub** como sistema de control de versiones, registrando cada cambio mediante commits descriptivos. Antes del despliegue se realizaron pruebas funcionales en un entorno local y los resultados fueron satisfactorios. Sin embargo, una vez publicada la nueva versión en el entorno de pruebas comenzaron a detectarse errores que impedían el correcto funcionamiento del sistema de autenticación.

Aunque el incidente no afectó usuarios finales, permitió realizar un análisis post-mortem orientado a identificar la causa raíz del problema y establecer mejoras para futuros despliegues.

---

# Problema

Luego del despliegue comenzaron a registrarse respuestas **HTTP 500** en los endpoints de autenticación.

Los principales síntomas observados fueron:

- Imposibilidad de iniciar sesión.
- Errores internos del servidor.
- Fallas en la conexión con la base de datos.
- Indisponibilidad temporal del servicio de autenticación.

Después de revisar los registros del sistema se identificó que una variable de entorno necesaria para establecer la conexión con PostgreSQL no había sido configurada correctamente durante el despliegue.

Como consecuencia, la aplicación iniciaba correctamente, pero fallaba al intentar acceder a la base de datos para validar las credenciales de los usuarios.

---

# Análisis Post-Mortem

El objetivo del análisis fue comprender qué ocurrió y cómo evitar que el incidente vuelva a repetirse, dejando de lado la búsqueda de responsables y enfocándose en la mejora continua.

## Causa raíz

La investigación permitió determinar que el problema no se encontraba en el código fuente, sino en el proceso de despliegue.

Durante la revisión previa únicamente se verificó el funcionamiento del código, sin validar aspectos relacionados con la infraestructura y la configuración del entorno.

Además, el equipo no disponía de un checklist que permitiera comprobar elementos críticos como:

- Variables de entorno.
- Credenciales de acceso.
- Configuración de la base de datos.
- Parámetros del servidor.

La ausencia de estas verificaciones permitió que el error llegara al entorno de pruebas.

---

# Acciones realizadas

Para restablecer el funcionamiento del sistema se llevaron a cabo las siguientes acciones:

- Restauración temporal de la versión estable anterior.
- Incorporación de las variables de entorno faltantes.
- Verificación de la conexión con la base de datos.
- Ejecución de pruebas funcionales sobre el módulo de autenticación.
- Revisión completa del procedimiento de despliegue.

Posteriormente se implementaron acciones preventivas para disminuir la probabilidad de incidentes similares:

- Creación de un checklist obligatorio antes de cada despliegue.
- Incorporación de validaciones automáticas mediante GitHub Actions.
- Actualización de la documentación técnica.
- Mejora del proceso de revisión de cambios.

---

# Evidencia del uso de control de versiones

Todo el proceso fue documentado mediante GitHub utilizando commits descriptivos que permiten reconstruir la evolución del trabajo.

Los commits realizados fueron:

- Initial blog structure
- Add incident description
- Document post-mortem analysis
- Add lessons learned
- Include radical candor reflection

El uso de mensajes claros permitió mantener la trazabilidad de cada modificación y facilitar la comprensión del historial del proyecto.

---

# Aprendizajes

Este incidente permitió comprender que una solución tecnológica de calidad depende tanto del desarrollo del software como de una correcta configuración del entorno donde se ejecuta.

Los principales aprendizajes obtenidos fueron:

- La documentación reduce errores repetitivos.
- Los checklists previos al despliegue ayudan a prevenir incidentes.
- El control de versiones facilita la trazabilidad de los cambios.
- Los análisis post-mortem fortalecen la mejora continua.
- La automatización de validaciones disminuye el riesgo de errores humanos.

Más allá de resolver el problema, el proceso permitió mejorar las prácticas de documentación, revisión y despliegue del proyecto.

---

# Reflexión sobre el feedback radicalmente sincero

Durante el análisis del incidente se aplicó el enfoque de **feedback radicalmente sincero**, promoviendo conversaciones abiertas y respetuosas entre los integrantes del equipo.

La discusión se centró en comprender las causas del problema y en identificar oportunidades de mejora, evitando atribuir responsabilidades individuales.

Este enfoque favoreció un ambiente de confianza, permitió compartir diferentes puntos de vista y facilitó la implementación de mejoras tanto técnicas como organizacionales.

---

# Conclusión

El incidente demostró que el desarrollo de software no depende únicamente de escribir código de calidad, sino también de mantener procesos sólidos de documentación, revisión y despliegue.

La utilización de GitHub como herramienta de control de versiones, junto con la elaboración de un análisis post-mortem y la aplicación de feedback radicalmente sincero, permitió transformar un error técnico en una oportunidad de aprendizaje.

Como resultado, el proyecto cuenta ahora con un proceso de trabajo más robusto, una documentación más clara y un conjunto de buenas prácticas que contribuirán a prevenir incidentes similares en futuras versiones.
