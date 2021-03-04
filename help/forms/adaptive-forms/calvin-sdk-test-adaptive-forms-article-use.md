---
title: 'Utilizzo di test automatici con AEM Adaptive Forms '
seo-title: 'Utilizzo di test automatici con AEM Adaptive Forms '
description: Test automatico dei moduli adattivi tramite Calvin SDK
seo-description: Test automatico dei moduli adattivi tramite Calvin SDK
feature: Moduli adattivi
topics: development
audience: developer
doc-type: article
activity: develop
version: 6.3,6.4,6.5
uuid: 3ad4e6d6-d3b1-4e4d-9169-847f74ba06be
topic: Sviluppo
role: Developer (Sviluppatore)
level: Principiante
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '465'
ht-degree: 1%

---


# Utilizzo di test automatici con AEM Adaptive Forms {#using-automated-tests-with-aem-adaptive-forms}

Test automatico dei moduli adattivi tramite Calvin SDK

Calvin SDK è un’API di utility per gli sviluppatori di Moduli adattivi per il test di Moduli adattivi. L&#39;SDK di Calvin è basato sul [framework di test di Hobbes.js](https://docs.adobe.com/docs/en/aem/6-3/develop/ref/test-api/index.html). L’SDK di Calvin è disponibile a partire da AEM Forms 6.3.

In questa esercitazione verrà creato quanto segue:

* Suite di test
* La suite di test contiene uno o più casi di test
* I casi di test contengono una o più azioni

## Guida introduttiva {#getting-started}

[Scarica e installa le risorse utilizzando Package ](assets/testingadaptiveformsusingcalvinsdk1.zip)ManagerIl pacchetto contiene script di esempio e diversi Moduli adattivi.Questi Moduli adattivi vengono creati con la versione AEM Forms 6.3. È consigliabile creare nuovi moduli specifici per la versione di AEM Forms in uso se si esegue il test su AEM Forms 6.4 o versione successiva. Gli script di esempio mostrano le varie API dell’SDK Calvin disponibili per testare i moduli adattivi. I passaggi generali per testare i moduli adattivi AEM sono i seguenti:

* Passa al modulo da testare
* Imposta il valore del campo
* Inviare il modulo adattivo
* Verifica messaggi di errore

Gli script di esempio nel pacchetto mostrano tutte le azioni di cui sopra.
Esploriamo il codice di `mortgageForm.js`

```javascript
var mortgageFormTS = new hobs.TestSuite("Mortgage Form Test", {
        path: '/etc/clientlibs/testingAFUsingCalvinSDK/mortgageForm.js',
        register: true
})
```

Il codice riportato sopra crea una nuova suite di test.

* In questo caso, il nome di TestSuite è &#39; `Mortgage Form Test` &#39;.
* Fornito è il percorso assoluto in AEM al file js che contiene la suite di test.
* Il parametro di registro quando è impostato su &#39; `true` &#39;, rende disponibile la suite di test nell&#39;interfaccia utente di test.

```javascript
.addTestCase(new hobs.TestCase("Calculate amount to borrow")
        // navigate to the mortgage form  which is to be tested
        .navigateTo("/content/forms/af/cal/mortgageform.html?wcmmode=disabled")
  .asserts.isTrue(function () {
            return calvin.isFormLoaded()
        })
```

>[!NOTE]
>
>Se stai sottoponendo a test questa funzionalità su AEM Forms 6.4 o versioni successive, crea un nuovo modulo adattivo e utilizzalo per eseguire i test.Non è consigliabile utilizzare il modulo adattivo fornito con il pacchetto.

È possibile aggiungere casi di test alla suite di test da eseguire su un modulo adattivo.

* Per aggiungere un test case alla suite di test, utilizza il metodo `addTestCase` dell&#39;oggetto TestSuite.
* Il metodo `addTestCase` utilizza un oggetto TestCase come parametro.
* Per creare TestCase, utilizzare il metodo `hobs.TestCase(..)` .
* Nota: Il primo parametro è il nome del caso di test che verrà visualizzato nell’interfaccia utente.
* Dopo aver creato un test case, puoi aggiungere azioni al test case.
* È possibile aggiungere azioni come `navigateTo`, `asserts.isTrue` come azioni al test case.

## Esecuzione dei test automatizzati {#running-the-automated-tests}

[](http://localhost:4502/libs/granite/testing/hobbes.html)OpenthetestsuiteEspandi la suite di test ed esegui i test. Se tutto viene eseguito correttamente, verrà visualizzato il seguente output.

![calvinsdk](assets/calvinimage.png)

## Prova le suite di test di esempio {#try-out-the-sample-test-suites}

Come parte del pacchetto di campioni ci sono tre ulteriori suite di test. Puoi provarli includendo i file appropriati nel file js.txt della clientlibrary come mostrato di seguito:

```javascript
#base=.

scriptingTest.js
validationTest.js
prefillTest.js
mortgageForm.js
```
