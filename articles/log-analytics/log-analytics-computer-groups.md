---
title: "Grupy komputerów w analizy dzienników dziennika wyszukiwania | Dokumentacja firmy Microsoft"
description: "Grupy komputerów w analizy dzienników umożliwiają zakres wyszukiwania dziennika do określonego zestawu komputerów.  W tym artykule opisano różne metody używanej do tworzenia grup komputerów i sposobu ich używania w wyszukiwaniu dziennika."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: 
ms.assetid: a28b9e8a-6761-4ead-aa61-c8451ca90125
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/02/2017
ms.author: bwren
ms.openlocfilehash: 17a59a38b6a445a7f42df171a711669f95fc84c2
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/04/2017
---
# <a name="computer-groups-in-log-analytics-log-searches"></a>Zaloguj się wyszukiwanie grup komputerów w analizy dzienników

Grupy komputerów w analizy dzienników umożliwiają zakresu [dziennika wyszukiwania](log-analytics-log-search-new.md) do określonego zestawu komputerów.  Każda grupa jest wypełniana z komputerami za pomocą zapytanie, które należy zdefiniować lub importując grup z różnych źródeł.  Podczas wyszukiwania dziennika zawiera grupy, wyniki są ograniczone do rekordów, które spełniają komputerów w grupie.

## <a name="creating-a-computer-group"></a>Tworzenie grupy komputerów
Można utworzyć grupę komputerów w analizy dzienników przy użyciu dowolnej z metod w poniższej tabeli.  W poniższych sekcjach znajdują się szczegółowe informacje o każdej z metod. 

| Metoda | Opis |
|:--- |:--- |
| Wyszukiwanie w dzienniku |Utwórz wyszukiwania dziennika, które zwraca listę komputerów. |
| Interfejs API wyszukiwania w dzienniku |Programowe tworzenie grupy komputerów, na podstawie wyników wyszukiwania dziennika za pomocą interfejsu API wyszukiwania dziennika. |
| Usługa Active Directory |Automatyczne skanowanie członkostwo w grupie na wszystkich komputerach agenta, które są członkami domeny usługi Active Directory i utworzyć grupę w analizy dzienników dla każdej grupy zabezpieczeń. |
| Configuration Manager | Zaimportować kolekcje w programie System Center Configuration Manager i utworzyć grupę w analizy dzienników dla każdego. |
| Windows Server Update Services |Skanowanie serwerów WSUS lub klientów do użycia z grup i automatycznie utworzyć grupę w analizy dzienników dla każdego. |

### <a name="log-search"></a>Wyszukiwanie w dzienniku
Grupy komputerów utworzone na podstawie wyszukiwania dziennika zawiera wszystkie komputery zwróconych przez zapytanie, które należy zdefiniować.  To zapytanie jest uruchamiane za każdym razem, grupa komputerów jest używany tak, aby wszelkie zmiany, ponieważ grupa została utworzona ta jest uwzględniana.  

Każde zapytanie można używać dla grupy komputerów, ale aplikacja musi zwracać różne zestaw komputerów przy użyciu `distinct Computer`.  Poniżej znajduje się wyszukiwanie typowym przykładem, które można użyć jako grupę komputerów.

    Heartbeat | where Computer contains "srv" | distinct Computer

W poniższej tabeli opisano właściwości, które definiują grupę komputerów.

| Właściwość | Opis |
|:---|:---|
| Nazwa wyświetlana   | Nazwa wyszukiwania, które mają być wyświetlane w portalu. |
| Kategoria       | Kategoria do organizowania wyszukiwania w portalu. |
| Zapytanie          | Zapytanie dla grupy komputerów. |
| Alias — funkcja | Unikatowego aliasu używany do identyfikowania grupy komputerów w zapytaniu. |

Poniższa procedura umożliwia utworzenie grupy komputerów z dziennika wyszukiwania w portalu Azure.

2. Otwórz **wyszukiwania dziennika** , a następnie kliknij przycisk **zapisane wyszukiwania** w górnej części ekranu.
3. Kliknij przycisk **Dodaj** i podaj wartości dla każdej właściwości grupy komputerów.
4. Wybierz **Zapisz to zapytanie jako grupę komputerów** i kliknij przycisk **OK**.


Poniższa procedura umożliwia utworzenie grupy komputerów z dziennika wyszukiwania w portalu OMS.

1. Otwórz **wyszukiwania dziennika** i utworzyć wyszukiwania dziennika dla grupy komputerów.  
2. Kliknij przycisk **zapisać** przycisk w górnej części ekranu.
3. Wybierz **tak** do **Zapisz to zapytanie jako grupę komputerów**.
5. Podaj wartość dla każdej właściwości grupy komputerów. 


