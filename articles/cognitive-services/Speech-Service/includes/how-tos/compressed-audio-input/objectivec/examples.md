---
author: IEvangelist
ms.service: cognitive-services
ms.topic: include
ms.date: 03/09/2020
ms.author: dapine
ms.openlocfilehash: 788903d802ca47c763734e7cf6ddbbf3b0032203
ms.sourcegitcommit: 8f4d54218f9b3dccc2a701ffcacf608bbcd393a6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2020
ms.locfileid: "78943829"
---
Erstellen Sie `SPXPullAudioInputStream` oder `SPXPushAudioInputStream` zum Streamen von komprimierten Audioformaten an den Speech-Dienst.

Der folgende Codeausschnitt zeigt, wie Sie eine `SPXAudioConfiguration` aus einer Instanz eines `SPXPushAudioInputStream` erstellen, indem Sie MP3 als Komprimierungsformat des Datenstroms angeben.

[!code-objectivec[Set up the input stream](~/samples-cognitive-services-speech-sdk/samples/objective-c/ios/compressed-streams/CompressedStreamsSample/CompressedStreamsSample/ViewController.m?range=67-76&highlight=2-11)]

Der nächste Codeausschnitt zeigt, wie komprimierte Audiodaten aus einer Datei gelesen und in das `SPXPushAudioInputStream` eingefügt werden können.

[!code-objectivec[Push compressed audio data into the stream](~/samples-cognitive-services-speech-sdk/samples/objective-c/ios/compressed-streams/CompressedStreamsSample/CompressedStreamsSample/ViewController.m?range=106-150&highlight=19-44)]
