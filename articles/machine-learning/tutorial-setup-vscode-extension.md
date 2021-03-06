---
title: 'Tutorial: Einrichten der Visual Studio Code-Erweiterung'
titleSuffix: Azure Machine Learning
description: Es wird beschrieben, wie Sie die Azure Machine Learning-Erweiterung für Visual Studio Code einrichten.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: luisquintanilla
ms.author: luquinta
ms.date: 02/24/2020
ms.openlocfilehash: 583071ee22e4fb9cffc741520b1583790002a5bf
ms.sourcegitcommit: 0cc25b792ad6ec7a056ac3470f377edad804997a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/25/2020
ms.locfileid: "77604869"
---
# <a name="set-up-azure-machine-learning-visual-studio-code-extension"></a>Einrichten der Azure Machine Learning-Erweiterung für Visual Studio Code

In diesem Artikel wird beschrieben, wie Sie die Azure Machine Learning-Erweiterung für Visual Studio Code verwenden, um Skripts zu installieren und auszuführen.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Installieren der Azure Machine Learning-Erweiterung für Visual Studio Code
> * Anmelden beim Azure-Konto über Visual Studio Code
> * Verwenden der Azure Machine Learning-Erweiterung zum Ausführen eines Beispielskripts

## <a name="prerequisites"></a>Voraussetzungen

