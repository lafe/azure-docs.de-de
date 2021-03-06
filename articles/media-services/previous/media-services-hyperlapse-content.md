---
title: Hyperlapsing von Mediendateien mit Azure Media Hyperlapse | Microsoft-Dokumentation
description: Azure Media Hyperlapse erzeugt ruckelfreie Zeitraffervideos aus der Ich-Perspektive oder Action-Kamera-Inhalten. In diesem Thema wird die Verwendung von Media Indexer erläutert.
services: media-services
documentationcenter: ''
author: asolanki
manager: johndeu
editor: ''
ms.assetid: 37d54db6-9cf3-4ae9-b3c6-0d29c744e965
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/10/2019
ms.author: adsolank
ms.openlocfilehash: c3ef977e7e8742f4d6de0f85d2d1684aa7eaa887
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2019
ms.locfileid: "55993757"
---
# <a name="hyperlapse-media-files-with-azure-media-hyperlapse"></a>Hyperlapsing von Mediendateien mit Azure Media Hyperlapse

> [!NOTE]
> Diese Previewfunktion von Azure Media Services wird in Kürze eingestellt. Ab dem 19. Dezember 2018 erfolgen in Media Services keine Änderungen oder Verbesserungen von Media Hyperlapse mehr. Am 29. März 2019 wird das Feature eingestellt und ist dann nicht mehr verfügbar.

