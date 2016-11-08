---
title: Licencjonowanie usługi Azure RemoteApp | Microsoft Docs
description: Dowiedz się, jak działa licencjonowanie w usłudze Azure RemoteApp.
services: remoteapp
documentationcenter: ''
author: lizap
manager: mbaldwin

ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/15/2016
ms.author: elizapo

---
# Jak działa licencjonowanie w usłudze Azure RemoteApp?
> [!IMPORTANT]
> Usługa Azure RemoteApp nie jest już obsługiwana. Szczegółowe informacje zawiera [powiadomienie](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Usługa Azure RemoteApp została skonfigurowana, utworzono szablony i wszystko jest gotowe do opublikowania aplikacji dla użytkowników. Pozostała jednak ostatnia rzecz do ustalenia: licencjonowanie. Jak działa licencjonowanie usługi RemoteApp i aplikacji udostępnianych za pośrednictwem usługi RemoteApp?

Usługa RemoteApp nie wymaga licencji dostępowych systemu Windows ani pulpitu zdalnego. Twoja subskrypcja sama dba o usługę RemoteApp. (Sprawdź szczegóły [planów cenowych](https://azure.microsoft.com/pricing/details/remoteapp)).

Jeśli używany jest jeden z obrazów dołączonych w ramach subskrypcji, można udostępnić dowolne aplikacje zainstalowane w tym obrazie bez konieczności posiadania oddzielnej licencji. Na przykład, jeśli używasz obrazu szablonu systemu Windows Server 2012 R2 do tworzenia kolekcji, możesz udostępniać użytkownikom program System Center Endpoint Protection. Jedyny wyjątek od tej reguły stanowi usługa Office 365 ProPlus, która wymaga oddzielnej subskrypcji, oraz pakiet Office 2013, którego nie można udostępniać w kolekcji dla środowiska produkcyjnego.

Jeśli chcesz używać obrazu szablonu usługi Office 365 dołączanego do usługi RemoteApp, musisz posiadać *aktualny* plan usługi Office 365 ProPlus. Dotyczy to dowolnych aplikacji usługi Office 365 opublikowanych przy użyciu szablonu niestandardowego. Aplikacje należy aktywować przy użyciu posiadanej subskrypcji. Dotyczy to zarówno wersji próbnych, jak i subskrypcji płatnych. Jeśli chcesz korzystać z obrazu szablonu usługi Office 365 w okresie próbnym *i nie masz jeszcze subskrypcji*, przejdź do strony usługi Office 365, aby [zarejestrować się](https://go.microsoft.com/fwlink/p/?LinkID=403802) w celu uzyskania subskrypcji wersji próbnej. Aby znaleźć więcej informacji, zobacz temat [Jak programy pakietu Office współpracują z usługą RemoteApp?](remoteapp-o365.md).

Jeśli podczas okresu próbnego nie chcesz uzyskać subskrypcji wersji próbnej usługi Office 365, możesz użyć obrazu szablonu pakietu Office 2013 Professional Plus, który jest dostarczany z usługą RemoteApp. Ten obraz szablonu może być używany tylko przez 30 dni i nie można go konwertować na kolekcję płatną.

W przypadku innych aplikacji musisz upewnić się, że masz licencję na udostępnienie aplikacji.

Ma to sens, prawda? Możesz opublikować dowolną aplikację, którą zgodnie z prawem możesz udostępniać. Należy upewnić się, że naprawdę możesz udostępniać swoje programy.

Należy pamiętać, że w przypadku kolekcji w chmurze nie można użyć umowy licencji zbiorczej ani licencji dostępowej. *Możesz* jednak aktywować aplikacje w kolekcji hybrydowej (z wyjątkiem usługi Office) przy użyciu umowy licencji zbiorczej. Wystarczy zainstalować je w obrazie szablonu z nośnika licencji zbiorczej. Postępuj zgodnie z informacjami otrzymanymi od dostawcy aplikacji w celu zainstalowania licencji w środowisku pulpitu zdalnego.

<!--HONumber=Sep16_HO3-->


