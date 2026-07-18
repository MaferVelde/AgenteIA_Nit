# 🤖 Bot Nit - Asistente de Atención al Estudiante 🧵✨

¡Bienvenido al repositorio oficial de **Nit**, el Agente de Inteligencia Artificial diseñado exclusivamente para la **Academia de Alta Costura y Confección** en Los Mochis, Sinaloa! 🇲🇽📐

Nit actúa como el primer frente de interacción con los alumnos, resolviendo de forma autónoma, precisa y amable cualquier duda sobre reglamentos internos, costos, políticas de reembolso, uso de la plataforma educativa y programas de becas.

---

## 📋 Índice
* [Sobre el Proyecto](#-sobre-el-proyecto)
* [Tecnologías Utilizadas](#-tecnologías-utilizadas)
* [Arquitectura del Agente](#-arquitectura-del-agente)
* [Guía de Configuración Local](#-guía-de-configuración-local)
* [Deployment Guide (Railway)](#-deployment-guide-railway)
* [Uso del Bot](#-uso-del-bot)
* [Políticas de Seguridad del Agente](#-políticas-de-seguridad-del-agente)

---

## 💡 Sobre el Proyecto

El objetivo principal de Nit es optimizar la atención de la comunidad estudiantil. A través de un flujo automatizado en **n8n**, el bot recibe mensajes vía Telegram, consulta una base de conocimientos indexada en vectores dentro de **Supabase** y genera respuestas contextualizadas utilizando modelos avanzados de **OpenAI**.

---

## 🛠️ Tecnologías Utilizadas

El ecosistema tecnológico detrás de Nit incluye:
*   **[n8n](https://n8n.io/)**: Motor principal de automatización de flujos visuales (Workflow Automation).
*   **[LangChain](https://www.langchain.com/)**: Framework de orquestación para agentes de IA integrados.
*   **[OpenAI (GPT-5 Mini)](https://openai.com/)**: Modelo de lenguaje (LLM) encargado del procesamiento natural y respuestas del agente.
*   **[Supabase (Vector Store)](https://supabase.com/)**: Base de datos Postgres con extensión `pgvector` para el almacenamiento de los fragmentos de documentos técnicos.
*   **[PostgreSQL](https://www.postgresql.org/)**: Gestión y persistencia del historial de conversación (Chat Memory).
*   **[Telegram Bot API](https://core.telegram.org/bots)**: Canal de comunicación e interfaz de usuario para los estudiantes.
*   **[Railway](https://railway.app/)**: Plataforma en la nube utilizada para el despliegue del motor de n8n.

---

## 🧠 Arquitectura del Agente

El bot procesa la información en base al siguiente flujo lógico dentro de n8n:
1.  **Trigger**: Recibe el mensaje del estudiante en Telegram.
2.  **Memoria**: Consulta los últimos 10 mensajes del historial en PostgreSQL usando el Chat ID de Telegram para mantener el contexto.
3.  **RAG (Generación Aumentada por Recuperación)**: Utiliza `text-embedding-ada-002` para buscar coincidencias exactas en la tabla `documents` de Supabase.
4.  **Agente de IA**: Orquesta las herramientas, evalúa los datos recuperados y redacta la respuesta final.
5.  **Output**: Regresa la solución al usuario final a través de Telegram de forma instantánea.

---

## 🚀 Guía de Configuración Local

Si deseas replicar o editar este flujo en tu propia instancia local de n8n:

### Prerrequisitos
*   Tener **n8n** instalado localmente o una cuenta en n8n Cloud.
*   Tokens y credenciales activas de:
    *   Telegram Bot API (`HTTP API Token`)
    *   OpenAI API Key
    *   Supabase (URL del proyecto y Service Role API Key)
    *   PostgreSQL Connection String

### Instalación
1.  Clona este repositorio:
    ```bash
    git clone [https://github.com/TU_USUARIO/bot-nit-academia.git](https://github.com/TU_USUARIO/bot-nit-academia.git)
    ```
2.  Abre tu panel de n8n.
3.  Crea un nuevo flujo de trabajo (Workflow).
4.  Haz clic en las opciones del flujo (esquina superior derecha) y selecciona **Import from File**.
5.  Selecciona el archivo ubicado en `workflows/bot_nit.json`.
6.  Configura tus credenciales correspondientes en cada nodo iluminado en rojo o con advertencia.
7.  Activa el flujo en la esquina superior derecha (**Active: True**).

---

## ☁️ Deployment Guide (Railway)

La infraestructura del backend de n8n está desplegada y operando de forma continua a través de la plataforma **Railway**.

*   **URL de Producción**: `https://primary-production-01644.up.railway.app/`

### Pasos para Re-despliegue en Railway:
1.  Crea un nuevo proyecto en Railway conectando tu repositorio de GitHub o usando la plantilla comunitaria de n8n.
2.  Configura las siguientes Variables de Entorno obligatorias en Railway:
    *   `N8N_ENCRYPTION_KEY`: Clave única para proteger tus credenciales.
    *   `WEBHOOK_URL`: Debe coincidir con tu dominio asignado por Railway (`https://primary-production-01644.up.railway.app/`).
    *   `PORT`: `5678`
3.  Asegúrate de que los Webhooks de Telegram se actualicen automáticamente al cambiar el estado del flujo a activo.

---

## 💬 Uso del Bot

Los alumnos interactúan con el bot de la siguiente forma:
*   **Consulta de Costos**: *"¿Cuánto cuesta la inscripción y la mensualidad del curso de corte?"* -> El bot responderá detallando la inscripción de \$500.00 MXN y mensualidad de \$800.00 MXN en base a los registros oficiales.
*   **Dudas de Políticas**: *"¿Qué pasa si pago el día 7 del mes?"* -> Informará sobre el recargo automático e irrevocable de \$50.00 MXN aplicable a partir del día 6.
*   **Límites de Información**: Si un estudiante solicita datos fuera del alcance institucional, Nit responderá amablemente redirigiendo a soporte humano (`soporte@escuela.com`).

---

## 🛡️ Políticas de Seguridad del Agente

Para garantizar la veracidad de la información, el prompt del sistema le prohíbe estrictamente al bot:
1.  **Inventar políticas** o excepciones que no se encuentren dentro de la documentación cargada en Supabase.
2.  **Omitir la consulta** a la base de vectores; está obligado a verificar la información en Supabase antes de emitir cualquier criterio técnico.
