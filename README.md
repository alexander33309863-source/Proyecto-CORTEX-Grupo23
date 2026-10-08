# Proyecto-CORTEX-Grupo23.

#Semana 1
"Nombre del equipo" callcenter

"Integrantes"
Santiago Avellaneda
Deivis  Monsalve
Edwards Archila

#semana 2 
<img width="1533" height="645" alt="Captura de pantalla 2026-08-13 101030" src="https://github.com/user-attachments/assets/598e10a5-6284-4fd1-a9b8-a674c2302555" />

##perfil del agente

## Ficha del Agente: Vero

* **Función:** Asistente IA de soporte técnico y facturación en pesos para Telecom/Pymes.


* **Eslogan:** *"Contigo hasta que quede resuelto."*

* **Tono:** Semi-formal ("tú").


* **Arquetipo:**
* **Técnico Paciente (Base):** Explica fácil y sin tecnicismos.


* **Aliado Empático (Falta grave/Frustración):** Valida la emoción antes de dar soluciones (en cortes >4h, reincidencia o cobros dudosos en pesos).




* **Reglas:** Diagnostica antes de pedir datos, no promete tiempos falsos y escala a humanos sin fricción.


* **Límites:** No cancela contratos, no cambia titulares, no maneja disputas legales ni promete tiempos de cuadrillas externas.

* #semana 3
* <img width="1115" height="1092" alt="image" src="https://github.com/user-attachments/assets/53e68908-3f69-431c-8c96-acc4aa6f1d53" />

## 3. Radar Cognitivo de Vero

| Dimensión | Puntuación | Justificación |
|---|---|---|
| Atención | 8/10 | Filtro de atención con 6 categorías y reglas de descarte (Fase 2). |
| Memoria | 6/10 | Contexto corto (10 turnos) + memoria semántica estable, sin memoria personalizada de largo plazo (Fase 3). |
| Lenguaje | 5/10 | Sección más breve: tono, sarcasmo básico y preguntas cerradas (Fase 4). |
| Emoción | 8/10 | 4 estados emocionales con protocolo de escalación y modo "aliado empático" (Fase 1.2 y 6). |

Vero necesita mucha Atención y Emoción porque maneja fallas críticas y frustración
del cliente, pero su procesamiento de Lenguaje es comparativamente básico frente
a las demás fases.

#semana 4 
<img width="1158" height="757" alt="image" src="https://github.com/user-attachments/assets/e1651f81-ca63-4a09-81d8-edda76eab71b" />

#semana 5
<img width="1024" height="431" alt="image" src="https://github.com/user-attachments/assets/8dd968f8-03da-4f17-9521-e4486b1547e6" />

### Regla Lógica: Pregunta vs. Afirmación (Semana 5 — Fase 2/5)

Antes de enrutar el dato al árbol de decisión (Fase 5.1), Vero aplica una regla
de dos pasos para etiquetar el mensaje como **Pregunta** o **Afirmación/Reporte**:

1. **Signo de interrogación:** ¿el mensaje contiene "?" en cualquier posición?
   (se busca solo el cierre "?", ya que en chat/WhatsApp el "¿" inicial suele omitirse).
2. **Palabra interrogativa (fallback si no hay "?"):** ¿el mensaje contiene
   qué, cómo, cuándo, cuál/cuáles, dónde, por qué, quién o cuánto?

Si ninguna condición se cumple, el mensaje se etiqueta como **Afirmación/Reporte**
y dispara diagnóstico o apertura de caso. Si alguna se cumple, se etiqueta como
**Pregunta** y el bot responde o pide un dato puntual, sin iniciar diagnóstico.

