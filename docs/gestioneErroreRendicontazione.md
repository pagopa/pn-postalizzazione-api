## Gestione errori di rendicontazione
E' possibile che gli eventi dei recapitisti veicolati tramite il consolidatore siano contrastanti in base alle informazioni su SEND. I tali casi SEND può , tramite il consolidatore, segnalare un errore di rendicontazione attraverso le seguente API 

```mermaid
sequenceDiagram
    participant SEND as SEND
    participant CONS as Consolidatore
    participant REC as Recapitista


SEND ->> CONS : Scarto
        CONS-->>SEND: 200/400/401/409 (esito presa in carico)
        CONS <<->> REC : Inoltro scarto K
        
    REC ->> REC : Verifica segnalazione
 


note over CONS,REC : Re-inoltro eventi
loop Avanzamento per eventi REC
    
    opt upload allegato
    REC ->> CONS : PUT attachment-preload (*)
    CONS ->> SEND : PUT attachment-preload
    SEND -->> CONS : 200 - UploadUrl 
    CONS -->> REC : 200 - UploadUrl (*)
    REC ->> CONS : Upload Document (UploadUrl,attachment) (*)
    CONS ->> SEND : Upload Document (UploadUrl,attachment)
    SEND -->> CONS : 200 -OK
    CONS -->> REC : 200 -OK (*)

    end
        REC ->> CONS : Webhook avanzamento postalizzazione
        CONS-->>SEND: Webhook avanzamento postalizzazione
    end


```