- Azure-Abonnement. Wenn Sie keins besitzen, können Sie sich für die [kostenlose oder kostenpflichtige Version von Azure Machine Learning](https://aka.ms/AMLFree) registrieren.
- Visual Studio Code. Sollte diese Komponente noch nicht vorhanden sein, [installieren Sie sie](https://code.visualstudio.com/docs/setup/setup-overview).
- [Python 3](https://www.python.org/downloads/)

## <a name="install-the-extension"></a>Installieren der Erweiterung

1. Öffnen Sie Visual Studio Code.
1. Wählen Sie in der **Aktivitätsleiste** das Symbol **Erweiterungen** aus, um die Ansicht „Erweiterungen“ zu öffnen.
1. Suchen Sie in der Ansicht „Erweiterungen“ nach „Azure Machine Learning“.
1. Wählen Sie **Installieren** aus.

    > [!div class="mx-imgBorder"]
    > ![Installieren der Azure Machine Learning-Erweiterung für VS Code](./media/tutorial-setup-vscode-extension/install-aml-vscode-extension.PNG)

> [!NOTE]
> Alternativ können Sie die Azure Machine Learning-Erweiterung auch über den Visual Studio Marketplace installieren, indem Sie das [Installationsprogramm direkt herunterladen](https://aka.ms/vscodetoolsforai). 

Die restlichen Schritte dieses Tutorials wurden mit **Version 0.6.8** der Erweiterung getestet.

## <a name="sign-in-to-your-azure-account"></a>Anmelden bei Ihrem Azure-Konto

Zum Bereitstellen von Ressourcen und Ausführen von Workloads in Azure müssen Sie sich mit Ihren Anmeldeinformationen für Ihr Azure-Konto anmelden. Zur Unterstützung der Kontoverwaltung wird die Azure-Kontoerweiterung von Azure Machine Learning automatisch installiert. Besuchen Sie die folgende Website, um [mehr zur Azure-Kontoerweiterung zu erfahren](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account).

1. Öffnen Sie hierzu die Befehlspalette, indem Sie in der Menüleiste **Ansicht > Befehlspalette** auswählen. 
1. Geben Sie den Befehl „Azure: Anmelden“ in die Befehlspalette ein, um den Anmeldeprozess zu starten.

## <a name="run-a-machine-learning-model-training-script-in-azure"></a>Ausführen eines Trainingsskripts für ein Machine Learning-Modell in Azure

Nachdem Sie sich nun mit Ihren Anmeldeinformationen für das Konto bei Azure angemeldet haben, können Sie mit den Schritten in diesem Abschnitt darüber informieren, wie Sie die Erweiterung zum Trainieren eines Machine Learning-Modells verwenden.

1. Laden Sie das [Repository mit den VS Code Tools für KI](https://github.com/microsoft/vscode-tools-for-ai/archive/master.zip) an einen beliebigen Speicherort auf Ihrem Computer herunter, und entzippen Sie es.
1. Öffnen Sie das Verzeichnis `mnist-vscode-docs-sample` in Visual Studio Code.
1. Wählen Sie in der Aktivitätsleiste das **Azure**-Symbol aus.
1. Wählen Sie oben in der Azure Machine Learning-Ansicht das Symbol **Experiment ausführen** aus.

    > [!div class="mx-imgBorder"]
    > ![Experiment ausführen](./media/tutorial-setup-vscode-extension/run-experiment.PNG)

1. Befolgen Sie die Aufforderungen, wenn die Befehlspalette erweitert wird.

    1. Wählen Sie Ihr Azure-Abonnement.
    1. Wählen Sie die Option **Create a new Azure ML workspace** (Neuen Azure ML-Arbeitsbereich erstellen) aus.
    1. Wählen Sie den Auftragstyp **TensorFlow Single-Node Training** (TensorFlow: Training auf einem einzelnen Knoten) aus.
    1. Geben Sie `train.py` als Skript ein, das trainiert werden soll. Dies ist die Datei, die Code für ein Machine Learning-Modell enthält, mit dem die Bilder von handschriftlichen Ziffern kategorisiert werden.
    1. Geben Sie die folgenden Pakete als Anforderungen für die Ausführung an.

        ```text
        pip: azureml-defaults; conda: python=3.6.2, tensorflow=1.15.0
        ```

1. An diesem Punkt wird im Text-Editor eine Konfigurationsdatei angezeigt, die der unten angegebenen Datei ähnelt. Die Konfiguration enthält die Informationen, die zum Trainieren des Auftrags benötigt werden. Dies umfasst beispielsweise die Datei mit dem Code zum Trainieren des Modells sowie alle Python-Abhängigkeiten, die im vorherigen Schritt angegeben wurden.

    ```json
    {
        "workspace": "WS01311608",
        "resourceGroup": "WS01311608-rg1",
        "location": "South Central US",
        "experiment": "WS01311608-exp1",
        "compute": {
            "name": "WS01311608-com1",
            "vmSize": "Standard_D1_v2, Cores: 1; RAM: 3.5GB;"
        },
        "runConfiguration": {
            "filename": "WS01311608-com1-rc1",
            "condaDependencies": [
                "python=3.6.2",
                "tensorflow=1.15.0"
            ],
            "pipDependencies": [
                "azureml-defaults"
            ]
        }
    }
    ```

1. Wenn Sie mit Ihrer Konfiguration zufrieden sind, übermitteln Sie Ihr Experiment, indem Sie die Befehlspalette öffnen und den folgenden Befehl eingeben:

    ```text
    Azure ML: Submit Experiment
    ```

    Hierbei werden die Datei `train.py` und die Konfigurationsdatei an Ihren Azure Machine Learning-Arbeitsbereich gesendet. Anschließend wird der Trainingsauftrag auf einer Computeressource in Azure gestartet.

### <a name="track-the-progress-of-the-training-script"></a>Nachverfolgen des Status des Trainingsskripts

Die Ausführung des Skripts kann mehrere Minuten dauern. So verfolgen Sie den Status:

1. Wählen Sie auf der Aktivitätsleiste das **Azure-Symbol** aus.
1. Erweitern Sie Ihren Abonnementknoten.
1. Erweitern Sie den Knoten Ihres aktuell ausgeführten Experiments. Dieser befindet sich innerhalb des Knotens `{workspace}/Experiments/{experiment}`. Die Werte für den Arbeitsbereich und das Experiment entsprechen hierbei den in der Konfigurationsdatei definierten Eigenschaften.
1. Alle Ausführungen für das Experiment sowie der jeweilige Status werden aufgeführt. Klicken Sie im oberen Bereich der Azure Machine Learning-Ansicht auf das Aktualisierungssymbol, um den neuesten Status abzurufen.

    > [!div class="mx-imgBorder"]
    > ![Nachverfolgen des Experimentstatus](./media/tutorial-setup-vscode-extension/track-experiment-progress.PNG)

### <a name="download-the-trained-model"></a>Herunterladen des trainierten Modells

Nach Abschluss der Experimentausführung wird ein trainiertes Modell ausgegeben. So laden Sie die Ausgaben lokal herunter:

1. Klicken Sie mit der rechten Maustaste auf die neueste Ausführung, und wählen Sie **Download outputs** (Ausgaben herunterladen) aus.

    > [!div class="mx-imgBorder"]
    > ![Herunterladen des trainierten Modells](./media/tutorial-setup-vscode-extension/download-trained-model.PNG)

1. Wählen Sie einen Speicherort für die Ausgaben aus.
1. Ein Ordner mit dem Namen Ihrer Ausführung wird lokal heruntergeladen. Navigieren Sie dorthin.
1. Die Modelldateien befinden sich im Verzeichnis `outputs/outputs/model`.

## <a name="next-steps"></a>Nächste Schritte

* [Tutorial: Trainieren und Bereitstellen eines TensorFlow-Modells für die Bildklassifizierung mit der Azure Machine Learning-Erweiterung für Visual Studio Code](tutorial-train-deploy-image-classification-model-vscode.md).
