# Servizio Consolidatore - Gestione Rendicontazioni duplicate

Questo documento descrive le specifiche e il flusso di integrazione richiesto poter realizzare il servizio di recupero di rendicontazioni sostitutive alle originali.


## Descrizione

Il consolidatore, raggiungibile tramite link diretto privato, espone le seguenti API REST:

- **POST**  `/piattaforma-notifiche-ingress/v1/paper-replicas-engagement`: richiesta generazione duplicato.
- **GET**  `/piattaforma-notifiche-ingress/v1/paper-replicas-progresses/{requestId}`: consultazione avanzamento richiesta duplicato.

Tutte le API richiedono autenticazione tramite header `x-pagopa-extch-service-id` e `x-api-key`.


