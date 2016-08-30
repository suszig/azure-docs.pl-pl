<properties
   pageTitle="Usługa Azure Backup — często zadawane pytania | Microsoft Azure"
   description="Odpowiedzi na często zadawane pytania dotyczące usługi kopii zapasowej, agenta kopii zapasowej, kopii zapasowej i przechowywania, odzyskiwania, zabezpieczeń i inne typowe pytania dotyczące tworzenia kopii zapasowej i odzyskiwania po awarii."
   services="backup"
   documentationCenter=""
   authors="markgalioto"
   manager="jwhit"
   editor=""
   keywords="backup and disaster recovery; backup service"/>

<tags
   ms.service="backup"
   ms.workload="storage-backup-recovery"
     ms.tgt_pltfrm="na"
     ms.devlang="na"
     ms.topic="get-started-article"
     ms.date="04/04/2016"
     ms.author="trinadhk; giridham; arunak; markgal; jimpark;"/>

# Usługa Azure Backup — często zadawane pytania

> [AZURE.SELECTOR]
- [Kopia zapasowa — często zadawane pytania dotyczące trybu klasycznego](backup-azure-backup-faq.md)
- [Kopia zapasowa — często zadawane pytania dotyczące trybu ARM](backup-azure-backup-ibiza-faq.md)

Ten artykuł zawiera listę często zadawanych pytań (i odpowiedzi na nie) dotyczących usługi Azure Backup. Nasza społeczność odpowiada szybko, a często zadawane pytania są dodawane do tego artykułu. Odpowiedzi na pytania zwykle zawierają odwołania lub informacje dotyczące pomocy technicznej. Pytania dotyczące usługi Azure Backup można zadawać w sekcji dyskusyjnej tego lub powiązanego artykułu. Pytania dotyczące usługi Azure Backup można zadawać także na [forum dyskusyjnym](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup).

## Instalacja i konfiguracja
**Pytanie 1. Jaka jest lista obsługiwanych systemów operacyjnych, z których można tworzyć kopie zapasowe na platformie Azure przy użyciu usługi Azure Backup?** <br/>
Odpowiedź 1. Usługa Azure Backup obsługuje poniższe systemy operacyjne


| System operacyjny        | Platforma           | SKU  |
| :------------- |-------------| :-----|
| Windows 8 i najnowsze dodatki Service Pack      | 64-bitowa | Enterprise, Pro |
| Windows 7 i najnowsze dodatki Service Pack      | 64-bitowa | Ultimate, Enterprise, Professional, Home Premium, Home Basic, Starter |
| Windows 8.1 i najnowsze dodatki Service Pack | 64-bitowa      |    Enterprise, Pro |
| Windows 10      | 64-bitowa | Enterprise, Pro, Home |
|Windows Server 2012 R2 i najnowsze dodatki Service Pack| 64-bitowa| Standard, Datacenter, Foundation|
|Windows Server 2012 i najnowsze dodatki Service Pack|    64-bitowa| Datacenter, Foundation, Standard|
|Windows Storage Server 2012 R2 i najnowsze dodatki Service Pack  |64-bitowa|    Standard, Workgroup|
|Windows Storage Server 2012 i najnowsze dodatki Service Pack |64-bitowa |Standard, Workgroup
|Windows Server 2012 R2 i najnowsze dodatki Service Pack  |64-bitowa|    Essential|
|Windows Server 2008 R2 SP1 |64-bitowa|    Standard, Enterprise, Datacenter, Foundation|
|Windows Server 2008 SP2    |64-bitowa|    Standard, Enterprise, Datacenter, Foundation|

