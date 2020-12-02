---
title: PrePopulate HTML5 Forms utilizzando l’attributo data.
seo-title: PrePopulate HTML5 Forms utilizzando l’attributo data.
description: Compilare moduli HTML5 recuperando i dati dall’origine back-end.
seo-description: Compilare moduli HTML5 recuperando i dati dall’origine back-end.
feature: integrations
topics: mobile-forms
audience: developer
doc-type: article
activity: implement
version: 6.3,6.4,6.5.
uuid: 889d2cd5-fcf2-4854-928b-0c2c0db9dbc2
discoiquuid: 3aa645c9-941e-4b27-a538-cca13574b21c
translation-type: tm+mt
source-git-commit: 1e615d1c51fa0c4c0db335607c29a8c284874c8d
workflow-type: tm+mt
source-wordcount: '510'
ht-degree: 0%

---


# Pre-popolare Forms HTML5 utilizzando l&#39;attributo di dati {#prepopulate-html-forms-using-data-attribute}

Visitate la pagina [ AEM Forms samples](https://forms.enablementadobe.com/content/samples/samples.html?query=0) per un collegamento a una dimostrazione live di questa funzionalità.

I modelli XDP sottoposti a rendering in formato HTML con  AEM Forms sono denominati HTML5 o Forms mobile. Un caso d’uso comune consiste nel precompilare questi moduli durante il rendering.

Esistono due modi per unire i dati con il modello xdp quando viene rappresentato come HTML.

**dataRef**: È possibile utilizzare il parametro dataRef nell’URL. Questo parametro specifica il percorso assoluto del file di dati unito al modello. Questo parametro può essere un URL di un servizio di supporto che restituisce i dati in formato XML.

**data**: Questo parametro specifica i byte di dati codificati UTF-8 uniti al modello. Se questo parametro viene specificato, il modulo HTML5 ignora il parametro dataRef. Come procedura ottimale, si consiglia di utilizzare l&#39;approccio basato sui dati.

L&#39;approccio consigliato consiste nell&#39;impostare l&#39;attributo di dati nella richiesta con i dati con cui si desidera precompilare il modulo.

slingRequest.setAttribute(&quot;data&quot;, contenuto);

In questo esempio, stiamo impostando l&#39;attributo data con il contenuto. Il contenuto rappresenta i dati con cui si desidera precompilare il modulo. In genere si recupera il &quot;contenuto&quot; effettuando una chiamata REST a un servizio interno.

Per ottenere questo caso d’uso è necessario creare un profilo personalizzato. I dettagli sulla creazione del profilo personalizzato sono chiaramente documentati in [ documentazione AEM Forms qui](https://helpx.adobe.com/aem-forms/6/html5-forms/custom-profile.html).

Una volta creato il profilo personalizzato, si crea quindi un file JSP che recupera i dati effettuando chiamate al sistema di back-end. Una volta estratti i dati, utilizzerete slingRequest.setAttribute(&quot;data&quot;, contenuto); per precompilare il modulo

Durante il rendering dell&#39;XDP, potete anche trasmettere alcuni parametri all&#39;xdp e in base al valore del parametro che potete recuperare i dati dal sistema di backend.

[Ad esempio, questo URL ha un parametro name](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=john)

La JSP che scrivete avrà accesso al parametro name tramite request.getParameter(&quot;name&quot;). Potete quindi passare il valore di questo parametro al processo di backend per recuperare i dati richiesti.
Per utilizzare questa funzionalità sul sistema, segui i passaggi indicati di seguito:

* [Scaricate e importate le risorse in AEM utilizzando ](assets/prepopulatemobileform.zip)
Gestione pacchettiIl pacchetto installerà quanto segue

   * CustomProfile
   * XDP di esempio
   * Endpoint POST di esempio che restituirà i dati per compilare il modulo

>[!NOTE]
>
>Se si desidera compilare il modulo chiamando il processo workbench, è possibile includere callWorkbenchProcess.jsp nella cartella /apps/AEMFormsDemoListings/customprofiles/PrepopulateForm/html.jsp anziché nel file setdata.jsp

* [Indicate il browser preferito a questo URL](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=Adobe%20Systems). Il modulo deve essere precompilato con il valore del parametro name
