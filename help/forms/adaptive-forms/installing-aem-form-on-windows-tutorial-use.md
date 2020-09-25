---
title: Passaggi semplificati per l'installazione  AEM Forms in Windows
seo-title: Passaggi semplificati per l'installazione  AEM Forms in Windows
description: Installazione rapida e semplice  AEM Forms in Windows
seo-description: Installazione rapida e semplice  AEM Forms in Windows
uuid: a148b8f0-83db-47f6-89d3-c8a9961be289
feature: adaptive-forms
topics: administration
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
discoiquuid: 1182ef4d-5838-433b-991d-e24ab805ae0e
translation-type: tm+mt
source-git-commit: 82127d5be9a4b969537738f9ba537efe07f38479
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 2%

---

# Passaggi semplificati per l&#39;installazione  AEM Forms in Windows

>[!NOTE]
>Non fate mai doppio clic sulla AEM barra di avvio rapido, se intendete utilizzare  AEM Forms.
>Inoltre, accertatevi che non vi siano spazi nel percorso  cartella di installazione di AEM Forms.
>Ad esempio, non installate  AEM Forms in c:\jack and jill\AEM Forms folder

>[!NOTE]
Se state installando  AEM Forms 6.5, accertatevi di aver installato i seguenti redistribuibili Microsoft Visual C++ a 32 bit.

* Microsoft Visual C++ 2008 ridistribuibile
* Microsoft Visual C++ 2010 ridistribuibile
* Microsoft Visual C++ 2012 ridistribuibile
* Microsoft Visual C++ 2013 ridistribuibile (dal 6.5)

Anche se si consiglia di seguire la documentazione [](https://helpx.adobe.com/experience-manager/6-3/forms/using/installing-configuring-aem-forms-osgi.html) ufficiale per l&#39;installazione  AEM Forms. Per installare e configurare  AEM Forms nell’ambiente Windows, effettuate le seguenti operazioni:

* Verificare che sia installato il JDK appropriato
   * AEM 6.2 è necessario: Oracle SE 8 JDK 1.8.x (64 bit)
* 
   * AEM 6.3 e AEM 6.4 è necessario: Oracle SE 8 JDK 1.8.x (64 bit)
* AEM 6.5 è necessario JDK 8 o JDK 11
* [Requisiti](https://helpx.adobe.com/experience-manager/6-3/sites/deploying/using/technical-requirements.html) JDK ufficiali elencati qui
* Assicuratevi che JAVA_HOME sia impostato in modo che punti al JDK installato.
   * Per creare la variabile JAVA_HOME in Windows, procedere come segue:
      * Fate clic con il pulsante destro del mouse su Risorse del computer e selezionate Proprietà
      * Nella scheda Avanzate, selezionare Variabili di ambiente e creare una nuova variabile di sistema denominata JAVA_HOME.
      * Impostate il valore della variabile su JDK installato nel sistema. Ad esempio c:\program files\java\jdk1.8.0_25

* Creare una cartella denominata AEMForms sull’unità C
* Individuate AEMQuickStart.Jar e spostatelo nella cartella AEMForms
* Copiare il file license.properties in questa cartella AEMForms
* Create un file batch denominato &quot;StartAemForms.bat&quot; con il contenuto seguente:
   * java -d64 -Xmx2048M -jar AEM_6.3_Quickstart.jar -gui.Qui AEM_6.3_Quickstart.jar è il nome del mio jar di avvio rapido AEM.
   * È possibile rinominare il file JAR con qualsiasi nome, ma assicurarsi che il nome sia riflesso nel file batch. Salvare il file batch nella cartella AEMForms.

* Aprite un nuovo prompt dei comandi e andate a c:\aemforms.

* Eseguire il file StartAemForms.bat dal prompt dei comandi.

* Dovrebbe essere visualizzata una piccola finestra di dialogo che indica l&#39;avanzamento dell&#39;avvio.

* Al termine dell&#39;avvio, aprite il file sling.properties. Si trova in c:\AEMForms\crx-quickstart\conf folder.

* Copiare le 2 righe seguenti verso il fondo del file
   * **sling.bootdelegation.class.com.rsa.jsafe.provider.JsafeJCE=com.rsa.*** **sling.bootdelegation.class.org.bouncyCastle.jce.provider.BouncyCastleProvider=org.bouncyCastle.***
* Queste due proprietà sono necessarie per il funzionamento di Document Services
* Salvate il file sling.properties

* [Accesso a Package Share](http://localhost:4502/crx/packageshare/login.html)

   * Per effettuare l&#39;accesso alla condivisione del pacchetto, è necessario disporre di AdobeId
   * Cercare  pacchetto AEM Forms Add on appropriato per la vostra versione  AEM Forms e sistema operativo
   * Oppure [è possibile scaricare il pacchetto di aggiunta moduli appropriato](https://helpx.adobe.com/it/aem-forms/kb/aem-forms-releases.html)
   * Dopo aver installato il pacchetto add on, è necessario seguire i seguenti passaggi

      * **Verificate che tutti i bundle siano nello stato attivo. (ad eccezione del bundle Firme AEMFD).**
      * **In genere sono necessari 5 o più minuti perché tutti i bundle raggiungano lo stato attivo.**
   * **Una volta che tutti i bundle sono attivi (ad eccezione del bundle AEMFD Signatures), riavviare il sistema per completare l&#39;installazione  AEM Forms**


* Aggiungete `sun.util.calendar` il pacchetto al elenco Consentiti :

   1. Aprire la console Web Felix nella finestra [del browser](http://localhost:4502/system/console/configMgr)
   2. Cerca e apri la configurazione del firewall di deserializzazione: `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl`
   3. Aggiungi `sun.util.calendar` come nuova voce in `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl.firewall.deserialization.whitelist.name`
   4. Salva le modifiche.

Congratulazioni!!! È ora installato e configurato  AEM Forms nel sistema.
A seconda delle esigenze è possibile configurare estensioni [di](https://helpx.adobe.com/experience-manager/6-3/forms/using/configuring-document-services.html) Reader o [ PDFG](https://helpx.adobe.com/experience-manager/6-3/forms/using/install-configure-pdf-generator.html) sul server
