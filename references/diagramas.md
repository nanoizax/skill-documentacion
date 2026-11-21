# Diagramas (Mermaid)

Los diagramas son texto versionado junto al documento: fuente en `docs/diagrams/src/*.mmd` y bloque Mermaid incrustado. GitHub y GitLab lo renderizan de forma nativa.

## Qué diagrama para qué pregunta

| Pregunta | Tipo |
|---|---|
| ¿Qué sistemas hablan con cuáles? | `flowchart` (contexto / contenedores) |
| ¿Cómo se ejecuta este caso de uso? | `sequenceDiagram` |
| ¿Cómo son los datos? | `erDiagram` |
| ¿Qué estados tiene un pedido? | `stateDiagram-v2` |
| ¿Cuál es el proceso de negocio? | `flowchart` |
| ¿Cómo se despliega? | `flowchart` con `subgraph` |
| ¿Cómo navega el usuario? | `flowchart` de pantallas |

## Reglas

- [ ] Una idea por diagrama; máximo 6-8 elementos y ~6 relaciones. Si crece, divide.
- [ ] Título y, si hace falta, leyenda. Flechas con verbo y protocolo.
- [ ] Nombres **idénticos** a los del código (módulos, tablas, rutas).
- [ ] Verifica que renderiza: un token inválido rompe el bloque sin avisar. Comprueba en el visor del repo o con el CLI de Mermaid (**verificar en el entorno**).
- [ ] Sin datos sensibles: nada de IPs, hosts internos reales, nombres de clientes, llaves.
- [ ] No dibujes lo que no verificaste; si es hipótesis, rotula "(inferido)".
- [ ] Nivel C4 1 y 2 siempre; 3 solo si aporta; 4 nunca a mano.

## Ejemplos mínimos

### Contexto (C4 nivel 1)

```mermaid
flowchart LR
  U[Cliente final] -->|usa / HTTPS| S[Sistema de pedidos]
  A[Administrador] -->|gestiona / HTTPS| S
  S -->|cobra / HTTPS API| P[(Pasarela de pago)]
  S -->|envía correos / SMTP| M[(Servicio de correo)]
```

### Contenedores (C4 nivel 2)

```mermaid
flowchart LR
  W[Web - SPA] -->|REST JSON| API[API - servicio backend]
  M[App móvil] -->|REST JSON| API
  AD[Panel admin] -->|REST JSON| API
  API -->|SQL| DB[(Base de datos)]
  API -->|encola| Q[[Cola de tareas]]
  Q --> WK[Worker]
  WK -->|SMTP| X[(Correo)]
```

### Secuencia de un flujo clave

```mermaid
sequenceDiagram
  actor U as Usuario
  participant W as Web
  participant A as API
  participant D as Base de datos
  U->>W: Envía el formulario de pedido
  W->>A: POST /orders (token)
  A->>A: Valida datos y permisos
  A->>D: INSERT pedido
  D-->>A: id
  A-->>W: 201 con id
  W-->>U: Muestra "Pedido creado"
  Note over A: Error de validación devuelve 422 con detalle
```

### Modelo de datos (ER)

```mermaid
erDiagram
  USER ||--o{ ORDER : realiza
  ORDER ||--|{ ORDER_ITEM : contiene
  PRODUCT ||--o{ ORDER_ITEM : aparece_en
  USER {
    bigint id PK
    text email UK
  }
  ORDER {
    bigint id PK
    bigint user_id FK
    text status
  }
```

### Estados

```mermaid
stateDiagram-v2
  [*] --> Borrador
  Borrador --> Pagado: pago aprobado
  Pagado --> Enviado: despacho
  Enviado --> Entregado
  Borrador --> Cancelado: usuario cancela
  Entregado --> [*]
```

### Despliegue

```mermaid
flowchart TB
  subgraph Internet
    C[Navegador / App]
  end
  subgraph Servidor
    N[Proxy inverso + TLS] --> B[Proceso backend]
    B --> DB[(Base de datos)]
  end
  C -->|HTTPS 443| N
```

### Navegación de pantallas (manual)

```mermaid
flowchart LR
  L[Inicio de sesión] --> H[Inicio]
  H --> P[Pedidos]
  P --> N[Nuevo pedido]
  N --> P
```

## Mantenimiento

- Un cambio de rutas, tablas o módulos obliga a revisar el diagrama que los nombra.
- PlantUML solo si hace falta algo que Mermaid no da (C4 completo); exige render en CI y guarda también el SVG exportado (**verificar en el entorno**).
- Si el documento se exporta a PDF o DOCX, exporta antes los diagramas a imagen y comprueba que quedaron legibles.
