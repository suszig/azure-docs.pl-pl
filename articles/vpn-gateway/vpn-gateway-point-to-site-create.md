<properties
   pageTitle="Konfigurowanie połączenia sieci VPN typu punkt-lokacja z siecią wirtualną platformy Azure przy użyciu portalu klasycznego| Microsoft Azure"
   description="Istnieje możliwość bezpiecznego połączenia się z siecią wirtualną platformy Azure poprzez utworzenie połączenia sieci VPN typu punkt-lokacja. Instrukcje dotyczące sieci wirtualnych utworzonych przy użyciu modelu wdrażania Service Management (określanego także jako model klasyczny)."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/30/2016"
   ms.author="cherylmc"/>

# Konfigurowanie połączenia sieci VPN typu punkt-lokacja z siecią wirtualną przy użyciu portalu klasycznego

> [AZURE.SELECTOR]
- [Program PowerShell — model usługi Resource Manager](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Portal — model klasyczny](vpn-gateway-point-to-site-create.md)

Konfiguracja typu punkt-lokacja pozwala utworzyć indywidualne bezpieczne połączenie z siecią wirtualną z poziomu komputera klienckiego. Połączenie sieci VPN jest nawiązywane poprzez zainicjowanie połączenia z komputera klienckiego. Połączenie typu punkt-lokacja to idealne rozwiązanie w przypadku, gdy celem użytkownika jest połączenie się z siecią wirtualną z lokalizacji zdalnej, np. z domu lub z konferencji, lub gdy konieczne jest połączenie z siecią wirtualną jedynie ograniczonej liczby klientów. 

