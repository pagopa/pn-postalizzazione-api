# Allegato Tecnico Recapitista 

Questa sezione descrive le specifiche e il flusso di integrazione richiesto per l'integrazione dei servizi di recapito connessi alla Piattaforma SEND.

Nella documentazione vegnono usate le keyword \[FASE2\] e \[FASE3\] per indicare che l'implementazione di API complessive o la specifica di elementi aggiuntivi nelle interazioni seguiranno una pianficazione dedicata.
Si distiguono da tutti gli elementi non marcati o taggati con \[FASE1\] che dovranno essere garantiti con la prima integrazione.

## Servizi di gestione del recapito
Per tutti i servizi legati al mondo del recapito si faccia riferimento ai seguenti documenti:
* [gestionePostalizzazione.md](docs/gestionePostalizzazione.md) - descrive l'integrazione con il processo di postalizzazione (stampa ed imbustamento, invio a recapito e successiva rendicontazione)
* [gestioneErroreRendicontazione.md](docs/gestioneErroreRendicontazione.md) - descrive la modalità di gestione degli errori generati da SEND in fase di validazione delle rendicontazioni ed il processo di recupero \[FASE2\]
* [gestioneRendicontazioniSostitutive.md](docs/gestioneRendicontazioniSostitutive.md) - descrive il processo di recupero di rendicontazioni sostitutive alle originali \[FASE3\]

Per completezza nella descrizione di tutti i flussi viene riportata anche l'integrazione tra SEND ed il Consolidatore. L'oggetto della specifica resta comunque l'integrazione con il Consolidatore

## Servizi Accessori
Questa sezione descrive i servizi accessori previsti dal capitolato

### Servizio di blocco di invio al recapito
Le specifiche API verranno definite a valle dell'aggiudicazione della gara - \[FASE3\]

### Servizio di invio al macero di materialità
Le specifiche API verranno definite a valle dell'aggiudicazione della gara - \[FASE3\]

### Servizio per la trasmissione della capacità produttiva
Le specifiche API verranno definite a valle dell'aggiudicazione della gara - \[FASE3\]