---
title: Hinzufügen einer Symbolebene zu Azure Maps | Microsoft-Dokumentation
description: Hinzufügen einer Symbolebene zur JavaScript-Karte
author: rbrundritt
ms.author: richbrun
ms.date: 12/2/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 718a679418790a6bf1207a96e5c204f7962de239
ms.sourcegitcommit: 82cdc26615829df3c57ee230d99eecfa1c4ba459
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2019
ms.locfileid: "54411253"
---
# <a name="add-a-symbol-layer-to-a-map"></a>Hinzufügen einer Symbolebene zu einer Karte

In diesem Artikel erfahren Sie, wie Sie Punktdaten aus einer Datenquelle als Symbolebene auf einer Karte rendern können. Symbolebenen werden mit WebGL gerendert und unterstützen deutlich mehr Datenpunkte als HTML-Marker. Sie unterstützen jedoch keine herkömmlichen CSS- und HTML-Elemente für die Formatierung.  

> [!TIP]
> Symbolebenen rendern in der Standardeinstellung die Koordinaten aller Geometrien in einer Datenquelle. Legen Sie die Eigenschaft `filter` der Ebene auf `['==', '$type', 'Point']` fest, um die Ebene dahingehend zu beschränken, dass nur Punktgeometriefunktionen gerendert werden.

## <a name="add-a-symbol-layer"></a>Hinzufügen einer Symbolebene

<iframe height='500' scrolling='no' title='Ändern der Stecknadel eines Standorts' src='//codepen.io/azuremaps/embed/ZqJjRP/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter <a href='https://codepen.io/azuremaps/pen/ZqJjRP/'>Ändern der Stecknadel eines Standorts</a> in Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

Mit dem ersten Block des obigen Codes wird ein Kartenobjekt erstellt. Eine Anleitung finden Sie unter [Erstellen einer Karte](./map-create.md).

Im zweiten Codeblock wird mit der Klasse [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) ein Datenquellenobjekt erstellt. Ein [Feature] mit einer [Punkt](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest)geometrie wird von der [Shape](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest)-Klasse umschlossen, um die Aktualisierung zu vereinfachen, und wird anschließend erstellt und zur Datenquelle hinzugefügt.

Der dritte Codeblock erstellt einen [Ereignislistener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) und aktualisiert die Koordinaten des Punktes bei Mausklick mit der [setCoordinates](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest)-Methode der Shape-Klasse.

Eine [Symbolebene](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) verwendet Text oder Symbole zum Rendern punktbasierter Daten, die in [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) als Symbole auf der Karte umschlossen sind.  Die Datenquelle, der Click-Ereignislistener und die Symbolebene werden erstellt und der Karte innerhalb der Funktion [Ereignislistener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) hinzugefügt, damit der Punkt angezeigt wird, nachdem die Karte vollständig geladen wurde.

## <a name="add-a-custom-icon-to-a-symbol-layer"></a>Hinzufügen eines benutzerdefinierten Zeichens zu einer Symbolebene

Symbolebenen werden mit WebGL gerendert. Daher müssen alle Ressourcen wie etwa Zeichenbilder in den WebGL-Kontext geladen werden. Dieses Beispiel zeigt, wie ein benutzerdefiniertes Zeichensymbol zu den Kartenressourcen hinzugefügt und anschließend zum Rendern von Punktdaten mit einem benutzerdefinierten Symbol auf der Karte verwendet wird. Die `textField`-Eigenschaft der Symbolebene erfordert, dass ein Ausdruck angegeben wird. In diesem Fall möchten wir die Temperatureigenschaft des Punktfeatures als Textwert rendern. Dies ist mit dem folgenden Ausdruck möglich: `['get', 'temperature']`. 

<br/>

<iframe height='500' scrolling='no' title='Benutzerdefiniertes Symbolbildzeichen' src='//codepen.io/azuremaps/embed/WYWRWZ/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/WYWRWZ/'>Custom Symbol Image Icon</a> (Benutzerdefiniertes Symbolbildzeichen) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="customize-a-symbol-layer"></a>Anpassen einer Symbolebene 

Die Symbolebene bietet zahlreiche Optionen für die Formatierung. Im Folgenden finden Sie ein Tool, mit dem Sie diese verschiedenen Formatoptionen testen können.

<br/>

<iframe height='700' scrolling='no' title='Symbolebenenoptionen' src='//codepen.io/azuremaps/embed/PxVXje/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/PxVXje/'>Symbol Layer Options</a> (Symbolebenenoptionen) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zu den in diesem Artikel verwendeten Klassen und Methoden:

> [!div class="nextstepaction"]
> [SymbolLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [SymbolLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.symbollayeroptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [IconOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.iconoptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [TexTOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.textoptions?view=azure-iot-typescript-latest)

In den folgenden Artikeln finden Sie weitere Codebeispiele, die Sie Ihren Karten hinzufügen können:

> [!div class="nextstepaction"]
> [Hinzufügen eines Popups](./map-add-popup.md)

> [!div class="nextstepaction"]
> [Hinzufügen einer Form](./map-add-shape.md)

> [!div class="nextstepaction"]
> [Hinzufügen einer Blasenebene](./map-add-bubble-layer.md)

> [!div class="nextstepaction"]
> [Hinzufügen von HTML-Markern](./map-add-bubble-layer.md)
