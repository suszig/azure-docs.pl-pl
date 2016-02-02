<properties
    pageTitle="Azure Active Directory ハイブリッド ID の設計上の考慮事項 - 次の段階 | Microsoft Azure"
    description="ハイブリッド ID の設計上の考慮事項ガイドを読んだ後の概要と次の手順"
    documentationCenter=""
    services="active-directory"
    authors="yurid"
    manager="stevenpo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="11/08/2015"
    ms.author="yurid"/>


# Azure Active Directory ハイブリッド ID の設計上の考慮事項 - 次の段階

要件を定義し、モバイル デバイスのすべての選択肢を検討したので、次の段階ではユーザーと組織に最適なサポート インフラストラクチャをデプロイします。

## ハイブリッド ID ソリューション

- ニーズに最適なソリューション シナリオを活用することは、モバイル デバイス管理インフラストラクチャ デプロイの詳細を確認し、計画する優れた方法です。 次のソリューションでは、最も一般的なモバイル デバイス管理シナリオをいくつか紹介します。

- [エンタープライズ環境のソリューションにモバイル デバイスと Pc の管理](https://technet.microsoft.com/en-us/library/dn582037.aspx) を Microsoft Intune でのクラウドに、内部設置型の System Center 2012 Configuration Manager インフラストラクチャを拡張することによって、モバイル デバイスを管理するのに役立ちます。 このハイブリッド インフラストラクチャは、中規模または大規模の環境で IT 専門家が BYOD とリモート アクセスを導入する場合に役立ちます。同時に管理の複雑性が緩和されます。
- [Configuration Manager 2007 向けのモバイル デバイスを管理する](https://technet.microsoft.com/en-us/library/dn508400.aspx) 、インフラストラクチャ上、System Center Configuration Manager 2007 に重ねると、モバイル デバイスを管理するのに役立ちます。 このソリューションでは、System Center 2012 Configuration Manager を実行する 1 台のサーバーを構築し、Microsoft Intune を実行し、その MDM 機能を最大限に活用する方法が紹介されています。
- [小規模な環境のソリューションでのモバイル デバイスを管理する](https://technet.microsoft.com/en-us/library/dn715906.aspx) MDM. をサポートする必要がある小規模企業向け Microsoft Intune を使用して現在のインフラストラクチャを拡張子、モバイル デバイス管理と BYOD をサポートする方法が説明されています。 このソリューションでは、Microsoft Intune をスタンドアロンで利用し、ローカル サーバーのないクラウドだけの構成という最も単純なシナリオについて説明されています。

## ハイブリッド ID のドキュメント

モバイル デバイス管理ソリューションを実装するとき、コンセプトに基づく手続型の計画、デプロイメント、管理コンテンツが役立ちます。

- [Microsoft System Center](https://technet.microsoft.com/en-us/library/cc507089.aspx) ソリューションを使用できますが、インフラストラクチャ、ポリシー、プロセス、およびベスト プラクティスに関する集計の知識を収集して、IT スタッフが管理システムを構築して、操作を自動化できるようにします。
- [Microsoft Intune](https://technet.microsoft.com/en-us/library/jj676587.aspx) コンピューターとモバイル デバイスを管理し、会社の情報をセキュリティで保護するのに役立つクラウド ベースのデバイス管理サービスです。
- [For Office 365 の MDM](https://technet.microsoft.com/en-us/library/ms.o365.cc.devicepolicy.aspx) 管理し、Office 365 組織に接続しているときに、モバイル デバイスをセキュリティ保護することができます。 MDM for Office 365 を利用し、デバイスのセキュリティ ポリシーとアクセス ルールを設定し、紛失または盗難時にモバイル デバイスのデータを消去できます。

## ハイブリッド ID リソース

次のリソースには、モバイル デバイス管理ソリューションに関する最新ニュースと更新が頻繁に掲載されます。

- [Microsoft エンタープライズ モビリティ ブログ](http://blogs.technet.com/b/enterprisemobility/)
- [クラウドでの Microsoft ブログ](http://blogs.technet.com/b/in_the_cloud/)
- [Microsoft Intune のブログ](http://blogs.technet.com/b/microsoftintune/)
- [Microsoft System Center Configuration Manager のブログ](http://blogs.technet.com/b/configurationmgr/)
- [Microsoft System Center Configuration Manager チーム ブログ](http://blogs.technet.com/b/configmgrteam/)

## 関連項目

[設計に関する考慮事項の概要](active-directory-hybrid-identity-design-considerations-overview.md)





