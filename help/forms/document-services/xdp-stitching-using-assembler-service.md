---
title: Stitching XDP utilizzando il servizio assemblatore
description: Utilizzo del servizio Assembler in AEM Forms per unire xdp
feature: Assembler
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2022-12-19T00:00:00Z
source-git-commit: 8f17e98c56c78824e8850402e3b79b3d47901c0b
workflow-type: tm+mt
source-wordcount: '357'
ht-degree: 2%

---

# Stitching XDP utilizzando il servizio assembler

Questo articolo fornisce le risorse per dimostrare la capacità di unire documenti xdp utilizzando il servizio assembler.
Il seguente codice jsp è stato scritto per inserire un sottomodulo chiamato **indirizzo** dal documento xdp denominato address.xdp in un punto di inserimento denominato **indirizzo** nel documento master.xdp. L&#39;xdp risultante è stato salvato nella cartella principale dell&#39;installazione AEM.

Il servizio Assembler si basa su documenti DDX validi per descrivere la manipolazione dei documenti PDF. Puoi fare riferimento alla [Documento di riferimento DDX qui](assets/ddxRef.pdf).Page 40 contiene informazioni sull’unione di xdp.

```java
    javax.servlet.http.Part ddxFile = request.getPart("xdpstitching.ddx");
    System.out.println("Got DDX");
    java.io.InputStream ddxIS = ddxFile.getInputStream();
    com.adobe.aemfd.docmanager.Document ddxDocument = new com.adobe.aemfd.docmanager.Document(ddxIS);
    javax.servlet.http.Part masterXdpPart = request.getPart("masterxdp.xdp");
    System.out.println("Got master xdp");
    java.io.InputStream masterXdpPartIS = masterXdpPart.getInputStream();
    com.adobe.aemfd.docmanager.Document masterXdpDocument = new com.adobe.aemfd.docmanager.Document(masterXdpPartIS);

    javax.servlet.http.Part fragmentXDPPart = request.getPart("fragment.xdp");
    System.out.println("Got fragment.xdp");
    java.io.InputStream fragmentXDPPartIS = fragmentXDPPart.getInputStream();
    com.adobe.aemfd.docmanager.Document fragmentXdpDocument = new com.adobe.aemfd.docmanager.Document(fragmentXDPPartIS);

    java.util.Map < String, Object > mapOfDocuments = new java.util.HashMap < String, Object > ();
    mapOfDocuments.put("master.xdp", masterXdpDocument);
    mapOfDocuments.put("address.xdp", fragmentXdpDocument);
    com.adobe.fd.assembler.service.AssemblerService assemblerService = sling.getService(com.adobe.fd.assembler.service.AssemblerService.class);
    if (assemblerService != null)
      System.out.println("Got assembler service");

    com.adobe.fd.assembler.client.AssemblerOptionSpec aoSpec = new com.adobe.fd.assembler.client.AssemblerOptionSpec();
    aoSpec.setFailOnError(true);

    com.adobe.fd.assembler.client.AssemblerResult assemblerResult = assemblerService.invoke(ddxDocument, mapOfDocuments, aoSpec);
    com.adobe.aemfd.docmanager.Document finalXDP = assemblerResult.getDocuments().get("stitched.xdp");
    finalXDP.copyToFile(new java.io.File("stitched.xdp"));
```

Il file DDX da inserire in un altro file xdp è elencato di seguito. Il DDX inserisce il sottomodulo  **indirizzo** da address.xdp nel punto di inserimento denominato **indirizzo** nel master.xdp. Il documento risultante denominato **stitched.xdp** viene salvato nel file system.

```xml
<?xml version="1.0" encoding="UTF-8"?> 
<DDX xmlns="http://ns.adobe.com/DDX/1.0/"> 
        <XDP result="stitched.xdp"> 
           <XDP source="master.xdp"> 
            <XDPContent insertionPoint="address" source="address.xdp" fragment="address"/> 
         </XDP> 
        </XDP>         
</DDX>
```

Per far funzionare questa funzionalità sul server AEM

* Scarica [Pacchetto di unione XDP](assets/xdp-stitching.zip) al sistema locale.
* Carica e installa il pacchetto utilizzando [gestore di pacchetti](http://localhost:4502/crx/packmgr/index.jsp)
* [Estrai il contenuto di questo file zip](assets/xdp-and-ddx.zip) per ottenere il file xdp e DDX di esempio

**Dopo aver installato il pacchetto dovrai inserire nell&#39;elenco Consentiti i seguenti URL in Adobe Granite CSRF Filter.**

1. Segui i passaggi indicati di seguito per inserire nell&#39;elenco Consentiti i percorsi sopra menzionati.
1. [Accedi a configMgr](http://localhost:4502/system/console/configMgr)
1. Ricerca filtro CSRF Granite Adobe
1. Aggiungi il seguente percorso nelle sezioni escluse e salva `/content/AemFormsSamples/assemblerservice`
1. Cerca &quot;filtro Sling Referrer&quot;
1. Selezionare la casella di controllo &quot;Consenti vuoto&quot;. (Questa impostazione deve essere utilizzata solo a scopo di test) Esistono diversi modi per testare il codice di esempio. La più rapida e semplice è quella di utilizzare l&#39;app Postman. Postman consente di effettuare richieste POST al server. Installa l&#39;app Postman sul tuo sistema.
Avvia l’app e immetti il seguente URL per testare l’API dei dati di esportazione http://localhost:4502/content/AemFormsSamples/assemblerservice.html

Fornisci i seguenti parametri di input come specificato nella schermata. È possibile utilizzare i documenti di esempio scaricati in precedenza,
![xdp-stitch-postman](assets/xdp-stitching-postman.png)

>[!NOTE]
>
>Assicurati che l&#39;installazione di AEM Forms sia completa. Tutti i tuoi bundle devono essere in stato attivo.
