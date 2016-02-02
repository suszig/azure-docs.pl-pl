<properties 
    pageTitle="Logic Apps とは" 
    description="App Service Logic Apps ついてさらに詳しく説明します" 
    authors="kevinlam1" 
    manager="dwrede" 
    editor="" 
    services="app-service\logic" 
    documentationCenter=""/>

<tags
    ms.service="app-service-logic"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/08/2015"
    ms.author="klam"/>


# Logic Apps とは

| クイック リファレンス|
| --------------- |
| [ロジック アプリ定義言語](https://msdn.microsoft.com/library/azure/dn948512.aspx?f=255&MSPPError=-2147217396)|
| [ロジック アプリのコネクタのドキュメント](https://azure.microsoft.com/documentation/articles/app-service-logic-connectors-list/)|
| [ロジック アプリのフォーラム](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps)|

Azure App Service は、Web アプリ、モバイル アプリ、統合アプリの作成を容易にする、開発者向けの完全に管理された PaaS (サービスとしてのプラットフォーム) です。 Logic Apps はこのスイートの一部であり、どのような専門的なユーザーや開発者でも、使いやすいビジュアル デザイナーを使用してビジネス プロセスの実行とワークフローを自動化できます。

何より、Logic Apps と組み合わせて使用できます [コネクタ ][connectors] 簡単にも注意が必要な統合シナリオを解決するために、Marketplace からです。

![フロー アプリ デザイナー](./media/app-service-logic-what-are-logic-apps/Designer.png)

自動的に SQL DB の新しいレコードをレプリケートしフロント デスクにメールまたは自動的にネガティブなツイートを見つけてスラック チャネルに送信する場合

## なぜ Logic Apps か

Logic Apps を使用することで、トリガーをきっかけに一連のステップを実行するワークフローの設計が可能になります。各ステップでは、認証と、チェックポイント処理や持続的な実行などのベスト プラクティスをしっかり処理しながら、App Service の API アプリを呼び出すことができます。

ビジネス プロセスの自動化 (ネガティブなツイートを見つけて組織内のスラック チャネルに送信する、SQL に新しい顧客レコードがあると、そのレコードを CRM システムにレプリケートするなど) が必要な場合は、Logic Apps を使用すると、クラウドからオンプレミスに異なるデータ ソースを簡単に統合できます。 チェック アウト、 [コネクタ ][connectors] ひらめいたらのおよび [開始 ][create] 何ができるかを表示するようになりました。

さらに、新機能、 [BizTalk API Apps ][biztalk] 数多く利用して統合シナリオのパワーを備えた拡張可能、 [ルール エンジンが ][rules], 、[取引先管理 ][tpm] などです。

- **使いやすいデザイン ツール** - Logic Apps は、ブラウザーで全体を設計することができます。 トリガーで開始 - 単純なスケジュールから会社についてのツイートが表示される各タイミングまで。 次に、豊富なコネクタのギャラリーを使用してアクションの任意の数を調整します。

- **SaaS を簡単に構成** - 簡単に説明できるような構成タスクでさえ、コードとして実装するには困難が伴います。 Logic Apps を使用すれば異なるシステムへの接続も簡単です。 Facebook や Twitter アカウントのアクティビティに基づいて CRM ソフトウェアにタスクを作成しようとしていますか? クラウド マーケティング ソリューションをオンプレミスの請求書作成システムに接続しようとしていますか? Logic Apps は、これらの問題にソリューションを提供する、最も迅速で信頼できる手段です。

- **テンプレートからすばやく開始** - 開始するヘルプを提供しています、 [テンプレート ][templates] 一般的なソリューションを迅速に作成するためのです。 高度な BizTalk ソリューションから単純な SaaS 接続まであるうえ、単なる 'お遊び用' のものも用意しています。Logic Apps の能力を理解するには、このギャラリーが一番の近道です。

- **組み込みの拡張機能** - 必要なコネクタが見つかりませんか? Logic Apps は、App Service スイートの一部であり、API Apps と動作するように設計されています。独自の API アプリを作成してコネクタとして簡単に使用できます。 自分だけの新しいアプリを作成するか、Marketplace で共有して収益化してください。

- **真の統合能力** - 手軽に始めて必要に応じて拡張できます。 Logic Apps は、Microsoft の業界屈指の統合ソリューション、BizTalk の能力を統合担当者が簡単に利用して必要なソリューションを構築できるようにしています。 について、 [App Services ][biztalk]します。

## Logic Apps の概念

Logic Apps の機能を構成する主な要素とは次のとおりです。

- **ワークフロー** - Logic Apps では、ビジネス プロセスを一連の手順やワークフローとしてグラフィカルにモデル化できます。
- **コネクタ** - ロジック アプリにはデータとサービスへのアクセスが必要です。 コネクタは、特殊な種類の API アプリです。 コネクタは、データへの接続とデータの操作を補助するために、特別に作成されます。 現在利用できるコネクタの一覧を参照してください [コネクタ ][connectors]します。
- **トリガー** - 一部のコネクタは、トリガーとしても動作します。 トリガーは、電子メールや Azure のストレージ アカウントの変更の到着などの特定のイベントに基づいて、ワークフローの新しいインスタンスを開始します。
-  **アクション** - ワークフローにおけるトリガーの後の各ステップは、アクションと呼ばれます。 通常、各アクションはコネクタまたはカスタム API アプリでの操作にマップされます。
- **BizTalk** - Azure App Services には、高度な統合シナリオ向けに Biztalk の機能が含まれています。 Biztalk は、マイクロソフトの業界屈指の統合プラットフォームです。 BizTalk API Apps によって、Logic Apps ワークフローに簡単に検証、変換、ルールなどを含めることができます。 詳細については、検索 [BizTalk API apps の ][biztalk]します。

## Getting Started (概要)

をロジック アプリを開始するには、次の [ロジック アプリの作成 ][create] チュートリアルです。

Azure App Service プラットフォームの詳細については、次を参照してください。 [Azure App Service ][appservice]します。


[biztalk]: app-service-logic-what-are-biztalk-api-apps.md 
[appservice]: ../app-service/app-service-value-prop-what-is.md 
[create]: app-service-logic-create-a-logic-app.md 
[connectors]: app-service-logic-connectors-list.md 
[tpm]: app-service-logic-create-a-trading-partner-agreement.md 
[rules]: app-service-logic-use-biztalk-rules.md 
[templates]: app-service-logic-use-logic-app-templates.md 

