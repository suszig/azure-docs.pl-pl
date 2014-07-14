<properties  linkid="manage-scenarios-choose-web-app-service" urlDisplayName="Web Options for Azure" pageTitle="Azure Web Sites, Cloud Services and Virtual Machines comparison" metaKeywords="Cloud Services, Virtual Machines, Web Sites" description="Learn when to use Azure Web Sites, Cloud Services, and Virtual Machines for hosting web applications. Review a feature comparison." metaCanonical="" services="web-sites,virtual-machines,cloud-services" documentationCenter="" title=" Cloud Services" authors="jroth" solutions="" manager="paulettm" editor="mollybos" />

# Porównanie usług systemu Azure: Witryny sieci Web, Usługi w chmurze i Maszyny wirtualne

System Azure oferuje kilka sposobów hostowania aplikacji sieci Web, takich jak usługi [Witryny sieci Web][1], [Usługi w chmurze][2] i
[Maszyny wirtualne][3]. Przyglądając się tym opcjom, możesz się zastanawiać, która najlepiej odpowiada Twoim potrzebom lub czym różni się model IaaS od modelu PaaS. Niniejszy artykuł ułatwia zrozumienie tych zagadnień i wybranie rozwiązania dostosowanego do aktualnego scenariusza sieci Web. Mimo że wszystkie trzy opcje umożliwiają uruchamianie wysoce skalowalnych aplikacji w systemie Azure, znajomość różnic między nimi może ułatwić optymalny wybór.

W wielu sytuacjach usługa Witryny sieci Web systemu Azure jest najlepszym rozwiązaniem. Udostępnia ona proste i elastyczne opcje wdrażania i zarządzania oraz obsługuje hosting witryn sieci Web o dużym obciążeniu. Możesz szybko utworzyć nową witrynę sieci Web przy użyciu popularnego oprogramowania (takiego jak WordPress), korzystając z galerii aplikacji sieci Web, lub możesz przenieść istniejącą witrynę sieci Web do usługi Witryny sieci Web systemu Azure. Za pomocą zestawu [Azure WebJobs SDK][4] (obecnie w wersji Preview) możesz również dodać funkcję przetwarzania zadań w tle.

Aplikacje sieci Web można również hostować w usłudze Maszyny wirtualne systemu Azure lub Usługi w chmurze systemu Azure. Te rozwiązania sprawdzają się wtedy, gdy warstwa sieci Web wymaga dodatkowego poziomu kontroli i dostosowania. Zwiększenie kontroli wiąże się jednak ze wzrostem złożoności procesów tworzenia i wdrażania aplikacji oraz zarządzania nimi. Na poniższym diagramie zilustrowano relacje między omawianymi trzema rozwiązaniami.

![Diagram opcji wyboru](./media/choose-web-site-cloud-service-vm/Websites_CloudServices_VMs_2.png)

Usługa Witryny sieci Web jest łatwiejsza do konfigurowania, zarządzania i monitorowania, ale udostępnia mniej opcji konfiguracji. Najważniejsze jest to, że jeśli nie ma istotnego powodu, aby umieszczać fronton sieci Web w usłudze Maszyny wirtualne lub Usługi w chmurze, należy korzystać z usługi Witryny sieci Web systemu Azure. W pozostałej części tego dokumentu znajdziesz informacje potrzebne do świadomego podjęcia decyzji. Są to:

