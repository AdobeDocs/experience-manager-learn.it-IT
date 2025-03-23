---
title: Punti di interruzione reattivi
description: Scopri come configurare nuovi punti di interruzione reattivi per l’Editor pagina reattivo di AEM.
version: Experience Manager as a Cloud Service
feature: Page Editor
topic: Mobile, Development
role: Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-01-05T00:00:00Z
jira: KT-11664
thumbnail: kt-11664.jpeg
exl-id: 8b48c28f-ba7f-4255-be96-a7ce18ca208b
duration: 52
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 0%

---

# Punti di interruzione reattivi

Scopri come configurare nuovi punti di interruzione reattivi per l’Editor pagina reattivo di AEM.

## Creare punti di interruzione CSS

Innanzitutto, crea punti di interruzione multimediali nel CSS AEM Responsive Grid a cui il sito responsive di AEM aderisce.

Nel file `/ui.apps/src/main/content/jcr_root/apps/[app name]/clientlibs/clientlib-grid/less/grid.less`, creare i punti di interruzione da utilizzare insieme all&#39;emulatore mobile. Prendi nota di `max-width` per ogni punto di interruzione, in quanto esegue il mapping dei punti di interruzione CSS ai punti di interruzione dell’Editor pagina reattivo di AEM.

![Crea nuovi punti di interruzione reattivi](./assets/responsive-breakpoints/create-new-breakpoints.jpg)

## Personalizzare i punti di interruzione del modello

Apri il file `ui.content/src/main/content/jcr_root/conf/<app name>/settings/wcm/templates/page-content/structure/.content.xml` e aggiorna `cq:responsive/breakpoints` con le nuove definizioni dei nodi dei punti di interruzione. Ogni [punto di interruzione CSS](#create-new-css-breakpoints) deve avere un nodo corrispondente in `breakpoints` con la relativa proprietà `width` impostata sul `max-width` del punto di interruzione CSS.

![Personalizzare i punti di interruzione reattivi del modello](./assets/responsive-breakpoints/customize-template-breakpoints.jpg)

## Creare emulatori

È necessario definire gli emulatori di AEM che consentano agli autori di selezionare la vista reattiva da modificare nell’Editor pagina.

Crea nodi emulatori in `/ui.apps/src/main/content/jcr_root/apps/<app name>/emulators`

Ad esempio, `/ui.apps/src/main/content/jcr_root/apps/wknd-examples/emulators/phone-landscape`. Copiare un nodo emulatore di riferimento da `/libs/wcm/mobile/components/emulators` in CRXDE Lite in e aggiornare la copia per accelerare la definizione del nodo.

![Crea nuovi emulatori](./assets/responsive-breakpoints/create-new-emulators.jpg)

## Crea gruppo di dispositivi

Raggruppa gli emulatori per [renderli disponibili in AEM Page Editor](#update-the-templates-device-group).

Crea la struttura del nodo `/apps/settings/mobile/groups/<name of device group>` in `/ui.apps/src/main/content/jcr_root`.

![Crea nuovo gruppo di dispositivi](./assets/responsive-breakpoints/create-new-device-group.jpg)

Crea un file `.content.xml` in `/apps/settings/mobile/groups/<device group name>` e definisci
i nuovi emulatori che utilizzano un codice simile a quello seguente:

![Crea nuovo dispositivo](./assets/responsive-breakpoints/create-new-device.jpg)

## Aggiornare il gruppo di dispositivi del modello

Infine, mappa nuovamente il gruppo di dispositivi sul modello della pagina in modo che gli emulatori siano disponibili nell’Editor pagina per le pagine create da questo modello.

Apri il file `ui.content/src/main/content/jcr_root/conf/[app name]/settings/wcm/templates/page-content/structure/.content.xml` e aggiorna la proprietà `cq:deviceGroups` per fare riferimento al nuovo gruppo mobile (ad esempio, `cq:deviceGroups="[mobile/groups/customdevices]"`)
