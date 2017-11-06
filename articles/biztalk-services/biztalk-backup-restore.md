---
title: "Tworzenie i przywracanie kopii zapasowej w usługach BizTalk | Dokumentacja firmy Microsoft"
description: "Usługi BizTalk Services zawiera kopii zapasowej i przywracania. Informacje o sposobie tworzenia i przywracania kopii zapasowej, aby ustalić kopiami zapasowymi. MABS, WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 59f91173-4683-48df-abd5-41262bfce6df
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: c55d1ab124441c42101b4ad60924a9ea28231408
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="biztalk-services-backup-and-restore"></a>BizTalk Services: Backup and Restore (Usługa BizTalk Services: tworzenie kopii zapasowej i przywracanie)

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Usługi BizTalk Azure obejmuje możliwości tworzenia kopii zapasowych i przywracania. W tym temacie opisano, jak utworzyć kopii zapasowej i przywracania usługi BizTalk Services przy użyciu klasycznego portalu Azure.

Można również tworzyć kopie zapasowe przy użyciu usługi BizTalk Services [interfejsu API REST usługi BizTalk](http://go.microsoft.com/fwlink/p/?LinkID=325584). 

> [!NOTE]
> Połączenia hybrydowe nie kopii zapasowej, niezależnie od wersji. Należy ponownie utworzyć połączeń hybrydowych.


## <a name="before-you-begin"></a>Przed rozpoczęciem
* Kopii zapasowej i przywracania może nie być dostępne dla wszystkich wersji. Zobacz [usługi BizTalk Services: wersje wykresu](biztalk-editions-feature-chart.md).
* Przy użyciu klasycznego portalu Azure, możesz utworzyć kopię zapasową na żądanie lub utworzyć zaplanowaną kopię zapasową. 
* Zawartość kopii zapasowej można przywrócić do tej samej usługi BizTalk lub nową usługę BizTalk. Aby przywrócić usługi BizTalk, przy użyciu takiej samej nazwie, należy usunąć istniejącej usługi BizTalk i nazwa musi być dostępna. Po usunięciu usługi BizTalk, może potrwać dłużej, niż żądana dla tej samej nazwie, które mają być dostępne. Jeśli użytkownik nie może czekać na tej samej nazwie, które mają być dostępne, następnie przywróć nową usługę BizTalk.
* Usługi BizTalk Services można przywrócić do tej samej wersji lub nowszą wersję. Przywracanie usługi BizTalk Services do niższej wersji, z po wykonaniu kopii zapasowej, nie jest obsługiwane.
  
    Na przykład można przywrócić kopii zapasowej przy użyciu podstawowych wydanie do wersji Premium. Nie można przywrócić kopii zapasowej przy użyciu wersji Premium do Standard Edition.
* Numery kontroli EDI kopię zapasową do kontynuacji numery kontroli. Wiadomości są przetwarzane po utworzeniu ostatniej kopii zapasowej, przywrócenia tej kopii zapasowej zawartości może spowodować numery zduplikowane kontroli.
* Partia ma aktywne wiadomości, proces partii **przed** wykonywania kopii zapasowej. Podczas tworzenia kopii zapasowej (jako wymagane lub zaplanowane), wiadomości w partiach nigdy nie są przechowywane. 
  
    **Jeśli wykonywana jest kopia zapasowa z active wiadomości w partii, te komunikaty nie kopii zapasowej i w związku z tym zostaną utracone.**
* Opcjonalnie: W portalu usługi BizTalk, Zatrzymaj wszystkie operacje zarządzania.

## <a name="create-a-backup"></a>Tworzenie kopii zapasowej
Kopię zapasową można wykonać w dowolnym momencie i całkowicie jest kontrolowany przez użytkownika. W tej sekcji przedstawiono kroki, aby utworzyć kopie zapasowe przy użyciu klasycznego portalu Azure, w tym:

[Na żądanie kopii zapasowej](#backupnow)

[Planowanie tworzenia kopii zapasowych](#backupschedule)

#### <a name="backupnow"></a>Na żądanie kopii zapasowej
1. W klasycznym portalu Azure, wybierz **usługi BizTalk Services**, a następnie wybierz usługę BizTalk, aby utworzyć kopię zapasową.
2. W **pulpitu nawigacyjnego** wybierz opcję **kopię zapasową** w dolnej części strony.
3. Wprowadź nazwę kopii zapasowej. Na przykład wprowadź *myBizTalkService*BU*data*.
4. Wybierz konto magazynu obiektów blob, a następnie wybierz znacznik wyboru, aby rozpocząć tworzenie kopii zapasowej.

Po wykonaniu kopii zapasowej, kontener z kopii zapasowej wprowadzona nazwa jest tworzony na koncie magazynu. Ten kontener zawiera konfiguracji kopii zapasowej usługi BizTalk.

#### <a name="backupschedule"></a>Planowanie tworzenia kopii zapasowych
1. W klasycznym portalu Azure, wybierz **usługi BizTalk Services**, wybierz nazwę usługi BizTalk, aby zaplanować tworzenie kopii zapasowej, a następnie wybierz **Konfiguruj** kartę.
2. Ustaw **kopię zapasową stanu** do **automatyczne**. 
3. Wybierz **konta magazynu** zapisana kopia zapasowa, wprowadź **częstotliwość** do tworzenia kopii zapasowych, a czas przechowywania kopii zapasowych (**dni przechowywania**):
   
    ![][AutomaticBU]
   
    **Uwagi**     
   
   * W **dni przechowywania**, okres przechowywania musi być większa niż częstotliwość wykonywania kopii zapasowych.
   * Wybierz **zawsze Zachowuj co najmniej jedną kopię zapasową**nawet wtedy, gdy po upływie okresu przechowywania.
4. Wybierz pozycję **Zapisz**.

Po uruchomieniu zaplanowanego zadania tworzenia kopii zapasowej, tworzy kontener (do przechowywania danych kopii zapasowej) na koncie magazynu, który został wprowadzony. Nazwa kontenera o nazwie *usługi BizTalk Nazwa daty i godziny*. 

Jeśli wyświetlane na pulpicie nawigacyjnym usługi BizTalk **niepowodzenie** stanu:

![Stan ostatniej kopii zapasowej według harmonogramu][BackupStatus] 

To łącze powoduje otwarcie dzienniki operacji usług zarządzania do rozwiązywania problemów. Zobacz [usługi BizTalk Services: Rozwiązywanie problemów przy użyciu dzienników operacji](http://go.microsoft.com/fwlink/p/?LinkId=391211).

## <a name="restore"></a>Przywracanie
Możesz przywracać kopie zapasowe, z klasycznego portalu Azure lub [przywrócić usług BizTalk — wersja interfejsu API REST usługi](http://go.microsoft.com/fwlink/p/?LinkID=325582). W tej sekcji wymieniono kroki, aby przywrócić przy użyciu klasycznego portalu.

#### <a name="before-restoring-a-backup"></a>Przed przywróceniem kopii zapasowej
* Nowe śledzenie, archiwizacji i monitorowania magazynów można wprowadzić podczas przywracania usługi BizTalk.
* Te same dane środowiska uruchomieniowego EDI został przywrócony. Kopia zapasowa środowiska uruchomieniowego EDI przechowuje numery kontroli. Numery przywróconej kontroli znajdują się w sekwencji z tworzenia kopii zapasowej. Wiadomości są przetwarzane po utworzeniu ostatniej kopii zapasowej, przywrócenia tej kopii zapasowej zawartości może spowodować numery zduplikowane kontroli.

#### <a name="restore-a-backup"></a>Przywracanie kopii zapasowej
1. W klasycznym portalu Azure, wybierz **nowy** > **usługi aplikacji** > **usługi BizTalk** > **przywrócić**:
   
    ![Przywracanie kopii zapasowej][Restore]
2. W **kopii zapasowej adresu URL**, wybierz ikonę folderu i rozwiń konto magazynu Azure, która przechowuje kopię zapasową konfiguracji usługi BizTalk. Rozwiń kontener i w okienku po prawej stronie, wybierz odpowiedni plik kopii zapasowej .txt. 
   <br/><br/>
   Wybierz **Otwórz**.
3. Na **usługi BizTalk przywracania** wprowadź **nazwa usługi BizTalk** i sprawdź **adresu URL domeny**, **wersji**, i **Region** przywróconej usługi BizTalk. **Utwórz nowe wystąpienie bazy danych SQL** dla bazy danych śledzenia:
   
    ![][RestoreBizTalkService]
   
    Wybierz strzałkę dalej.
4. Sprawdź nazwę bazy danych SQL, wprowadź serwerze fizycznym, w której zostanie utworzona z bazą danych SQL i nazwę użytkownika/hasło dla tego serwera.

    Jeśli chcesz skonfigurować wersji bazy danych SQL, rozmiar i inne właściwości, wybierz **skonfigurować zaawansowane ustawienia bazy danych**. 

    Wybierz strzałkę dalej.

1. Utwórz nowe konto magazynu, lub wprowadź istniejącego konta magazynu dla usługi BizTalk.
2. Wybierz znacznik wyboru, aby rozpocząć przywracanie.

Po pomyślnym zakończeniu przywracania, nową usługę BizTalk znajduje się w stanie wstrzymania na stronie usługi BizTalk Services w klasycznym portalu Azure.

### <a name="postrestore"></a>Po przywróceniu kopii zapasowej
Usługi BizTalk zawsze jest przywracany **zawieszone** stanu. W tym stanie można wprowadzać żadnych zmian konfiguracji, zanim nowe środowisko będzie działać, w tym:

* Jeśli utworzono usługę BizTalk aplikacji przy użyciu zestawu SDK usługi Azure BizTalk, może być konieczne można zaktualizować poświadczeń kontroli dostępu (ACS) w tych aplikacji, aby pracować z przywróconą środowiska.
* Możesz przywrócić usługi BizTalk do replikowania istniejącego środowiska usługi BizTalk. W takiej sytuacji, jeśli są skonfigurowane w portalu usługi BizTalk Services oryginalnej Umowy, korzystających z folderu źródłowego FTP, może być konieczne Aktualizacja umów w środowisku nowo przywrócony do korzystania z folderu FTP innego źródła. W przeciwnym razie może być dwóch różnych umów próby pobierać ten sam komunikat.
* Jeśli przywracany mają wiele środowisk usługi BizTalk, upewnij się, że poprawne środowisko w aplikacji Visual Studio, poleceń cmdlet programu PowerShell, interfejsów API REST lub handlem partnera OM interfejsów API Management są przeznaczone.
* Jest dobrym rozwiązaniem, aby skonfigurować automatyczne tworzenie kopii zapasowych na nowo przywrócony środowiska usługi BizTalk.

Aby uruchomić usługę BizTalk w klasycznym portalu Azure, wybierz usługę BizTalk przywrócone, a następnie wybierz **Wznów** na pasku zadań. 

## <a name="what-gets-backed-up"></a>Kopiami zapasowymi
Po utworzeniu kopii zapasowej, tworzy kopię zapasową następujących elementów:

<table border="1"> 
<tr bgcolor="FAF9F9">
<th> </th>
<TH>Elementy kopii zapasowej</TH> 
</tr> 
<tr>
<td colspan="2">
 <strong>Portal usług Azure BizTalk</strong></td>
</tr> 
<tr>
<td>Konfiguracja i środowiska wykonawczego</td> 
<td>
<ul>
<li>Szczegóły partnera i profilu</li>
<li>Umowy z partnerami</li>
<li>Niestandardowe zestawy wdrożony</li>
<li>Mostków wdrożony</li>
<li>Certyfikaty</li>
<li>Transformacje wdrożony</li>
<li>Potoki</li>
<li>Szablony utworzony i zapisany w portalu usługi BizTalk</li>
<li>X12 mapowania ST01 i GS01</li>
<li>Numery kontroli (EDI)</li>
<li>Wartości AS2 komunikat Micznych</li>
</ul>
</td>
</tr> 

<tr>
<td colspan="2">
 <strong>Usługi BizTalk Azure</strong></td>
</tr> 
<tr>
<td>Certyfikat SSL</td> 
<td>
<ul>
<li>Dane certyfikatu SSL</li>
<li>Hasło certyfikatu SSL</li>
</ul>
</td>
</tr> 
<tr>
<td>Ustawienia usługi BizTalk</td> 
<td>
<ul>
<li>Liczba jednostek skali</li>
<li>Wersja</li>
<li>Wersja produktu:</li>
<li>Region/centrum danych</li>
<li>Dostęp do usługi kontroli (ACS) w przestrzeni nazw i klucza</li>
<li>Parametry połączenia bazy danych śledzenia</li>
<li>Archiwizowanie parametry połączenia konta magazynu</li>
<li>Monitorowanie parametrów połączenia konta magazynu</li>
</ul>
</td>
</tr> 
<tr>
<td colspan="2">
 <strong>Dodatkowe elementy</strong></td>
</tr> 
<tr>
<td>Śledzenie bazy danych</td> 
<td>Po utworzeniu usługi BizTalk, Szczegóły śledzenia bazy danych zostały wprowadzone, łącznie z serwerem bazy danych SQL Azure i Nazwa śledzenia bazy danych. Baza danych śledzenia nie jest automatycznie kopii zapasowej.
<br/><br/>
<strong>Ważne</strong><br/>
Jeśli baza danych śledzenia zostaje usunięta i odzyskać na potrzeby bazy danych, musi istnieć poprzedniej kopii zapasowej. Jeśli kopia zapasowa nie istnieje, bazy danych śledzenia i jego dane nie są możliwe do odzyskania. W takiej sytuacji należy utworzyć nową bazę danych śledzenia o takiej samej nazwie bazy danych. Replikacja geograficzna jest zalecane.</td>
</tr> 
</table>

## <a name="next"></a>Następne kroki
Aby utworzyć usługi BizTalk Azure w klasycznym portalu Azure, przejdź do [usługi BizTalk Services: klasycznego portalu Azure za pomocą udostępniania](http://go.microsoft.com/fwlink/p/?LinkID=302280). Aby rozpocząć tworzenie aplikacji, przejdź do artykułu [Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=235197) (Usługa Azure BizTalk Services).

## <a name="see-also"></a>Zobacz też
* [Usługi BizTalk kopii zapasowej](http://go.microsoft.com/fwlink/p/?LinkID=325584)
* [Przywracanie z kopii zapasowej usługi BizTalk](http://go.microsoft.com/fwlink/p/?LinkID=325582)
* [Usługi BizTalk Services: Developer, podstawowa, standardowa i Premium Edition wykresu](http://go.microsoft.com/fwlink/p/?LinkID=302279)
* [Usługi BizTalk Services: Klasyczny portal Azure przy użyciu inicjowania obsługi](http://go.microsoft.com/fwlink/p/?LinkID=302280)
* [BizTalk Services: Provisioning Status Chart (Usługa BizTalk Services: aprowizowanie wykresu stanu)](http://go.microsoft.com/fwlink/p/?LinkID=329870)
* [BizTalk Services: Dashboard, Monitor and Scale tabs (Usługa BizTalk Services: karty Pulpit nawigacyjny, Monitor i Skalowanie)](http://go.microsoft.com/fwlink/p/?LinkID=302281)
* [BizTalk Services: Throttling (Usługa BizTalk Services: ograniczanie przepływności)](http://go.microsoft.com/fwlink/p/?LinkID=302282)
* [BizTalk Services: Issuer Name and Issuer Key (Usługa BizTalk Services: nazwa i klucz wydawcy)](http://go.microsoft.com/fwlink/p/?LinkID=303941)
* [Jak rozpocząć pracę z zestawem SDK usługi Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[BackupStatus]: ./media/biztalk-backup-restore/status-last-backup.png
[Restore]: ./media/biztalk-backup-restore/restore-ui.png
[AutomaticBU]: ./media/biztalk-backup-restore/AutomaticBU.png
[RestoreBizTalkService]: ./media/biztalk-backup-restore/RestoreBizTalkServiceWindow.png

