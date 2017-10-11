<!--author=SharS last changed: 9/17/15-->

W tej procedurze obejmują:

1. [Przygotowanie do uruchomienia pliku wykonywalnego Element utrzymujący](#to-prepare-to-run-the-maintainer) .
2. [Przygotowanie bazy danych zawartości i Kosza do natychmiastowego usunięcia obiektów blob oddzielone](#to-prepare-the-content-database-and-recycle-bin-to-immediately-delete-orphaned-blobs).
3. [Uruchom Maintainer.exe](#to-run-the-maintainer).
4. [Przywróć bazę danych zawartości i ustawienia Kosza](#to-revert-the-content-database-and-recycle-bin-settings).

#### <a name="to-prepare-to-run-the-maintainer"></a>Aby przygotować się do uruchomienia Element utrzymujący
1. Na serwerze frontonu sieci Web Otwórz powłokę zarządzania programu SharePoint 2013 jako administrator.
2. Przejdź do folderu *dysk rozruchowy*: \Program Files\Microsoft 10.50\Maintainer zdalnego magazynu obiektów Blob SQL\.
3. Zmień nazwę **Microsoft.Data.SqlRemoteBlobs.Maintainer.exe.config** do **web.config**.
4. Użyj `aspnet_regiis -pdf connectionStrings` do odszyfrowywania pliku web.config.
5. W pliku web.config odszyfrowane w obszarze `connectionStrings` węzła, Dodaj parametry połączenia dla wystąpienia programu SQL server i nazwę bazy danych zawartości. Zobacz poniższy przykład.
   
    `<add name=”RBSMaintainerConnectionWSSContent” connectionString="Data Source=SHRPT13-SQL12\SHRPT13;Initial Catalog=WSS_Content;Integrated Security=True;Application Name=&quot;Remote Blob Storage Maintainer for WSS_Content&quot;" providerName="System.Data.SqlClient" />`
6. Użyj `aspnet_regiis –pef connectionStrings` zaszyfrowanie pliku web.config. 
7. Zmień nazwę pliku web.config na Microsoft.Data.SqlRemoteBlobs.Maintainer.exe.config. 

#### <a name="to-prepare-the-content-database-and-recycle-bin-to-immediately-delete-orphaned-blobs"></a>Aby przygotować zawartość bazy danych i Kosza można natychmiast usunąć oddzielone obiektów blob
1. Na serwerze SQL w programie SQL Management Studio, uruchomić następujące kwerendy aktualizacji dla docelowej bazy danych zawartości: 
   
       `use WSS_Content`
   
       `exec mssqlrbs.rbs_sp_set_config_value ‘garbage_collection_time_window’ , ’time 00:00:00’`
   
       `exec mssqlrbs.rbs_sp_set_config_value ‘delete_scan_period’ , ’time 00:00:00’`
2. Na serwerze frontonu sieci web w obszarze **administracji centralnej**, Edytuj **ustawienia ogólne aplikacji sieci Web** dla żądanej zawartości bazy danych można tymczasowo wyłączyć Kosza. Ta akcja spowoduje usunięcie również zawartości Kosza na wszystkie powiązane zbiorach witryn. Aby to zrobić, kliknij przycisk **administracji centralnej** -> **Zarządzanie aplikacjami** -> **aplikacji sieci Web (Zarządzaj aplikacji sieci web)**  ->  **SharePoint — 80** -> **ogólne aplikacji ustawienia**. Ustaw **stan Kosza** do **OFF**.
   
    ![Ustawienia ogólne aplikacji sieci Web](./media/storsimple-sharepoint-adapter-garbage-collection/HCS_WebApplicationGeneralSettings-include.png)

#### <a name="to-run-the-maintainer"></a>Aby uruchomić Element utrzymujący
* Na serwerze frontonu sieci web w powłoce zarządzania programu SharePoint 2013, uruchom Element utrzymujący w następujący sposób:
  
      `Microsoft.Data.SqlRemoteBlobs.Maintainer.exe -ConnectionStringName RBSMaintainerConnectionWSSContent -Operation GarbageCollection -GarbageCollectionPhases rdo`
  
  > [!NOTE]
  > Tylko `GarbageCollection` operacja jest obsługiwana dla urządzenia StorSimple w tej chwili. Należy również zauważyć, że parametry wystawiony dla Microsoft.Data.SqlRemoteBlobs.Maintainer.exe jest uwzględniana wielkość liter. 
  > 
  > 

#### <a name="to-revert-the-content-database-and-recycle-bin-settings"></a>Aby przywrócić bazę danych zawartości i ustawienia Kosza
1. Na serwerze SQL w programie SQL Management Studio, uruchomić następujące kwerendy aktualizacji dla docelowej bazy danych zawartości:
   
      `use WSS_Content`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘garbage_collection_time_window’ , ‘days 30’`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘delete_scan_period’ , ’days 30’`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘orphan_scan_period’ , ’days 30’`
2. Na serwerze frontonu sieci web w **administracji centralnej**, Edytuj **ustawienia ogólne aplikacji sieci Web** dla żądanej zawartości bazy danych do ponownego włączania Kosza. Aby to zrobić, kliknij przycisk **administracji centralnej** -> **Zarządzanie aplikacjami** -> **aplikacji sieci Web (Zarządzaj aplikacji sieci web)**  ->  **SharePoint — 80** -> **ogólne aplikacji ustawienia**. Ustaw stan Bin odtworzenia **ON**.