>[!NOTE]
> Jeśli nadal używa obszaru roboczego [języka zapytań starszej wersji analizy dzienników](log-analytics-log-search-upgrade.md) , a następnie użyć tej samej procedury tworzenia grupy komputerów, ale należy użyć składni języka kwerend starszej wersji.


### <a name="log-search-api"></a>Dziennik wyszukiwania interfejsu API
Grupy komputerów utworzone za pomocą interfejsu API Search dziennika są takie same jak wyszukiwania utworzonych za pomocą wyszukiwania dziennika.  Aby uzyskać więcej informacji na temat tworzenia grupy komputerów przy użyciu interfejsu API Search dziennika zobacz [grup komputerów w dzienniku analizy dzienników wyszukiwania interfejsu API REST](log-analytics-log-search-api.md#computer-groups).

### <a name="active-directory"></a>Usługa Active Directory
Podczas konfigurowania analizy dzienników do zaimportowania członkostwa w grupach usługi Active Directory, analizuje członkostwo w grupie komputerów przyłączonych do domeny z agentem pakietu OMS.  Grupa komputerów jest tworzony w analizy dzienników dla każdej grupy zabezpieczeń w usłudze Active Directory, a każdy komputer jest dodawany do odpowiadającego do grupy zabezpieczeń, które są oni członkami grupy komputerów.  To członkostwo jest stale aktualizowany co 4 godziny.  

Konfigurowanie analizy dzienników do zaimportowania grup zabezpieczeń usługi Active Directory z analizy dzienników **Zaawansowane ustawienia** w portalu Azure.  Wybierz **grup komputerów**, **usługi Active Directory**, a następnie **członkostwa w grupach usługi Active Directory importu z komputerów**.  Nie są wymagane żadne dalsze czynności konfiguracyjne.

![Grupy komputerów z usługi Active Directory](media/log-analytics-computer-groups/configure-activedirectory.png)

Grupy zostały zaimportowane, menu znajdują się liczba komputerów z członkostwem grupy wykryte i liczby grup zaimportowane.  Możesz kliknąć jeden z poniższych linków, aby zwrócić **Grupa_komputerów** rekordy z tymi informacjami.

### <a name="windows-server-update-service"></a>Windows Server Update Service
Podczas konfigurowania analizy dzienników do zaimportowania członkostwa w grupach usług WSUS, analizuje docelowych członkostwo w grupie komputerów z agentem pakietu OMS.  Jeśli używasz klienta przeznaczonych dla dowolnego komputera, który jest połączony z usługą OMS i jest częścią żadnych WSUS przypisywania grup ma jego przynależność zaimportowany do analizy dzienników. Jeśli używasz po stronie serwera celem OMS powinien być zainstalowany agent na serwerze programu WSUS w taki sposób, aby informacje o członkostwie grupy do zaimportowania z usługą OMS.  To członkostwo jest stale aktualizowany co 4 godziny. 

Konfigurowanie analizy dzienników do zaimportowania grup usług WSUS z analizy dzienników **Zaawansowane ustawienia** w portalu Azure.  Wybierz **grup komputerów**, **WSUS**, a następnie **importu programu WSUS członkostw**.  Nie są wymagane żadne dalsze czynności konfiguracyjne.

![Grupy komputerów programu WSUS](media/log-analytics-computer-groups/configure-wsus.png)

Grupy zostały zaimportowane, menu znajdują się liczba komputerów z członkostwem grupy wykryte i liczby grup zaimportowane.  Możesz kliknąć jeden z poniższych linków, aby zwrócić **Grupa_komputerów** rekordy z tymi informacjami.

### <a name="system-center-configuration-manager"></a>System Center Configuration Manager
Podczas konfigurowania analizy dzienników do Importuj członkostwa w kolekcji programu Configuration Manager tworzy grupy komputerów dla każdej kolekcji.  Informacje dotyczące członkostwa kolekcji jest pobierana co 3 godziny, aby zapewnić aktualność grupy komputerów. 

Przed zaimportowaniem kolekcji programu Configuration Manager, należy najpierw [łączenia programu Configuration Manager do analizy dzienników](log-analytics-sccm.md).  Następnie można skonfigurować Importuj z analizy dzienników **Zaawansowane ustawienia** w portalu Azure.  Wybierz **grup komputerów**, **SCCM**, a następnie **członkostwa w kolekcji programu Configuration Manager importu**.  Nie są wymagane żadne dalsze czynności konfiguracyjne.

![Grupy komputerów z programu SCCM](media/log-analytics-computer-groups/configure-sccm.png)

Kolekcje zostały zaimportowane, menu znajdują się liczba komputerów z członkostwem grupy wykryte i liczby grup zaimportowane.  Możesz kliknąć jeden z poniższych linków, aby zwrócić **Grupa_komputerów** rekordy z tymi informacjami.

## <a name="managing-computer-groups"></a>Zarządzanie grupami komputerów
Możesz wyświetlić grupy komputerów, które zostały utworzone na podstawie wyszukiwania dziennika lub interfejsu API dziennika wyszukiwania z analizy dzienników **Zaawansowane ustawienia** w portalu Azure.  Wybierz **grup komputerów** , a następnie **zapisane grupy**.  

Kliknij przycisk **x** w **Usuń** kolumna usunąć grupę komputerów.  Kliknij przycisk **Wyświetlanie członków** ikona grupy do uruchamiania grupy dziennik wyszukiwania, które zwraca jej elementów członkowskich.  Nie można zmodyfikować grupy komputerów, ale zamiast tego należy usunąć i ponownie utworzyć przy użyciu zmodyfikowanych ustawień.

![Grupy komputerów zapisane](media/log-analytics-computer-groups/configure-saved.png)


## <a name="using-a-computer-group-in-a-log-search"></a>W wyszukiwaniu dziennika przy użyciu grupy komputerów
Możesz użyć grupy komputerów utworzone na podstawie wyszukiwania dziennika w zapytaniu traktując jej aliasem jako funkcja, zwykle przy użyciu następującej składni:

  `Table | where Computer in (ComputerGroup)`

Na przykład można użyć następujących do zwrócenia UpdateSummary rekordów tylko komputery należące do grupy komputerów o nazwie mycomputergroup.
 
  `UpdateSummary | where Computer in (mycomputergroup)`


Zaimportowany komputer grup i komputerów uwzględnione są przechowywane w **Grupa_komputerów** tabeli.  Na przykład poniższe zapytanie zwróci listę komputerów w grupie komputerów w domenie z usługi Active Directory. 

  `ComputerGroup | where GroupSource == "ActiveDirectory" and Group == "Domain Computers" | distinct Computer`

Następujące zapytanie zwróci UpdateSummary rekordów tylko komputery w domenie komputerach.

  ```
  let ADComputers = ComputerGroup | where GroupSource == "ActiveDirectory" and Group == "Domain Computers" | distinct Computer;
  UpdateSummary | where Computer in (ADComputers)
  ```



  

>[!NOTE]
> Jeśli nadal używa obszaru roboczego [języka zapytań starszej wersji analizy dzienników](log-analytics-log-search-upgrade.md)>, a następnie użyj następującej składni w odwołaniu do grupy komputerów w wyszukiwaniu dziennika.  Określanie **kategorii** > jest opcjonalny i tylko wymagane, jeśli masz grupy komputerów o takiej samej nazwie w różnych kategoriach. 
>
>    `$ComputerGroups[Category: Name]`
>
>Grupy komputerów są zazwyczaj używane z **IN** klauzuli w wyszukiwania dziennika, jak w poniższym przykładzie:
>
>    `Type=UpdateSummary Computer IN $ComputerGroups[My Computer Group]`



## <a name="computer-group-records"></a>Rekordów grupy komputerów
Zostaje utworzony rekord w repozytorium OMS dla każdego członkostwa grup komputerów utworzone na podstawie usługi Active Directory lub usług WSUS.  Te rekordy mieć typu **Grupa_komputerów** i mieć właściwości w poniższej tabeli.  Rekordy nie są tworzone dla grup komputerów opartych na dziennik wyszukiwania.

| Właściwość | Opis |
|:--- |:--- |
| Typ |*Grupa_komputerów* |
| SourceSystem |*SourceSystem* |
| Computer (Komputer) |Nazwa komputera członkowskiego. |
| Grupa |Nazwa grupy. |
| GroupFullName |Pełna ścieżka do grupy, takie jak źródło i nazwa źródła. |
| GroupSource |Źródło tej grupy zostały zebrane z. <br><br>Polecenie cmdlet<br>PROGRAM WSUS<br>WSUSClientTargeting |
| GroupSourceName |Nazwa źródła, które zostały zebrane grupy.  Dla usługi Active Directory jest to nazwa domeny. |
| ManagementGroupName |Nazwa grupy zarządzania agentów SCOM.  W przypadku innych agentów jest AOI -\<identyfikator obszaru roboczego\> |
| TimeGenerated |Data i godzina utworzenia lub aktualizacji grupy komputerów. |

## <a name="next-steps"></a>Następne kroki
* Dowiedz się więcej o [dziennika wyszukiwania](log-analytics-log-searches.md) analizować dane zebrane ze źródeł danych i rozwiązania.  

