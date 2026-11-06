# 🏦 Coink Analytics + 🎭 Chatbot con Personalidad - Actividad 6

**Desarrollado por:** Miguel Angel Muete

Este es mi proyecto doble desarrollado en Python con **Streamlit**. Está dividido en dos partes principales:

1. **🏦 Coink Analytics:** Un dashboard interactivo donde analizo los datos de depósitos en las máquinas OINK de la fintech Coink y calculo una métrica propia llamada **Coink Score** para clasificar a los usuarios.
2. **🎭 Chatbot con Personalidad:** Un bot interactivo que imita la forma de hablar y la personalidad de 5 compañeros específicos (**Karen, Lian, Paulo, Davan Garzón y Paula**), con opción de chat de texto y voz.

---

## 📋 Tabla de contenido
- 🏦 Parte 1: Coink Analytics
- 🎭 Parte 2: Chatbot con Personalidad
- 4. Vigilancia Tecnológica
- 5. Servicios de Nube
- 🚀 Instalación y Uso

---

## 🏦 Parte 1: Coink Analytics

En esta parte construí un tablero para visualizar el comportamiento de ahorro de los usuarios en la fintech colombiana Coink.

### ¿Qué es Coink y cómo funciona un OINK?
Coink es una app de ahorro que utiliza máquinas físicas llamadas **OINKs** (alcancías gigantes que funcionan como cajeros al revés). El usuario mete su efectivo (monedas y billetes) y la máquina le abona ese dinero inmediatamente a su saldo digital en la aplicación.

### El flujo de procesamiento de un OINK
Diseñé el siguiente flujo lógico para explicar todo el proceso desde que el usuario inserta la moneda hasta la gestión de sus retiros:

```text
┌─────────────────────────────────────────────┐
│ ( INICIO ) Usuario inserta moneda en OINK   │
└─────────────────────────────────────────────┘
                    │
                    ▼
        ◇ ¿Sensor valida la moneda? ◇
         │No                    │Sí
         ▼                      ▼
   [Devolver moneda]    [Clasificador cuenta]
   [ + registrar  ]     [ denominación     ]
   [   intento    ]            │
         │                     ▼
         │            [Usuario se autentica]
         │            [  QR / Huella / Cédula]
         │                     │
         │                     ▼
         │          ◇ ¿API Coink valida? ◇
         │           │No           │Sí
         │           ▼             ▼
         │      [Rechazar]   [Backend crea]
         │      [notificar]  [registro depósito]
         │           │             │
         │           │             ▼
         │           │     [( PostgreSQL ACID )]
         │           │             │
         │           │             ▼
         │           │     [Actualizar saldo]
         │           │     [billetera digital]
         │           │             │
         │           │             ▼
         │           │     [Publicar Kafka]
         │           │             │
         │           │             ▼
         │           │     [App recibe push]
         │           │             │
         │           │             ▼
         │           │     [Usuario ve saldo]
         │           │             │
         │           │             ▼
         │           │   ◇ ¿Desea retirar? ◇
         │           │    │No         │Sí
         │           │    │           ▼
         │           │    │   [Selecciona monto]
         │           │    │   [ y método      ]
         │           │    │           │
         │           │    │           ▼
         │           │    │  ◇ ¿Saldo suficiente? ◇
         │           │    │   │No        │Sí
         │           │    │   ▼          ▼
         │           │    │ [Error]  [Validar]
         │           │    │ [saldo]  [antifraude]
         │           │    │   │          │
         │           │    │   │          ▼
         │           │    │   │   [Pasarela pagos]
         │           │    │   │   [  PSE / ACH  ]
         │           │    │   │          │
         │           │    │   │          ▼
         │           │    │   │   [Debitar billetera]
         │           │    │   │          │
         │           │    │   │          ▼
         │           │    │   │   [Confirmar usuario]
         │           │    │   │          │
         ▼           ▼    ▼   ▼          ▼
        ┌─────────────────────────────────────┐
        │           ( FIN )                   │
        └─────────────────────────────────────┘

<img width="900" height="1600" alt="image" src="https://github.com/user-attachments/assets/dad74bc9-f8f9-4bc0-996b-4a4e5fd12b1a" />

Opinión propia: Yo entiendo que modelar este flujo no es solo trazar líneas de procesos, sino comprender que la arquitectura física de una máquina OINK debe integrarse de forma síncrona con bases de datos transaccionales. Si el hardware falla en validar una denominación, todo el pipeline se rompe.Mi fórmula: Coink ScoreDiseñé un algoritmo ponderado para calificar el hábito de ahorro de cada usuario:$$\text{Coink\_Score} = (0.30 \times \text{Frecuencia}) + (0.30 \times \text{Monto Total}) + (0.20 \times \text{Ticket Promedio}) + (0.20 \times \text{Antigüedad})$$Rangos: 🥇 Oro (≥ 70) | 🥈 Plata (40-69) | 🥉 Bronce (< 40).¿Qué base de datos le conviene a Coink?PostgreSQL (Relacional): Obligatoria para garantizar ACID en el manejo del dinero y cuentas.MongoDB (NoSQL): Ideal para la telemetría IoT y registro masivo de eventos de las máquinas OINK.🎭 Parte 2: Chatbot con PersonalidadDesarrollé un agente conversacional basado en perfiles reales de compañeros de clase, integrando un enfoque analítico:CompañeroCanciónPelículaDeporteMateriaEnfoque Innovador (Yo entiendo esto por esto)KarenLa CherryEl viaje de ChihiroNingunoMatemáticasSu mentalidad opera con la eficiencia de un algoritmo optimizado: procesamiento directo sin rodeos.LianBlinding LightsSpider-ManParkourFísicaSu arquitectura mental funciona como un sistema dinámico en tiempo real con precisión cinética.PauloNumbMatrixFútbolProgramaciónSu estructura de pensamiento es modular, depurando problemas con precisión de red neuronal.Davan GarzónEye of the TigerInterestelarCiclismoCálculo / BDActúa como el nodo principal de procesamiento lógico coordinando estrategias con persistencia.PaulaHawáiEn el corazón...VoleibolMultimediaEquilibra la creatividad visual con la exactitud técnica en flujos impecables.4. Vigilancia TecnológicaCajeros con depósito seguro (US20100247000): Sellado automático del contenedor para evitar robos internos (BBDD Relacional para auditoría).Cassettes con NFC inalámbrico (WO2015082346A1): Telemetría de energía y estado mediante inducción (BBDD NoSQL).ATM con API para terceros (US11494747): Ejecución de apps externas de forma aislada y segura.Reconocimiento biométrico (US9697698): Cámaras y sensores de seguridad física para bloquear intentos no autorizados.Gestión de efectivo integrada (WO2017042618A1): Reciclaje de pagos en comercios para recargar cajeros cercanos (Data Warehouse).5. Servicios de Nube para Bases de DatosTransacciones (ACID): AWS Aurora / Azure SQL / Oracle Autonomous.IoT / Telemetría: AWS DynamoDB / GCP Bigtable.Analítica: GCP BigQuery / AWS Redshift.
