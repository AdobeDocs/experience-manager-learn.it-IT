---
title: Integrare AEM Forms as a Cloud Service e Marketo (Parte 3)
description: Scopri come integrare AEM Forms e Marketo utilizzando AEM Forms Form Data Model.
feature: Form Data Model,Integration
version: Experience Manager as a Cloud Service
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="Integrazione" type="positive"
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
last-substantial-update: 2024-07-24T00:00:00Z
jira: KT-15876
exl-id: 43737765-b1ea-4594-853a-d78f41136b5e
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 2%

---

# Crea modello dati modulo

Dopo aver configurato l’origine dati, il passaggio successivo consiste nel creare un modello dati del modulo basato sull’origine dati configurata nel passaggio precedente. Per creare un modello dati modulo, segui i passaggi seguenti:

Puntare il browser alla pagina [&#x200B; integrazioni dati.](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm) Elenca tutte le integrazioni di dati create nell&#39;istanza AEM.

1. Fai clic su Crea. | Modello dati modulo
1. Fornisci un titolo significativo come FormsAndMarketo e fai clic su Avanti
1. Seleziona l’origine dati configurata nel passaggio precedente, quindi fai clic su Crea e modifica per aprire il modello dati del modulo in modalità di modifica
1. Espandere il nodo &quot;FormsAndMarketo&quot;. Espandere il nodo Servizi
1. Seleziona la prima operazione &quot;Get&quot;
1. Fai clic su Aggiungi selezionati
1. Fate clic su &quot;Seleziona tutto&quot; nella finestra di dialogo &quot;Aggiungi oggetti modello associati&quot;, quindi fate clic su Aggiungi
1. Salvare il modello dati del modulo facendo clic sul pulsante Salva
1. Scheda della scheda Servizi
1. Seleziona l’unico servizio elencato e fai clic su Test Service
1. Specifica un leadId valido e fai clic su Prova. Se tutto va bene, dovresti recuperare i dettagli del lead come mostrato nella schermata seguente
   ![risultati del test](assets/testresults.png)

Ora puoi creare un modulo adattivo basato su questo modello dati modulo per inserire e recuperare oggetti Marketo.
