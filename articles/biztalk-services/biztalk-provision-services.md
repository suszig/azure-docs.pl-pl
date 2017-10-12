---
title: "Tworzenie usługi Azure BizTalk Services w witrynie Azure Portal | Microsoft Docs"
description: "Informacje o inicjowaniu obsługi lub tworzeniu usługi Azure BizTalk Services w portalu Azure; MABS, WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 3ad18876-a649-40d6-9aa0-1509c1d62c43
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: eca77b4a82eb67e1755717bb4429f8d450a64dc5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-biztalk-services-using-the-azure-portal"></a>Tworzenie usługi BizTalk Services przy użyciu portalu Azure

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]


> [!TIP]
> Do zalogowania się do portalu Azure potrzebne jest konto Azure i subskrypcja platformy Azure. Jeśli nie masz konta, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Zobacz [Bezpłatna wersja próbna platformy Azure](http://go.microsoft.com/fwlink/p/?LinkID=239738).


## <a name="CreateService"></a>Tworzenie usługi BizTalk
W zależności od wybranej wersji nie wszystkie ustawienia usługi BizTalk mogą być dostępne.

1. Zaloguj się do [portalu Azure](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. W dolnym okienku nawigacji wybierz przycisk **NOWE**:  
   ![Wybieranie przycisku Nowe][NEWButton]
3. Wybierz kolejno opcje **USŁUGI APLIKACJI** > **USŁUGA BIZTALK** > **TWORZENIE NIESTANDARDOWE**:  
   ![Wybieranie kolejno opcji Usługa BizTalk i Tworzenie niestandardowe][NewBizTalkService]
4. Wprowadź ustawienia usługi BizTalk:
   
    <table border="1">
    <tr>
    <td><strong>Nazwa usługi BizTalk</strong></td>
    <td>Można wprowadzić dowolną nazwę, ale musi ona być dokładna. Oto niektóre przykłady:<br/><br/>
    <em>mojafirma</em>.biztalk.windows.net<br/>
    <em>mojafirmamojaaplikacja</em>.biztalk.windows.net<br/>
    <em>mojaaplikacja</em>.biztalk.windows.net<br/><br/>Do wprowadzanej nazwy zostanie automatycznie dodany element „.biztalk.windows.net”. Spowoduje to utworzenie adresu URL służącego do uzyskania dostępu do usługi BizTalk, np. <strong>https://<em>mojaaplikacja</em>.biztalk.windows.net</strong>.
    </td>
    </tr>
    <tr>
    <td><strong>Wersja</strong></td>
    <td>Jeśli jesteś w fazie testowania/programowania, wybierz opcję <strong>Deweloper</strong>. Jeśli jesteś w fazie produkcyjnej, zapoznaj się z tematem <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302279">BizTalk Services: Editions Chart</a> (Usługa BizTalk Services: wykres wersji), aby ustalić, czy odpowiednim wyborem dla scenariusza biznesowego jest wersja <strong>Premium</strong>, <strong>standardowa</strong> czy <strong>podstawowa</strong>.
    </td>
    </tr>
    <tr>
    <td><strong>Region</strong></td>
    <td>Wybierz region geograficzny do obsługi usługi BizTalk.</td>
    </tr>
    <tr>
    <td><strong>Adres URL domeny</strong></td>
    <td><strong>Opcjonalnie</strong>. Domyślnie adresem URL domeny jest <em>NazwaUsługiBizTalk</em>.biztalk.windows.net. Można również wprowadzić niestandardową nazwę domeny.  Na przykład jeśli domeną jest <em>contoso</em>, możesz wprowadzić: <br/><br/>
    <em>MojaFirma</em>.contoso.com<br/>
    <em>MojaFirmaMojaAplikacja</em>.contoso.com<br/>
    <em>MojaAplikacja</em>.contoso.com<br/>
    <em>NazwaUsługiBizTalk</em>.contoso.com<br/>
    </td>
    </tr>
    </table>
Wybierz strzałkę DALEJ.
5. Wprowadź ustawienia usług Storage i Baza danych:  <table border="1">
    <tr>
    <td><strong>Konto monitorowania/archiwizowania magazynu</strong></td>
    <td>Wybierz istniejące konto magazynu lub utwórz nowe. <br/><br/>Jeśli utworzysz nowe konto usługi Storage, wypełnij pole <strong>Nazwa konta usługi Storage</strong>.</td>
    </tr>
    <tr>
    <td><strong>Baza danych śledzenia</strong></td>
    <td>Jeśli używasz istniejącej bazy danych SQL Azure, nie można jej użyć w innej usłudze BizTalk. Po utworzeniu tego serwera bazy danych SQL Azure należy wprowadzić nazwę logowania i hasło.<br/><br/><strong>PORADA</strong> Utwórz bazę danych śledzenia i konto magazynu monitorowania/archiwizowania w tym samym regionie co usługa BizTalk.</td>
    </tr>
    </table>
Wybierz strzałkę DALEJ.
6. Wprowadź ustawienia bazy danych:  <table border="1">
    <tr>
    <td><strong>Nazwa</strong></td>
    <td>Ustawienie dostępne, jeśli na poprzednim ekranie wybrano opcję <strong>Utwórz nowe wystąpienie bazy danych SQL</strong>.
    <br/><br/>
Wprowadź nazwę bazy danych SQL do użycia przez usługę BizTalk.</td>
    </tr>
    <tr>
    <td><strong>Serwer</strong></td>
    <td>Ustawienie dostępne, jeśli na poprzednim ekranie wybrano opcję <strong>Utwórz nowe wystąpienie bazy danych SQL</strong>.
    <br/><br/>
Wybierz istniejący serwer bazy danych SQL lub utwórz nowy.</td>
    </tr>
    <tr>
    <td><strong>Nazwa logowania do serwera</strong></td>
    <td>Wprowadź nazwę logowania użytkownika.</td>
    </tr>
    <tr>
    <td><strong>Hasło logowania do serwera</strong></td>
    <td>Wprowadź hasło logowania.</td>
    </tr>
    <tr>
    <td><strong>Region</strong></td>
    <td>Ustawienie dostępne, jeśli wybrano opcję <strong>Utwórz nowe wystąpienie bazy danych SQL</strong>. Wybierz region geograficzny do obsługi bazy danych SQL.</td>
    </tr>
    </table>

Kliknij znacznik wyboru, aby zakończyć działanie kreatora. Zostanie wyświetlona ikona postępu:  
![Ikona postępu zostanie wyświetlona po ukończeniu][ProgressComplete]

Po ukończeniu kreatora usługa Azure BizTalk Services jest utworzona i gotowa dla aplikacji. Domyślne ustawienia są wystarczające. Jeśli chcesz zmienić ustawienia domyślne, wybierz opcję **BIZTALK SERVICES** w lewym okienku nawigacji, a następnie wybierz usługę BizTalk. Dodatkowe ustawienia są wyświetlane w [kartach Pulpit nawigacyjny, Monitor i Skala](biztalk-dashboard-monitor-scale-tabs.md) u góry.

W zależności od stanu usługi BizTalk niektórych operacji nie można ukończyć. Aby uzyskać listę tych operacji, przejdź do artykułu [BizTalk Services State Chart](biztalk-service-state-chart.md) (Wykres stanu usługi BizTalk Services).

## <a name="post-provisioning-steps"></a>Kroki po zainicjowaniu obsługi
* [Instalacja certyfikatu na komputerze lokalnym](#InstallCert)
* [Dodanie certyfikatu gotowego do produkcji](#AddCert)
* [Uzyskanie przestrzeni nazw kontroli dostępu](#ACS)

#### <a name="InstallCert"></a>Instalacja certyfikatu na komputerze lokalnym
W ramach inicjowania obsługi usługi BizTalk zostaje utworzony certyfikat z podpisem własnym, skojarzony z subskrypcją usługi BizTalk. Należy go pobrać i zainstalować na komputerach, z których będą wdrażane aplikacje usługi BizTalk lub wysyłane wiadomości do punktu końcowego usługi BizTalk.

1. Zaloguj się do [portalu Azure](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. Wybierz opcję **BIZTALK SERVICES** w lewym okienku nawigacyjnym, a następnie wybierz swoją subskrypcję usługi BizTalk.
3. Wybierz kartę **Pulpit nawigacyjny**.
4. Wybierz opcję **Pobierz certyfikat SSL**:  
   ![Modyfikowanie certyfikatu SSL][QuickGlance]
5. Kliknij dwukrotnie certyfikat, a następnie uruchom kreatora, aby go zainstalować. Pamiętaj, aby zainstalować certyfikat w obszarze magazynu **Zaufane główne urzędy certyfikacji**.

#### <a name="AddCert"></a>Dodanie certyfikatu gotowego do produkcji
Certyfikat z podpisem własnym, tworzony automatycznie podczas tworzenia usługi BizTalk Services, jest przeznaczony do użytku tylko w środowiskach programowania. Na potrzeby scenariuszy produkcyjnych zastąp go certyfikatem gotowym do produkcji.

1. Na karcie **Pulpit nawigacyjny** wybierz opcję **Aktualizuj certyfikat SSL**.
2. Przejdź do prywatnego certyfikatu SSL (*NazwaCertyfikatu*.pfx) zawierającego nazwę usługi BizTalk, wprowadź hasło, a następnie kliknij znacznik wyboru.

#### <a name="ACS"></a>Uzyskanie przestrzeni nazw usługi Access Control
1. Zaloguj się w witrynie [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. Wybierz opcję **BIZTALK SERVICES** w lewym okienku nawigacyjnym, a następnie wybierz swoją usługę BizTalk.
3. Na pasku zadań wybierz opcję **Informacje o połączeniu**:  
   ![Wybieranie opcji Informacje o połączeniu][ACSConnectInfo]
4. Skopiuj wartości kontroli dostępu.

Podczas wdrażania projektu usługi BizTalk w programie Visual Studio należy wprowadzić obszar nazw kontroli dostępu. Przestrzeń nazw kontroli dostępu dla usługi BizTalk jest tworzona automatycznie.

Wartości kontroli dostępu można używać z dowolną aplikacją. Podczas tworzenia usługi Azure BizTalk Services ta przestrzeń nazw kontroli dostępu kontroluje uwierzytelnianie za pomocą wdrożenia usługi BizTalk. Jeśli chcesz zmienić subskrypcję lub zarządzać przestrzenią nazw, wybierz opcję **ACTIVE DIRECTORY** w lewym okienku nawigacji, a następnie wybierz przestrzeń nazw. Pasek zadań zawiera opcje.

Kliknięcie przycisku **Zarządzaj** powoduje otwarcie portalu zarządzania kontroli dostępu. W portalu zarządzania kontroli dostępu usługa BizTalk korzysta z **tożsamości usługi**:  
![Tożsamości usługi ACS w portalu zarządzania usługi Access Control][ACSServiceIdentities]

Tożsamość usługi kontroli dostępu jest zestawem poświadczeń, który pozwala aplikacjom lub klientom uwierzytelniać się bezpośrednio za pomocą kontroli dostępu i odebrać token.

> [!IMPORTANT]
> Usługa BizTalk używa opcji **Właściciel** dla domyślnej tożsamości usługi oraz wartości **Hasło**. Jeśli używasz wartości Klucz symetryczny zamiast wartości Hasło, może wystąpić następujący błąd.<br/><br/>*Nie można nawiązać połączenia z kontem usługi zarządzania Access Control za pomocą podanych poświadczeń*
> 
> 

Niektóre wskazówki i zalecenia znajdują się w artykule [Managing Your ACS Namespace](https://msdn.microsoft.com/library/azure/hh674478.aspx) (Zarządzanie obszarem nazw ACS).

## <a name="requirements-explained"></a>Wyjaśniono wymagania
Wymagania te nie dotyczą wersji bezpłatnej.

<table border="1">
<tr bgcolor="FAF9F9">
        <td><strong>Co jest potrzebne</strong></td>
        <td><strong>Do czego służy</strong></td>
</tr>
<tr>
<td>Subskrypcja platformy Azure</td>
<td>Subskrypcja określa, kto może zalogować się do portalu Azure. Właściciel konta tworzy subskrypcję w witrynie <a HREF="https://account.windowsazure.com/Subscriptions">Subskrypcje Azure</a>.
<br/><br/>
Konto platformy Azure może mieć wiele subskrypcji i może być zarządzane przez dowolną osobę, która ma odpowiednie uprawnienia. Na przykład właściciel konta Azure tworzy subskrypcję o nazwie <em>BizTalkServiceSubscription</em> i nadaje administratorom usługi BizTalk w firmie (na przykład ContosoBTSAdmins@live.com) prawo dostępu do niej. W tym scenariuszu administratorzy usługi BizTalk logują się do portalu Azure i mają pełne prawa administratora do wszystkich hostowanych usług w ramach subskrypcji, łącznie z usługą Azure BizTalk Services. Administratorzy usługi BizTalk nie są właścicielami konta Azure i dlatego nie mają dostępu do żadnych informacji dotyczących rozliczeń.
<br/><br/>Więcej informacji znajduje się w artykule 
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=267577">Manage Subscriptions and Storage Accounts in the Azure portal</a> (Zarządzanie subskrypcjami i kontami usługi Storage w witrynie Azure Portal).
</td>
</tr>
<tr>
<td>Usługa Azure SQL Database</td>
<td>Przechowuje tabele, widoki i procedury składowane używane przez usługę BizTalk, w tym dane śledzenia.
<br/><br/>
Podczas tworzenia usługi BizTalk można użyć istniejącego serwera Azure SQL Server, bazy danych SQL Azure lub automatycznie utworzyć nowy serwer bądź nową bazę danych.
<br/><br/>
Skala bazy danych SQL zostaje automatycznie skonfigurowana. Zwykle dla usługi BizTalk domyślna skala jest wystarczająca. Zmiana skali ma wpływ na cenę. Zobacz artykuł <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=234930"> Accounts and Billing in Azure SQL Database</a>
 (Konta i rozliczenia w bazie danych SQL Azure)<br/><br/>
<strong>Uwagi</strong>
<br/>
<ul>
<li> Po utworzeniu nowego serwera Azure SQL Server i bazy danych usługi Azure zostaną automatycznie włączone. Usługa BizTalk wymaga, by usługi Azure zostały włączone.</li>
<li>W przypadku tworzenia nowej bazy danych SQL Azure na istniejącym serwerze Azure SQL Server reguły zapory serwera nie zostają zmienione. W związku z tym jest możliwe, że inne usługi Azure nie mają prawa dostępu do bazy danych serwera.</li>
</ul>
</td>
</tr>
<tr>
<td>Przestrzeń nazw kontroli dostępu Azure</td>
<td>Uwierzytelnia za pomocą usługi Azure BizTalk Services. Podczas wdrażania projektu usługi BizTalk w programie Visual Studio należy wprowadzić obszar nazw kontroli dostępu. Podczas tworzenia usługi BizTalk zostaje automatycznie utworzona przestrzeń nazw kontroli dostępu.</td>
</tr>

<tr>
<td>Konto usługi Azure Storage</td>
<td>Zapewnia dostęp do tabel, obiektów blob i kolejek używanych przez usługę BizTalk do zapisania:

<ul>
<li>Plików dziennika, które monitorują usługę BizTalk. Dane wyjściowe monitorowania są również wyświetlane na karcie **Monitorowanie** w portalu Azure.</li>
<li>Podczas tworzenia umowy X12 lub AS2 między partnerami możesz włączyć funkcję archiwizacji, aby przechować właściwości komunikatów. Te dane są zapisywane na koncie usługi Storage.</li>
</ul>
<br/>
Podczas tworzenia usługi BizTalk można użyć istniejącego konta usługi Storage lub automatycznie utworzyć nowe.
<br/><br/>
Domyślne ustawienia usługi Storage są wystarczające dla usługi BizTalk.
<br/><br/>
Podczas tworzenia konta usługi Storage następuje automatyczne utworzenie klucza podstawowego i klucza pomocniczego. Klucze te kontrolują dostęp do konta usługi Storage. Usługa BizTalk automatycznie używa klucza podstawowego.
<br/><br/>
Więcej informacji znajduje się w artykule <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285671"> Storage</a>.
</td>
</tr>

<tr>
<td>Prywatny certyfikat SSL</td>
<td>
Po utworzeniu usługi Azure BizTalk Services zostaje również utworzony adres URL HTTPS zawierający nazwę usługi BizTalk. Ten adres URL zostaje automatycznie skonfigurowany do używania certyfikatu z podpisem własnym tylko do programowania. Do produkcji potrzebny jest prywatny certyfikat SSL.
<br/><br/>
<strong>Ważne informacje dotyczące certyfikatów SSL</strong>

<ul>
<li>Certyfikat musi wygasać nie później niż po 5 latach.</li>
<li>Wszystkie certyfikaty prywatne wymagają hasła. Zapamiętaj to hasło i udostępnij je swoim administratorom.</li>
<li>Certyfikaty z podpisem własnym są używane w środowisku testowania/programowania. W przypadku korzystania z certyfikatów z podpisem własnym zaimportuj certyfikat do osobistego magazynu certyfikatów oraz magazynu certyfikatów zaufanych głównych urzędów certyfikacji.</li>
</ul>
<br/>Podczas wysyłania żądania certyfikatu produkcji do urzędu certyfikacji należy podać następujące właściwości certyfikatu:
<br/>

<ul>
<li><strong>Ulepszone użycie klucza</strong>: usługa Azure BizTalk Services wymaga przynajmniej uwierzytelniania przez serwer.</li>
<li><strong>Nazwa pospolita</strong>: wprowadź w pełni kwalifikowaną nazwę domeny (FQDN) adresu URL usługi Azure BizTalk Services. Zobacz temat <a HREF="#CreateService">Create a BizTalk Service</a> (Tworzenie usługi BizTalk) w tym artykule.</li>
</ul>
<br/>
Po utworzeniu usługi BizTalk można dodać nowy lub inny certyfikat.
</td>
</tr>
</table>
<!---Loc Comment: Please, check link [Create a BizTalk Service] since it is not redirecting to any location.--->



## <a name="hybrid-connections"></a>Połączenia hybrydowe
Podczas tworzenia usługi Azure BizTalk Services dostępna jest karta **Połączenia hybrydowe**:

![Karta Połączenia hybrydowe][HybridConnectionTab]

Połączenia hybrydowe są używane do łączenia witryny sieci Web Azure lub usługi mobilnej Azure z dowolnym zasobem lokalnym, który używa statycznego portu TCP, takim jak serwer SQL Server, MySQL, interfejsy API HTTP Web, Mobile Services i większość niestandardowych usług sieci Web.  Połączenia hybrydowe i usługa adaptera BizTalk różnią się od siebie. Usługa adaptera BizTalk służy do łączenia usługi Azure BizTalk Services z lokalnym systemem Line of Business (LOB).

 Więcej informacji, m.in. na temat tworzenia połączeń hybrydowych i zarządzania nimi, znajduje się w artykule [Hybrid Connections](integration-hybrid-connection-overview.md) (Połączenia hybrydowe).

## <a name="next-steps"></a>Następne kroki
Po utworzeniu usługi BizTalk zapoznaj się z różnymi [usługami BizTalk Services: kartami Pulpit nawigacyjny, Monitor i Skala](biztalk-dashboard-monitor-scale-tabs.md). Usługa BizTalk jest gotowa dla aplikacji. Aby rozpocząć tworzenie aplikacji, przejdź do artykułu [Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=235197) (Usługa Azure BizTalk Services).

## <a name="see-also"></a>Zobacz też
* [BizTalk Services: Editions Chart (Usługa BizTalk Services: zestawienie wersji)](biztalk-editions-feature-chart.md)<br/>
* [BizTalk Services: State Chart (Usługa BizTalk Services: tabela stanów)](biztalk-service-state-chart.md)<br/>
* [BizTalk Services: Backup and Restore (Usługa BizTalk Services: tworzenie kopii zapasowej i przywracanie)](biztalk-backup-restore.md)<br/>
* [BizTalk Services: Throttling (Usługa BizTalk Services: ograniczanie przepływności)](biztalk-throttling-thresholds.md)<br/>
* [BizTalk Services: Issuer Name and Issuer Key (Usługa BizTalk Services: nazwa i klucz wydawcy)](biztalk-issuer-name-issuer-key.md)<br/>
* [Jak rozpocząć pracę z zestawem SDK usługi Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
* [Połączenia hybrydowe](integration-hybrid-connection-overview.md)

[NewBizTalkService]: ./media/biztalk-provision-services/WABS_NewBizTalkService.png
[NEWButton]: ./media/biztalk-provision-services/WABS_New.png
[ProgressComplete]: ./media/biztalk-provision-services/WABS_ProgressComplete.png
[ACSConnectInfo]: ./media/biztalk-provision-services/WABS_ACSConnectInformation.png
[QuickGlance]: ./media/biztalk-provision-services/WABS_QuickGlance.png
[ACSServiceIdentities]: ./media/biztalk-provision-services/WABS_ACSServiceIdentities.png
[HybridConnectionTab]: ./media/biztalk-provision-services/WABS_HybridConnectionTab.png
