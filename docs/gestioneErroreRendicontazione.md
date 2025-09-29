## Gestione errori di rendicontazione
E' possibile che gli eventi dei recapitisti veicolati tramite il consolidatore siano contrastanti in base alle informazioni su SEND. 
In tali casi SEND può, tramite il consolidatore, segnalare un errore di rendicontazione attraverso una specifica API secondo il flusso qui descritto.
La specifica API verrà definita a valle dell'aggiudicazione della gara.

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