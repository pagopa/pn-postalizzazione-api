## Gestione errori di rendicontazione
E' possibile che gli eventi dei recapitisti veicolati tramite il consolidatore siano contrastanti in base alle informazioni su SEND. 
In tali casi SEND può, tramite il consolidatore, segnalare un errore di rendicontazione attraverso una specifica API secondo il flusso qui descritto.
La segnalazione viene effettuata tramite l'operazione 
`sendPaperDeliveryReconciliationErrorReport` (`POST /piattaforma-notifiche-ingress/v1/paper-deliveries-recon-error-report`), 
definita in [consolidatore-v1.yml](/Users/giacomo.vallorani/src/pn-postalizzazione-api/docs/openapi/consolidatore-v1.yml), 
a cui SEND comunica il `requestId` della spedizione impattata insieme ai dettagli dell'errore riscontrato, così 
che il consolidatore possa inoltrare la segnalazione al recapitista e automatizzare il processo di re-invio degli eventi corretti.

```mermaid
sequenceDiagram
    participant SEND as SEND
    participant CONS as Consolidatore
    participant REC as Recapitista


SEND ->> CONS : Scarto
        CONS-->>SEND: 200/400/401/409 (esito presa in carico)
        CONS ->> REC : Inoltro scarto K
        
    REC ->> REC : Verifica segnalazione
 


note over CONS,REC : Re-inoltro eventi
loop Avanzamento per eventi REC

    opt upload allegato
    note over CONS,REC : Rendicontazione evidenze recapito (*)
    CONS ->> SEND : PUT presignedUploadRequest
    SEND -->> CONS : 200 - presignedUploadUrl 
    CONS ->> SEND : Upload Document (UploadUrl,attachment)
    SEND -->> CONS : 200 -OK

    end
    note over CONS,REC: Rendicontazione eventi recapito (*)
    CONS-->>SEND: Webhook avanzamento postalizzazione
    end

```

(*) Fare riferimento alla specifica di integrazione tra Consolidatore e Recapitisiti