**Pytanie 2. Gdzie można pobrać najnowszą wersję agenta usługi Azure Backup?** <br/>
Odpowiedź 2. Najnowszą wersję agenta tworzenia kopii zapasowej systemu Windows Server, System Center DPM lub klienta systemu Windows można pobrać [tutaj](http://aka.ms/azurebackup_agent). Aby utworzyć kopię zapasową maszyny wirtualnej, należy skorzystać z agenta maszyny wirtualnej (który automatycznie instaluje właściwe rozszerzenie). Agent maszyny wirtualnej znajduje się już na maszynie wirtualnej, która została utworzona z poziomu galerii Azure.

**Pytanie 3. Która wersja serwera SCDPM jest obsługiwana?** <br/>
Odpowiedź 3. Zaleca się zainstalowanie [najnowszego](http://aka.ms/azurebackup_agent) agenta usługi Azure Backup z najnowszego pakietu zbiorczego aktualizacji SCDPM (w lipcu 2015 r. jest to UR6)

**Pytanie 4. Podczas konfigurowania agenta usługi Azure Backup pojawia się monit o wprowadzenie **poświadczeń magazynu**. Czy poświadczenia magazynu wygasają?
Odpowiedź 4. Tak, poświadczenia magazynu wygasają po upływie 48 godzin. Jeśli plik wygaśnie, należy zalogować się do portalu Azure i pobrać pliki poświadczeń magazynu z magazynu usługi Backup.

**Pytanie 5. Czy istnieje ograniczenie liczby magazynów kopii zapasowych, które można utworzyć w każdej subskrypcji platformy Azure?** <br/>
Odpowiedź 5. Tak. Według stanu na lipiec 2015 r. można utworzyć 25 magazynów na subskrypcję. Jeśli potrzebna jest większa liczba magazynów, należy utworzyć nową subskrypcję.

**Pytanie 6. Czy należy traktować magazyn jako jednostkę rozliczeniową?** <br/>
Odpowiedź 6. Chociaż można uzyskać szczegółowe rozliczenie dla każdego magazynu, zdecydowanie zaleca się traktowanie subskrypcji platformy Azure jako jednostki rozliczeniowej. Jest ona spójna dla wszystkich usług i łatwiejsza w zarządzaniu.

**Pytanie 7. Czy istnieją jakiekolwiek ograniczenia dotyczące liczby serwerów/maszyn, które można zarejestrować w każdym magazynie?** <br/>
Odpowiedź 7. Tak, w magazynie można zarejestrować maksymalnie 50 maszyn. W przypadku maszyny wirtualnej IaaS platformy Azure ograniczenie wynosi 200 maszyn wirtualnych w magazynie. Jeśli trzeba zarejestrować więcej maszyn, należy utworzyć nowy magazyn.

**Pytanie 8. Czy istnieją jakiekolwiek ograniczenia dotyczące ilości danych, jakie można umieszczać w kopii zapasowej z poziomu serwera/klienta systemu Windows lub serwera SCDPM?** <br/>
Odpowiedź 8. Nie.

**Pytanie 9. Jak można zarejestrować serwer w innym centrum danych?**<br/>
Odpowiedź 9. Dane kopii zapasowej są wysyłane do centrum danych usługi Backup, w której jest ona zarejestrowana. Najprostszym sposobem zmiany centrum danych jest odinstalowanie agenta oraz ponowne zainstalowanie i zarejestrowanie go w nowym centrum danych.

**Pytanie 10. Co się stanie, jeśli zostanie zmieniona nazwa serwera Windows, który wykonuje kopie zapasowe danych na platformie Azure?**<br/>
Odpowiedź 10. Po zmianie nazwy serwera wszystkie aktualnie skonfigurowane kopie zapasowe są zatrzymywane.
Należy zarejestrować nową nazwę serwera w magazynie usługi Backup. Podczas tworzenia nowej rejestracji pierwszą operacją tworzenia kopii zapasowej jest pełna, a nie przyrostowa kopia zapasowa. Dane, których kopia zapasowa wykonana wcześniej przy użyciu starej nazwy serwera znajduje się w magazynie, można odzyskać przy użyciu opcji [**Inny serwer**](backup-azure-restore-windows-server.md#recover-to-an-alternate-machine) w kreatorze **Odtwarzanie danych**.


**Pytanie 11. Z jakich typów dysków można tworzyć kopie zapasowe plików i folderów?** <br/>
Odpowiedź 11. Nie można utworzyć kopii zapasowej z następującego zestawu dysków/woluminów:

- Nośniki wymienne: dysk musi być stały, aby mógł posłużyć jako źródło elementu kopii zapasowej.
- Woluminy tylko do odczytu: wolumin musi mieć możliwość zapisu ze względu na działanie usługi kopiowania woluminów w tle (VSS, volume shadow copy).
- Woluminy offline: wolumin musi być w trybie online, aby mogła działać usługa VSS.
- Udział sieciowy: wolumin musi być lokalny dla serwera, aby możliwe było wykonanie kopii zapasowej za pomocą usługi kopii zapasowej online.
- Woluminy chronione przez funkcję BitLocker: wolumin musi być odblokowany przed próbą wykonania kopii zapasowej.
- Identyfikacja systemu plików: w tej wersji usługi kopii zapasowej online jest obsługiwany tylko system NTFS.

**Pytanie 12. Jakie typy plików i folderów z serwera można umieszczać w kopiach zapasowych?**<br/>
Odpowiedź 12. Obsługiwane są następujące typy:

- Zaszyfrowane
- Skompresowane
- Rozrzedzone
- Skompresowane i rozrzedzone
- Twarde linki: nieobsługiwane, pomijane
- Punkt ponownej analizy: nieobsługiwane, pomijane
- Zaszyfrowane i skompresowane: nieobsługiwane, pomijane
- Zaszyfrowane i rozrzedzone: nieobsługiwane, pomijane
- Skompresowany strumień: nieobsługiwane, pomijane
- Rozrzedzony strumień: nieobsługiwane, pomijane

**Pytanie 13. Jaki jest minimalny wymagany rozmiar folderu pamięci podręcznej?** <br/>
Odpowiedź 13. Rozmiar folderu pamięci podręcznej określa ilość danych, które można umieścić w kopii zapasowej. Folder pamięci podręcznej powinien udostępniać 5% miejsca wymaganego do przechowywania danych.

**Pytanie 14. Jeśli organizacja ma jeden magazyn kopii zapasowych, w jaki sposób można odizolować dane z jednego serwera od danych z innego serwera podczas przywracania danych?**<br/>
Odpowiedź 14. Wszystkie serwery, które są zarejestrowane w tym samym magazynie, mogą odzyskać dane umieszczone w kopii zapasowej przez inne serwery, *które korzystają z tego samego hasła*. Jeśli dane kopii zapasowej pewnych serwerów mają zostać odizolowane od innych serwerów w organizacji, należy użyć w przypadku tych serwerów innego hasła. Na przykład serwery zarządzania zasobami ludzkimi mogą korzystać z jednego hasła szyfrowania, serwery księgowości z drugiego, a serwery pamięci masowej z trzeciego.

**Pytanie 15. Czy można „migrować” dane kopii zapasowej pomiędzy subskrypcjami?** <br/>
Odpowiedź 15: Nie.

**Pytanie 16. Czy można „migrować” magazyn kopii zapasowych pomiędzy subskrypcjami?** <br/>
Odpowiedź 16: Nie. Magazyn jest tworzony na poziomie subskrypcji i po utworzeniu nie można go ponownie przypisać do innej subskrypcji.

**Pytanie 17. Czy agent usługi Azure Backup działa na serwerze, który korzysta z deduplikacji systemu Windows Server 2012?** <br/>
Odpowiedź 17: Tak. Podczas przygotowywania do wykonania kopii zapasowej usługa agenta konwertuje deduplikowane dane do zwykłej postaci. Następnie optymalizuje dane pod kątem utworzenia kopii zapasowej, szyfruje je, po czym wysyła zaszyfrowane dane do usługi kopii zapasowej online.

**Pytanie 18. Czy w przypadku anulowania uruchomionego zadania tworzenia kopii zapasowej przesyłane dane zostaną usunięte?** <br/>
Odpowiedź 18: Nie. Magazyn kopii zapasowych przechowuje dane kopii zapasowej przesyłane aż do momentu anulowania. Usługa Azure Backup używa mechanizmu, który podczas tworzenia kopii zapasowej co pewien czas dodaje punkty kontrolne do danych kopii zapasowej. Ponieważ w danych kopii zapasowej umieszczone są punkty kontrolne, następny proces kopii zapasowej może sprawdzić integralność plików. W przypadku danych, które wcześniej posłużyły do tworzenia kopii zapasowej, kolejny uruchomiony proces kopii zapasowej będzie przyrostowy. Przyrostowa kopia zapasowa zapewnia lepsze wykorzystanie przepustowości, ponieważ nie trzeba wielokrotnie przesyłać tych samych danych.

**Pytanie 19. Dlaczego wyświetlany jest komunikat ostrzegawczy „Proces tworzenia kopii zapasowych Azure nie został skonfigurowany dla tego serwera”, mimo że wcześniej zostały zaplanowane regularne kopie zapasowe?** <br/>
Odpowiedź 19: Ostrzeżenie to występuje, gdy ustawienia harmonogramu tworzenia kopii zapasowych przechowywane na serwerze lokalnym nie są takie same, jak ustawienia przechowywane w magazynie kopii zapasowych. Gdy serwer lub ustawienia zostały odzyskane do znanego, dobrego stanu, harmonogramy tworzenia kopii zapasowych mogą utracić synchronizację. W przypadku wystąpienia tego ostrzeżenia należy [zmienić konfigurację zasad tworzenia kopii zapasowej](backup-azure-manage-windows-server.md), a następnie wybrać opcję **Wykonaj kopię zapasową**, aby ponownie zsynchronizować lokalny serwer z platformą Azure.

**Pytanie 20. Jakie reguły zapory należy skonfigurować na potrzeby usługi Azure Backup?** <br/>
Odpowiedź 20. Aby zapewnić bezproblemową ochronę danych lokalnych i obciążeń na platformie Azure, warto umożliwić zaporze komunikowanie się z następującymi adresami URL:

- www.msftncsi.com
- \*.Microsoft.com
- \*.WindowsAzure.com
- \*.microsoftonline.com
- \*.windows.net

**Pytanie 21. Czy można zainstalować agenta usługi Azure Backup na maszynie wirtualnej, której kopia zapasowa została już utworzona przez usługę Azure Backup przy użyciu rozszerzenia maszyny wirtualnej?** <br/>
Odpowiedź 21. Naturalnie. Usługa Azure Backup udostępnia funkcję tworzenia kopii zapasowych na poziomie maszyny wirtualnej dla maszyn wirtualnych platformy Azure przy użyciu rozszerzenia maszyny wirtualnej. Można zainstalować agenta usługi Azure Backup w systemie operacyjnym Windows gościa do ochrony plików i folderów znajdujących się w tym systemie operacyjnym gościa.

**Pytanie 22. Czy można zainstalować agenta usługi Azure Backup na maszynie wirtualnej Azure, aby wykonywać kopie zapasowe plików i folderów znajdujących się w magazynie tymczasowym dostarczanym przez maszynę wirtualną platformy Azure?** <br/>
Odpowiedź 22. Agenta usługi Azure Backup można zainstalować w systemie operacyjnym Windows gościa i wykonywać kopie zapasowe plików i folderów do magazynu tymczasowego. Należy jednak pamiętać, że tworzenie kopii zapasowej zakończy się niepowodzeniem, gdy dane magazynu tymczasowego zostaną wyczyszczone. Ponadto usunięcie danych magazynu tymczasowego powoduje, że można je przywracać tylko do trwałego magazynu.

**Pytanie 23. Agent usługi Azure Backup został zainstalowany w celu ochrony plików i folderów. Czy można teraz zainstalować program SCDPM do pracy z agentem usługi Azure Backup, aby chronić obciążenia aplikacji lokalnych i maszyn wirtualnych na platformie Azure?** <br/>
Odpowiedź 23. Aby używać usługi Azure Backup z programem SCDPM, najlepiej najpierw zainstalować program SCDPM, a dopiero potem agenta usługi Azure Backup. Zapewnia to bezproblemową integrację agenta usługi Azure Backup z programem SCDPM oraz umożliwia ochronę plików i folderów, obciążeń aplikacji i maszyn wirtualnych na platformie Azure bezpośrednio z konsoli zarządzania programu SCDPM. Instalowanie programu SCDPM po zainstalowaniu agenta usługi Azure Backup do celów wymienionych powyżej nie jest zalecane ani obsługiwane.

**Pytanie 24. Jaka jest długość ścieżki pliku, którą można określić w ramach zasad usługi Azure Backup przy użyciu agenta usługi Azure Backup?** <br/>  
Odpowiedź 24. Agent usługi Azure Backup bazuje na systemie plików NTFS. [Specyfikacja długości ścieżki pliku jest ograniczona przez interfejs API systemu Windows](https://msdn.microsoft.com/library/aa365247.aspx#fully_qualified_vs._relative_paths). Gdy podczas tworzenia kopii zapasowej plików długość ścieżki pliku jest większa niż określona przez interfejs API systemu Windows, klienci mogą wybrać utworzenie kopii zapasowej plików z folderu nadrzędnego lub napędu dyskowego.  

**Pytanie 25. Jakie znaki są dozwolone w ścieżce pliku zasad usługi Azure Backup przy użyciu agenta usługi Azure Backup?** <br>  
Odpowiedź 25. Agent usługi Azure Backup bazuje na systemie plików NTFS. Dopuszcza [znaki obsługiwane w systemie NTFS](https://msdn.microsoft.com/library/aa365247.aspx#naming_conventions) w ramach specyfikacji pliku.  

**Pytanie 26. Czy można używać programu Azure Backup Server do tworzenia kopii zapasowej z odzyskiwaniem systemu od zera (BMR) serwera fizycznego?** <br/>
Odpowiedź 26. Tak.

**Pytanie 27. Czy można skonfigurować usługę Backup w taki sposób, aby wysyłała wiadomości e-mail, jeśli zadanie tworzenia kopii zapasowej nie powiedzie się?** <br/>
Odpowiedź 27. Tak, usługa Backup ma kilka alertów opartych na zdarzeniach, z których można korzystać za pomocą skryptu programu PowerShell. Pełny opis można znaleźć w temacie [Alert notifications](backup-azure-manage-vms.md#alert-notifications) (Powiadomienia za pomocą alertów)



## Tworzenie kopii zapasowej i przechowywanie
**Pytanie 1. Czy istnieje ograniczenie rozmiaru poszczególnych źródeł danych, dla których tworzona jest kopia zapasowa?** <br/>
Odpowiedź 1. Według stanu na sierpień 2015 r. maksymalny rozmiar źródła danych dla obsługiwanych systemów operacyjnych wynosi:

|L.p. | System operacyjny |  Maksymalny rozmiar źródła danych |
| :-------------: |:-------------| :-----|
|1| Windows Server 2012 lub nowszy| 54 400 GB|
|2| Windows 8 lub nowszy| 54 400 GB|
|3| Windows Server 2008, Windows Server 2008 R2 | 1700 GB|
|4| Windows 7 | 1700 GB|

W poniższej tabeli opisano sposób ustalania rozmiaru dla każdego źródła danych.

|   Źródło danych  |   Szczegóły |
| :-------------: |:-------------|
|Wolumin |Ilość danych kopii zapasowej z jednego woluminu komputera serwera lub klienta|
|Maszyna wirtualna z funkcją Hyper-V | Suma danych wszystkich dysków VHD maszyny wirtualnej, której kopia zapasowa jest wykonywana|
|Baza danych Microsoft SQL Server | Rozmiar pojedynczej bazy danych SQL, której kopia zapasowa jest wykonywana |
|Microsoft SharePoint |Suma baz danych z zawartością i danymi konfiguracyjnymi w farmie programu SharePoint, której kopia zapasowa jest wykonywana|
|Microsoft Exchange |Suma wszystkich baz danych programu Exchange w serwerze Exchange, którego kopia zapasowa jest wykonywana|
|Stan systemu/BMR |Każda pojedyncza kopia BMR lub stanu systemu komputera, którego kopia zapasowa jest wykonywana|

**Pytanie 2. Czy istnieją ograniczenia zaplanowanej częstotliwości wykonywania zadań tworzenia kopii zapasowej?**<br/>
Odpowiedź 2. Tak, zadania tworzenia kopii zapasowej na serwerze lub kliencie systemu Windows można uruchamiać maksymalnie trzy razy w ciągu dnia. Zadania tworzenia kopii zapasowej w programie System Center DPM można uruchamiać maksymalnie dwa razy dziennie. Zadanie tworzenia kopii zapasowej maszyn wirtualnych IaaS można uruchamiać jeden raz w ciągu dnia.

**Pytanie 3. Czy istnieje różnica między zasadami planowania w programie DPM i systemie Windows Server (tj. w systemie Windows Server bez programu DPM)?** <br/>
Odpowiedź 3. Tak. Za pomocą programu DPM można określić dzienne, tygodniowe, miesięczne i roczne harmonogramy uruchamiania zadań. System Windows Server (bez programu DPM) umożliwia określenie tylko dziennego i tygodniowego harmonogramu.

**Pytanie 4. Czy istnieje różnica między zasadami przechowywania w programie DPM i systemie Windows Server/kliencie systemu Windows (tj. w systemie Windows Server bez programu DPM)?**<br/>
Odpowiedź 4. Nie, zarówno program DPM, jak i system Windows Server/klient systemu Windows mają dzienne, tygodniowe, miesięczne i roczne zasady przechowywania.

**Pytanie 5. Czy można skonfigurować zasady przechowywania selektywnie, na przykład skonfigurować zasady tygodniowe i dziennie, ale nie roczne i miesięczne?**<br/>
Odpowiedź 5. Tak, struktura przechowywania usługi Azure Backup umożliwia w pełni elastyczne definiowanie zasad przechowywania zgodnie z wymaganiami użytkownika.

**Pytanie 6. Czy można „zaplanować utworzenie kopii zapasowej” o godzinie 18:00 i określić „zasady przechowywania” na inną godzinę?**<br/>
Odpowiedź 6. Nie. Zasady przechowywania mogą być stosowane wyłącznie w punktach kopii zapasowej. Na poniższej ilustracji określono zasady przechowywania dla kopii zapasowych wykonanych o godzinie 12:00 i 18:00. <br/>

![Harmonogram tworzenia kopii zapasowej i przechowywania](./media/backup-azure-backup-faq/Schedule.png)
<br/>

**Pytanie 7. Czy przyrostowa kopia jest przesyłana zgodnie z zaplanowanymi zasadami przechowywania?** <br/>
Odpowiedź 7. Nie, przyrostowa kopia jest wysyłana na podstawie czasu podanego na stronie harmonogramu tworzenia kopii zapasowych. Punkty, które mogą być przechowywane, są określane na podstawie zasad przechowywania.

**Pytanie 8. Czy w przypadku, gdy kopia zapasowa przechowywana jest przez długi czas, odzyskanie danych z wcześniejszego punktu danych trwa dłużej?** <br/>
Odpowiedź 8. Nie — czas odzyskania najstarszego i najnowszego punktu jest taki sam. Każdy punkt odzyskiwania zachowuje się jak pełny punkt.

**Pytanie 9. Skoro każdy punkt odzyskiwania jest traktowany jak pełny punkt, czy ma to wpływ na całkowitą fakturowaną przestrzeń dyskową dla kopii zapasowych?**<br/>
Odpowiedź 9.  Typowe produkty punktów długoterminowego przechowywania przechowują dane kopii zapasowych jako pełne punkty. Pełne punkty są *nieefektywne* z punktu widzenia magazynu, ale są łatwiejsze i szybsze do przywrócenia. Przyrostowe kopie są *efektywne* z punktu widzenia magazynu, ale wymagają przywrócenia łańcucha danych, co wpływa na czas odzyskiwania. Architektura magazynu usługi Azure Backup oferuje zalety obu rozwiązań dzięki optymalnemu przechowywaniu danych, umożliwiającemu szybkie ich przywrócenie, oraz niskim kosztom magazynowania. Takie podejście do magazynu danych zapewnia efektywne wykorzystanie przepustowości ruchu przychodzącego i wychodzącego. Zarówno ilość pamięci masowej, jak i czas potrzebny do odzyskania danych są ograniczone do minimum.

**Pytanie 10. Czy istnieje ograniczenie liczby punktów odzyskiwania, które można utworzyć?**<br/>
Odpowiedź 10. Nie. Firma Microsoft wyeliminowała ograniczenia dotyczące punktów odzyskiwania. Można utworzyć dowolną liczbę takich punktów.

**Pytanie 11. Dlaczego ilość danych przesyłanych w kopii zapasowej nie jest równa ilości danych, których kopia zapasowa jest tworzona?**<br/>
Odpowiedź 11. Wszystkie dane, których kopia zapasowa jest tworzona, są przed przesłaniem kompresowane i szyfrowane. Po zastosowaniu kompresji i szyfrowania dane w magazynie kopii zapasowych są mniejsze o 30–40%.

**Pytanie 12. Czy istnieje sposób dostosowania wielkości przepustowości, która będzie używana przez usługę Backup?**<br/>
Odpowiedź 12. Tak, aby ustawić przepustowość, należy skorzystać z opcji **Zmień właściwości** agenta kopii zapasowej. Można dostosować wielkość przepustowości oraz czas, w którym będzie ona używana. Więcej informacji można znaleźć w temacie [Network Throttling](../backup-configure-vault.md#enable-network-throttling) (Ograniczanie przepustowości sieci).

**Pytanie 13. Przepustowość sieci Internet jest ograniczona do ilości danych, jakie mają zostać umieszczone w kopii zapasowej. Czy istnieje sposób przeniesienia danych do określonej lokalizacji za pomocą dużego potoku sieciowego i wypchnięcia tych danych do platformy Azure?** <br/>
Odpowiedź 13. Kopię zapasową można wykonywać na platformie Azure przy użyciu standardowego procesu tworzenia kopii zapasowej online lub korzystając z usługi Azure Import/Export do przesyłania danych do magazynu obiektów blob na platformie Azure. Nie istnieją żadne inne sposoby umieszczenia danych kopii zapasowej w magazynie platformy Azure. Więcej informacji na temat korzystania z usługi Azure Import/Export za pomocą usługi Azure Backup można znaleźć w artykule [Offline Backup workflow](backup-azure-backup-import-export.md) (Przepływ pracy kopii zapasowej w trybie offline).


## Odzyskiwanie
**Pytanie 1. Ile zadań odzyskiwania można wykonać na danych, których kopie zapasowe utworzono na platformie Azure?**<br/>
Odpowiedź 1. Nie ma ograniczenia liczby operacji odzyskiwania z usługi Azure Backup.

**Pytanie 2. Czy trzeba płacić za ruch wyjściowy z centrum danych Azure podczas operacji odzyskiwania?**<br/>
Odpowiedź 2. Nie. Operacje odzyskiwania są bezpłatne i nie są naliczane opłaty za ruch wyjściowy.

## Bezpieczeństwo
**Pytanie 1. Czy dane wysyłane do platformy Azure są szyfrowane?** <br/>
Odpowiedź 1. Tak. Dane są szyfrowane na lokalnym serwerze/kliencie/komputerze z programem SCDPM za pomocą algorytmu szyfrowania AES-256, a następnie przesyłane za pośrednictwem bezpiecznego łącza HTTPS.

**Pytanie 2. Czy dane kopii zapasowej na platformie Azure są również szyfrowane?**<br/>
Odpowiedź 2. Tak. Dane wysyłane do platformy Azure pozostają zaszyfrowane (nieaktywne). Firma Microsoft nie odszyfrowuje danych kopii zapasowej w żadnym punkcie.

**Pytanie 3. Jaka jest minimalna długość klucza szyfrowania używanego do szyfrowania danych w kopii zapasowej?** <br/>
Odpowiedź 3. Klucz szyfrowania powinien mieć co najmniej 16 znaków.

**Pytanie 4. Co się stanie, jeśli klucz szyfrowania zostanie zgubiony przez użytkownika? Czy użytkownik lub firma Microsoft może odzyskać dane?** <br/>
Odpowiedź 4. Klucz używany do szyfrowania danych kopii zapasowej jest przechowywany tylko lokalnie przez klienta. Firma Microsoft nie przechowuje kopii klucza na platformie Azure i nie ma do niego dostępu. Jeśli klient zgubi klucz, firma Microsoft nie może odzyskać danych kopii zapasowej.
 

## Pamięć podręczna kopii zapasowej

**Pytanie 1. W jaki sposób można zmienić lokalizację pamięci podręcznej określoną dla agenta usługi Azure Backup?**<br/>
Odpowiedź 1. Aby zmienić lokalizację pamięci podręcznej, należy wykonać kolejno czynności wymienione na poniższej liście.
- Należy zatrzymać aparat kopii zapasowej za pomocą następującego polecenia w wierszu polecenia z podwyższonym poziomem uprawnień:

  ```PS C:\> Net stop obengine```

- Nie należy przenosić plików. Zamiast tego należy skopiować folder z obszarem pamięci podręcznej na inny dysk z wystarczającą ilością miejsca. Pierwotny obszar pamięci podręcznej można usunąć po potwierdzeniu, że kopie zapasowe działają z nowym obszarem pamięci podręcznej.

- Należy zaktualizować następujące wpisy rejestru ścieżką do nowego folderu obszaru pamięci podręcznej.<br/>

|Ścieżka rejestru | Klucz rejestru | Wartość |
| ------ | ------- | ------|
| `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config` | ScratchLocation | *Nowa lokalizacja folderu pamięci podręcznej* |
| `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider` | ScratchLocation | *Nowa lokalizacja folderu pamięci podręcznej* |

- Należy ponownie uruchomić aparat kopii zapasowej, wykonując następujące polecenie w wierszu polecenia z podwyższonym poziomem uprawnień:

  ```PS C:\> Net start obengine```

  Po pomyślnym zakończeniu tworzenia kopii zapasowej w nowej lokalizacji pamięci podręcznej można usunąć pierwotny folder pamięci podręcznej.

**Pytanie 2. Gdzie można umieścić folder pamięci podręcznej, aby agent usługi Azure Backup działał zgodnie z oczekiwaniami?**<br/>
Odpowiedź 2. Nie zaleca się używać następujących lokalizacji dla folderu pamięci podręcznej:

- Udział sieciowy lub nośniki wymienne: folder pamięci podręcznej musi być lokalny dla serwera, który wymaga wykonywania kopii zapasowych przy użyciu usługi kopii zapasowej online. Lokalizacje sieciowe ani nośniki wymienne, takie jak dyski USB, nie są obsługiwane.
- Woluminy offline: folder pamięci podręcznej musi być w trybie online dla oczekiwanej kopii zapasowej wykonywanej za pomocą agenta usługi Azure Backup.

**Pytanie 3. Czy jakiekolwiek atrybuty folderu pamięci podręcznej nie są obsługiwane?**<br/>
Odpowiedź 3. Następujące atrybuty folderu pamięci podręcznej ani ich kombinacje nie są obsługiwane:

- Zaszyfrowane
- Deduplikowane
- Skompresowane
- Rozrzedzone
- Punkt ponownej analizy

Aby działanie agenta usługi Azure Backup przebiegało zgodnie z oczekiwaniami, ani folder pamięci podręcznej, ani dysk VHD metadanych nie powinny mieć żadnego z powyższych atrybutów.



<!--HONumber=jun16_HO2-->


