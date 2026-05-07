---
title: Sviluppo basato sull’intelligenza artificiale
description: Scopri lo sviluppo basato sull’intelligenza artificiale che utilizza un IDE o agenti di codifica basati sull’intelligenza artificiale insieme a AGENTS.md, Agent Skills e server MCP per contribuire a produrre codice di alta qualità pronto per la produzione per i progetti su AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
feature: Developer Tools
role: Developer
level: Beginner
doc-type: Article
duration: 0
last-substantial-update: 2026-04-24T00:00:00Z
jira: KT-20899
thumbnail: KT-20899.pngKT-20899
exl-id: 19b7ab0b-2f47-434a-a141-17701f432fac
source-git-commit: 6f303c8fbec523227716fe0bc1bff8fceffad1f9
workflow-type: tm+mt
source-wordcount: '906'
ht-degree: 0%

---

# Sviluppo basato sull’intelligenza artificiale

Lo sviluppo basato sull’intelligenza artificiale utilizza un IDE o agenti di codifica basati sull’intelligenza artificiale insieme a `AGENTS.md`, Abilità dell’agente e server MCP per produrre codice di alta qualità pronto per la produzione per i progetti AEM as a Cloud Service.

Strumenti quali [Cursore](https://www.cursor.com/), [Copilota GitHub in Visual Studio Code](https://code.visualstudio.com/docs/copilot/overview), [Claude Code](https://code.claude.com/docs/en/overview) e IDE basati su IA e agenti di codifica simili consentono di eseguire le operazioni seguenti in alcuni modi:

- **Iterazione più rapida**: genera o riesegui il factoring del codice dai prompt del linguaggio naturale che descrivono la funzionalità o la modifica desiderata.
- **Materiale di apprendimento**: spiega percorsi di codice, configurazione, concetti o best practice non familiari quando richiesto.

Tuttavia, questi vantaggi dipendono in larga misura dal contesto _disponibile per l&#39;agente di codifica_. I dati di formazione generici e un singolo snapshot dell&#39;archivio sono spesso _insufficienti_ per produrre in modo affidabile il codice AEM pronto per la produzione.

## Perché l’intelligenza artificiale da sola è insufficiente

Senza il contesto giusto, i modelli di IA (tramite un IDE o un agente di codifica basato sull’intelligenza artificiale) possono:

- **API o cicli di vita allucinati**: suggerisci codici o configurazioni non in linea con le best practice o le funzionalità più recenti di AEM as a Cloud Service.
- **Passaggi non validi**: ometti i passaggi richiesti non visibili nell&#39;archivio del codice o nei dati di formazione.
- **Deriva dagli standard del progetto**: ignora i modelli stabiliti per i componenti, i servizi OSGi, i flussi di lavoro o la configurazione di Dispatcher.

Questo gap è il luogo in cui _contesto strutturato_ (Agent Skills and AGENTS.md) e _visibilità runtime_ (server MCP) diventano essenziali per rendere lo sviluppo basato sull&#39;intelligenza artificiale _produttivo_ e _affidabile_.

## Come Adobe supporta lo sviluppo basato sull’intelligenza artificiale

Per i progetti AEM as a Cloud Service, Adobe fornisce:

- Agent Skills e AGENTS.md tramite [Adobe Skills for AI Coding Agents](https://github.com/adobe/skills)
- Server MCP locali per AEM SDK e Dispatcher locale tramite il portale [Distribuzione software](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=mcp*&1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&1_group.propertyvalues.operation=equals&1_group.propertyvalues.0_values=software-type%3Atooling&orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&orderby.sort=desc&layout=list&p.offset=0&p.limit=3)
- Server AEM MCP ospitati da Adobe per contenuti e flussi di lavoro Cloud Manager dall&#39;applicazione IDE o chat. Vedere [Server MCP in AEM](../mcp/overview.md)

Nelle sezioni seguenti viene fornito un riepilogo di ogni elemento. Utilizzare le sezioni **Configurazione** e **Casi d&#39;uso** alla fine di questa pagina per l&#39;installazione e le procedure dettagliate per lo sviluppo assistito da IA.

## Cosa sono le abilità dell’agente

Le abilità dell&#39;agente sono _conoscenze procedurali o competenze_ per aiutare gli agenti di codifica _a eseguire il lavoro in modo affidabile_. Per ulteriori informazioni, vedere [Abilità agente](https://agentskills.io).

Per un progetto AEM as a Cloud Service, le abilità dell&#39;agente sono disponibili nell&#39;archivio [Competenze Adobe per agenti di codifica AI](https://github.com/adobe/skills).

## Cos’è AGENTS.md

AGENTS.md fornisce _contesto e istruzioni_ per consentire agli agenti di codifica _di lavorare sul progetto_. Per ulteriori informazioni, vedere [AGENTS.md](https://agents.md/).

Per un progetto AEM as a Cloud Service, l&#39;abilità di avvio `ensure-agents-md` crea **AGENTS.md** nella directory principale dell&#39;archivio **quando manca**. L&#39;abilità esamina il progetto (ad esempio, la radice `pom.xml` e i moduli) e genera indicazioni personalizzate anziché utilizzare un file statico. Se **AGENTS.md** esiste già, **non** verrà sovrascritto.

Una volta che il file è presente, puoi modificarlo per aggiungere altro contesto e istruzioni per le best practice del team o dell’organizzazione. L&#39;abilità può anche creare **CLAUDE.md** che fa riferimento a **AGENTS.md** in modo che gli strumenti basati su Claude raccolgano la stessa guida.

## Cosa sono i server MCP

I server MCP espongono strumenti e dati all&#39;agente di codifica tramite il [Model Context Protocol](https://modelcontextprotocol.io/), che supporta azioni quali il debug, l&#39;ispezione, l&#39;esecuzione e la convalida delle modifiche. Un server MCP può essere eseguito sulla workstation (**local**) o come servizio ospitato (**remote**).

Per **sviluppo locale** rispetto a AEM SDK e Dispatcher, installare questi **server MCP locali** dal portale [Distribuzione software](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=mcp*&1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&1_group.propertyvalues.operation=equals&1_group.propertyvalues.0_values=software-type%3Atooling&orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&orderby.sort=desc&layout=list&p.offset=0&p.limit=3):

- **Server MCP locale Quickstart di AEM**: espone i dati live runtime da un&#39;istanza AEM SDK locale per supportare la risoluzione dei problemi e lo sviluppo. Per ulteriori informazioni, vedere [AEM Quickstart MCP Server](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/local-development-with-ai-tools#aem-quickstart-mcp-server).
- **Server MCP locale di Dispatcher**: consente la convalida e l&#39;ispezione in fase di esecuzione di un&#39;istanza Dispatcher locale. Per ulteriori informazioni, vedere [Dispatcher MCP Server](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/local-development-with-ai-tools#dispatcher-mcp-server).

Per i server AEM MCP ospitati da Adobe (ad esempio, contenuto, contenuto di sola lettura e Cloud Manager), vedere [Server MCP in AEM](../mcp/overview.md).

## Configurazione

<!-- 
CARDS
{target = _self}

* ./setup/agent-skills.md
    {title = Set up AEM Agent Skills}
    {description = Learn how to set up AEM Agent Skills for AI-assisted development.}
    {image = ./assets/agent-skills/select-aem-agent-skills-to-install.png}
    {cta = Install AEM Agent Skills}

-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Set up AEM Agent Skills">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./setup/agent-skills.md" title="Configurare le abilità dell’agente AEM" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/agent-skills/select-aem-agent-skills-to-install.png" alt="Configurare le abilità dell’agente AEM"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./setup/agent-skills.md" target="_self" rel="referrer" title="Configurare le abilità dell’agente AEM">Configura le abilità dell'agente AEM</a>
                    </p>
                    <p class="is-size-6">Scopri come configurare le competenze dell’agente AEM per lo sviluppo basato sull’intelligenza artificiale.</p>
                </div>
                <a href="./setup/agent-skills.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Installa le abilità dell'agente AEM</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## Casi d’uso

<!-- 
CARDS
{target = _self}

* ./use-cases/component-development.md    
    {title = Create AEM Component with AI-assisted development}
    {description = Learn how to use AI-assisted development to develop AEM components.}
    {image = ./assets/component-development/review-generated-code.png}
    {cta = Create AEM Component}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Create AEM Component with AI-assisted development">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/component-development.md" title="Creare un componente AEM con sviluppo basato sull’intelligenza artificiale" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/component-development/review-generated-code.png" alt="Creare un componente AEM con sviluppo basato sull’intelligenza artificiale"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/component-development.md" target="_self" rel="referrer" title="Creare un componente AEM con sviluppo basato sull’intelligenza artificiale">Creare un componente AEM con sviluppo basato sull'intelligenza artificiale</a>
                    </p>
                    <p class="is-size-6">Scopri come utilizzare lo sviluppo basato sull’intelligenza artificiale per sviluppare componenti AEM.</p>
                </div>
                <a href="./use-cases/component-development.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Crea componente AEM</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## Risorse aggiuntive

- [Sviluppo locale con strumenti IA](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/local-development-with-ai-tools)

- [Abilità di Adobe per gli agenti di codifica AI](https://github.com/adobe/skills)

- [AGENTS.md](https://agents.md/)

- [Abilità agente](https://agentskills.io/home)
