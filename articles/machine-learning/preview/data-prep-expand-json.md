---
title: "Rozwiń węzeł przekształcania JSON przy użyciu usługi Azure Machine Learning Workbench"
description: "W dokumencie referencyjnym transformacji \"Rozwiń JSON\""
services: machine-learning
author: ranvijaykumar
ms.author: ranku
manager: mwinkle
ms.reviewer: 
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, reference
ms.topic: article
ms.date: 09/14/2017
ms.openlocfilehash: 57f461f9a39e8069654985d86bfe3ae23dd0855a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="expand-json-transformation"></a>Rozwiń węzeł przekształcania JSON
**Rozwiń JSON** przekształcenia umożliwia użytkownikom rozwiń istniejąca kolumna zawiera nieprawidłowy tekst JSON do wielu kolumn.

## <a name="how-to-perform-this-transformation"></a>Jak wykonać tego przekształcenia

Wykonaj następujące kroki do wykonania tej transformacji:
1. Wybierz kolumny źródłowej, która zawiera tekst JSON.
2. Wybierz **rozwiń JSON** z **przekształca** menu. Lub kliknij prawym przyciskiem myszy nagłówek kolumny źródłowej i wybierz **rozwiń JSON** z menu kontekstowego. 
3. Kliknij przycisk **OK**. 
 
Obok kolumny źródłowej są dodawane nowe kolumny. Te kolumny zawierają właściwości z jednego poziomu hierarchii w tekście JSON. Klucz właściwości, jeśli jest obecny, służy do tworzenia nazwa odpowiedniej kolumny. Właściwości zagnieżdżone są zachowywane jako tekstu JSON użytkownika wielokrotnie powtarzane można rozwinąć lub odrzucić zgodnie z potrzebami.

## <a name="examples"></a>Przykłady

Columnn źródła *klienta* jest rozwinięta w dwóch kolumnach *Customer.Name* i *Customer.Phone*.

| Klienta                                                | Customer.Name   | Customer.Phone |
|---------------------------------------------------------|-----------------|----------------|
| {"Name": "Carrie Dodson", "Phone": "123-4567-890"}   | Carrie Dodson   | 123-4567-890   |
| {"Name": "Leonard Robledo", "Phone": "456-7890-123"} | Leonard Robledo | 456-7890-123   |