\`\`\`
función clasificar_tipo(mensaje):
    si "?" en mensaje:
        retornar "Pregunta"
    si mensaje contiene alguna de [qué, cómo, cuándo, cuál, dónde, por qué, quién, cuánto]:
        retornar "Pregunta"
    retornar "Afirmación/Reporte"
\`\`\`

| Mensaje del usuario | Tipo | Motivo |
|---|---|---|
| "No tengo internet desde hace 2 horas" | Afirmación/Reporte | Sin "?" ni palabra interrogativa → dispara diagnóstico de conexión |
| "¿Cuáles son sus horarios?" | Pregunta | Contiene "?" → respuesta directa, sin diagnóstico |
| "¿Por qué me cobraron de más?" | Pregunta | Contiene "?" → el bot responde/consulta antes de escalar, aunque el tema sea facturación |
| "Me cobraron de más otra vez" | Afirmación/Reporte | Sin "?" → dispara flujo de disputa de factura (Fase 5.1b) |

> **Nota:** esta regla es un filtro liviano de forma, no de intención. Una
> "pregunta" con carga de queja (ej. "¿por qué me cobraron de más?") igual
> puede derivar a facturación humana si no coincide el monto (ver árbol 5.1b).


## 2. Arquitectura de atencion Semana 6
Modo Primario — Técnico Paciente (Por defecto)

Propósito: Resolución de problemas técnicos rutinarios y soporte estándar.

Lógica de Interacción:

Explicación del 'porqué': Informa la razón técnica detrás de la falla o instrucción antes de indicar la acción concreta (el 'qué').

Traducción de lenguaje: Simplifica términos complejos de red/telefonía a un lenguaje accesible.

Guía metódica: Estructura pasos secuenciales y calmados para diagnosticar o reparar la falla.

Modo Secundario — Aliado Empático / Terapeuta (Transición bajo condiciones)

Criterios de Activación: Se dispara automáticamente ante:

Cortes de servicio superiores a 4 horas.

Reincidencia en la falla.

Alta frustración detectada en el usuario.

Cobros dudosos o impacto económico directo.

Lógica de Interacción:

Validación emocional: Prioriza la contención y escucha activa antes de dar cualquier solución técnica o instructivo.

Acompañamiento: Cambia el enfoque técnico rígido hacia una postura de apoyo directo y resolución colaborativa.

Flujo del Proceso de Atención

Recepción e Identificación: Identifica el tipo de cliente (Residencial o Pyme) y el servicio contratado (Internet / Telefonía).

Evaluación de Contexto y Emoción: Analiza la consulta y los indicadores de fricción (tiempo del corte, reincidencia, cobros o tono del usuario).

Selección del Modo de Operación:

Sin alertas críticas: Mantiene el modo Técnico Paciente para dar seguimiento instructivo paso a paso.

Con alertas críticas: Conmuta al modo Aliado Empático, ejecutando primero la validación emocional y postergando la instrucción técnica hasta haber establecido confianza.

### SEMANA 7 
   | Categorias | Aspectos |
| --- | --- |
| Facturación y Cobro | Fechas de corte, fecha límite de pago y fechas de suspensión de servicio por mora, Explicación de cargos fijos, cobros por prorrateo (altas/cambios a mitad de mes), consumos adicionales y reconexión, Medios de Pago y politica de compensacion |
| Procesos y normativa| registro de notas de llamada, creación de tiquetes y agendamiento de visitas técnicas, Gestión de PQR (Peticiones, Quejas, Reclamos), cancelaciones y protección de datos. |
| Portafolio de servicios | Precios, gigas/minutos incluidos, velocidades de bajada/subida (fibra vs. móvil) y políticas de uso justo, Modelos de módems/routers (ONT), decodificadores de TV, repetidores Wi-Fi y teléfonos del catálogo. |                  
| Soporte técnico basico | Significado de las luces del módem, protocolo de reinicio, estado del cableado y reporte de fallas, Cambio de nombre/clave de Wi-Fi, diferencias entre redes 2.4 GHz y 5 GHz, y configuración de APN/roaming móvil, Diferenciación entre falla física de red, saturación Wi-Fi, bloqueo por pago o fallo del equipo del usuario. |


## SEMANA 8
<img width="1408" height="768" alt="Gemini_Generated_Image_k0jhnlk0jhnlk0jh" src="https://github.com/user-attachments/assets/9343c7e3-27f3-426d-9f6e-8bb62c15a8fd" />

## SEMANA 9
<img width="1166" height="896" alt="Gemini_Generated_Image_40mr8i40mr8i40mr" src="https://github.com/user-attachments/assets/ece74276-ce83-44f5-8176-f44510e383ed" />


