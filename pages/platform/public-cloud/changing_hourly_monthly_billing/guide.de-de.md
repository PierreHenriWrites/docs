---
title: 'Von der stündlichen auf monatliche Abrechnung umstellen'
excerpt: 'Erfahren Sie hier, wie Sie die Abrechnungsart Ihrer Public Cloud Instanz ändern'
slug: abrechnungsart-aendern-public-cloud
section: Projektverwaltung
---

**Letzte Aktualisierung am 21.09.2021**

## Ziel

Wenn Sie eine Public Cloud Instanz erstellen, können Sie wählen, ob die Abrechnung stündlich oder monatlich erfolgen soll. Instanzen mit Stundensatz werden nach dem Verfahren *Pay as you go* abgerechnet, d.h. am Monatsende werden den Nutzern die spezifischen Ressourcen in Rechnung gestellt, die sie verwendet haben. 

Instanzen mit monatlicher Fälligkeit können im Voraus bezahlt werden und werden zu einem niedrigeren Preis berechnet (50% weniger als bei der verbrauchsspezifischen Abrechnung). Wenn Sie ursprünglich die stündliche Abrechnung ausgewählt haben, können Sie jederzeit zur monatlichen Abrechnung wechseln.

**In dieser Anleitung wird erläutert, wie Sie von der stündlichen zur monatlichen Abrechnung wechseln.**

> [!warning]
>
> Sie können nicht von der monatlichen zur stündlichen Abrechnung wechseln. Um Ihre Instanz im Stundensatz abrechnen zu lassen, müssen Sie die monatlich abgerechnete Instanz löschen, eine neue erstellen und die stündliche Abrechnung auswählen. In diesem Fall empfehlen wir Ihnen das folgende Verfahren:
>
>- Erstellen Sie einen Snapshot Ihrer aktuellen Instanz.
>
>- Erstellen Sie eine neue Instanz basierend auf diesem Snapshot.
>
>- Löschen Sie die alte Instanz.
>

## Voraussetzungen

- Sie verfügen über eine [Public Cloud Instanz](https://www.ovhcloud.com/de/public-cloud).
- Sie haben Zugriff auf Ihr [OVHcloud Kundencenter](https://www.ovh.com/auth/?action=gotomanager&from=https://www.ovh.de/&ovhSubsidiary=de).


## In der praktischen Anwendung

Wählen Sie im [OVHcloud Kundencenter](https://www.ovh.com/auth/?action=gotomanager&from=https://www.ovh.de/&ovhSubsidiary=de) unter `Instances`{.action} die Instanz für welche Sie das Abrechnungsintervall ändern möchten. Öffnen Sie das Kontextmenü indem Sie auf `...`{.action} rechts neben der Instanz klicken. Sie sehen dann die Option `Auf monatliche Abrechnung umstellen`{.action}:

![Change billing calculation](images/switch_to_monthly_updated.png){.thumbnail}

Sie müssen dann bestätigen, dass Sie die Abrechnungsart ändern möchten:

![Confirm billing calculation change](images/confirm_to_monthly_updated.png){.thumbnail}

Sobald Sie Ihre Wahl bestätigt haben, erhalten Sie eine anteilige Monatsrechnung. Die nächste Rechnung enthält den Stundensatzanteil des Monats (1. des Monats bis zur Änderung) und die neue monatliche Gebühr.


## Weiterführende Informationen

Für den Austausch mit unserer User Community gehen Sie auf <https://community.ovh.com/en/>.
