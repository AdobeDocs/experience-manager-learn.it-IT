---
title: PrePopolare HTML5 Forms utilizzando l’attributo dati.
description: Popola i moduli HTML5 recuperando i dati dall’origine back-end.
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: ab0f5282-383b-4be6-9c57-cded6ab37528
last-substantial-update: 2020-01-09T00:00:00Z
duration: 94
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '449'
ht-degree: 0%

---

# PrePopolare HTML5 Forms utilizzando l’attributo dei dati {#prepopulate-html-forms-using-data-attribute}


I modelli XDP renderizzati in formato HTML utilizzando AEM Forms sono denominati HTML5 o Mobile Forms. Un caso d’uso comune consiste nel precompilare questi moduli durante il rendering.

Esistono 2 modi per unire i dati con il modello xdp quando viene eseguito il rendering come HTML.

**dataRef**: è possibile utilizzare il parametro dataRef nell&#39;URL. Questo parametro specifica il percorso assoluto del file di dati unito al modello. Questo parametro può essere un URL di un servizio rest che restituisce i dati in formato XML.

**dati**: questo parametro specifica i byte di dati con codifica UTF-8 che vengono uniti al modello. Se si specifica questo parametro, il modulo HTML5 ignora il parametro dataRef. Come best practice, consigliamo di utilizzare l’approccio dati.

L’approccio consigliato consiste nell’impostare l’attributo dati nella richiesta con i dati con cui si desidera precompilare il modulo.

slingRequest.setAttribute(&quot;data&quot;, contenuto);

In questo esempio, stiamo impostando l’attributo dati con il contenuto. Il contenuto rappresenta i dati con cui si desidera precompilare il modulo. In genere, per recuperare il &quot;contenuto&quot; si effettua una chiamata REST a un servizio interno.

Per ottenere questo caso d’uso è necessario creare un profilo personalizzato. I dettagli sulla creazione di un profilo personalizzato sono chiaramente documentati nella [documentazione AEM Forms disponibile qui](https://helpx.adobe.com/it/aem-forms/6/html5-forms/custom-profile.html).

Una volta creato il profilo personalizzato, creerai un file JSP che recupererà i dati effettuando chiamate al sistema backend. Una volta recuperati i dati, utilizzerai slingRequest.setAttribute(&quot;dati&quot;, contenuto); per precompilare il modulo

Durante il rendering dell’XDP, puoi anche trasmettere alcuni parametri all’xdp e in base al valore del parametro puoi recuperare i dati dal sistema backend.

[Ad esempio, questo URL contiene il parametro name](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=john)

La JSP che scrivi avrà accesso al parametro name attraverso request.getParameter(&quot;name&quot;). Puoi quindi passare il valore di questo parametro al processo di backend per recuperare i dati richiesti.
Per utilizzare questa funzionalità sul sistema, attenersi alla procedura indicata di seguito:

* [Scarica e importa le risorse in AEM tramite Gestione pacchetti](assets/prepopulatemobileform.zip)
Il pacchetto installerà i seguenti elementi

   * ProfiloPersonalizzato
   * XDP di esempio
   * Endpoint POST di esempio che restituirà i dati per compilare il modulo

>[!NOTE]
>
>Se si desidera compilare il modulo chiamando il processo di Workbench, è possibile includere callWorkbenchProcess.jsp nel /apps/AEMFormsDemoListings/customprofiles/PrepopulateForm/html.jsp anziché setdata.jsp

* [Indirizza il browser preferito a questo URL](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=Adobe%20Systems). Il modulo deve essere precompilato con il valore del parametro name