Połączenia typu punkt-lokacja nie wymagają do prawidłowego działania urządzenia sieci VPN ani publicznego adresu IP. Aby uzyskać więcej informacji na temat połączeń typu punkt-lokacja, zobacz artykuły [VPN Gateway FAQ](vpn-gateway-vpn-faq.md#point-to-site-connections) (Brama sieci VPN — często zadawane pytania) i [About cross-premises connections](vpn-gateway-cross-premises-options.md) (Informacje o połączeniach obejmujących wiele lokalizacji).

Ten artykuł odnosi się do połączeń bramy sieci VPN typu punkt-lokacja z siecią wirtualną utworzoną z użyciem **klasycznego modelu wdrażania** (Service Management) oraz klasycznego portalu. W krokach dotyczących portalu Azure na tej stronie podano łącza do tego artykułu.

**Modele wdrażania i narzędzia używane w kontekście połączeń typu punkt-lokacja**

[AZURE.INCLUDE [vpn-gateway-table-point-to-site](../../includes/vpn-gateway-table-point-to-site-include.md)] 


**Modele wdrażania Azure — informacje**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

![Diagram: połączenie typu punkt-lokacja](./media/vpn-gateway-point-to-site-create/point2site.png "point-to-site")



## Tworzenie połączenia typu punkt-lokacja — informacje
 
Poniższe kroki opisują czynności niezbędne w celu utworzenia bezpiecznego połączenia typu punkt-lokacja z siecią wirtualną. Choć konfiguracja połączenia typu punkt-lokacja wymaga wykonania wielu kroków, to jest to doskonały sposób na nawiązanie bezpiecznego połączenia między komputerem a siecią wirtualną bez potrzeby uzyskania i skonfigurowania urządzenia sieci VPN. 

Proces konfigurowania połączenia typu punkt-lokacja został podzielony na 4 sekcje. Ważna jest kolejność przeprowadzania poszczególnych czynności konfiguracyjnych, dlatego też nie należy pomijać żadnych kroków ani przechodzić od razu do kolejnych czynności.


- **Sekcja 1** jest poświęcona procesowi tworzenia sieci wirtualnej i bramy sieci VPN.
- **Sekcja 2** zawiera informacje na temat tworzenia certyfikatów używanych do uwierzytelniania oraz ich przekazywania.
- **Sekcja 3** stanowi opis kroków niezbędnych do wyeksportowania i zainstalowania certyfikatów klienta.
- **Sekcja 4** stanowi opis kroków koniecznych do skonfigurowania klienta sieci VPN.

## Sekcja 1 — tworzenie sieci wirtualnej i bramy sieci VPN


### Część 1. Tworzenie sieci wirtualnej

1. Zaloguj się do [klasycznego portalu Azure](https://manage.windowsazure.com/). Zwróć uwagę na fakt, że te kroki odnoszą się do portalu klasycznego, a nie do portalu Azure.

2. W lewym dolnym rogu ekranu kliknij opcję **Nowe**. W okienku nawigacji kliknij opcję **Usługi sieciowe**, a następnie opcję **Sieć wirtualna**. Kliknij przycisk **Tworzenie niestandardowe**, aby rozpocząć korzystanie z kreatora konfiguracji.

3. Na stronie **Szczegóły sieci wirtualnej** wprowadź następujące informacje, a następnie kliknij strzałkę w prawym dolnym rogu, aby przejść dalej.
    - **Nazwa**: nadaj nazwę sieci wirtualnej. Na przykład: „VNetEast”. Tej nazwy będziesz używać, odwołując się do sieci podczas wdrażania maszyn wirtualnych i wystąpień PaaS w ramach danej sieci wirtualnej.
    - **Lokalizacja**: omawiana lokalizacja jest bezpośrednio związana z lokalizacją fizyczną (regionem), w której mają się znajdować zasoby (maszyny wirtualne). Jeśli na przykład maszyny wirtualne wdrażane w ramach tej sieci wirtualnej mają znajdować się we wschodnim regionie USA, należy wybrać tę właśnie lokalizację. Po utworzeniu sieci wirtualnej nie można zmienić powiązanego z nią regionu.

4. Na stronie **DNS Servers and VPN Connectivity** (Serwery DNS i połączenia sieci VPN) wprowadź następujące informacje, a następnie kliknij strzałkę w prawym dolnym rogu, aby przejść dalej.
    - **Serwery DNS**: wprowadź nazwę serwera DNS i jego adres IP lub wybierz zarejestrowany wcześniej serwer DNS z menu skrótów. To ustawienie nie powoduje utworzenia serwera DNS, a jedynie pozwala określić, które serwery mają być używane do rozpoznawania nazw w danej sieci wirtualnej. Chcąc skorzystać z domyślnej usługi rozpoznawania nazw platformy Azure, należy pozostawić to pole puste.
    - **Konfiguracja połączenia sieci VPN typu punkt-lokacja**: zaznacz to pole wyboru.

5. Na stronie **Łączność typu punkt-lokacja** określ zakres adresów IP, z których klienci sieci VPN będą otrzymywać adresy IP po nawiązaniu połączenia. Istnieje kilka zasad dotyczących zakresów adresów, które można określić. Należy koniecznie sprawdzić, czy określony zakres nie pokrywa się z żadnym z zakresów należących do sieci lokalnej.

6. Wprowadź następujące informacje, a następnie kliknij strzałkę, aby przejść dalej.
 - **Przestrzeń adresowa**: należy uwzględnić początkowy adres IP oraz wartość CIDR (liczbę adresów).
 - **Dodaj przestrzeń adresową**: przestrzeń adresową należy dodawać tylko wtedy, gdy jest to wymagane z racji struktury sieci.

7. Na stronie **Przestrzenie adresowe sieci wirtualnej** określ zakres adresów, który ma być używany w ramach sieci wirtualnej. Są to dynamiczne adresy IP (adresy DIP), które zostaną przypisane maszynom wirtualnym i innym wystąpieniom roli wdrażanym w ramach tej sieci wirtualnej. Szczególnie istotne jest, aby wybrać zakres, który nie nakłada się na żaden z zakresów używanych w sieci lokalnej. Należy skontaktować się z administratorem sieci — niezbędne może okazać się wydzielenie przez niego z przestrzeni adresowej sieci lokalnej zakresu adresów IP, które zostaną użyte w ramach sieci wirtualnej.

8. Wprowadź następujące informacje, a następnie kliknij znacznik wyboru, aby rozpocząć tworzenie sieci wirtualnej.
 - **Przestrzeń adresowa**: dodaj wewnętrzny zakres adresów IP, którego chcesz używać w ramach tej sieci wirtualnej, uwzględniając początkowy adres IP i liczbę adresów. Istotne jest, aby wybrać zakres, który nie nakłada się na żaden z zakresów używanych w sieci lokalnej. Należy skontaktować się z administratorem sieci — niezbędne może okazać się wydzielenie przez niego z przestrzeni adresowej sieci lokalnej zakresu adresów IP, które zostaną użyte w ramach sieci wirtualnej.
 - **Dodaj podsieć**: dodatkowe podsieci nie są wymagane, można jednak utworzyć osobne podsieci dla maszyn wirtualnych, które będą miały statyczne adresy DIP. Maszyny wirtualne mogą także tworzyć podsieć niezależną od innych wystąpień roli.
 - **Dodaj podsieć bramy**: podsieć bramy jest wymagana w przypadku połączeń sieci VPN typu punkt-lokacja. Kliknij, aby dodać podsieć bramy. Podsieć bramy jest używana tylko w odniesieniu do bramy sieci wirtualnej.

9. Po utworzeniu sieci wirtualnej w obszarze **Stan** na stronie sieci w klasycznym portalu Azure pojawi się wartość **Utworzona**. Po utworzeniu sieci wirtualnej można utworzyć bramę o dynamicznym routingu.

### Część 2: Tworzenie bramy o dynamicznym routingu

Podczas konfiguracji należy wybrać dynamiczny typ bramy. Bramy o statycznym routingu nie będą działać z tą funkcją.

1. Na stronie **Sieci** w klasycznym portalu Azure kliknij sieć wirtualną, która została właśnie utworzona, a następnie przejdź na stronę **Pulpit nawigacyjny**.

2. Kliknij przycisk **Utwórz bramę** w dolnej części strony **Pulpit nawigacyjny**. Zostanie wyświetlony komunikat z pytaniem: **Czy chcesz utworzyć bramę sieci wirtualnej „twojasieć”?**. Kliknij opcję **Tak**, aby rozpocząć tworzenie bramy. Proces tworzenia bramy może potrwać około 15 minut.

## Sekcja 2 — generowanie i przekazywanie certyfikatów

Certyfikaty są używane do uwierzytelniania klientów sieci VPN w obrębie sieci VPN typu punkt-lokacja. Można stosować certyfikaty przedsiębiorstw, a także certyfikaty z podpisem własnym. Na platformę Azure można przekazać do 20 certyfikatów głównych.

Aby użyć certyfikatu z podpisem własnym, wykonaj poniższe kroki. Jeżeli ma zostać użyty certyfikat przedsiębiorstwa, kroki w poszczególnych sekcjach będą się różnić, nadal będzie jednak wymagane wykonanie następujących czynności:

### Część 1: Identyfikacja lub wygenerowanie certyfikatu głównego

Jeśli nie używasz certyfikatu przedsiębiorstwa, musisz wygenerować certyfikat główny z podpisem własnym. Kroki opisane w tej sekcji zostały opracowane pod kątem systemu Windows 8. Kroki dla systemu Windows 10 można znaleźć w temacie [Working with self-signed root certificates for Point-to-Site configurations](vpn-gateway-certificates-point-to-site.md) (Praca z certyfikatami głównymi z podpisem własnym w konfiguracjach typu punkt-lokacja).

Jednym ze sposobów tworzenia certyfikatu X.509 jest użycie narzędzia Certificate Creation Tool (makecert.exe). Aby użyć narzędzia makecert, pobierz i zainstaluj bezpłatny program [Microsoft Visual Studio Express](https://www.visualstudio.com/products/visual-studio-express-vs.aspx).

1. Przejdź do folderu Visual Studio Tools i uruchom wiersz polecenia jako administrator.

2. Polecenie w poniższym przykładzie spowoduje utworzenie i zainstalowanie certyfikatu głównego w osobistym magazynie certyfikatów na komputerze użytkownika oraz utworzenie odpowiedniego pliku *cer*, który zostanie później przekazany do klasycznego portalu Azure.

3. Przejdź do katalogu, w którym ma znajdować się plik .cer, a następnie uruchom następujące polecenie, gdzie *RootCertificateName* to nazwa certyfikatu. Wynikiem użycia poniższego przykładu bez wprowadzania zmian będzie utworzenie certyfikatu głównego i odpowiedniego pliku *RootCertificateName.cer*.

>[AZURE.NOTE] Jako że utworzono certyfikat główny, z którego będą generowane certyfikaty klienta, warto wyeksportować certyfikat główny wraz z jego kluczem prywatnym i zapisać go w bezpiecznej lokalizacji, z której w razie potrzeby będzie można go odzyskać.

    makecert -sky exchange -r -n "CN=RootCertificateName" -pe -a sha1 -len 2048 -ss My "RootCertificateName.cer"

### Część 2: Przekazywanie pliku .cer certyfikatu głównego do klasycznego portalu Azure

Niezbędne jest przekazanie na platformę Azure odpowiedniego pliku .cer dla każdego z certyfikatów głównych. Można przekazać do 20 certyfikatów.

1. Podczas generowania certyfikatu głównego we wcześniejszym etapie procedury został utworzony plik *.cer*. Teraz należy przekazać ten plik do klasycznego portalu Azure. Należy pamiętać, że plik .cer nie zawiera klucza prywatnego certyfikatu głównego. Można przekazać do 20 certyfikatów głównych.

2. W klasycznym portalu Azure na stronie **Certyfikaty** odnoszącej się do danej sieci wirtualnej kliknij pozycję **Przekaż certyfikat główny**.

3. Na stronie **Przekaż certyfikat** wybierz lokalizację pliku .cer certyfikatu głównego, a następnie kliknij znacznik wyboru.

### Część 3: Generowanie certyfikatu klienta

Poniższe kroki pozwalają wygenerować certyfikat klienta z certyfikatu głównego z podpisem własnym. Kroki opisane w tej sekcji zostały opracowane pod kątem systemu Windows 8. Kroki dla systemu Windows 10 można znaleźć w temacie [Working with self-signed root certificates for Point-to-Site configurations](vpn-gateway-certificates-point-to-site.md) (Praca z certyfikatami głównymi z podpisem własnym w konfiguracjach typu punkt-lokacja). Jeśli korzystasz z certyfikatu przedsiębiorstwa, postępuj zgodnie z wytycznymi dla rozwiązania, którego używasz. 

1. Na komputerze, którego użyto do utworzenia certyfikatu głównego z podpisem własnym, otwórz okno wiersza polecenia programu Visual Studio jako administrator.

2. Zmień katalog na lokalizację, w której chcesz zapisać plik certyfikatu klienta. Pozycja *RootCertificateName* odwołuje się do wygenerowanego certyfikatu głównego z podpisem własnym. Jeśli korzystasz z poniższego przykładu (zmieniając wartość RootCertificateName na nazwę swojego certyfikatu głównego), w wyniku tej procedury w osobistym magazynie certyfikatów zostanie wygenerowany certyfikat klienta o nazwie „ClientCertificateName”.

3. Wpisz następujące polecenie:

        makecert.exe -n "CN=ClientCertificateName" -pe -sky exchange -m 96 -ss My -in "RootCertificateName" -is my -a sha1

4. Wszystkie certyfikaty są przechowywane w osobistym magazynie certyfikatów na komputerze używanym do wykonywania opisanych czynności. Zaznacz pozycję *certmgr*, aby przeprowadzić weryfikację. Korzystając z tej procedury, można wygenerować dowolną liczbę certyfikatów klienta, zgodnie z bieżącymi potrzebami. Zaleca się utworzenie unikatowych certyfikatów klienta dla każdego komputera, który ma zostać połączony z siecią wirtualną.

## Sekcja 3 — eksportowanie i instalowanie certyfikatu klienta

Zainstalowanie certyfikatu klienta na każdym komputerze, który zostanie połączony z siecią wirtualną, stanowi krok obowiązkowy. Poniższe instrukcje pozwalają przeprowadzić ręczną instalację certyfikatu klienta.

1. Certyfikat klienta należy zainstalować na każdym komputerze, który zostanie połączony z siecią wirtualną. W praktyce oznacza to, że prawdopodobnie konieczne będzie utworzenie wielu certyfikatów klienta, a następnie wyeksportowanie ich. Aby wyeksportować certyfikaty klienta, należy użyć narzędzia *certmgr.msc*. Kliknij prawym przyciskiem myszy certyfikat klienta, który chcesz wyeksportować, a następnie kliknij przycisk **wszystkie zadania** i opcję **eksportuj**.
2. Wyeksportuj *certyfikat klienta* z kluczem prywatnym. Klucz znajduje się w pliku *.pfx*. Zapisz lub zapamiętaj hasło (klucz) ustawione dla tego certyfikatu.
3. Skopiuj plik *.pfx* na komputer kliencki. Na komputerze klienckim kliknij dwukrotnie plik *.pfx*, aby go zainstalować. Gdy zostanie wyświetlony stosowny monit, wprowadź hasło. Nie zmieniaj lokalizacji instalacji.

## Sekcja 4 — konfigurowanie klienta VPN

Aby nawiązać połączenie z siecią wirtualną, należy skonfigurować także klienta sieci VPN. W celu nawiązania połączenia klient wymaga zarówno certyfikatu klienta, jak i odpowiedniej konfiguracji klienta sieci VPN. Aby skonfigurować klienta sieci VPN, wykonaj po kolei następujące czynności.

### Część 1: Tworzenie pakietu konfiguracyjnego klienta VPN

1. W klasycznym portalu Azure przejdź ze strony **Pulpit nawigacyjny** sieci wirtualnej do menu szybkiego dostępu w prawym górnym rogu i kliknij pakiet sieci VPN powiązany z klientem, którego chcesz połączyć z siecią wirtualną.

2. 
Obsługiwane są następujące systemy operacyjne klientów:
 - Windows 7 (32-bitowy i 64-bitowy)
 - Windows Server 2008 R2 (tylko 64-bitowy)
 - Windows 8 (32-bitowy i 64-bitowy)
 - Windows 8.1 (32-bitowy i 64-bitowy)
 - Windows Server 2012 (tylko 64-bitowy)
 - Windows Server 2012 R2 (tylko 64-bitowy)
 - Windows 10

3. Wybierz pakiet pobierania, który odpowiada systemowi operacyjnemu klienta, na którym zostanie zainstalowany:
 - W przypadku klientów 32-bitowych należy wybrać opcję **Pobierz pakiet dla 32-bitowego klienta VPN**.
 - W przypadku klientów 64-bitowych należy wybrać opcję **Pobierz pakiet dla 64-bitowego klienta VPN**.

4. Utworzenie pakietu pobierania może potrwać kilka minut. Po zakończeniu procesu tworzenia pakietu można pobrać plik. Pobrany plik *.exe* można bezpiecznie przechowywać na komputerze lokalnym.

5. Po wygenerowaniu pakietu klienta VPN i pobraniu go z klasycznego portalu Azure można go zainstalować na komputerze klienckim, z którego ma zostać nawiązane połączenie z siecią wirtualną. Chcąc zainstalować pakiet klienta VPN na wielu komputerach klienckich, należy się upewnić, że na każdym z komputerów jest także zainstalowany certyfikat klienta. Pakiet klienta VPN zawiera informacje konfiguracyjne umożliwiające skonfigurowanie oprogramowania klienta VPN wbudowanego w system Windows. Instalacja pakietu nie powoduje instalacji dodatkowego oprogramowania.

### Część 2: Instalacja pakietu konfiguracyjnego sieci VPN na komputerze klienckim i rozpoczęcie połączenia

1. Skopiuj lokalnie plik konfiguracji na komputer, który chcesz połączyć z siecią wirtualną, a następnie kliknij dwukrotnie plik z rozszerzeniem .exe. Gdy pakiet zostanie zainstalowany, możesz uruchomić połączenie sieci VPN.
Warto zwrócić uwagę, że pakiet konfiguracyjny nie jest podpisany przez firmę Microsoft. Możesz podpisać pakiet, korzystając z urządzenia używanego w tym celu w Twojej organizacji, lub samodzielnie, przy użyciu narzędzia [SignTool]( http://go.microsoft.com/fwlink/p/?LinkId=699327). Z pakietu można korzystać nawet, jeśli nie jest podpisany. Jeśli jednak pakiet nie jest podpisany, podczas instalowania go zostanie wyświetlone ostrzeżenie.
2. Na komputerze klienckim przejdź do połączeń sieci VPN i wyszukaj wcześniej utworzone połączenie sieci VPN. Połączenie będzie miało taką samą nazwę jak sieć wirtualna. Kliknij przycisk **Połącz**.
3. Pojawi się komunikat podręczny umożliwiający utworzenie certyfikatu z podpisem własnym dla punktu końcowego bramy. Kliknij przycisk **Kontynuuj**, aby skorzystać z podwyższonego poziomu uprawnień.
4. Na stronie stanu **Połączenie** kliknij przycisk **Połącz**, aby rozpocząć połączenie.
5. Jeśli widzisz ekran **Wybierz certyfikat**, sprawdź, czy wyświetlany certyfikat klienta to ten, który ma zostać użyty do nawiązania połączenia. Jeśli nie, kliknij strzałkę na liście rozwijanej, aby wybrać właściwy certyfikat, a następnie kliknij przycisk **OK**.
6. Po nawiązaniu podłączenia z siecią wirtualną otrzymujesz pełny dostęp do usług i maszyny wirtualnej hostowanej w sieci wirtualnej.

### Część 3: Sprawdzenie połączenia sieci VPN

1. Aby sprawdzić, czy połączenie sieci VPN jest aktywne, otwórz wiersz polecenia z podwyższonym poziomem uprawnień, a następnie uruchom polecenie *ipconfig/all*.
2. Przejrzyj wyniki. Zwróć uwagę na fakt, że otrzymany adres IP jest jednym z adresów z zakresu dla połączenia typu punkt-lokacja określonego podczas tworzenia sieci wirtualnej. Wyniki powinny być podobne do poniższego kodu:

Przykład:



    PPP adapter VNetEast:
        Connection-specific DNS Suffix .:
        Description.....................: VNetEast
        Physical Address................:
        DHCP Enabled....................: No
        Autoconfiguration Enabled.......: Yes
        IPv4 Address....................: 192.168.130.2(Preferred)
        Subnet Mask.....................: 255.255.255.255
        Default Gateway.................:
        NetBIOS over Tcpip..............: Enabled

## Następne kroki

Do sieci wirtualnej można dodać maszyny wirtualne. Zobacz artykuł [How to create a custom virtual machine](../virtual-machines/virtual-machines-windows-classic-createportal.md) (Tworzenie niestandardowej maszyny wirtualnej).

Aby uzyskać więcej informacji na temat sieci wirtualnych, zobacz artykuł [Virtual Network Documentation](https://azure.microsoft.com/documentation/services/virtual-network/) (Dokumentacja sieci wirtualnych).



<!--HONumber=jun16_HO2-->


