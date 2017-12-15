---
title: "Azure Analysis Services alias serwerów nazw | Dokumentacja firmy Microsoft"
description: "Opisuje sposób tworzenia i używania aliasów nazwy serwera."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
ms.assetid: 
ms.service: analysis-services
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: owend
ms.openlocfilehash: a947dde1551c653faa54f088c1712c41c7657aa0
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/14/2017
---
# <a name="alias-server-names"></a>Nazwy serwerów aliasu

Przy użyciu alias nazwę serwera, użytkownicy mogą nawiązywać połączenie z serwerem usług Azure Analysis Services o krótszej *alias* zamiast nazwy serwera. Podczas nawiązywania połączenia z aplikacji klienckiej, alias jest określony jako punktu końcowego za pomocą **łącza: / /** format protokołu. Punkt końcowy zwraca nazwę serwera rzeczywistego celu połączenia z.

Alias serwera nazw są odpowiednie dla:

- Migrowanie modele między serwerami bez wpływu na użytkowników. 
- Serwer przyjazne nazwy są łatwiejsze do zapamiętania przez użytkowników. 
- Bezpośrednie użytkownikom na różnych serwerach w różnych porach dnia. 
- Bezpośrednie użytkowników w różnych regionach z wystąpieniami, które są od siebie lokalizacjach geograficznych bliżej, podobnie jak przy użyciu usługi Azure Traffic Manager. 

Punkt końcowy HTTP, która zwraca prawidłową nazwę serwera usług Azure Analysis Services może służyć jako alias.

![Alias przy użyciu formatu łącza](media/analysis-services-alias/aas-alias-browser.png)

Podczas nawiązywania połączenia z klientem, nazwę serwera alias jest wprowadzane przy użyciu **łącza: / /** format protokołu. Na przykład w programie Power BI Desktop:

![Power BI Desktop połączenia](media/analysis-services-alias/aas-alias-connect-pbid.png)

## <a name="create-an-alias"></a>Tworzenie aliasu

Aby utworzyć alias punktu końcowego, używając dowolnej metody, która zwraca prawidłową nazwę serwera usług Azure Analysis Services. Na przykład odwołania do pliku w magazynie obiektów Blob Azure zawierających rzeczywistych server name, lub tworzenie i publikowanie aplikacji formularzy sieci Web ASP.NET.

W tym przykładzie w Visual Studio zostanie utworzona aplikacja formularzy sieci Web ASP.NET. Formant odwołania i użytkownika strony wzorcowej są usuwane z Default.aspx strony. Zawartość Default.aspx jest po prostu następujące dyrektywy strony:

```
<%@ Page Title="Home Page" Language="C#" AutoEventWireup="true" CodeBehind="Default.aspx.cs" Inherits="FriendlyRedirect._Default" %>
```

Zdarzenie Page_Load w Default.aspx.cs używa metody metody Response.Write(), aby zwrócić nazwę serwera usług Azure Analysis Services.

```
protected void Page_Load(object sender, EventArgs e)
{
    this.Response.Write("asazure://<region>.asazure.windows.net/<servername>");
}
```

## <a name="see-also"></a>Zobacz też

[Biblioteki klienta](analysis-services-data-providers.md)   
[Nawiązywanie połączenia z Power BI Desktop](analysis-services-connect-pbi.md)
