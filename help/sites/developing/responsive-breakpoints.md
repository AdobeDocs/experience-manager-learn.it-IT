---
title: Punti di interruzione reattivi
description: Scopri come configurare nuovi punti di interruzione reattivi per l’Editor pagina reattivo per AEM.
version: Cloud Service
feature: Page Editor
topic: Mobile, Development
role: Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-01-05T00:00:00Z
jira: KT-11664
thumbnail: kt-11664.jpeg
exl-id: 8b48c28f-ba7f-4255-be96-a7ce18ca208b
duration: 70
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 0%

---

# Punti di interruzione reattivi

Scopri come configurare nuovi punti di interruzione reattivi per l’Editor pagina reattivo per AEM.

## Creare punti di interruzione CSS

Innanzitutto, crea punti di interruzione multimediali nel CSS della griglia reattiva dell’AEM a cui aderisce il sito dell’AEM reattivo.

In entrata `/ui.apps/src/main/content/jcr_root/apps/[app name]/clientlibs/clientlib-grid/less/grid.less` creare i punti di interruzione da utilizzare insieme all&#39;emulatore mobile. Prendi nota di `max-width` per ogni punto di interruzione, poiché questo mappa i punti di interruzione CSS ai punti di interruzione dell’Editor pagina reattivo dell’AEM.

![Creare nuovi punti di interruzione reattivi](./assets/responsive-breakpoints/create-new-breakpoints.jpg)

## Personalizzare i punti di interruzione del modello

Apri `ui.content/src/main/content/jcr_root/conf/<app name>/settings/wcm/templates/page-content/structure/.content.xml` file e aggiornamento `cq:responsive/breakpoints` con le nuove definizioni dei nodi di punto di interruzione. Ogni [Punto di interruzione CSS](#create-new-css-breakpoints) deve avere un nodo corrispondente in `breakpoints` con i relativi `width` proprietà impostata sul punto di interruzione CSS `max-width`.

![Personalizzare i punti di interruzione reattivi del modello](./assets/responsive-breakpoints/customize-template-breakpoints.jpg)

## Creare emulatori

È necessario definire gli emulatori AEM che consentano agli autori di selezionare la vista reattiva da modificare nell’Editor pagina.

Creare nodi di emulatori in `/ui.apps/src/main/content/jcr_root/apps/<app name>/emulators`

Ad esempio: `/ui.apps/src/main/content/jcr_root/apps/wknd-examples/emulators/phone-landscape`. Copiare un nodo emulatore di riferimento da `/libs/wcm/mobile/components/emulators` in CRXDE Liti per aggiornare la copia e velocizzare la definizione del nodo.

![Creare nuovi emulatori](./assets/responsive-breakpoints/create-new-emulators.jpg)

## Crea gruppo di dispositivi

Raggruppa gli emulatori in [rendili disponibili nell’Editor pagina per AEM](#update-the-templates-device-group).

Crea `/apps/settings/mobile/groups/<name of device group>` struttura dei nodi in `/ui.apps/src/main/content/jcr_root`.

![Crea nuovo gruppo di dispositivi](./assets/responsive-breakpoints/create-new-device-group.jpg)

Creare un `.content.xml` file in `/apps/settings/mobile/groups/<device group name>` e definisci i nuovi emulatori utilizzando un codice simile a quello seguente:

![Crea nuovo dispositivo](./assets/responsive-breakpoints/create-new-device.jpg)

## Aggiornare il gruppo di dispositivi del modello

Infine, mappa nuovamente il gruppo di dispositivi sul modello della pagina in modo che gli emulatori siano disponibili nell’Editor pagina per le pagine create da questo modello.

Apri `ui.content/src/main/content/jcr_root/conf/[app name]/settings/wcm/templates/page-content/structure/.content.xml` file e aggiorna il `cq:deviceGroups` proprietà per fare riferimento al nuovo gruppo mobile (ad esempio, `cq:deviceGroups="[mobile/groups/customdevices]"`)
