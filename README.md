# context.claude

> **Un protocolo desacoplado y asíncrono para interactuar con Claude CLI mediante el sistema de archivos, optimizando tokens y priorizando la arquitectura antes del código.**

---

## La Filosofía

Los asistentes de IA en la terminal son extraordinarios programando, pero el chat convencional tiene fallas fundamentales cuando se trata de ingeniería de software seria:

1. **El desperdicio masivo de tokens:** Mantener una conversación larga en consola reenvía historial redundante, saturando la ventana de contexto y encareciendo cada interacción.
2. **La prisa por codificar:** Ante cualquier instrucción, el modelo suele lanzarse a escribir código sin antes validar la arquitectura, las dependencias ni los efectos secundarios en el proyecto.
3. **El ruido:** Tomar decisiones técnicas en un chat efímero dificulta la auditoría, la revisión por pares y el control de versiones.

**context.claude** cambia el paradigma: deja de tratar al modelo como un chatbot y comienza a tratarlo como un **compilador asíncrono**.

---

## Principios de Diseño

- **Cero charla en consola:** La terminal se reserva estrictamente para señales de control (`continue`, `Accept`). Todo el intercambio técnico ocurre en archivos Markdown.
- **Diseño primero, código después:** La IA está obligada a generar una propuesta técnica acumulada y esperar tu feedback antes de tocar un solo archivo de tu aplicación.
- **Fuente única de la verdad (SSOT):** Cada iteración consolida los requerimientos en un solo documento vivo, eliminando especificaciones fragmentadas.
- **Limpieza y memoria técnica:** Las discusiones temporales se destruyen automáticamente al finalizar el desarrollo, dejando únicamente un resumen consolidado en tu bitácora de sesiones.

---

## Documentación

Para ver el paso a paso de uso, diagramas de flujo y configuración inicial, revisa:

- 📖 **[Guía para Desarrolladores (DOC.MD)](DOC.MD)**: Explicación paso a paso pensada para humanos.
- ⚙️ **[Protocolo y Máquina de Estados (architecture.md)](architecture.md)**: Especificación técnica y directivas del motor.
