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
<img width="1146" height="789" alt="image" src="https://github.com/user-attachments/assets/a72701c4-f5a2-4e83-8525-05820eb6a6ac" />
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

