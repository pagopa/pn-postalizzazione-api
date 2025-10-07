# Allegato Tecnico Consolidatore 

Questa sezione descrive le specifiche e il flusso di integrazione richiesto per realizzare i Servizi a monte e a valle del recapito connessi alla Piattaforma SEND.

Nella documentazione vegnono usate le keyword \[FASE2\] e \[FASE3\] per indicare che l'implementazione di API complessive o la specifica di elementi aggiuntivi nelle interazioni seguiranno una pianficazione dedicata.
Si distiguono da tutti gli elementi non marcati o taggati con \[FASE1\] che dovranno essere garantiti con la prima integrazione.

## Servizi di gestione del recapito
Per tutti i servizi legati al mondo della stampa e del recapito si faccia riferimento ai seguenti documenti:
* [gestionePostalizzazione.md](docs/gestionePostalizzazione.md) - descrive l'integrazione con il processo di postalizzazione (stampa ed imbustamento, invio a recapito e successiva rendicontazione)
* [gestioneErroreRendicontazione.md](docs/gestioneErroreRendicontazione.md) - descrive la modalità di gestione degli errori generati da SEND in fase di validazione delle rendicontazioni ed il processo di recupero \[FASE2\]
* [gestioneRendicontazioniSostitutive.md](docs/gestioneRendicontazioniSostitutive.md) - descrive il processo di recupero di rendicontazioni sostitutive alle originali \[FASE3\]

## Servizi Accessori
Questa sezione descrive i servizi accessori previsti dal capitolato

### Servizi di normalizzazione e deduplica indirizzi
Per questi servizi fare riferimento al seguente documento [normalizzazioneIndirizzi.md](docs/normalizzazioneIndirizzi.md) dove viene descritta l'integrazione con il sistema di normalizzazione

### Servizio per la trasmissione dell'originale digitale dell’AAR stampato
Per le modalità di trasmissione dell'originale dell'AAR stampato fare riferimento al seguente documento [con020.md](docs/con020.md)

### Servizio di verifica di stampabilità di documenti
Le specifiche API verranno definite a valle dell'aggiudicazione della gara - \[FASE3\]

### Servizio di blocco di invio al recapito
Le specifiche API verranno definite a valle dell'aggiudicazione della gara - \[FASE3\]

### Servizio di invio al macero di materialità
Le specifiche API verranno definite a valle dell'aggiudicazione della gara - \[FASE3\]

### Servizio per la sospensione temporanea della rendicontazione
Le specifiche API verranno definite a valle dell'aggiudicazione della gara - \[FASE3\]

### Servizio per la trasmissione della capacità produttiva dei recapitisti
Le specifiche API verranno definite a valle dell'aggiudicazione della gara - \[FASE3\]
