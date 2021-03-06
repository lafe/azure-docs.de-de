---
title: Extrahieren von Text mit OCR – Maschinelles Sehen
titleSuffix: Azure Cognitive Services
description: Konzepte im Zusammenhang mit dem Extrahieren von Text mittels optischer Zeichenerkennung (OCR) unter Verwendung der Maschinelles Sehen-API.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 0f43724218994818908e87834ed1b70f4bca330b
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55873801"
---
# <a name="extracting-text-with-optical-character-recognition"></a>Extrahieren von Text mit optischer Zeichenerkennung

Die optische Zeichenerkennungstechnologie (OCR) beim maschinellen Sehen erkennt Textinhalte in einem Bild und extrahiert den erkannten Text in einen computerlesbaren Zeichendatenstrom. Sie können das Ergebnis für die Suche und zahlreiche andere Zwecke wie medizinische Aufzeichnungen, Sicherheit und das Bankwesen verwenden. Die Sprache wird automatisch erkannt. OCR spart Zeit und erleichtert Benutzern das Leben, da sie den Text nicht mehr transkribieren müssen, sondern ihn fotografieren können.

OCR unterstützt 25 Sprachen. Diese Sprachen sind: Arabisch, Chinesisch (traditionell), Chinesisch (vereinfacht), Dänisch, Deutsch, Englisch, Finnisch, Französisch, Griechisch, Italienisch, Japanisch, Koreanisch, Niederländisch, Norwegisch, Polnisch, Portugiesisch, Rumänisch, Russisch, Schwedisch, Serbisch (kyrillisch und lateinisch), Slowakisch, Spanisch, Tschechisch, Türkisch und Ungarisch.

Bei Bedarf korrigiert OCR die Drehung des erkannten Texts in Grad um die horizontale Bildachse. OCR stellt die Frame-Koordinaten jedes Worts bereit, wie es in der folgenden Abbildung dargestellt ist.

![Diagramm, das zeigt, wie ein Bild gedreht und der darauf befindliche Text gelesen und eingegrenzt wird](./Images/vision-overview-ocr.png)

## <a name="ocr-requirements"></a>OCR-Anforderungen

Maschinelles Sehen kann Text mithilfe von OCR aus Bildern extrahieren, die folgende Anforderungen erfüllen:

* Das Bild muss im JPEG-, PNG-, GIF- oder BMP-Format vorliegen.
* Die Größe des Eingabebilds muss zwischen 50 x 50 und 4200 x 4200 Pixel liegen.


Das Eingabebild kann um ein beliebiges Vielfaches von 90 Grad plus einem kleinen Winkel von bis zu 40 Grad gedreht sein.

## <a name="improving-ocr-accuracy"></a>Verbesserung der OCR-Genauigkeit

Die Genauigkeit der Texterkennung hängt von der Qualität des Bilds ab. Ein ungenaues Lesen kann durch Folgendes verursacht werden:

* Unscharfe Bilder
* Handschriftlicher oder kursiver Text
* Künstlerische Schriftarten
* Geringe Textgröße
* Komplexe Hintergründe, Schatten oder Blendung über dem Text oder eine perspektivische Verzerrung
* Übergroße oder fehlende Großbuchstaben am Anfang von Wörtern
* Tiefgestellter, hochgestellter oder durchgestrichener Text

### <a name="ocr-limitations"></a>OCR-Einschränkungen

Bei Fotos, auf denen Text dominiert, kann es durch teilweise erkannte Wörter zu falsch positiven Ergebnissen kommen. Bei einigen Fotos, insbesondere bei Fotos ohne Text, kann die Genauigkeit je nach Bildtyp sehr stark variieren.

## <a name="next-steps"></a>Nächste Schritte

Lernen Sie Konzepte zum [Erkennen von gedrucktem und handschriftlichem Text](concept-recognizing-text.md) kennen.
