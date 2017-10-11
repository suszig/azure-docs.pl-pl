<!--author=SharS last changed: 9/17/15-->

### <a name="upgrade-sharepoint-2010-to-sharepoint-2013-and-then-install-the-storsomple-adapter-for-sharepoint"></a>Uaktualnienie programu SharePoint 2010 do programu SharePoint 2013, a następnie zainstaluj kartę StorSomple dla programu SharePoint
> [!IMPORTANT]
> Wszystkie pliki, które wcześniej zostały przeniesione do magazynu zewnętrznego za pośrednictwem SPZ nie będą dostępne dopiero po zakończeniu uaktualniania i ponownego włączenia funkcji SPZ. Aby ograniczyć wpływ użytkownika, należy wykonać wszelkie uaktualnienia lub ponownej instalacji w oknie zaplanowanej konserwacji.
> 
> 

#### <a name="to-upgrade-sharepoint-2010-to-sharepoint-2013-and-then-install-the-adapter"></a>Aby uaktualnić program SharePoint 2010 do programu SharePoint 2013, a następnie zainstaluj kartę
1. W farmie programu SharePoint 2010 należy pamiętać, externalized obiektów blob i zawartości baz danych, dla których włączono SPZ ścieżki magazynu obiektów BLOB. 
2. Instalowanie i konfigurowanie nowej farmy programu SharePoint 2013. 
3. Przenoszenie baz danych, aplikacji i kolekcji witryn z farmy programu SharePoint 2010 do nowej farmy programu SharePoint 2013. Aby uzyskać instrukcje, przejdź do [Omówienie procesu uaktualniania do programu SharePoint 2013](https://technet.microsoft.com/library/cc262483.aspx).
4. Zainstaluj StorSimple dla programu SharePoint w nowej farmie serwerów. Przejdź do [zainstalować StorSimple Adapter dla programu SharePoint](#install-the-storsimple-adapter-for-sharepoint) procedury.
5. Korzystając z informacji zanotowaną w kroku 1, Włącz SPZ dla tego samego zestawu baz danych zawartości i podaj ścieżkę tego samego magazynu obiektów BLOB, używanego w instalacji programu SharePoint 2010. Przejdź do [skonfigurować SPZ](#configure-rbs) procedury. Po wykonaniu tego kroku pliki wcześniej externalized powinien być dostępny w nowej farmie serwerów. 

### <a name="upgrade-the-storsimple-adapter-for-sharepoint"></a>Uaktualnienie karty StorSimple dla programu SharePoint
> [!IMPORTANT]
> Należy zaplanować tego uaktualnienia wystąpić w trakcie okna zaplanowanej konserwacji z następujących powodów:
> 
> * Wcześniej externalized zawartość nie będzie dostępne, dopóki nie zostanie ponownie zainstalowana karta.
> * Wszelkie zawartość przesłana do lokacji, po odinstalować poprzednią wersję karty StorSimple dla programu SharePoint, ale przed zainstalowaniem nowej wersji, będą przechowywane w bazie danych zawartości. Należy przenieść tej zawartości na urządzeniu StorSimple, po zainstalowaniu nowej karty. Można użyć programu Microsoft` RBS Migrate()` dołączone do programu SharePoint, aby przenieść zawartość polecenia cmdlet programu PowerShell. Aby uzyskać więcej informacji, zobacz [migrowania zawartości do lub wychodzący SPZ](https://technet.microsoft.com/library/ff628255.aspx). 
> 
> 

#### <a name="to-upgrade-the-storsimple-adapter-for-sharepoint"></a>Aby uaktualnić karty StorSimple dla programu SharePoint
1. Odinstaluj poprzednią wersję karty StorSimple dla programu SharePoint.
   
   > [!NOTE]
   > SPZ zostanie automatycznie wyłączony w bazach danych zawartości. Jednak pozostanie istniejące obiekty BLOB na urządzeniu StorSimple. Ponieważ SPZ jest wyłączona i nie zostały poddane migracji obiektów blob do bazy danych zawartości, wszystkie żądania dla tych obiektów blob nie powiedzie się. 
   > 
   > 
2. Zainstaluj nową kartę StorSimple dla programu SharePoint. Nowa karta będzie automatycznie rozpoznawać baz danych zawartości, które wcześniej były włączone lub wyłączone SPZ i użyje poprzednie ustawienia.

