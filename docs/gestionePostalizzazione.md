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
1. Il consolidatore invia notifiche di avanzamento tramite [webhook](./openapi/send-ec-v1.yml).
1. Il consolidatore si integra con lo specifico recapitista per trasmettere la richiesta di postalizzazione
1. Il consolidatore riceve avanzamenti sulla specifica postalizzazione tramite l'integrazione descritta nel -documento tbd- che inoltra a sua volta a SEND mediante [webhook](./openapi/send-ec-v1.yml).
5. **SEND** può interrogare lo stato tramite `GET /paper-deliveries-progresses/{requestId}`.
paper-replicas-progresses/{requestId}`.

### Sequence Diagram

```mermaid
sequenceDiagram

    participant SEND as SEND
    participant CONS as Consolidatore
    participant REC as Recapitista

    SEND->>CONS: POST sendPaperEngageRequest
    CONS-->>SEND: 200


rect rgba(204, 210, 211, 0.22)
note over CONS : Scaricamento Allegati
    CONS->>SEND: GET getFile (API download allegati)
    SEND-->>CONS: 200 (presignedFileUrl )
    CONS ->> SEND: GET presignedFileUrl
    SEND -->> CONS : 200 ( allegato ) 
end

rect rgba(204, 210, 211, 0.22)
note over CONS : Stampa ed imbustamento

    loop Avanzamento per eventi CONS
    
    opt upload allegato
    CONS ->> SEND : PUT presignedUploadRequest
    SEND -->> CONS : 200 - presignedUploadUrl 
    CONS ->> SEND : POST presignedUploadUrl (attachment)
    SEND -->> CONS : 200 -OK

    end
        CONS-->>SEND: PUT sendPaperProgressStatusRequest
        SEND -->> CONS : 200 -OK (Validazione sinte)
    end
end

note over CONS,REC : Invio materialità a recapito   

rect rgba(204, 210, 211, 0.22)
note over CONS,REC : Processo di rendicontazione (*)
loop Avanzamento per eventi REC

    opt upload allegato
    note over CONS,REC : Rendicontazione evidenze recapito (**)
    CONS ->> SEND : PUT presignedUploadRequest
    SEND -->> CONS : 200 - presignedUploadUrl 
    CONS ->> SEND : Upload Document (UploadUrl,attachment)
    SEND -->> CONS : 200 -OK

    end
    note over CONS,REC: Rendicontazione eventi recapito (**)
    CONS-->>SEND: PUT sendPaperProgressStatusRequest
    end

end
```

(*) Ogni prodotto postale segue una specifica rendicontazione con eventi dedicati, sequenze dedicate ed evidenze di recapito. Fare riferimento alla documentazione specifica
* [Atto giudiziario 890](diagrams/890.md)
* [Raccomandata AR nazionale](diagrams/AR.md)
* [Raccomanata Semplice nazionale](diagrams/RS.md)
* [Raccomanata AR Internazionale](diagrams/RIR.md)
* [Raccomandata Semplice Internazionale](diagrams/RIS.md)


Per gli approfondimenti sugli stati fare riferimento alle appendici:
* [statusCode](appendices/PaperProgressStatusEvent_statusCode.csv)
* [documentType](appendices/PaperProgressStatusEvent_documentType.csv)
* [deliveryFailureCause](appendices/PaperProgressStatusEvent_deliveryFailureCause.csv)

(**) Fare riferimento alla specifica di integrazione tra Consolidatore e Recapitisiti descritta nell'appendice [Consolidatore-Integrazione_sistemi_Postalizzazione_e_Recapito](docs/appendices/Consolidatore/Consolidatore-Integrazione_sistemi_Postalizzazione_e_Recapito.docx)
