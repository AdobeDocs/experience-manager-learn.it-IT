---
title: Passaggi semplificati per l’installazione di AEM Forms su Windows
seo-title: Passaggi semplificati per l’installazione di AEM Forms su Windows
description: Passaggi semplici e rapidi per installare AEM Forms in Windows
seo-description: Passaggi semplici e rapidi per installare AEM Forms in Windows
uuid: a148b8f0-83db-47f6-89d3-c8a9961be289
feature: Adaptive Forms
topics: administration
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
discoiquuid: 1182ef4d-5838-433b-991d-e24ab805ae0e
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '618'
ht-degree: 4%

---


# Passaggi semplificati per l’installazione di AEM Forms su Windows

>[!NOTE]
>
>Non fare mai doppio clic sul JAR di avvio rapido AEM, se si intende utilizzare AEM Forms.
>
>Inoltre, assicurati che non ci siano spazi nel percorso della cartella di installazione di AEM Forms.
>
>Ad esempio, non installare AEM Forms in c:\jack and jill\AEM Forms folder

>[!NOTE]
>
>Se stai installando AEM Forms 6.5, assicurati di aver installato i seguenti redistribuibili Microsoft Visual C++ a 32 bit.
>
>* Ridistribuibile Microsoft Visual C++ 2008
>* Ridistribuibile Microsoft Visual C++ 2010
>* Ridistribuibile Microsoft Visual C++ 2012
>* Microsoft Visual C++ 2013 ridistribuibile (a partire dalla versione 6.5)


Anche se si consiglia di seguire la [documentazione ufficiale](https://helpx.adobe.com/it/experience-manager/6-3/forms/using/installing-configuring-aem-forms-osgi.html) per l&#39;installazione di AEM Forms. Per installare e configurare AEM Forms nell’ambiente Windows, procedi come segue:

* Assicurati di avere installato il JDK appropriato
   * AEM 6.2 è necessario: Oracle SE 8 JDK 1.8.x (64 bit)
* 
   * AEM 6.3 e AEM 6.4 sono necessari: Oracle SE 8 JDK 1.8.x (64 bit)
* AEM 6.5 è necessario JDK 8 o JDK 11
* [I ](https://helpx.adobe.com/experience-manager/6-3/sites/deploying/using/technical-requirements.html) requisiti JDK ufficiali sono elencati qui
* Assicurati che JAVA_HOME sia impostato per puntare al JDK che hai installato.
   * Per creare la variabile JAVA_HOME in windows, segui la procedura seguente:
      * Fare clic con il pulsante destro del mouse su Risorse del computer e selezionare Proprietà
      * Nella scheda Avanzate , seleziona Variabili di ambiente e crea una nuova variabile di sistema denominata JAVA_HOME.
      * Imposta il valore della variabile in modo che punti a JDK installato sul tuo sistema. Ad esempio c:\program files\java\jdk1.8.0_25

* Crea una cartella denominata AEMForms sull&#39;unità C
* Individua AEMQuickStart.Jar e spostalo nella cartella AEMForms
* Copia il file license.properties nella cartella AEMForms
* Crea un file batch denominato &quot;StartAemForms.bat&quot; con il seguente contenuto:
   * java -d64 -Xmx2048M -jar AEM_6.3_Quickstart.jar -gui.Here AEM_6.3_Quickstart.jar è il nome del mio jar AEM quickstart.
   * È possibile rinominare il jar con qualsiasi nome, ma assicurarsi che il nome sia riportato nel file batch.Salvare il file batch nella cartella AEMForms.

* Apri un nuovo prompt dei comandi e passa a c:\aemforms.

* Esegui il file StartAemForms.bat dal prompt dei comandi.

* Si dovrebbe ottenere una piccola finestra di dialogo che indica l&#39;avanzamento dell&#39;avvio.

* Una volta completato l&#39;avvio, apri il file sling.properties . Si trova in c:\AEMForms\crx-quickstart\conf folder.

* Copiare le 2 righe seguenti verso il fondo del file
   * **sling.bootdelegation.class.com.rsa.jsafe.provider.JsafeJCE=com.rsa.*** **sling.bootdelegation.class.org.bouncyCastle.jce.provider.BouncyCastleProvider=org.bouncyCastle.***
* Queste due proprietà sono necessarie per il funzionamento di document services
* Salva il file sling.properties

* [Accesso a Package Share](http://localhost:4502/crx/packageshare/login.html)

   * Avrai bisogno di AdobeId per accedere alla condivisione del pacchetto
   * Cerca il pacchetto AEM Forms Add on appropriato per la tua versione di AEM Forms e Operating System
   * Oppure [è possibile scaricare il pacchetto di aggiunta appropriato per i moduli](https://helpx.adobe.com/it/aem-forms/kb/aem-forms-releases.html)
   * Dopo aver installato il pacchetto add on, segui i seguenti passaggi

      * **Assicurati che tutti i bundle siano in stato attivo. (Ad eccezione del bundle Firme AEMFD).**
      * **Normalmente ci vorrebbero 5 o più minuti perché tutti i bundle raggiungano lo stato attivo.**
   * **Una volta che tutti i bundle sono attivi (ad eccezione del bundle AEMFD Signatures), riavvia il sistema per completare l&#39;installazione di AEM Forms**


* Aggiungi il pacchetto `sun.util.calendar` all&#39;elenco Consentiti:

   1. Apri la console Web Felix nella [finestra del browser](http://localhost:4502/system/console/configMgr)
   2. Cerca e apri la configurazione del firewall di deserializzazione: `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl`
   3. Aggiungi `sun.util.calendar` come nuova voce in `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl.firewall.deserialization.whitelist.name`
   4. Salva le modifiche.

Congratulazioni!!! Ora hai installato e configurato AEM Forms sul tuo sistema.
A seconda delle esigenze è possibile configurare [Reader Extensions](https://helpx.adobe.com/experience-manager/6-3/forms/using/configuring-document-services.html) o [ PDFG](https://helpx.adobe.com/experience-manager/6-3/forms/using/install-configure-pdf-generator.html) sul server
