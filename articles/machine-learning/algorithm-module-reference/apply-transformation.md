---
title: 'Apply Transformation: Modulreferenz'
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie mit dem Modul Apply Transformation in Azure Machine Learning ein Eingabedataset basierend auf einer zuvor berechneten Transformation ändern.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 03/05/2020
ms.openlocfilehash: ccf9d0c3eef50c7dfd838f1929e52506e8984879
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2020
ms.locfileid: "78395262"
---
# <a name="apply-transformation-module"></a>Apply Transformation-Modul

In diesem Artikel wird ein Modul in Azure Machine Learning-Designer (Vorschauversion) beschrieben.

Verwenden Sie dieses Modul, um einen Eingabedatensatz basierend auf einer zuvor berechneten Transformation zu ändern.

Wenn Sie beispielsweise Z-Ergebnisse zum Normalisieren Ihrer Trainingsdaten mit dem Modul **Normalize Data** (Normalisieren Sie Daten) verwendet haben, möchten Sie auch den Z-Ergebniswert verwenden, der für das Training während der Bewertungsphase berechnet wurde. In Azure Machine Learning können Sie die Normalisierungsmethode als Transformation speichern und dann **Apply Transformation** (Anwenden einer Transformation) verwenden, um das Z-Ergebnis vor der Bewertung auf die Eingabedaten anzuwenden.

## <a name="how-to-save-transformations"></a>Speichern von Transformationen

Mit dem Designer können Sie Datentransformationen als **Datasets** speichern, damit Sie sie in anderen Pipelines verwenden können.

1. Wählen Sie ein Datentransformationsmodul aus, das erfolgreich ausgeführt wurde.

1. Wählen Sie die Registerkarte **Ausgaben + Protokolle** aus.

1. Wählen Sie das Symbol **Speichern** aus, um die **Ergebnistransformation** zu speichern.

## <a name="how-to-use-apply-transformation"></a>Verwenden des Moduls „Apply Transformation“  
  
1. Fügen Sie das Modul **Apply Transformation** (Transformation anwenden) Ihrer Pipeline hinzu. Sie finden dieses Modul im Abschnitt zur **Modellbewertung und -auswertung** der Modulpalette. 
  
1. Suchen Sie die gespeicherte Transformation, die Sie verwenden möchten, unter **Datasets** > **My Datasets** (Meine Datasets) in der Modulpalette.

1. Verbinden Sie die Ausgabe der gespeicherten Transformation mit dem linken Eingangsport des Moduls **Apply Transformation** (Transformation anwenden).

    Der Datensatz muss genau das gleiche Schema (Anzahl der Spalten, Spaltennamen, Datentypen) wie der Datensatz haben, für den die Transformation zuvor vorgesehen war.  
  
1. Verbinden Sie die Datasetausgabe des gewünschten Moduls mit dem rechten Eingangsport des Moduls **Apply Transformation** (Transformation anwenden).
  
1. Um eine Transformation auf das neue Dataset anzuwenden, führen Sie die Pipeline aus.  

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich den [Satz der verfügbaren Module](module-reference.md) für Azure Machine Learning an. 