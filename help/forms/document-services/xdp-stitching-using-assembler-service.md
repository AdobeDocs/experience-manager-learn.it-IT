---
title: Unione XDP tramite il servizio Assembler
description: Utilizzo del servizio Assembler in AEM Forms per unire xdp
feature: Assembler
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2022-12-19T00:00:00Z
exl-id: e116038f-7d86-41ee-b1b0-7b8569121d6d
duration: 91
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '346'
ht-degree: 0%

---

# Stitching XDP tramite il servizio Assembler

Questo articolo fornisce le risorse per dimostrare la capacità di unire i documenti XDP utilizzando il servizio Assembler.
Il seguente codice jsp è stato scritto per inserire un sottomodulo denominato **address** da un documento xdp denominato address.xdp in un punto di inserimento denominato **address** nel documento master.xdp. L’XDP risultante è stato salvato nella cartella principale dell’installazione di AEM.

Il servizio Assembler si basa su documenti DDX validi per descrivere la manipolazione dei documenti PDF. È possibile fare riferimento al documento di riferimento [DDX qui](assets/ddxRef.pdf).La pagina 40 contiene informazioni sull&#39;unione xdp.

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

Il file DDX per inserire frammenti in un altro xdp è elencato di seguito. Il DDX inserisce il sottomodulo **address** da address.xdp nel punto di inserimento denominato **address** nel master.xdp. Il documento risultante denominato **stitched.xdp** è stato salvato nel file system.

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

Per utilizzare questa funzionalità sul server AEM

* Scarica il pacchetto di unione [XDP](assets/xdp-stitching.zip) nel sistema locale.
* Carica e installa il pacchetto utilizzando [Gestione pacchetti](http://localhost:4502/crx/packmgr/index.jsp)
* [Estrai il contenuto di questo file zip](assets/xdp-and-ddx.zip) per ottenere il file xdp e DDX di esempio

**Dopo aver installato il pacchetto, sarà necessario inserire nell&#39;elenco Consentiti i seguenti URL nel filtro CSRF di Adobe Granite.**

1. Segui i passaggi indicati di seguito per inserire nell&#39;elenco Consentiti i percorsi menzionati in precedenza.
1. [Accesso a configMgr](http://localhost:4502/system/console/configMgr)
1. Cerca filtro CSRF di Adobe Granite
1. Aggiungi il seguente percorso nelle sezioni escluse e salva `/content/AemFormsSamples/assemblerservice`
1. Cerca &quot;Sling Referrer filter&quot; (Filtro referrer Sling)
1. Selezionare la casella di controllo Consenti vuoto. (questa impostazione deve essere utilizzata solo a scopo di test)
Esistono diversi modi per testare il codice di esempio. Il metodo più rapido e semplice consiste nell’utilizzare l’app Postman. Postman consente di effettuare richieste POST al server. Installa l’app Postman sul sistema.
Avvia l’app e immetti il seguente URL per testare l’API di esportazione dei dati
http://localhost:4502/content/AemFormsSamples/assemblerservice.html

Fornire i seguenti parametri di input come specificato nella schermata. Puoi utilizzare i documenti di esempio scaricati in precedenza,
![xdp-stitch-postman](assets/xdp-stitching-postman.png)

>[!NOTE]
>
>Assicurati che l’installazione di AEM Forms sia stata completata. Tutti i bundle devono essere in stato attivo.
>
