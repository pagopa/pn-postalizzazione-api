# Servizio Consolidatore - Specifica API

Questo documento descrive le specifiche e il flusso di integrazione richiesto per realizzare il servizio di consolidatore secondo lo standard PagoPA.

## Descrizione

Il consolidatore, raggiungibile tramite link diretto privato, espone le seguenti API REST:

- **POST** `/piattaforma-notifiche-ingress/v1/paper-deliveries-engagement`: ricezione richieste di invio corrispondenza cartacea.
- **GET** `/piattaforma-notifiche-ingress/v1/paper-deliveries-progresses/{requestId}`: consultazione avanzamento postalizzazione.

Tutte le API richiedono autenticazione tramite header `x-pagopa-extch-service-id` e `x-api-key`.

## Gestione degli Allegati

Una richiesta di postalizzazione ( e suoi avanzamenti ) possono contenere allegati. Tutti gli allegati sono gestiti all'interno della SEND tramite il servizio SafeStorage. 
In particolare un consolidatore / recapitista sarà in grado di. : 
- acquisire allegati presenti all'interno di una richiesta di postalizzazione 
- caricare allegati che saranno associati ad uno stato di avanzamento


## Flusso Principale

1. **SEND** invia una richiesta di postalizzazione tramite `POST /paper-deliveries-engagement`.
1. Il consolidatore valida e registra la richiesta.
1. Il consolidatore scarica gli allegati tramite le API della piattaforma.
1. Il consolidatore invia notifiche di avanzamento tramite [webhook](./openapi/send2consolidatore-v1.yml).
1. Il consolidatore si integra con lo specifico recapitista per trasmettere la richiesta di postalizzazione
1. Il consolidatore riceve avanzamenti sulla specifica postalizzazione tramite [API](./openapi/recapitista2consolidatore-v1.yml) che inoltra a sua volta a SEND mediante [webhook](./openapi/send2consolidatore-v1.yml).
5. **SEND** può interrogare lo stato tramite `GET /paper-deliveries-progresses/{requestId}`.
paper-replicas-progresses/{requestId}`.

## Sequence Diagram

```mermaid
sequenceDiagram
    participant SEND as SEND
    participant CONS as Consolidatore
    participant REC as Recapitista

    SEND->>CONS: POST /paper-deliveries-engagement (richiesta postalizzazione)
    CONS-->>SEND: 200/400/401/409 (esito presa in carico)

rect LIGHTGREY
note over CONS : Scaricamento Allegati
    CONS->>SEND: GET Scarica allegati (API download allegati)
    SEND-->>CONS: 200 (riferimenti allegato )
    CONS ->> SEND: GET fileUrl
    SEND -->> CONS : 200 ( allegato ) 
end

rect LIGHTGREY
note over CONS : Stampa ed imbustamento

    loop Avanzamento per eventi CONS
    
    opt upload allegato
    CONS ->> SEND : PUT attachment-preload
    SEND -->> CONS : 200 - UploadUrl 
    CONS ->> SEND : Upload Document (UploadUrl,attachment)
    SEND -->> CONS : 200 -OK

    end
        CONS-->>SEND: Webhook avanzamento postalizzazione
        SEND -->> CONS : 200 -OK (Validazione sinte)
    end
end

note over CONS,REC : Invio materialità a recapito   

rect LIGHTGREY
note over CONS,REC : Ricezione e presa in carico eventi di postalizzazione (*)
loop Avanzamento per eventi REC
    
    opt upload allegato
    REC ->> CONS : PUT attachment-preload (**)
    CONS ->> SEND : PUT attachment-preload
    SEND -->> CONS : 200 - UploadUrl 
    CONS -->> REC : 200 - forward UploadUrl
    REC ->> CONS : Upload Document (UploadUrl,attachment) (**)
    CONS ->> SEND : Upload Document (UploadUrl,attachment)
    SEND -->> CONS : 200 -OK
    CONS -->> REC : 200 -OK 

    end
        REC ->> CONS : Webhook avanzamento postalizzazione (**)
        CONS-->>SEND: Webhook avanzamento postalizzazione
    end

end

```

(*) Ogni prodotto postale segue una specifica rendicontazione con i propri eventi e la propria sequenza di eventi. Fare riferimento alla documentazione specifica
* [Atto giudiziario 890](diagrams/890.md)
* [Raccomandata AR nazionale](diagrams/AR.md)
* [Raccomanata Semplice nazionale](diagrams/RS.md)
* [Raccomanata AR Internazionale](diagrams/RIR.md)
* [Raccomandata Semplice Internazionale](diagrams/RIS.md)

(**) Specifica per l'integrazione coi Recapitisiti
