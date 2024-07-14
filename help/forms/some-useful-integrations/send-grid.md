---
title: Integrare AEM Forms con SendGrid
description: Sfrutta la piattaforma di consegna e-mail basata su cloud SengGrid utilizzando AEM Forms.
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-13605
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2023-07-14T00:00:00Z
exl-id: 62b73f4b-69d8-4ede-9d57-3d6472d25d5a
duration: 118
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '534'
ht-degree: 0%

---

# Integrare AEM Forms con SendGrid

Ti diamo il benvenuto in questa guida tecnica, dove esploreremo il processo di invio di e-mail con i modelli dinamici SendGrid da AEM Forms. Questa guida ha lo scopo di fornire una chiara comprensione di come sfruttare i modelli dinamici per personalizzare in modo efficace il contenuto delle e-mail.

I modelli dinamici ti consentono di creare modelli e-mail che possono mostrare contenuti diversi ai destinatari in base ai dati acquisiti nel modulo adattivo. Utilizzando le variabili di personalizzazione, puoi fornire esperienze e-mail mirate e personalizzate che suonano con il tuo pubblico.

Inoltre, approfondiremo l’utilizzo del file Swagger, che consente di personalizzare ulteriormente le e-mail includendo il nome e l’indirizzo e-mail del cliente, nonché selezionando il modello e-mail dinamico appropriato.

Segui le istruzioni dettagliate riportate in questo documento per sfruttare la potenza dei modelli dinamici SendGrid e di AEM Forms, e migliorare le tue comunicazioni e-mail per renderle di più alto livello. Cominciamo!

## Prerequisiti

Prima di procedere con l’invio di e-mail utilizzando i modelli dinamici SendGrid da AEM Forms, assicurati di aver soddisfatto i seguenti prerequisiti:

1. **Account SendGrid**: registrati a un account SendGrid all&#39;indirizzo [https://sendgrid.com](https://sendgrid.com) per accedere ai servizi di consegna e-mail. Per integrare SendGrid con AEM Forms sono necessarie le credenziali dell’account.
1. **Familiarità con la creazione di origini dati**: acquisisci conoscenze pratiche sulla creazione di origini dati in AEM Forms. Se necessario, consultare la documentazione relativa alla [creazione di origini dati](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) per istruzioni dettagliate.
1. **Familiarità con il modello dati modulo**: comprendere il concetto di modello dati modulo in AEM Forms. Se necessario, consulta la documentazione sulla [creazione di modelli di dati del modulo](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html) per assicurarti di avere le informazioni necessarie.

Soddisfacendo questi prerequisiti, avrai a disposizione le conoscenze e le risorse essenziali per inviare efficacemente le e-mail utilizzando i modelli dinamici SendGrid di AEM Forms.

## Risorse di esempio

Le risorse di esempio fornite con questo articolo includono:

* **[File Swagger](assets/SendGridWithDynamicTemplate.yaml)**: questo file ti consente di inviare e-mail utilizzando un modello e-mail dinamico. Fornisce le specifiche e le configurazioni necessarie da integrare con SendGrid e AEM Forms per una distribuzione fluida delle e-mail.

Puoi utilizzare il file Swagger fornito come riferimento o punto di partenza per implementare la funzionalità e-mail con i modelli dinamici.

## Istruzioni per la prova

Per testare la funzionalità descritta in questa guida, segui questi passaggi:

1. Scarica il [file Swagger](assets/SendGridWithDynamicTemplate.yaml) fornito nella cartella delle risorse.
2. Creare un&#39;origine dati Restful utilizzando il file Swagger scaricato e le credenziali SendGrid.
3. Crea un modello dati modulo basato sull’origine dati Restful.
4. Richiama l&#39;operazione POST `mail/send` del modello dati modulo in base alle tue esigenze. Ad esempio, puoi attivare l’e-mail al clic del pulsante o includerla nel flusso di lavoro di AEM Forms.

Il payload di esempio per il servizio è il seguente. Sostituire i valori dei segnaposto con i propri dati:

```json
{
    "sendgridpayload": {
        "from": {
            "email": "gs@xyz.com"
        },
        "personalizations": [{
            "to": [{
                "email": "johndoe@xyz.com"
            }],
            "dynamic_template_data": {
                "customerName": "John Doe"
            }
        }],
        "template_id": "d-72aau292a3bd60b5300c"
    }
}
```

Verificare che `template_id` corrisponda all&#39;ID del modello di posta elettronica dinamica SendGrid e che gli indirizzi di posta elettronica siano validi e verificati da SendGrid. I valori nella sezione `personalizations` ti consentono di personalizzare l&#39;e-mail utilizzando i dati immessi dall&#39;utente dal modulo adattivo.

Seguendo questi passaggi e personalizzando il payload fornito, puoi testare in modo efficace l’integrazione dei modelli dinamici SendGrid con AEM Forms.
