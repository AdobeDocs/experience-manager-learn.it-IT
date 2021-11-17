---
title: Precompilare HTML5 Forms utilizzando l’attributo dati.
description: Compilare moduli HTML5 recuperando i dati dall’origine back-end.
feature: Adaptive Forms
version: 6.3,6.4,6.5.
topic: Development
role: Developer
level: Experienced
exl-id: ab0f5282-383b-4be6-9c57-cded6ab37528
source-git-commit: 9529b1f6d1a863fc570822c8ecd6c4be01b36729
workflow-type: tm+mt
source-wordcount: '471'
ht-degree: 0%

---

# Precompilare HTML5 Forms utilizzando l’attributo dati {#prepopulate-html-forms-using-data-attribute}


I modelli XDP di cui è stato eseguito il rendering in formato HTML con AEM Forms sono denominati HTML5 o Mobile Forms. Un caso d’uso comune consiste nel precompilare i moduli durante il rendering.

Esistono 2 modi per unire i dati con il modello xdp quando viene eseguito il rendering come HTML.

**dataRef**: Puoi utilizzare il parametro dataRef nell’URL. Questo parametro specifica il percorso assoluto del file di dati unito al modello. Questo parametro può essere un URL di un servizio rest che restituisce i dati in formato XML.

**dati**: Questo parametro specifica i byte di dati codificati UTF-8 uniti al modello. Se si specifica questo parametro, il modulo HTML5 ignora il parametro dataRef. Come best practice, si consiglia di utilizzare l’approccio dati.

L’approccio consigliato consiste nell’impostare l’attributo dei dati nella richiesta con i dati con cui si desidera precompilare il modulo.

slingRequest.setAttribute(&quot;data&quot;, contenuto);

In questo esempio, stiamo impostando l’attributo di dati con il contenuto. Il contenuto rappresenta i dati con cui si desidera precompilare il modulo. In genere si recupera il &quot;contenuto&quot; effettuando una chiamata REST a un servizio interno.

Per ottenere questo caso d’uso è necessario creare un profilo personalizzato. I dettagli sulla creazione di un profilo personalizzato sono chiaramente documentati in [Documentazione di AEM Forms qui](https://helpx.adobe.com/aem-forms/6/html5-forms/custom-profile.html).

Una volta creato il profilo personalizzato, creerai un file JSP che recupererà i dati effettuando chiamate al sistema di back-end. Una volta recuperati i dati, utilizzerai slingRequest.setAttribute(&quot;data&quot;, content); precompilazione del modulo

Quando viene eseguito il rendering di XDP, puoi anche passare alcuni parametri all’xdp e in base al valore del parametro puoi recuperare i dati dal sistema di back-end.

[Ad esempio, questo URL ha il parametro name](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=john)

Il JSP che scrivi avrà accesso al parametro name tramite request.getParameter(&quot;name&quot;) . Puoi quindi passare il valore di questo parametro al processo di backend per recuperare i dati richiesti.
Per far funzionare questa funzionalità sul sistema, segui i passaggi indicati di seguito:

* [Scaricare e importare le risorse in AEM utilizzando Gestione pacchetti](assets/prepopulatemobileform.zip)
Il pacchetto installerà quanto segue

   * CustomProfile
   * XDP di esempio
   * Endpoint POST di esempio per la restituzione dei dati necessari per la compilazione del modulo

>[!NOTE]
>
>Se si desidera compilare il modulo richiamando il processo di workbench, è possibile includere il file callWorkbenchProcess.jsp nel /apps/AEMFormsDemoListings/customprofiles/PrepopulateForm/html.jsp anziché setdata.jsp.

* [Posiziona il tuo browser preferito in questo URL](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=Adobe%20Systems). Il modulo deve essere precompilato con il valore del parametro name
