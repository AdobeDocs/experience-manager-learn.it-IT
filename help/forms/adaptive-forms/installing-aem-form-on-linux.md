---
title: Installazione di AEM Forms su Linux
description: Scopri come installare librerie a 32 bit per AEM Forms per lavorare sull’installazione Linux.
feature: Adaptive Forms
type: Tutorial
version: 6.4, 6.5
topic: Development
role: Developer
level: Beginner
kt: 7593
exl-id: b9809561-e9bd-4c67-bc18-5cab3e4aa138
last-substantial-update: 2019-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '945'
ht-degree: 0%

---

# Installazione della versione a 32 bit delle librerie condivise

Quando AEM FORMS OSGi o AEM Forms j2EE è implementato su Linux, è necessario assicurarsi che le versioni a 32 bit di un set di librerie condivise siano installate e disponibili.  Le descrizioni provengono dai pacchetti stessi.

* expat (libreria C del parser XML orientata allo streaming per l&#39;analisi del codice XML, scritta da James Clark)
* fontconfig (libreria di configurazione e personalizzazione dei font progettata per individuare i font all&#39;interno del sistema e selezionarli in base ai requisiti specificati dalle applicazioni)
* freetype (motore di rendering dei font, sviluppato per fornire supporto di font avanzati per una varietà di piattaforme e ambienti. Può aprire e gestire i file di font e caricare, hint ed eseguire il rendering dei singoli glifi in modo efficiente. Non si tratta di un server di font o di una libreria completa di rendering del testo)
* glibc (librerie di base per il sistema GNU e i sistemi GNU/Linux, così come molti altri sistemi che utilizzano Linux come kernel)
* libcurl (libreria di trasferimento URL lato client)
* libICE (libreria Exchange Inter-Client)
* libicu (Libreria che fornisce supporto per Unicode e impostazioni internazionali, componenti internazionali per Unicode, affidabili e completi). È necessaria sia la versione a 64 bit che quella a 32 bit di questa libreria
* libSM (libreria di gestione della sessione X11)
* libuuid (libreria di identificatori universali univoci compatibile con DCE - utilizzata per generare identificatori univoci per oggetti che possono essere accessibili al di fuori del sistema locale)
* libX11 (libreria lato client X11)
* libXau (Protocollo di autorizzazione X11 - utile per limitare l&#39;accesso del client al display)
* libxcb (X protocollo C-language Binding - XCB)
* libXext (libreria per estensioni comuni al protocollo X11)
* libXinerama (estensione X11 che fornisce il supporto per l’estensione di un desktop su più schermi. Il nome è un gioco di parole su Cinerama, un formato video widescreen che ha utilizzato più proiettori. libXinerama è la libreria che si interfaccia con l&#39;estensione RandR)
* libXrandr (l&#39;estensione Xinerama è ampiamente obsoleta al giorno d&#39;oggi - è stata sostituita dall&#39;estensione RandR)
* libXrender (X Rendering Extension client library) nss-softokn-freebl (libreria Freebl per servizi di sicurezza di rete)
* zlib (libreria di compressione dati a scopo generale, senza brevetto, senza perdita di dati)

A partire da Red Hat Enterprise Linux 6, l&#39;edizione a 32 bit di una libreria avrà l&#39;estensione del nome file .686, mentre l&#39;edizione a 64 bit avrà .x86_64. Esempio, expat.i686. Prima di RHEL 6, le edizioni a 32 bit avevano l’estensione .i386. Prima di installare le edizioni a 32 bit, assicurati che siano installate le ultime edizioni a 64 bit. Se l&#39;edizione a 64 bit di una libreria è precedente alla versione a 32 bit installata, si verifica un errore come ad esempio:

0mError: Versioni a più livelli protette: libsepol-2.5-10.el7.x86_64 != libsepol-2.5-6.el7.i686 [0mError: Sono stati rilevati problemi di versione con più livelli.]

## Prima installazione

Su Red Hat Enterprise Linux, utilizza il modificatore YellowDog Update (YUM) per l&#39;installazione, come mostrato di seguito:

1. yum installa expat.i686
2. yum installa fontconfig.i686
3. yum installa freetype.i686
4. yum installa glibc.i686
5. yum installa libcurl.i686
6. yum installa libICE.i686
7. yum installa libicu.i686
8. yum installa libicu
9. yum installa libSM.i686
10. yum installa libuuid.i686
11. yum installa libX11.i686
12. yum installa libXau.i686
13. yum installa libxcb.i686
14. yum installa libXext.i686
15. yum installa libXinerama.i686
16. yum installa libXrandr.i686
17. yum installa libXrender.i686
18. yum installa nss-softokn-freebl.i686
19. yum installa zlib.i686

## Collegamenti

Inoltre, è necessario creare collegamenti simbolici libcurl.so, libcrypto.so e libssl.so che puntano alle versioni più recenti a 32 bit rispettivamente delle librerie libcurl, libcrypto e libssl. Puoi trovare i file in /usr/lib/ ln -s /usr/lib/libcurl.so.4.5.0 /usr/lib/libcurl.so ln -s /usr/lib/libcrypto.so.1.1.1c /usr/lib/libcrypto.so ln -s /usr/lib/libssl.so.1.1.1c /usr/lib/libssl.so

## Aggiornamenti al sistema esistente

possono verificarsi conflitti tra le architetture x86_64 e i686 durante gli aggiornamenti, ad esempio: Errore: Errore di controllo transazione: file /lib/ld-2.28.so dall&#39;installazione di glibc-2.28-72.el8.i686 conflitti con il file dal pacchetto glibc32-2.28-42.1.el8.x86_64

Se ti imbatti in questo, disinstalla prima il pacchetto offensivo, come in questo caso: yum rimuovere glibc32-2.28-42.1.el8.x86_64

Detto questo, si desidera che le versioni x86_64 e i686 siano esattamente uguali, come ad esempio da questo output al comando: glibc info yum

Ultimo controllo di scadenza metadati: 0:41:33 fa su Sat 18 gen 2020 11:37:08.00 EST.
Nome pacchetti installati : versione glibc : Versione 2.28 : Architettura 72.el8 : Dimensioni i686 : 13 M Fonte : archivio glibc-2.28-72.el8.src.rpm : @System Dall&#39;archivio : Riepilogo BaseOS : URL delle librerie di libreria GNU : http://www.gnu.org/software/glibc/ Licenza : LGPLv2+ e LGPLv2+ con eccezioni e GPLv2+ e GPLv2+ con eccezioni e BSD e Inner-Net e ISC e Public Domain e GFDL Descrizione : Il pacchetto glibc contiene librerie standard utilizzate da : più programmi sul sistema. Per risparmiare spazio su disco e : il codice di sistema comune è: mantenuti in un unico luogo e condivisi tra i programmi. Questo particolare pacchetto : contiene i set più importanti di librerie condivise: la norma C : libreria e libreria matematica standard. Senza queste due librerie, un : Il sistema Linux non funzionerà.

Nome : versione glibc : Versione 2.28 : Architettura 72.el8 : x86_64 Dimensione : 15 M Fonte : archivio glibc-2.28-72.el8.src.rpm : @System Dall&#39;archivio : Riepilogo BaseOS : URL delle librerie di libreria GNU : http://www.gnu.org/software/glibc/ Licenza : LGPLv2+ e LGPLv2+ con eccezioni e GPLv2+ e GPLv2+ con eccezioni e BSD e Inner-Net e ISC e Public Domain e GFDL Descrizione : Il pacchetto glibc contiene librerie standard utilizzate da : più programmi sul sistema. Per risparmiare spazio su disco e : il codice di sistema comune è: mantenuti in un unico luogo e condivisi tra i programmi. Questo particolare pacchetto : contiene i set più importanti di librerie condivise: la norma C : libreria e libreria matematica standard. Senza queste due librerie, un : Il sistema Linux non funzionerà.

## Alcuni utili comandi yum

yum list installato yum search [part_of_package_name]
yum cosa fornisce [nome_pacchetto]
installazione yum [nome_pacchetto]
reinstallazione yum [nome_pacchetto]
yum info [nome_pacchetto]
depauperamento del yum [nome_pacchetto]
yum remove [nome_pacchetto]
yum check-update [nome_pacchetto]
aggiornamento anno [nome_pacchetto]
