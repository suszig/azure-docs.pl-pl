<properties
    pageTitle ="Azure RemoteApp のアプリの要件 |Microsoft Azure]
    description ="Azure RemoteApp で使用するアプリケーションの要件について説明します"。
    サービス =「remoteapp」
    documentationCenter=""
    authors ="lizap"
    manager ="mbaldwin"/>

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/05/2015"
    ms.author="elizapo"/>



# アプリの要件

Azure RemoteApp は、Windows Server 2012 R2 イメージの 32 ビットまたは 64 ビットの Windows ベース アプリケーションのストリーミングをサポートします。 既存の 32 ビットまたは 64 ビットの Windows ベース アプリケーションのほとんどは、Azure RemoteApp (リモート デスクトップ サービス、以前のターミナル サービス) 環境で "そのまま" 動作します。 ただし、"動作すること" と "うまく動作すること" は異なります。アプリケーションの中には正常に問題なく機能するものもあれば、そうでないものもあります。 以下の情報は、リモート デスクトップ サービス環境でのアプリケーション開発、および互換性を保証するためのテストのガイダンスを示したものです。

ヒント: Microsoft では、アプリケーションの実行例の作成に取り組んでいます。 RemoteApp での Microsoft Access、QuickBooks、App-V の使用に関する新しいトピックを掲載する予定です。

## 必要条件

これらの 3 つの要件に従うと、アプリケーションが RemoteApp で快適に動作するのに役立ちます。

1.  アプリケーションをすべて満たす [Windows デスクトップ アプリ認定要件](https://msdn.microsoft.com/library/windows/desktop/hh749939.aspx) に準拠している [リモート デスクトップ サービスのプログラミング ガイドライン](https://msdn.microsoft.com/library/aa383490.aspx) RemoteApp との互換性を持ちます。
2.  アプリケーションで、失われる可能性があるイメージ上または RemoteApp インスタンス上にローカルにデータを格納しないでください。 RemoteApp コレクションを作成した後、インスタンスは複製されてステートレスとなり、アプリケーションのみを含む必要があります。 外部ソース、またはユーザーのプロファイル内にデータを格納します。
3.  カスタム イメージには、失われる可能性があるデータを含めないでください。

## アプリケーションのテスト

次の手順を使用して、アプリケーションをテストします。

1.  Windows Server 2012 R2 とアプリケーションのインストール
2.  リモート デスクトップの有効化
3.  2 つのユーザー アカウントの作成 (UserA と UserB、両方のユーザー アカウントをリモート デスクトップ セキュリティ グループに追加する)
4.  アプリケーションの起動時に PC に対する 2 つの同時 RDS セッションを確立することにより、マルチセッションの互換性を確認
5.  アプリケーションの動作の検証

## アプリケーション開発のガイドライン

以下のガイドラインを使用して、RemoteApp 向けのアプリケーションを開発します。

### 複数のユーザー

- インストールする、 [アプリケーションを 1 人のユーザーに ](https://msdn.microsoft.com/library/aa380661.aspx)マルチ ユーザー環境での問題を作成することができます。
- アプリケーションにする必要があります [ユーザー固有の情報を格納](https://msdn.microsoft.com/library/aa383452.aspx) ユーザー固有の場所で、すべてのユーザーに適用されるグローバル情報と切り離しています。
- 使用して複数の RemoteApp [カーネル オブジェクトの名前空間](https://msdn.microsoft.com/library/aa382954.aspx); クライアント/サーバー アプリケーションのサービスでは、主に、グローバル名前空間を使用します。
- あると想定する安全ではありません、コンピューター名または [IP アドレス](https://msdn.microsoft.com/library/aa382942.aspx) コンピューターに割り当てられている複数のユーザーがログオンできる同時にリモート デスクトップ セッション ホスト (RD セッション ホスト) サーバーにあるために、1 人のユーザーに関連付けられました。

### パフォーマンス

- 無効にする [グラフィック効果](https://msdn.microsoft.com/library/aa380822.aspx) アプリを RemoteApp に追加する前にします。
- すべてのユーザーに対して CPU の可用性を最大化、無効にするか [バック グラウンド タスク ](https://msdn.microsoft.com/library/aa380665.aspx) リソースを消費するは効率的なバック グラウンド タスクを作成します。
- 調整し、アプリケーションのバランスをとる必要があります [スレッドの使用法](https://msdn.microsoft.com/library/aa383520.aspx) マルチ ユーザー、マルチプロセッサ環境にします。
- パフォーマンスを最適化するアプリケーションのことをお勧めは [検出](https://msdn.microsoft.com/library/aa380798.aspx) クライアント セッションで実行されているかどうか。





