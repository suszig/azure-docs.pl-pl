<properties
   pageTitle="クラウド サービスのロールに .NET をインストールする"
   description="この記事では、クラウド サービスの Web ロールと worker ロールに .NET Framework を手動でインストールする方法について説明します。"
   services="cloud-services"
   documentationCenter=".net"
   authors="sbtron"
   manager="timlt"
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="12/11/2015"
   ms.author="saurabh"/>

# クラウド サービスのロールに .NET をインストールする 

この記事では、クラウド サービスの Web ロールと worker ロールに .NET Framework をインストールする方法について説明します。 次の手順を使用すると、Azure ゲスト OS ファミリ 4 に .NET Framework 4.5.2 または .NET 4.6 をインストールすることができます。 最新情報については、ゲスト OS のリリース版を参照してください [Azure ゲスト OS リリースと SDK の互換性対応表](cloud-services-guestos-update-matrix.md)します。

クラウド プロジェクトおよびロールのスタートアップ タスクの一部として、インストーラーの起動の一環として .NET インストーラー パッケージを含めて、web ロールとワーカー ロールに .NET をインストールするプロセスが含まれます。  

## プロジェクトに .NET インストーラーを追加する
1. インストールする .NET Framework の Web インストーラーをダウンロードします。
    - [.NET 4.5.2 Web インストーラー](http://go.microsoft.com/fwlink/p/?LinkId=397703)
    - [.NET 4.6 Web インストーラー](http://go.microsoft.com/fwlink/?LinkId=528259)
    - [.NET 4.6.1 Web インストーラー](http://go.microsoft.com/fwlink/?LinkId=671729)
2. Web ロールの場合
  1.  **ソリューション エクスプ ローラー**, [で **ロール** でクラウド サービス プロジェクトを右クリックしての役割と選択 **追加 > 新しいフォルダー**します。 という名前のフォルダーを作成する *箱*
  2. 右クリックして、 **bin** フォルダー **追加 > 既存項目の**です。 .NET インストーラーを選択して bin フォルダーに追加します。
3. worker ロールの場合
  1. 選択し、ロールを右クリックして **追加 > 既存項目の**です。 .NET インストーラーを選択して、ロールに追加します。 

この方法でロールの Content フォルダーに追加したファイルは、クラウド サービス パッケージに自動的に追加され、仮想マシン上の一貫性のある場所にデプロイされます。 クラウド サービス内のすべての Web および worker ロールについて、すべてのロールがインストーラーのコピーを保持するように、このプロセスを繰り返します。

![インストーラー ファイルを持つロール コンテンツ][1]

## ロールのスタートアップ タスクを定義する
スタートアップ タスクでは、ロールが起動する前に操作を実行できます。 .NET Framework をスタートアップ タスクとしてインストールすると、任意のアプリケーション コードが実行される前に Framework がインストールされます。 詳細については、スタートアップ タスクを参照してください: [Azure におけるスタートアップ タスクの実行](https://msdn.microsoft.com/library/azure/hh180155.aspx)します。 

1. 次のコードを追加、 *ServiceDefinition.csdef* ファイルの下にある、 **WebRole** または **WorkerRole** すべての役割ノード。
    
    ```xml
     <LocalResources>
      <LocalStorage name="NETFXInstall" sizeInMB="1024" cleanOnRoleRecycle="false" />
    </LocalResources>
    <Startup>
      <Task commandLine="install.cmd" executionContext="elevated" taskType="simple">
        <Environment>
          <Variable name="PathToNETFXInstall">
            <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='NETFXInstall']/@path" />
          </Variable>
        </Environment>
      </Task>
    </Startup>
    ```

    上記の構成は、コンソールのコマンドを実行 *install.cmd* 、.NET framework をインストールできるように、管理特権を使用します。 構成では、名前の LocalStorage もが作成 *NETFXInstall*します。 スタートアップ スクリプトは、.NET framework インストーラーがダウンロードされ、このローカル ストレージ リソースからインストールされるように、このリソースを使用する一時フォルダーを設定します。 このリソースのサイズは少なくとも 1024 MB に設定して、フレームワークが適切にインストールされるようにする必要があります。 詳細については「: [スタートアップ中ファイルを格納するローカル ストレージを使用](https://msdn.microsoft.com/library/azure/hh974419.aspx) 

2. ファイルを作成する **install.cmd** ロールを右クリックしてすべてのロールに追加しを選択すると **追加 > 既存の項目]**します。 これで、すべてのロールに .NET インストーラー ファイルと、install.cmd ファイルが設定されました。
    
    ![すべてのファイルを持つロール コンテンツ][2]

    > [AZURE.NOTE] このファイルを作成するのにには、メモ帳などの単純なテキスト エディターを使用します。 Visual Studio を使用してテキスト ファイルを作成し、名前を '.cmd' に変更した場合、ファイルにまだ UTF-8 バイト オーダー マークが含まれていることがあるため、スクリプトの最初の行を実行するとエラーが発生します。 Visual Studio を使用してファイルを作成した場合は、実行時に無視されるよう REM (コメント) をファイルの最初の行に追加してください。 

3. 次のスクリプトの追加、 **install.cmd** ファイル。

    ```
    REM Set the value of netfx to install appropriate .NET Framework. 
    REM ***** To install .NET 4.5.2 set the variable netfx to "NDP452" *****
    REM ***** To install .NET 4.6 set the variable netfx to "NDP46" *****
    REM ***** To install .NET 4.6.1 set the variable netfx to "NDP461" *****
    set netfx="NDP46"
        
    
    REM ***** Needed to correctly install .NET 4.6.1, otherwise you may see an out of disk space error *****
    set TMP=%PathToNETFXInstall%
    set TEMP=%PathToNETFXInstall%
    
        
    REM ***** Setup .NET filenames and registry keys *****
    if %netfx%=="NDP461" goto NDP461
    if %netfx%=="NDP46" goto NDP46
        set netfxinstallfile="NDP452-KB2901954-Web.exe"
        set netfxregkey="0x5cbf5"
        goto logtimestamp
        
    :NDP46
    set netfxinstallfile="NDP46-KB3045560-Web.exe"
    set netfxregkey="0x60051"
    goto logtimestamp
        
    :NDP461
    set netfxinstallfile="NDP461-KB3102438-Web.exe"
    set netfxregkey="0x6041f"
        
    :logtimestamp
    REM ***** Setup LogFile with timestamp *****
    set timehour=%time:~0,2%
    set timestamp=%date:~-4,4%%date:~-10,2%%date:~-7,2%-%timehour: =0%%time:~3,2%
    md "%PathToNETFXInstall%\log"
    set startuptasklog="%PathToNETFXInstall%log\startuptasklog-%timestamp%.txt"
    set netfxinstallerlog="%PathToNETFXInstall%log\NetFXInstallerLog-%timestamp%"
    
    echo Logfile generated at: %startuptasklog% >> %startuptasklog%
    echo TMP set to: %TMP% >> %startuptasklog%
    echo TEMP set to: %TEMP% >> %startuptasklog%
    
    REM ***** Check if .NET is installed *****
    echo Checking if .NET (%netfx%) is installed >> %startuptasklog%
    reg query "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full" /v Release | Find %netfxregkey%
    if %ERRORLEVEL%== 0 goto end
        
    REM ***** Installing .NET *****
    echo Installing .NET: start /wait %~dp0%netfxinstallfile% /q /serialdownload /log %netfxinstallerlog% >> %startuptasklog%
    start /wait %~dp0%netfxinstallfile% /q /serialdownload /log %netfxinstallerlog% >> %startuptasklog% 2>>&1
        
    :end
    echo install.cmd completed: %date:~-4,4%%date:~-10,2%%date:~-7,2%-%timehour: =0%%time:~3,2% >> %startuptasklog%

    ```
    
    > [AZURE.IMPORTANT] 値を更新、 *netfx* framework のバージョンと一致するスクリプトで変数をインストールします。 .NET 4.5.2 をインストールする、 *netfx* 変数に設定する必要があります *"NDP452"*, を .NET 4.6 をインストールする、 *netfx* 変数に設定する必要があります *"NDP46"* 4.6.1 .NET をインストールして、 *netfx* 変数に設定する必要があります *"NDP461"*
        
    インストール スクリプトにより、指定されたバージョンの .NET Framework が既にコンピューターにインストールされているかどうか、レジストリを照会することで確認されます。 対象の .NET バージョンがインストールされていない場合、.Net Web インストーラーが起動します。 問題がないトラブルシューティングに役立つ、スクリプトはすべてログに記録という名前のファイルに *(startuptasklog currentdatetime) .txt* に格納されている *InstallLogs* ローカル記憶域。
 
      

## 診断でスタートアップ タスク ログが BLOB ストレージに転送されるように構成する (オプション)
Azure Diagnostics を、スタートアップ スクリプトまたは .NET インストーラーが生成したログ ファイルを BLOB ストレージに転送するように構成できます。この設定はオプションです。 この方法では、リモート デスクトップをロールに移動するのではなく、ログ ファイルを BLOB ストレージから単にダウンロードしてログを表示できます。

診断のオープンを構成する、 *diagnostics.wadcfgx* の下に次を追加し、 **ディレクトリ** ノード。 

```xml 
<DataSources>
 <DirectoryConfiguration containerName="netfx-install">
  <LocalResource name="NETFXInstall" relativePath="log"/>
 </DirectoryConfiguration>
</DataSources>
```

これですべてのファイル転送に azure 診断が構成されます、 *ログ* 下にあるディレクトリ、 *NETFXInstall* 診断ストレージ アカウントにするリソース、 *netfx インストール* blob コンテナーです。

## サービスのデプロイ 
サービスをデプロイすると、スタートアップ タスクが実行され、またインストールされていない場合、.NET Framework がインストールされます。 Framework のインストール中、ロールはビジー状態になります。また、Framework のインストールで必要な場合、ロールが再起動されることがあります。 


## その他のリソース

- [.NET Framework のインストール][]
- [方法 : インストールされている .NET Framework バージョンを確認する][]
- [.NET Framework のインストールおよびアンインストールのブロックのトラブルシューティング][]

[How to: Determine Which .NET Framework Versions Are Installed]: https://msdn.microsoft.com/library/hh925568.aspx
[Installing the .NET Framework]: https://msdn.microsoft.com/library/5a4x27ek.aspx
[Troubleshooting .NET Framework Installations]: https://msdn.microsoft.com/library/hh925569.aspx

<!--Image references-->
[1]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithinstallerfiles.png
[2]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithallfiles.png

 


