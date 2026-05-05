---
title: AI-assisted development
description: Learn about AI-assisted development that uses an AI-powered IDE or coding agents along with AGENTS.md, Agent Skills, and MCP servers to help produce high-quality, production-ready code for projects on AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
feature: Developer Tools
role: Developer, Architect
level: Beginner
doc-type: Article
duration: 0
last-substantial-update: 2026-04-24T00:00:00Z
jira: KT-20899
thumbnail: KT-20899.pngKT-20899
source-git-commit: e3ef450cfe9005ba940ff1897c216681654341b3
workflow-type: tm+mt
source-wordcount: '906'
ht-degree: 0%

---


# AI-assisted development

AI-assisted development uses an AI-powered IDE or coding agents along with `AGENTS.md`, Agent Skills, and MCP servers to help produce high-quality, production-ready code for AEM as a Cloud Service projects.

Tools such as [Cursor](https://www.cursor.com/), [GitHub Copilot in Visual Studio Code](https://code.visualstudio.com/docs/copilot/overview), [Claude Code](https://code.claude.com/docs/en/overview), and similar AI-powered IDEs and coding agents help in a few key ways:

- **Faster iteration**: generate or refactor code from natural language prompts that describe the desired feature or change.
- **Learning aid**: explain unfamiliar code paths, configuration, concepts, or best practices when prompted.

However, these benefits depend heavily on the _context available to the coding agent_. Generic training data and a single repository snapshot are often _not sufficient_ to reliably produce production-ready AEM code.

## Why AI alone is insufficient

Without the right context, AI models (via an AI-powered IDE or coding agent) can:

- **Hallucinate APIs or lifecycles**: suggest code or configurations that do not align with AEM as a Cloud Service best practices or latest features.
- **Miss procedural steps**: omit required steps not visible in code repository or training data.
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

- **AEM Quickstart Local MCP server**: Exposes live runtime data from a local AEM SDK instance to support troubleshooting and development. For more information, see [AEM Quickstart MCP Server](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/local-development-with-ai-tools#aem-quickstart-mcp-server).
- **Dispatcher Local MCP server**: Enables runtime validation and inspection of a local Dispatcher instance. For more information, see [Dispatcher MCP Server](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/local-development-with-ai-tools#dispatcher-mcp-server).

For Adobe-hosted AEM MCP servers (for example, content, read-only content, and Cloud Manager), see [MCP Servers in AEM](../mcp/overview.md).

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
                        <a href="./setup/agent-skills.md" target="_self" rel="referrer" title="Configurare le abilità dell’agente AEM">Set up AEM Agent Skills</a>
                    </p>
                    <p class="is-size-6">Scopri come configurare le competenze dell’agente AEM per lo sviluppo basato sull’intelligenza artificiale.</p>
                </div>
                <a href="./setup/agent-skills.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Install AEM Agent Skills</span>
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

- [Local development with AI Tools](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/local-development-with-ai-tools)

- [Abilità di Adobe per gli agenti di codifica AI](https://github.com/adobe/skills)

- [AGENTS.md](https://agents.md/)

- [Abilità agente](https://agentskills.io/home)