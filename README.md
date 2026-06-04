# Analisis Integral Voximplant

**BaldeCash** | Periodo: Marzo - Mayo 2026

> [Ver Dashboard](https://miguelbaldecash.github.io/Analisis-Integral-Voximplant/)

---

## Que es este reporte

Analiza la efectividad del canal Voximplant (llamadas PSTN + WhatsApp HSM) en la cobranza de BaldeCash. Responde a preguntas como:

- Cuantas llamadas se realizan y cuantas conectan?
- Cuantos mensajes WhatsApp se entregan?
- Que porcentaje de morosos contactados termina pagando?
- Cual subcanal es mas efectivo: llamadas o WhatsApp?
- Como rinden los gestores individuales?

## Subcanales

| Subcanal | Descripcion | Periodo |
|---|---|---|
| **Llamadas PSTN** | Dialer predictivo, campanas de cobranza | Mar-May 2026 |
| **WhatsApp HSM** | Templates de cobranza via Voximplant Kit | Feb-May 2026 |

## Secciones del reporte

### Resumen ejecutivo
KPIs principales + graficos de barras (llamadas por mes, contestadas por mes).

### Seccion 1: Llamadas PSTN
- Metricas por mes: 256,066 llamadas, 6% tasa de contacto
- Contactos efectivos, duracion, intentos por cliente
- Composicion de mayo: 71K sin campaign, 37K con campaign, 843 entrantes
- Distribucion por producto y tramo de mora (marzo, CSV)
- Volumen API vs factura

### Seccion 2: Mensajes WhatsApp HSM
- Volumen y tasa de aceptacion (96.4% promedio)
- Interaccion con asesores: chats, perdidos, tiempo de resolucion
- Templates por tipo (moderada_v2, moderada, gestores)

### Seccion 3: Costo del canal
- Desglose de factura Voximplant (Kit + PSTN + WA + AMD + otros)
- Costo unitario por llamada y por mensaje
- Anomalia de tarifa WA HSM (marzo $0.002 vs abril $0.028)

### Seccion 4: Embudo de conversion
- **Embudo PSTN:** Llamadas -> contestaron -> efectivos -> compromisos -> pagos morosos
- **Embudo WA HSM:** HSM aceptados -> clientes unicos -> chats -> compromisos -> pagos morosos
- Graficos de embudo visuales (abril)
- Gestiones de cobranza por canal (Blip, Llamada, Voximplant, WA Web)
- Nota: correlacion, no causalidad (traslape entre canales)

### Seccion 5: Efectividad comparada
Llamadas PSTN vs WhatsApp HSM lado a lado: volumen, tasa de contacto, interaccion, costo.

### Seccion 6: Desglose por gestor
- Llamadas por gestor PDS (Yosmar, Jahayra, Lidy, Julio, Rita, Liz, Janet)
- Abandonadas y perdidas por gestor
- Tiempo promedio de atencion
- Llamadas outbound directas por agente

### Seccion 7: Hallazgos y recomendaciones
8 hallazgos clave + 5 recomendaciones priorizadas.

## Fuentes de datos

| Fuente | Datos | Metodo |
|---|---|---|
| **Voximplant API** (searchCalls) | Llamadas PSTN mar-may | API paginada con cursor |
| **DB** (voximplant_whatsapp_logs) | WA HSM enviados/aceptados/fallidos | Query directo |
| **DB** (gestion, pago, compromiso, moroso_historico) | Embudo: gestiones, pagos, compromisos | Cruce por solicitud_id |
| **Voximplant Kit dashboard** | Chats, perdidos, tiempo resolucion | Screenshots |
| **Reportes PDS** (Excel) | Colas por gestor, abandonadas, handle time | Exportacion dashboard |
| **Facturas Voximplant** | Costos desglosados mar/abr | Documentos de facturacion |

## Metodologia

### Embudo de conversion
- Se filtran solo pagos de solicitudes en `moroso_historico` (clientes en mora)
- Se cruzan con solicitudes contactadas por cada canal en el mes
- Pagos buscados dentro del mes + 7 dias
- **No implica causalidad:** un mismo moroso es contactado por multiples canales, los porcentajes se traslapan

### Llamadas PSTN
- API `searchCalls` captura todas las llamadas (salientes + entrantes)
- El endpoint `agentCampaigns` solo cuenta campaign items del dialer (subconjunto)
- Clasificacion: Contestada (duration > 0 o Call_Answered), Voicemail (AMD), Rechazada, Otra

### Gestores PDS
- Datos de reportes Excel exportados del dashboard Voximplant Kit
- Colas agrupadas por nombre de gestor
- Handle time es promedio ponderado de todas las colas del gestor

## Limitaciones

- Llamadas ene/feb no extraidas via API (solo datos de factura/recargas)
- Mayo WA HSM: datos hasta el 26, no mes completo
- Embudo: correlacion entre canales, no causalidad directa
- Producto y mora solo disponibles para marzo (CSV con metadata)
- Anomalia de costo WA HSM marzo vs abril sin resolver (posible trial)
- Gestores PDS: "Handled Calls" siempre 0, la actividad real se mide por total_calls en las colas

## Archivos

| Archivo | Descripcion |
|---|---|
| `index.html` | Dashboard principal (auto-contenido, sin dependencias externas excepto Chart.js CDN) |

---

*Generado: Junio 2026 | Balde K S.A.C.*
