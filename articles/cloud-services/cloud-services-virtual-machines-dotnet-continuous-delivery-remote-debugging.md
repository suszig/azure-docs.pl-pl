<properties
    pageTitle="継続的な配信でのリモート デバッグを有効にする | Microsoft Azure"
    description="継続的な配信を使用した Azure へのデプロイ時にリモート デバッグを有効にする方法を説明します。"
    services="cloud-services"
    documentationCenter=".net"
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/19/2015"
    ms.author="tarcher"/>

# 継続的な配信を使用して Azure に発行する場合にリモート デバッグを有効にする

Azure でクラウド サービスまたは仮想マシンの場合にリモート デバッグを使用するを有効にする [継続的な配信](cloud-services-dotnet-continuous-delivery.md) 次の手順に従って Azure に発行します。

## クラウド サービス用にリモート デバッグを有効にする

1. ビルド エージェントで初期環境を Azure の設定」の説明に従って [の Azure コマンド ライン ビルド](http://msdn.microsoft.com/library/hh535755.aspx)します。
2. リモート デバッグ ランタイム (msvsmon.exe) は、パッケージに必要なために、インストール、 [Visual Studio 2015 用リモート ツール](http://www.microsoft.com/en-us/download/details.aspx?id=48155) (または [Microsoft Visual Studio 2013 Update 5 用リモート ツール](https://www.microsoft.com/en-us/download/details.aspx?id=48156) Visual Studio 2013 を使用している場合)。 代替方法として、Visual Studio がインストールされているシステムからリモート デバッグ バイナリをコピーすることもできます。
3. 証明書の作成」の説明に従って [Azure クラウド サービスの証明書の概要](cloud-services-certs-create.md)します。 .pfx および RDP 証明書の拇印を保持し、その証明書をターゲット クラウド サービスにアップロードします。
4. MSBuild コマンド ラインで次のオプションを使用し、リモート デバッグを有効にしてビルドおよびパッケージ化します (山かっこの項目については、システムおよびプロジェクト ファイルの実際のパスに置き換えてください。)

        msbuild /TARGET:PUBLISH /PROPERTY:Configuration=Debug;EnableRemoteDebugger=true;VSX64RemoteDebuggerPath="<remote tools path>";RemoteDebuggerConnectorCertificateThumbprint="<thumbprint of the certificate added to the cloud service>";RemoteDebuggerConnectorVersion="2.7" "<path to your VS solution file>"

    `VSX64RemoteDebuggerPath` for Visual Studio リモート ツールの msvsmon.exe を含むフォルダーへのパスです。

5. 前の手順で生成されたパッケージと .cscfg ファイルを使用してターゲット クラウド サービスに発行します。
6. Visual Studio と Azure SDK for .NET がインストールされているマシンに証明書 (.pfx ファイル) をインポートします。

## 仮想マシン用にリモート デバッグを有効にする

1. Azure 仮想マシンを作成します。 参照してください [Windows Server を実行する仮想マシンの作成](virtual-machines-windows-tutorial.md) または [を作成し、Visual Studio での Azure の仮想マシンの管理](vs-azure-tools-virtual-machines-create-manage.md)します。
2. [Azure クラシック ポータル ページ](http://go.microsoft.com/fwlink/p/?LinkID=269851), 、して、バーチャル マシンのバーチャル マシンのダッシュ ボードの表示 **RDP 証明書の拇印**します。 この値は、使用、 `ServerThumbprint` 拡張機能の構成の値。
3. クライアント証明書を作成する」の説明に従って [Azure クラウド サービスの証明書の概要](cloud-services-certs-create.md) (.pfx および RDP 証明書のサムプリントを保持)。
4. Azure Powershell のインストール (version 0.7.4 以降)」の説明に従って [をインストールして、Azure PowerShell を構成する方法](powershell-install-configure.md)します。
5. 次のスクリプトを実行して RemoteDebug 拡張機能を有効にします。 パスおよび個人用データを自分自身のサブスクリプション名、サービス名、サムプリントなどで置き換えます。
 >[AZURE.NOTE] このスクリプトは Visual Studio 2015 用に構成されています。 Visual Studio 2013 を使用する場合は、ReferenceName と ExtensionName に対して "RemoteDebugVS2013" を使用します。

<pre>
                  
 Add-AzureAccount
Select-AzureSubscription "My Microsoft Subscription"
$vm = Get-AzureVM -ServiceName "mytestvm1" -Name "mytestvm1"
$endpoints = @(
,@{Name="RDConnVS2013"; PublicPort=30400; PrivatePort=30398}
,@{Name="RDFwdrVS2013"; PublicPort=31400; PrivatePort=31398}
)
foreach($endpoint in $endpoints)
{
Add-AzureEndpoint -VM $vm -Name $endpoint.Name -Protocol tcp -PublicPort $endpoint.PublicPort -LocalPort $endpoint.PrivatePort
}
$referenceName = "Microsoft.VisualStudio.WindowsAzure.RemoteDebug.RemoteDebugVS2015"
$publisher = "Microsoft.VisualStudio.WindowsAzure.RemoteDebug"
$extensionName = "RemoteDebugVS2015"
$version = "1.*"
$publicConfiguration ="
                  <PublicConfig>
                    <Connector.Enabled>true</Connector.Enabled>
                    <ClientThumbprint>56D7D1B25B472268E332F7FC0C87286458BFB6B2</ClientThumbprint>
                    <ServerThumbprint>E7DCB00CB916C468CC3228261D6E4EE45C8ED3C6</ServerThumbprint>
                    <ConnectorPort>30398</ConnectorPort>
                    <ForwarderPort>31398</ForwarderPort>
                  </PublicConfig>
                  "
$vm |Set-azurevmextension `
ReferenceName $referenceName `
-パブリッシャー $publisher `
-Extensionname $extensionName `
-Version $version `
-PublicConfiguration $publicConfiguration
foreach($extension in $vm.VM.ResourceExtensionReferences)
{
場合 (($extension.ReferenceName-eq $referenceName) `
- し ($extension します。パブリッシャー-eq $publisher) `
- し ($extension します。-Eq $extensionName という名前を) `
-and ($extension.Version -eq $version))
{
$extension.ResourceExtensionParameterValues[0].Key = 'config.txt'
break
{
{
$vm | Update-AzureVM
 
                </pre>

6. Visual Studio と Azure SDK for .NET がインストールされているマシンに証明書 (.pfx) をインポートします。





