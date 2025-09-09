# Macchina a stati prodotto RIS

![RIS](RIS.png)

<details>
  <summary>Codice Mermaid</summary>

```
---
config:
  layout: elk
  elk:
    mergeEdges: false
    nodePlacementStrategy: NETWORK_SIMPLEX
  theme: redux
---
stateDiagram-v2
  direction TB

  state NonConsegnato {
    direction TB
    [*] --> Preesito1
    Preesito1 --> Demat1:RECRSI004B [Plico]
    Demat1 --> [*]
  }

  %% Presa in carico
  [*] --> PresaInCarico
  PresaInCarico --> AvviatoEstero:RECRSI001
  PresaInCarico --> Furto:RECRSI005

  AvviatoEstero --> IngressoPaeseEstero:RECRSI002
  IngressoPaeseEstero --> Consegnato:RECRSI003C
  IngressoPaeseEstero --> NonConsegnato:RECRSI004A

  %% Chiusura fascicolo
  NonConsegnato --> [*]:RECRSI004C
  Consegnato --> [*]

  %% Furto
  AvviatoEstero --> Furto:RECRSI005
  Furto --> [*]

  %% Etichette
  AvviatoEstero:Avviato all'estero
  IngressoPaeseEstero:Ingresso nel paese estero
  NonConsegnato:Non consegnato
  Preesito1:Preesito
  Demat1:Demat
  PresaInCarico:Presa in carico
```
</details>