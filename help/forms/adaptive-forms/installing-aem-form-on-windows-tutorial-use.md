---
title: Passaggi semplificati per l’installazione di AEM Forms su Windows
description: Procedura rapida e semplice per installare AEM Forms in Windows
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
topic: Development
role: Admin
level: Beginner
exl-id: 80288765-0b51-44a9-95d3-3bdb2da38615
last-substantial-update: 2020-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '578'
ht-degree: 6%

---

# Passaggi semplificati per l’installazione di AEM Forms su Windows

>[!NOTE]
>
>Non fare mai doppio clic sul jar di avvio rapido AEM se si intende utilizzare AEM Forms.
>
>Inoltre, assicurati che non ci siano spazi nel percorso della cartella di installazione di AEM Forms.
>
>Ad esempio, non installare AEM Forms in c:\jack and jill\AEM Forms folder

>[!NOTE]
>
>Se stai installando AEM Forms 6.5, assicurati di aver installato i seguenti redistribuibili Microsoft Visual C++ a 32 bit.
>
>* Microsoft Visual C++ 2008 ridistribuibile
>* Microsoft Visual C++ 2010 ridistribuibile
>* Microsoft Visual C++ 2012 ridistribuibile
>* Microsoft Visual C++ 2013 ridistribuibile (a partire dalla versione 6.5)


Anche se consigliamo di seguire [documentazione ufficiale](https://helpx.adobe.com/it/experience-manager/6-3/forms/using/installing-configuring-aem-forms-osgi.html) per l’installazione di AEM Forms. Per installare e configurare AEM Forms nell’ambiente Windows, effettua le seguenti operazioni:

* Assicurati di avere installato il JDK appropriato
   * AEM 6.2 è necessario: Oracle SE 8 JDK 1.8.x (64 bit)
* 
   * AEM 6.3 e AEM 6.4 è necessario: Oracle SE 8 JDK 1.8.x (64 bit)
* AEM 6.5 è necessario JDK 8 o JDK 11
* [Requisiti JDK ufficiali](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/introduction/technical-requirements.html?lang=it) sono elencati qui
* Assicurati che JAVA_HOME sia impostato per puntare al JDK che hai installato.
   * Per creare la variabile JAVA_HOME in windows, segui la procedura seguente:
      * Fare clic con il pulsante destro del mouse su Risorse del computer e selezionare Proprietà
      * Nella scheda Avanzate , seleziona Variabili di ambiente e crea una nuova variabile di sistema denominata JAVA_HOME.
      * Imposta il valore della variabile in modo che punti a JDK installato sul tuo sistema. Ad esempio c:\program files\java\jdk1.8.0_25

* Crea una cartella denominata AEMForms sull&#39;unità C
* Individua AEMQuickStart.Jar e spostalo nella cartella AEMForms
* Copia il file license.properties nella cartella AEMForms
* Crea un file batch denominato &quot;StartAemForms.bat&quot; con il seguente contenuto:
   * `java -d64 -Xmx2048M -jar AEM_6.5_Quickstart.jar -gui`
      * Qui AEM_6.5_Quickstart.jar è il nome del mio jar AEM quickstart.
   * È possibile rinominare il jar con qualsiasi nome, ma assicurarsi che il nome sia riportato nel file batch. Salva il file batch nella cartella AEMForms.

* Apri un nuovo prompt dei comandi e passa a _c:\aemforms_.

* Esegui il file StartAemForms.bat dal prompt dei comandi.

* Si dovrebbe ottenere una piccola finestra di dialogo che indica l&#39;avanzamento dell&#39;avvio.

* Una volta completato l&#39;avvio, apri il file sling.properties . Si trova in c:\AEMForms\crx-quickstart\conf folder.

* Copiare le 2 righe seguenti verso il fondo del file
   * **sling.bootdelegation.class.com.rsa.jsafe.provider.JsafeJCE=com.rsa.&#42;** **sling.bootdelegation.class.org.bouncyCastle.jce.provider.BouncyCastleProvider=org.bouncyCastle.&#42;**
* Queste due proprietà sono necessarie per il funzionamento di document services
* Salva il file sling.properties
* [Scarica il pacchetto aggiuntivo per i moduli appropriati](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/forms-updates/aem-forms-releases.html?lang=en)
* Installa il pacchetto add-on dei moduli utilizzando [gestore dei pacchetti.](http://localhost:4502/crx/packmgr/index.jsp)
* Dopo aver installato add on package, segui i seguenti passaggi

       * **Assicurati che tutti i bundle siano in stato attivo. (Ad eccezione del bundle Firme AEMFD).**
       * **Normalmente ci vorrebbero 5 o più minuti perché tutti i bundle raggiungano lo stato attivo.**
   
   * **Una volta che tutti i bundle sono attivi (ad eccezione del bundle AEMFD Signatures), riavvia il sistema per completare l&#39;installazione di AEM Forms**

## pacchetto sun.util.calendar all&#39;elenco Consentiti

1. Apri la console web Felix nel tuo [finestra del browser](http://localhost:4502/system/console/configMgr)
2. Cerca e apri la configurazione del firewall di deserializzazione: `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl`
3. Aggiungi `sun.util.calendar` come nuova voce in `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl.firewall.deserialization.whitelist.name`
4. Salva le modifiche.

Congratulazioni!!! Ora hai installato e configurato AEM Forms sul tuo sistema.
A seconda delle esigenze è possibile configurare  [Estensioni Reader](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/configuring-reader-extension-osgi.html?lang=en) o [ PDFG](https://experienceleague.adobe.com/docs/experience-manager-64/forms/install-aem-forms/osgi-installation/install-configure-document-services.html?lang=it) sul server