Azure Media Hyperlapse ist ein Medienprozessor, der ruckelfreie Zeitraffervideos aus der Ich-Perspektive oder Action-Kamera-Inhalten erzeugt.  Als cloudbasiertes Pendant zu [Desktop Hyperlapse Pro von Microsoft Research und dem smartphonebasierten Hyperlapse Mobile](https://aka.ms/hyperlapse)nutzt Microsoft Hyperlapse für Azure Media Services die umfassende Skalierbarkeit der Medienverarbeitungsplattform von Azure Media Services zum horizontalen Skalieren und Parallelisieren der Hyperlapse-Massenverarbeitung.

> [!IMPORTANT]
> Microsoft Hyperlapse ist auf das Arbeiten mit Inhalten aus der Ich-Perspektive mit einer sich bewegenden Kamera ausgelegt. Obwohl Fotokameras ggf. auch geeignet sind, können die Leistung und Qualität des Azure Media Hyperlapse-Medienprozessors für andere Arten von Inhalten nicht garantiert werden.
> 
> 

Ein Azure Media Hyperlapse-Auftrag verwendet als Eingabe eine MP4-, MOV- oder WMV-Mediendatei zusammen mit einer Konfigurationsdatei, die die Videoframes für den Zeitraffer und die Geschwindigkeit (z. B. 10.000 Frames bei doppelter Geschwindigkeit) angibt.  Die Ausgabe ist eine stabilisierte Zeitrafferwiedergabe der Videoeingabe.

## <a name="hyperlapse-an-asset"></a>Hyperlapsing eines Medienobjekts
Zuerst müssen Sie die gewünschte Eingabedatei in Azure Media Services hochladen.  Weitere Informationen zu den Konzepten zum Hochladen und Verwalten von Inhalten finden Sie im [Content Management-Artikel](media-services-portal-vod-get-started.md).

### <a id="configuration"></a>Konfigurationsvoreinstellung für Hyperlapse
Sobald Ihre Inhalte sich in Ihrem Media Services-Konto befinden, müssen Sie die Konfigurationsvoreinstellung festlegen.  Die folgende Tabelle erläutert die benutzerdefinierten Felder:

| Feld | BESCHREIBUNG |
| --- | --- |
| StartFrame |Der Frame, bei dem die Microsoft Hyperlapse-Verarbeitung beginnen soll. |
| NumFrames |Die Anzahl der zu verarbeitenden Frames. |
| Geschwindigkeit |Der Faktor, mit dem das Eingabevideo beschleunigt werden soll. |

Es folgt ein Beispiel einer geeigneten Konfigurationsdatei in XML und JSON:

**XML-Voreinstellung:**
```xml
    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
        <Sources>
            <Source StartFrame="0" NumFrames="10000" />
        </Sources>
        <Options>
            <Speed>12</Speed>
        </Options>
    </Preset>
```

**JSON-Voreinstellung:**
```json
    {
        "Version":1.0,
        "Sources": [
            {
                "StartFrame":0,
                "NumFrames":2147483647
            }
        ],
        "Options": {
            "Speed":1,
            "Stabilize":false
        }
    }
```

### <a id="sample_code"></a> Microsoft Hyperlapse mit dem AMS .NET SDK
Mit der folgenden Methode werden eine Mediendatei als Asset hochgeladen und ein Auftrag mit dem Azure Media Hyperlapse-Medienprozessor erstellt.

> [!NOTE]
> Sie sollten bereits über einen CloudMediaContext verfügen, der den Namen „Context“ enthält, damit dieser Code funktioniert.  Weitere Informationen dazu finden Sie im [Content Management-Artikel](media-services-dotnet-get-started.md).
> 
> [!NOTE]
> Das Zeichenfolgenargument „HyperConfig“ muss (wie zuvor beschrieben) eine geeignete Konfigurationsvoreinstellung in XML oder JSON sein.
> 
> 

```csharp
        static bool RunHyperlapseJob(string input, string output, string hyperConfig)
        {
            // create asset with input file
            IAsset asset = context
            .Assets
            .CreateAssetAndUploadSingleFile(input, "My Hyperlapse Input", AssetCreationOptions.None);

            // grab instances of Azure Media Hyperlapse MP
            IMediaProcessor mp = context
            .MediaProcessors
            .GetLatestMediaProcessorByName("Azure Media Hyperlapse");

            // create Job with Hyperlapse task
            IJob job = context
            .Jobs
            .Create(String.Format("Hyperlapse {0}", input));

            if (String.IsNullOrEmpty(hyperConfig))
            {
            // config cannot be empty
            return false;
            }

            hyperConfig = File.ReadAllText(hyperConfig);

            ITask hyperlapseTask = job.Tasks.AddNew("Hyperlapse task",
            mp,
            hyperConfig,
            TaskOptions.None);
            hyperlapseTask.InputAssets.Add(asset);
            hyperlapseTask.OutputAssets.AddNew("Hyperlapse output",
            AssetCreationOptions.None);

            job.Submit();

            // Create progress printing and querying tasks
            Task progressPrintTask = new Task(() =>
            {

            IJob jobQuery = null;
            do
            {
                var progressContext = context;
                jobQuery = progressContext.Jobs
                .Where(j => j.Id == job.Id)
                .First();
                Console.WriteLine(string.Format("{0}\t{1}\t{2}",
                DateTime.Now,
                jobQuery.State,
                jobQuery.Tasks[0].Progress));
                Thread.Sleep(10000);
            }
            while (jobQuery.State != JobState.Finished &&
                                   jobQuery.State != JobState.Error &&
                                   jobQuery.State != JobState.Canceled);
                });
                
            progressPrintTask.Start();

            Task progressJobTask = job.GetExecutionProgressTask(
                                                 CancellationToken.None);
            progressJobTask.Wait();

            // If job state is Error, the event handling
            // method for job progress should log errors.  Here we check
            // for error state and exit if needed.
            if (job.State == JobState.Error)
            {
                ErrorDetail error = job.Tasks.First().ErrorDetails.First();
                Console.WriteLine(string.Format("Error: {0}. {1}",
                                                error.Code,
                                                error.Message));  
                return false;                  
            }

        DownloadAsset(job.OutputMediaAssets.First(), output);
        return true;
    }

    static void DownloadAsset(IAsset asset, string outputDirectory)
    {
        foreach (IAssetFile file in asset.AssetFiles)
        {
            file.Download(Path.Combine(outputDirectory, file.Name));
        }
    }


    static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
    {
        IAsset asset = context.Assets.Create(assetName, options);

        var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
        assetFile.Upload(filePath);

        return asset;
    }

    static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = context.MediaProcessors
        .Where(p => p.Name == mediaProcessorName)
        .ToList()
        .OrderBy(p => new Version(p.Version))
        .LastOrDefault();

        if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor",
                                                       mediaProcessorName));

        return processor;
    }
```

### <a id="file_types"></a>Unterstützte Dateitypen
* MP4
* MOV
* WMV

## <a name="media-services-learning-paths"></a>Media Services-Lernpfade
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>Verwandte Links
[Azure Media Services Analytics – Übersicht](media-services-analytics-overview.md)

[Azure Media Analytics-Demos](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