* [Scenariusze](#scenarios)
* [Podsumowania dotyczące usług](#services)
* [Porównanie funkcji](#features)

## <a name="scenarios"></a>Scenariusze

### Jestem właścicielem małej firmy i potrzebuję niedrogiego hostingu mojej witryny uwzględniającego dalsze plany rozwoju

W przypadku tego scenariusza najlepszym rozwiązaniem jest usługa Witryny sieci Web systemu Azure, ponieważ można zacząć z niej korzystać bezpłatnie i dodawać kolejne funkcje w miarę potrzeb. Na przykład każda bezpłatna witryna sieci Web ma domenę systemu Azure (*nazwa_firmy*.azurewebsites.net). Gdy zechcesz korzystać z własnej domeny, możesz dodać tę funkcję za jedyne 9,80 USD miesięcznie (stan na styczeń 2014). Istnieje wiele innych usług i opcji skalowania umożliwiających rozbudowę funkcji witryny wraz ze wzrostem zapotrzebowania użytkowników. Za pomocą usługi **Witryny sieci Web systemu Azure** możesz:

* Zacząć od warstwy bezpłatnej, a następnie skalować rozwiązanie zgodnie
  z zapotrzebowaniem.
* Szybko konfigurować popularne aplikacje sieci Web, takie jak
  WordPress, korzystając z galerii aplikacji.
* Dodawać do aplikacji kolejne usługi i funkcje systemu Azure w miarę
  potrzeb.
* Zabezpieczyć witrynę sieci Web przy użyciu protokołu HTTPS i
  certyfikatu dostarczonego dla domeny o nazwie
  *Twoja_firma*.azurewebsites.net.

### Jestem projektantem sieci Web i grafikiem — chcę projektować i konstruować witryny sieci Web dla swoich klientów

W przypadku deweloperów sieci Web usługa Witryny sieci Web systemu Azure oferuje wszystkie elementy potrzebne do tworzenia zaawansowanych aplikacji sieci Web. Usługa Witryny sieci Web umożliwia ścisłą integrację z takimi narzędziami jak Visual Studio i Baza danych SQL. Za pomocą usługi **Witryny sieci Web** deweloperzy mogą:

* Tworzyć [zadania zautomatyzowane][5] przy użyciu narzędzi wiersza
  polecenia.
* Korzystać z popularnych języków, takich jak [.Net][6], [PHP][7],
  [Node.js][8] i [Python][9].
* Wybierać spośród trzech poziomów skalowania — aż do bardzo dużych
  pojemności.
* Integrować swoje rozwiązania z innymi usługami systemu Azure, takimi
  jak [Baza danych SQL][10], [Magistrala usług][11] i [Magazyn][12], lub
  z ofertami partnerów dostępnymi w [Sklepie Azure][13], takimi jak
  MySQL i MongoDB.
* Integrować swoje rozwiązania z takimi narzędziami jak Visual Studio,
  Git, WebMatrix, WebDeploy, TFS i FTP.

### Przeprowadzam migrację swojej wielowarstwowej aplikacji z frontonem sieci Web do chmury

Jeśli masz uruchomioną wielowarstwową aplikację, taką jak serwer sieci Web, który komunikuje się z serwerem bazy danych w celu przechowywania i pobierania danych witryny sieci Web, w systemie Azure masz kilka opcji architektonicznych do wyboru. Są to: Witryny sieci Web, Usługi w chmurze i Maszyny wirtualne. Pierwsza z nich, **Witryny sieci Web**, jest odpowiednia dla warstwy sieci Web aktualnego rozwiązania i można z niej korzystać w połączeniu z usługą Baza danych SQL Azure do tworzenia architektury dwuwarstwowej. Usługa Witryny sieci Web umożliwia również uruchamianie procesów w tle oraz uruchamianie długo działających procesów za pomocą zestawu Azure WebJobs SDK w wersji Preview. Jeśli potrzebna jest bardziej złożona architektura lub bardziej elastyczne opcje skalowania, lepszym rozwiązaniem będzie usługa Maszyny wirtualne lub Usługi w chmurze.

Usługa **Usługi w chmurze** umożliwia:

* Hostowanie usług sieci Web, usług warstwy środkowej i usług zaplecza w
  ramach skalowalnych ról sieci Web i procesów roboczych.
* Hostowanie tylko usług warstwy środkowej i usług zaplecza w ramach ról
  procesów roboczych i pozostawienie frontonu w usłudze Witryny sieci
  Web systemu Azure.
* Niezależne od siebie skalowanie usług frontonu i zaplecza.

Usługa **Maszyny wirtualne** umożliwia:

* Łatwiejsze migrowanie wysoce dostosowanych środowisk jako obrazu
  maszyny wirtualnej.
* Uruchamianie oprogramowania lub usług, których nie można konfigurować
  w usłudze Witryny sieci Web lub Usługi w chmurze.

### Działanie mojej aplikacji zależy od wysoce dostosowanych środowisk systemu Windows lub Linux

Jeśli Twoja aplikacja wymaga złożonej instalacji lub konfiguracji oprogramowania i systemu operacyjnego, usługa Maszyny wirtualne jest prawdopodobnie najlepszym rozwiązaniem. Za pomocą usługi **Maszyny wirtualne** możesz:

* Zacząć od systemu operacyjnego, takiego jak Windows lub Linux,
  korzystając z galerii maszyn wirtualnych, a następnie dostosować ten
  system do wymagań aplikacji.
* Utworzyć i przekazać obraz niestandardowy istniejącego serwera
  lokalnego, tak aby działał na maszynie wirtualnej w systemie Azure.

### Moja witryna korzysta z oprogramowania open source i chcę ją hostować w systemie Azure

Wszystkie trzy opcje umożliwiają hostowanie języków i platform open source. **Usługi w chmurze** wymagają użycia zadań uruchamiania do instalowania i konfigurowania każdego wymaganego oprogramowania open source uruchamianego w systemie Windows. W przypadku usługi **Maszyny wirtualne** oprogramowanie jest instalowane i konfigurowane na obrazie maszyny w środowisku Windows lub Linux. Jeśli używana platforma open source jest obsługiwana w usłudze Witryny sieci Web, hostowanie tych typów aplikacji jest łatwiejsze, ponieważ usługę tę można automatycznie konfigurować do obsługi języków i platform potrzebnych dla danej aplikacji. Usługa **Witryny sieci Web** umożliwia:

* Korzystanie z wielu popularnych języków open source, takich jak
  [.NET][6], [PHP][7], [Node.js][8] i [Python][9].
* Konfigurowanie programów WordPress, Drupal, Umbraco, DNN i wielu
  innych aplikacji sieci Web innych producentów.
* Migrowanie istniejącej aplikacji lub utworzenie nowej aplikacji w
  galerii aplikacji.

### Mam aplikację biznesową, która musi się łączyć z siecią korporacyjną

Jeśli chcesz utworzyć aplikację biznesową, Twoja witryna sieci Web może wymagać bezpośredniego dostępu do usług lub danych w sieci korporacyjnej. Jest to możliwe w usługach **Witryny sieci Web**, **Usługi w chmurze** i **Maszyny wirtualne**. Poszczególne rozwiązania różnią się od siebie w następującym zakresie:

* Usługa Witryny sieci Web może się bezpiecznie łączyć z zasobami
  lokalnymi przy użyciu usługi przekazywania usługi Magistrala usług.
  Dzięki temu usługi w sieci korporacyjnej mogą wykonywać zdania w
  imieniu aplikacji sieci Web bez konieczności przenoszenia wszystkiego
  do chmury ani konfigurowania sieci wirtualnej.
* Usługi Maszyny wirtualne i Usługi w chmurze mogą korzystać z sieci
  wirtualnej. Dzięki temu maszyny uruchomione w systemie Azure mogą się
  łączyć z siecią lokalną. System Azure staje się tym samym
  rozszerzeniem korporacyjnego centrum danych.

### Chcę hostować interfejs REST API lub usługę sieci Web dla klientów mobilnych

Usługi sieci Web oparte na protokole HTTP umożliwiają obsługę wielu klientów, w tym klientów mobilnych. Takie platformy jak ASP.NET Web API integrują się ze środowiskiem Visual Studio, co ułatwia tworzenie i używanie usług REST. Te usługi są udostępniane z punktu końcowego sieci Web, więc w systemie Azure można zapewnić obsługę tego scenariusza przy użyciu dowolnej techniki hostingu sieci Web. **Witryny sieci Web** to jednak najlepsze rozwiązanie do hostingu interfejsów REST API. Za pomocą usługi Witryny sieci Web możesz:

* Szybko utworzyć witrynę sieci Web do hostingu usługi sieci Web HTTP w
  jednym z globalnie rozproszonych centrów danych systemu Azure.
* Migrować istniejące usługi lub tworzyć nowe, z możliwością korzystania
  ze składnika ASP.NET Web API w programie Visual Studio.
* Dotrzymać warunków umowy SLA w zakresie dostępności dla pojedynczego
  wystąpienia lub skalować rozwiązanie na wiele maszyn dedykowanych.
* Udostępnić interfejsy REST API dowolnym klientom HTTP, w tym klientom
  mobilnym, za pomocą opublikowanej witryny.

## <a name="services"></a>Podsumowania dotyczące usług

Usługa [Witryny sieci Web systemu Azure][1] umożliwia szybkie tworzenie wysoko skalowalnych witryn sieci Web w systemie Azure. Za pomocą portalu systemu Azure lub narzędzi wiersza polecenia można skonfigurować witrynę sieci Web, korzystając z popularnych języków, takich jak .NET, PHP, Node.js i Python. Obsługiwane platformy są już wdrożone i nie wymagają dodatkowego instalowania. Galeria witryn sieci Web systemu Azure zawiera wiele aplikacji innych producentów, takich jak Drupal i WordPress, a także architektur deweloperskich, takich jak Django i CakePHP. Po utworzeniu witryny można przeprowadzić migrację istniejącej witryny sieci Web lub utworzyć zupełnie nową witrynę. Usługa Witryny sieci Web eliminuje konieczność zarządzania sprzętem fizycznym, a także udostępnia kilka opcji skalowania. Można przejść od modelu wielodostępnego (udostępnianie obsługi wielu podmiotom) do trybu standardowego, w którym ruch przychodzący jest obsługiwany przez dedykowane maszyny. Usługa Witryny sieci Web umożliwia również integrację z innymi usługami systemu Azure, takimi jak Baza danych SQL, Magistrala usług i Magazyn. Za pomocą zestawu [Azure WebJobs SDK][4] w wersji Preview można dodać funkcję przetwarzania w tle. Podsumowując, usługa Witryny sieci Web systemu Azure ułatwia skoncentrowanie się na opracowywaniu aplikacji, ponieważ obsługuje wiele języków i liczne aplikacje open source oraz metodyki wdrażania (FTP, Git, Web Deploy i TFS). Jeśli nie ma specjalnych wymagań wymuszających korzystanie z usług Maszyny wirtualne czy Usługi w chmurze, witryna sieci Web systemu Azure będzie najlepszym rozwiązaniem.

[Usługi w chmurze][2] umożliwiają tworzenie wysoko dostępnych i skalowalnych aplikacji sieci Web w zaawansowanym środowisku „Platforma jako usługa” (PaaS, Platform as a Service). W przeciwieństwie do usługi Witryny sieci Web usługę w chmurze najpierw tworzy się w środowisku deweloperskim, takim jak Visual Studio, a potem wdraża w systemie Azure. Platformy, takie jak PHP, wymagają wdrażania niestandardowego i wykonania czynności lub zadań powodujących zainstalowanie tej platformy przy uruchamianiu roli. Główną zaletą usług w chmurze jest możliwość obsługi wielu złożonych architektur wielowarstwowych. Pojedyncza usługa w chmurze może się składać z roli sieci Web frontonu i co najmniej jednej roli procesu roboczego. Każdą warstwę można skalować indywidualnie. Usługa zapewnia również większy poziom kontroli nad infrastrukturą aplikacji sieci Web. Można na przykład użyć pulpitu zdalnego na maszynach, na których są uruchomione wystąpienia ról. Można również oskryptować bardziej zaawansowane zmiany konfiguracji usług IIS i maszyn uwzględniane przy uruchamianiu roli, w tym zadania wymagające kontroli administratora.

Usługa [Maszyny wirtualne][3] umożliwia uruchamianie aplikacji sieci Web na maszynach wirtualnych w systemie Azure. Funkcja ta jest znana pod nazwą „Infrastruktura jako usługa” (IaaS, Infrastructure as a Service). Z poziomu portalu można utworzyć nowe maszyny z systemem Windows Server lub Linux albo przekazać obraz istniejącej maszyny wirtualnej. Usługa Maszyny wirtualne zapewnia najwięcej kontroli nad systemem operacyjnym, konfiguracją oraz zainstalowanymi usługami i oprogramowaniem. To dobre rozwiązanie w przypadku szybkiego migrowania złożonych lokalnych aplikacji sieci Web do chmury, ponieważ maszyny można przenosić jako całość. Dzięki sieciom wirtualnym można również łączyć te maszyny wirtualne z lokalnymi sieciami korporacyjnymi. Podobnie jak w przypadku usług w chmurze można korzystać ze zdalnego dostępu do tych maszyn i możliwości wprowadzania zmian w konfiguracji na poziomie administracyjnym. W przeciwieństwie jednak do usług Witryny sieci Web i Usługi w chmurze zarządzanie obrazami maszyn wirtualnych i architekturą aplikacji musi się odbywać całkowicie na poziomie infrastruktury. Typowym przykładem może być konieczność samodzielnego stosowania poprawek do systemu operacyjnego.

## <a name="features"></a>Porównanie funkcji

W poniższej tabeli przedstawiono porównanie funkcji usług Witryny sieci Web, Usługi w chmurze i Maszyny wirtualne, które ułatwi Ci wybranie najlepszego rozwiązania. Pola z gwiazdką mają dodatkowe objaśnienia pod tabelą.

<table  cellspacing="0" border="1">
<tr>
   <th  align="left" valign="middle">Funkcja</th>

   <th  align="left" valign="middle">Witryny sieci Web</th>

   <th  align="left" valign="middle">Usługi w chmurze (role sieci Web)</th>

   <th  align="left" valign="middle">Maszyny wirtualne</th>

</tr>

<tr>
   <td  valign="middle"><p>Dostęp do takich usług jak Magistrala usług, Magazyn, Baza danych SQL</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>Hosting sieci Web lub warstwa usług sieci Web w przypadku architektury wielowarstwowej</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>Hosting warstwy środkowej w przypadku architektury wielowarstwowej</p>
</td>

   <td  valign="middle" />

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>Zintegrowana obsługa modelu „MySQL jako usługa”</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X <sup>1</sup>
</td>

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>Obsługa platformy ASP.NET, klasycznych aplikacji ASP i języków Node.js, PHP, Python</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>Skalowanie do wielu wystąpień bez ponownego wdrażania</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X <sup>2</sup>
</td>

</tr>

<tr>
   <td  valign="middle"><p>Obsługa protokołu SSL</p>
</td>

   <td  valign="middle">X <sup>3</sup>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>Integracja z programem Visual Studio</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>Zdalne debugowanie</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>Wdrażanie kodu przy użyciu oprogramowania TFS</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>Wdrażanie kodu przy użyciu oprogramowania GIT, FTP</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle" />

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>Wdrażanie kodu przy użyciu oprogramowania Web Deploy</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle"><sup>4</sup>
</td>

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>Obsługa programu WebMatrix</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle" />

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>Niemal błyskawiczne wdrażanie</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle" />

   <td  valign="middle" />

</tr>

<tr>
   <td  valign="middle"><p>Zawartość i konfiguracja wspólne dla wystąpień</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle" />

   <td  valign="middle" />

</tr>

<tr>
   <td  valign="middle"><p>Skalowanie do większych maszyn bez ponownego wdrażania</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle" />

   <td  valign="middle" />

</tr>

<tr>
   <td  valign="middle"><p>Środowiska wielokrotnego wdrażania (produkcyjne i przemieszczania)</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

   <td  valign="middle" />

</tr>

<tr>
   <td  valign="middle"><p>Izolacja sieci dzięki usłudze Sieć wirtualna systemu Azure</p>
</td>

   <td  valign="middle" />

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>Obsługa usługi Menedżer ruchu systemu Azure</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>Dostęp do serwerów za pomocą pulpitu zdalnego</p>
</td>

   <td  valign="middle" />

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>Możliwość definiowania/wykonywania zadań uruchamiania</p>
</td>

   <td  valign="middle" />

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>Automatyczne zarządzanie aktualizowaniem systemu operacyjnego</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

   <td  valign="middle" />

</tr>

<tr>
   <td  valign="middle"><p>Zintegrowane monitorowanie punktu końcowego</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>Bezproblemowe przełączanie platformy (32-bitowa/64-bitowa)</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

   <td  valign="middle" />

</tr>

</table>

<sup>1</sup> Role sieci Web lub procesów roboczych mogą obejmować integrację modelu „MySQL jako usługa” dzięki ofercie produktów ClearDB, ale nie w ramach przepływu pracy Portalu zarządzania.

<sup>2</sup> Mimo że usługa Maszyny wirtualne może obsługiwać skalowanie do wielu wystąpień, usługi działające na tych maszynach muszą być napisane pod kątem obsługi takiego skalowania. W celu przekierowywania żądań między maszynami należy skonfigurować dodatkową usługę równoważenia obciążenia. Dla wszystkich maszyn korzystających z tej samej roli należy utworzyć grupę koligacji w celu zapewnienia im ochrony przed równoczesnym ponownym uruchamianiem po awariach dotyczących obsługi lub sprzętu.

<sup>3</sup> W przypadku usługi Witryny sieci Web protokół SSL dla niestandardowych nazw domen jest obsługiwany tylko dla trybu standardowego. Aby uzyskać więcej informacji na temat używania protokołu SSL wraz z usługą Witryny sieci Web, zobacz [Konfigurowanie certyfikatu SSL dla witryny sieci Web systemu Azure][14].

<sup>4</sup> W przypadku usług w chmurze program Web Deploy jest obsługiwany podczas wdrażania w rolach o jednym wystąpieniu. Aby zostały spełnione warunki umowy Azure SLA, role produkcyjne wymagają jednak wielu wystąpień. Z tego powodu program Web Deploy nie jest odpowiednim mechanizmem wdrażania w przypadku usług w chmurze w środowisku produkcyjnym.



[1]: http://go.microsoft.com/fwlink/?LinkId=306051
[2]: http://go.microsoft.com/fwlink/?LinkId=306052
[3]: http://go.microsoft.com/fwlink/?LinkID=306053
[4]: http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/getting-started-with-windows-azure-webjobs
[5]: http://www.windowsazure.com/en-us/documentation/scripts/?services=web-sites
[6]: http://www.windowsazure.com/en-us/develop/net/
[7]: http://www.windowsazure.com/en-us/develop/php/
[8]: http://www.windowsazure.com/en-us/develop/nodejs/
[9]: http://www.windowsazure.com/en-us/develop/python/
[10]: http://www.windowsazure.com/en-us/documentation/services/sql-database/
[11]: http://www.windowsazure.com/en-us/documentation/services/service-bus/
[12]: http://www.windowsazure.com/en-us/documentation/services/storage/
[13]: http://www.windowsazure.com/en-us/gallery/store/
[14]: http://www.windowsazure.com/en-us/develop/net/common-tasks/enable-ssl-web-site/