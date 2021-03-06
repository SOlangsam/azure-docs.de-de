---
author: IEvangelist
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 01/31/2020
ms.author: dapine
ms.openlocfilehash: e9f02f95693552180a0eed1550cc59d8f975416b
ms.sourcegitcommit: fa6fe765e08aa2e015f2f8dbc2445664d63cc591
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2020
ms.locfileid: "76961411"
---
In dieser Schnellstartanleitung wird Text unter Verwendung des [Speech SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) in synthetisierte Sprache umgewandelt. Der Sprachsynthesedienst bietet zahlreiche Optionen für synthetisierte Stimmen. Weitere Informationen finden Sie unter [Text-zu-Sprache](../../../language-support.md#text-to-speech). Nachdem Sie einige Voraussetzungen erfüllt haben, erfordert das Rendern synthetisierter Sprache über die Standardlautsprecher nur vier Schritte:
> [!div class="checklist"]
> * Erstellen eines Objekts vom Typ `SpeechConfig` auf der Grundlage Ihres Abonnementschlüssels und Ihrer Region
> * Erstellen eines Objekts vom Typ `SpeechSynthesizer` unter Verwendung des obigen Objekts `SpeechConfig`
> * Verwenden Sie das `SpeechSynthesizer`-Objekt, um den Text zu sprechen.
> * Überprüfen Sie das zurückgegebene `SpeechSynthesisResult`-Objekt auf Fehler.
