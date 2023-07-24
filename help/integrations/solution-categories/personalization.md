---
title: Personalizzazione su larga scala
description: Rendi le esperienze personalizzate parte di ogni momento.
source-git-commit: ed53392381fa568de8230288e6b85c87540222cf
workflow-type: tm+mt
source-wordcount: '542'
ht-degree: 0%

---


# Personalizzazione su larga scala

In un panorama altamente competitivo e guidato dalla tecnologia digitale di oggi, i clienti si aspettano esperienze personalizzate in base alle loro preferenze ed esigenze specifiche. Sfruttando le funzionalità di Adobe Experience Cloud possiamo raccogliere e analizzare numerosi dati sui clienti, fornendo informazioni preziose su comportamenti, interessi e preferenze. Questa profonda comprensione facilita la consegna di esperienze personalizzate attraverso vari punti di contatto, garantendo interazioni significative e coinvolgenti. Sfruttare la potenza di Adobe Experience Cloud sfrutta appieno il potenziale della personalizzazione, promuovendo connessioni più solide con i clienti, coltivando la fedeltà e guidando la crescita del business.

<table>
  <thead>
    <tr>
      <th>Caso di utilizzo dell’integrazione</th>
      <th>Descrizione dell’integrazione</th>
      <th>Esempi</th>
      <th>applicazioni Experience Cloud</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><strong>Creare documenti PDF personalizzati</strong></td>
      <td>
        Genera documenti di comunicazione per la firma in base a selezioni utente/preferenze.
      </td>
      <td>
        <ul>
          <li>
            Presentare un NDA generato in modo dinamico in base ai dati di un invio AEM Forms per la firma
          </li>
        </ul>
      </td>
      <td>
        <a
          href="../integrations-between-applications/experience-manager/experience-manager-acrobat-sign.md"
          target="_blank"
          rel="noopener noreferrer"
          >AEM Forms e Sign</a
        >
      </td>
    </tr>
    <tr>
      <td rowspan="2"><strong>Analisi dei dati e reporting</strong></td>
      <td>
        Analizzare i dati comportamentali dalle esperienze digitali <br />Utilizza i dati comportamentali di Adobe Analytics in Analysis Workspace in Customer Journey Analytics.
      </td>
      <td>
        <ul>
          <li>Analisi dei percorsi di conversione superiore/inferiore</li>
          <li>Analizzare il coinvolgimento e la conversione dei canali</li>
          <li>Comprendere il contenuto visualizzato più in alto</li>
          <li>Comprendere le categorie e i prodotti principali</li>
          <li>
            Eseguire analisi dell’utilizzo degli strumenti per ottimizzare le esperienze self-service
          </li>
        </ul>
      </td>
      <td>
        <a
          href="../integrations-between-applications/analytics/analytics-customer-journey-analytics.md"
          target="_blank"
          rel="noopener noreferrer"
          >Analytics e Customer Journey Analytics</a
        >
      </td>
    </tr>
    <tr>
      <td>
        Generazione di rapporti per le attività di personalizzazione<br />Analizza i risultati dei test di ottimizzazione, tra cui test A/B, utilizzando Adobe Target e generando rapporti completi tramite Adobe Analytics.
      </td>
      <td>
        <ul>
          <li>Mostrare i risultati del test A/B nei rapporti di analisi avanzati</li>
        </ul>
      </td>
      <td>
        <a
          href="../integrations-between-applications/analytics/analytics-target.md"
          target="_blank"
          rel="noopener noreferrer"
          >Analytics e Target</a
        >
      </td>
    </tr>
    <tr>
      <td><strong>Personalizzare le consegne e-mail</strong></td>
      <td>
        Personalizza le consegne e-mail con contenuti dinamici sfruttando le funzionalità di Adobe Target.
      </td>
      <td>
        <ul>
          <li>Aggiungere offerte personalizzate alle e-mail dei clienti</li>
        </ul>
      </td>
      <td>
        <a
          href="../integrations-between-applications/campaign//campaign-target.md"
          target="_blank"
          rel="noopener noreferrer"
          >Campaign e Target</a
        >
      </td>
    </tr>
    <tr>
      <td rowspan="2">
        <strong>Espandere i tipi di pubblico per le piattaforme di personalizzazione e annunci</strong>
      </td>
      <td>
        Utilizza i segmenti Audience Manager per creare tipi di pubblico in Real-Time CDP da utilizzare nelle tattiche di personalizzazione e remarketing.
      </td>
      <td>
        <ul>
          <li>
            Eseguire il targeting e la personalizzazione anonimi del pubblico digitale sul sito web, sull’app mobile o sui canali pubblicitari supportati
          </li>
          <li>
            Ottimizza la pagina di destinazione e le esperienze di preautenticazione in base alle caratteristiche del dispositivo e del comportamento note
          </li>
          <li>
            Sfrutta la rete dati di terze parti Audience Manager per perfezionare ed espandere ulteriormente i tipi di pubblico per il targeting
          </li>
          <li>Condividere segmenti di Audience Manager in RTCDP</li>
        </ul>
      </td>
      <td>
        <a
          href="../integrations-between-applications/aam/aam-rtcdp.md"
          target="_blank"
          rel="noopener noreferrer"
          >AUDIENCE MANAGER e REAL-TIME CUSTOMER DATA PLATFORM</a
        >
      </td>
    </tr>
    <tr>
      <td>
        Utilizza i dati di Analytics per creare tipi di pubblico da utilizzare nelle tattiche di personalizzazione o remarketing.
      </td>
      <td>
        <ul>
          <li>
            Esegui il targeting e la personalizzazione del pubblico digitale su dispositivi o canali pubblicitari supportati.
          </li>
          <li>
            Ottimizza le pagine di destinazione note dei clienti ed esperienze anonime in base agli attributi del dispositivo e del comportamento.
          </li>
          <li>Attiva i tipi di pubblico su canali noti, ad esempio e-mail e SMS.</li>
        </ul>
      </td>
      <td>
        <a
          href="../integrations-between-applications/analytics/analytics-customer-journey-analytics.md"
          target="_blank"
          rel="noopener noreferrer"
          >Analytics e Real-time Customer Data Platform</a
        >
      </td>
    </tr>
    <tr>
      <td rowspan="2"><strong>Personalizzare le esperienze web</strong></td>
      <td>
        Personalizza le esperienze delle applicazioni a pagina singola (SPA) utilizzando in modo efficace AEM Headless in combinazione con Adobe Target.
      </td>
      <td>
        <ul>
          <li>Personalizzazione di SPA e app mobili</li>
          <li>Risposte API personalizzate.</li>
          <li>Consegna mirata dei contenuti.</li>
          <li>Variazioni del test A/B.</li>
        </ul>
      </td>
      <td>
        <a
          href="../integrations-between-applications/experience-manager/experience-manager-target.md"
          target="_blank"
          rel="noopener noreferrer"
          >AEM Headless e Target</a
        >
      </td>
    </tr>
    <tr>
      <td>
        Fornisci esperienze personalizzate sui siti web utilizzando in modo efficace AEM Sites e Adobe Target per la personalizzazione.
      </td>
      <td>
        <ul>
          <li>Personalizzazione del sito web AEM.</li>
          <li>Personalizzare il contenuto del sito web.</li>
          <li>Ottimizzare le esperienze utente.</li>
          <li>Variazioni del test A/B.</li>
        </ul>
      </td>
      <td>
        <a
          href="../integrations-between-applications/experience-manager/experience-manager-target.md"
          target="_blank"
          rel="noopener noreferrer"
          >AEM Sites e Target</a
        >
      </td>
    </tr>
    <tr>
      <td><strong>Personalizzare le esperienze digitali</strong></td>
      <td>
        Utilizza Real-time Customer Profile e segmenti di Platform gestiti a livello centrale per personalizzare la messaggistica su canali web, mobili e altri canali digitali
      </td>
      <td>
        <ul>
          <li>Personalizzazione dei contenuti per i visitatori noti</li>
          <li>Aumentare l’iscrizione e la partecipazione dei clienti fidelizzati</li>
          <li>Identificare e coinvolgere i clienti a rischio di abbandono</li>
          <li>Personalizzazione delle offerte in tempo reale</li>
        </ul>
      </td>
      <td>
        <a
          href="../integrations-between-applications/rtcdp/rtcdp-target.md"
          target="_blank"
          rel="noopener noreferrer"
          >Real-time Customer Data Platform e Target</a
        >
      </td>
    </tr>
    <tr>
      <td><strong>Migliora la generazione di lead</strong></td>
      <td>
        Utilizza Real-time Customer Profile e segmenti di Platform gestiti a livello centrale per personalizzare la messaggistica su canali web, mobili e altri canali digitali
      </td>
      <td>
        <ul>
          <li>Personalizzazione dei contenuti per i visitatori noti</li>
        </ul>
      </td>
      <td>
        <a
          href="../integrations-between-applications/rtcdp/rtcdp-target.md"
          target="_blank"
          rel="noopener noreferrer"
          >Real-time Customer Data Platform e Target</a
        >
      </td>
    </tr>
  </tbody>
</table>
