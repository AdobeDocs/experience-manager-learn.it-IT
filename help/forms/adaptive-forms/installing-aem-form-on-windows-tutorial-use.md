---
title: Passaggi semplificati per l’installazione di AEM Forms su Windows
description: Passaggi semplici e veloci per installare AEM Forms su Windows
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Admin
level: Beginner
exl-id: 80288765-0b51-44a9-95d3-3bdb2da38615
last-substantial-update: 2020-06-09T00:00:00Z
duration: 149
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '545'
ht-degree: 1%

---

# Passaggi semplificati per l’installazione di AEM Forms su Windows

>[!NOTE]
>
>Se intendi utilizzare AEM Forms, non fare mai doppio clic sul file jar di Avvio rapido dell’AEM.
>
>Inoltre, accertati che nel percorso della cartella di installazione di AEM Forms non siano presenti spazi.
>
>Ad esempio, non installare AEM Forms nella cartella c:\jack e jill\AEM Forms

>[!NOTE]
>
>Se si sta installando AEM Forms 6.5, assicurarsi di aver installato i seguenti ridistribuibili di Microsoft Visual C++ a 32 bit.
>
>* Microsoft Visual C++ 2008 ridistribuibile
>* Microsoft Visual C++ 2010 ridistribuibile
>* Microsoft Visual C++ 2012 ridistribuibile
>* Microsoft Visual C++ 2013 ridistribuibile (versione 6.5)

Anche se si consiglia di seguire [documentazione ufficiale](https://helpx.adobe.com/it/experience-manager/6-3/forms/using/installing-configuring-aem-forms-osgi.html) per installare AEM Forms. Per installare e configurare AEM Forms in un ambiente Windows è possibile seguire la procedura descritta di seguito.

* Assicurarsi di aver installato il JDK appropriato
   * AEM 6.2 richiesto: Oracle SE 8 JDK 1.8.x (64 bit)
   * AEM 6.3 e AEM 6.4: Oracle SE 8 JDK 1.8.x (64 bit)
   * AEM 6.5 sono necessari JDK 8 o JDK 11
   * [Requisiti ufficiali JDK](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/introduction/technical-requirements.html?lang=it) sono elencati qui
* Verificare che JAVA_HOME sia impostato in modo da puntare al JDK installato.
   * Per creare la variabile JAVA_HOME in Windows, effettuare le seguenti operazioni:
      * Fare clic con il pulsante destro del mouse su Risorse del computer e selezionare Proprietà
      * Nella scheda Avanzate selezionare Variabili di ambiente e creare una nuova variabile di sistema denominata JAVA_HOME.
      * Imposta il valore della variabile in modo che punti a JDK installato nel sistema. Ad esempio c:\programmi\java\jdk1.8.0_25

* Creare una cartella denominata AEMForms sull&#39;unità C
* Individua AEMQuickStart.Jar e spostalo nella cartella AEMForms
* Copiare il file license.properties in questa cartella AEMForms
* Crea un file batch denominato &quot;StartAemForms.bat&quot; con il seguente contenuto:
   * `java -d64 -Xmx2048M -jar AEM_6.5_Quickstart.jar -gui`
      * Qui AEM_6.5_Quickstart.jar è il nome del file jar quickstart dell&#39;AEM.
   * Puoi rinominare il file jar con qualsiasi nome, ma assicurati che tale nome sia riportato nel file batch. Salvate il file batch nella cartella AEMForms.

* Aprire un nuovo prompt dei comandi e passare a _c:\aemforms_.

* Eseguire il file StartAemForms.bat dal prompt dei comandi.

* Dovrebbe essere visualizzata una piccola finestra di dialogo che indica l&#39;avanzamento dell&#39;avvio.

* Al termine dell’avvio, apri il file sling.properties. Si trova nella cartella c:\AEMForms\crx-quickstart\conf.

* Copia le 2 righe seguenti verso la parte inferiore del file
   * **sling.bootdelegation.class.com.rsa.jsafe.provider.JsafeJCE=com.rsa.&#42;** **sling.bootdelegation.class.org.bouncycastle.jce.provider.BouncyCastleProvider=org.bouncycastle.&#42;**
* Queste due proprietà sono necessarie per il funzionamento dei servizi documentali
* Salvare il file sling.properties
* [Scaricare il pacchetto aggiuntivo appropriato per i moduli](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/forms-updates/aem-forms-releases.html?lang=en)
* Installare il pacchetto di componenti aggiuntivi per Forms tramite [gestione pacchetti](http://localhost:4502/crx/packmgr/index.jsp).
* Dopo aver installato il pacchetto add on, è necessario seguire i seguenti passaggi

   * **Assicurati che tutti i bundle siano in stato attivo. (Ad eccezione del bundle Firme AEMFD).**
   * **In genere sono necessari 5 o più minuti perché tutti i bundle raggiungano lo stato attivo.**

   * **Una volta che tutti i bundle sono attivi (eccetto il bundle Firme AEMFD), riavvia il sistema per completare l’installazione di AEM Forms**

## pacchetto sun.util.calendar all’elenco Consentiti

1. Apri la console web Felix nel tuo [finestra del browser](http://localhost:4502/system/console/configMgr)
1. Cerca e apri Configurazione firewall deserializzazione: `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl`
1. Aggiungi `sun.util.calendar` come nuova voce in `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl.firewall.deserialization.whitelist.name`
1. Salva le modifiche.

Complimenti!!! Ora hai installato e configurato AEM Forms sul tuo sistema.
A seconda delle tue esigenze puoi configurare  [Estensioni Reader](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/configuring-reader-extension-osgi.html) o [PDFG](https://experienceleague.adobe.com/docs/experience-manager-65/forms/install-aem-forms/osgi-installation/install-configure-document-services.html) sul server
