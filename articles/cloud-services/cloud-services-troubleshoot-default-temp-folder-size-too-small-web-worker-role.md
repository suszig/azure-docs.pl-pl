<properties 
   pageTitle="ロールに対する既定の TEMP フォルダーのサイズが小さすぎる | Microsoft Azure"
   description="クラウド サービス ロールでは、TEMP フォルダー用の領域量が限られています。この記事では、領域不足の回避方法に関する推奨事項をいくつか示します。"
   services="cloud-services"
   documentationCenter=""
   authors="dalechen"
   manager="msmets"
   editor=""
   tags="top-support-issue"/>
<tags 
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="10/14/2015"
   ms.author="daleche" />


# クラウド サービスの Web/worker ロールに対する既定の一時フォルダーのサイズが小さすぎる

クラウド サービスの worker または Web ロールの既定の一時ディレクトリの最大サイズは 100 MB ですが、ある時点でいっぱいになる可能性があります。 この記事では、一時ディレクトリの領域不足を回避する方法について説明します。
>[AZURE.NOTE] これは Web および worker ロールを使用する Azure SDK 1.0 から SDK 1.4 にのみ適用されます。

## Azure カスタマー サポートへの問い合わせ

この記事のどの時点でもその他のヘルプを必要がある場合で Azure のエキスパートに連絡することができます [MSDN Azure とスタック オーバーフロー フォーラム](http://azure.microsoft.com/support/forums/)します。

または、Azure サポート インシデントを送信できます。 移動して、 [Azure サポート サイト](http://azure.microsoft.com/support/options/) ] をクリック **サポートにお問い合わせ**します。 Azure のサポートを使用する方法の詳細については、読み取り、 [Microsoft Azure サポートに関する FAQ](http://azure.microsoft.com/support/faq/)します。


## 領域が不足する理由

標準的な Windows 環境変数 TEMP と TMP は、アプリケーションで実行されているコードで使用可能です。 TEMP と TMP は両方とも、最大サイズが 100 MB の 1 つのディレクトリを指します。 このディレクトリに格納されているデータは、ホステッド サービスのライフサイクル全体で保持されません。ホステッド サービスのロール インスタンスがリサイクルされると、ディレクトリはクリーニングされます。

## 問題の修正に関する推奨事項

次のいずれかの代替手段を実装します。

- ローカル ストレージ リソースを構成し、**TEMP** や **TMP** を使用せずに直接アクセスします。 アプリケーション内で実行されるコードからローカル ストレージ リソースにアクセスするには、呼び出し、 [RoleEnvironment.GetLocalResource](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.getlocalresource.aspx) メソッドです。 ローカル ストレージ リソースの設定の詳細については、次を参照してください。 [ローカル ストレージ リソースの構成](cloud-services-configure-local-storage-resources.md)します。

- ローカル ストレージ リソースを構成し、TEMP と TMP ディレクトリがローカル ストレージ リソースのパスを指すように指定します。 内でこのような変更を実行する必要があります、 [RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) メソッドです。

次のコード例では、OnStart メソッド内から TEMP および TMP のターゲット ディレクトリを変更する方法を示します。


```csharp
using System;
using Microsoft.WindowsAzure.ServiceRuntime;

namespace WorkerRole1
{
    public class WorkerRole : RoleEntryPoint
    {
        public override bool OnStart()
        {
            // The local resource declaration must have been added to the
            // service definition file for the role named WorkerRole1:
            //
            // <LocalResources>
            //    <LocalStorage name="CustomTempLocalStore" 
            //                  cleanOnRoleRecycle="false" 
            //                  sizeInMB="1024" />
            // </LocalResources>

            string customTempLocalResourcePath = 
            RoleEnvironment.GetLocalResource("CustomTempLocalStore").RootPath;
            Environment.SetEnvironmentVariable("TMP", customTempLocalResourcePath);
            Environment.SetEnvironmentVariable("TEMP", customTempLocalResourcePath);

            // The rest of your startup code goes here…

            return base.OnStart();
        }
    }
}
```

## 次のステップ

さらに表示 [トラブルシューティングに関する記事](..\?tag=top-support-issue&service=cloud-services) のクラウド サービスです。






