---
title: Azure 管理グループでリソースを整理する
description: 管理グループとその使用方法について説明します。
author: rthorn17
manager: rithorn
ms.assetid: 482191ac-147e-4eb6-9655-c40c13846672
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 9/28/2018
ms.author: rithorn
ms.openlocfilehash: 6b369c8209e62ff3c98b3fdf78378b403b0a0d2d
ms.sourcegitcommit: 7bc4a872c170e3416052c87287391bc7adbf84ff
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/02/2018
ms.locfileid: "48017655"
---
# <a name="organize-your-resources-with-azure-management-groups"></a>Azure 管理グループでリソースを整理する

組織に多数のサブスクリプションがある場合は、これらのサブスクリプションのアクセス、ポリシー、およびコンプライアンスを効率的に管理する方法が必要になることがあります。 Azure 管理グループの範囲は、サブスクリプションを上回ります。 "管理グループ" と呼ばれるコンテナーにサブスクリプションを整理して、管理グループに管理条件を適用できます。 管理グループ内のすべてのサブスクリプションは、管理グループに適用された条件を自動的に継承します。 管理グループを使うと、サブスクリプションの種類に関係なく、大きな規模でエンタープライズ レベルの管理を行うことができます。

たとえば、仮想マシン (VM) の作成に使えるリージョンを制限するポリシーを管理グループに適用できます。 そのリージョンでの VM の作成を許可するだけで、その管理グループの下にあるすべての管理グループ、サブスクリプション、リソースに、このポリシーが適用されます。

## <a name="hierarchy-of-management-groups-and-subscriptions"></a>管理グループとサブスクリプションの階層

管理グループとサブスクリプションの柔軟な構造を作成し、リソースを階層に整理して、統一されたポリシーとアクセス管理を適用できます。 次の図は、管理グループを使用した管理のための階層を作成する例を示します。

![ツリー](./media/MG_overview.png)

この例のような階層を作成することでポリシーを適用できます。たとえば、"インフラストラクチャ チームの管理グループ" というグループの VM の場所を米国西部リージョンに制限して、内部コンプライアンスやセキュリティ ポリシーを有効にすることができます。 このポリシーは、その管理グループ下の両方の EA サブスクリプションに継承され、それらのサブスクリプションの下にあるすべての VM に適用されます。 このポリシーは管理グループからサブスクリプションに継承されるため、このセキュリティ ポリシーは、ガバナンスを改善するためにリソースまたはサブスクリプションの所有者が変更することができません。

管理グループを使用するもう 1 つのシナリオは、ユーザーに複数のサブスクリプションへのアクセスを提供する場合です。 その管理グループの下に複数のサブスクリプションを移動することで、管理グループに対する 1 つの[ロールベースのアクセス制御](../../role-based-access-control/overview.md) (RBAC) 割り当てを作成できます。これにより、すべてのサブスクリプションにアクセスが継承されます。
複数のサブスクリプションに RBAC を割り当てるスクリプトを作成しなくても、管理グループへ 1 つ割り当てることで、ユーザーは必要なものすべてにアクセスできます。

### <a name="important-facts-about-management-groups"></a>管理グループに関する重要な事実

- 1 つのディレクトリでは、10,000 個の管理グループをサポートできます。
- 管理グループのツリーは、最大 6 レベルの深さをサポートできます。
  - この制限には、ルート レベルまたはサブスクリプション レベルは含まれません。
- 各管理グループとサブスクリプションは、1 つの親のみをサポートできます。
- 各管理グループは、複数の子を持つことができます。
- すべてのサブスクリプションと管理グループは、各ディレクトリの 1 つの階層内に格納されます。 プレビュー中の例外については、[ルート管理グループに関する重要な情報](#important-facts-about-the-root-management-group)を参照してください。

## <a name="root-management-group-for-each-directory"></a>各ディレクトリのルート管理グループ

各ディレクトリには、"ルート" 管理グループと呼ばれる 1 つの最上位管理グループがあります。
このルート管理グループは階層に組み込まれており、すべての管理グループとサブスクリプションはルート管理グループにまとめられます。 ルート管理グループにより、グローバル ポリシーと RBAC の割り当てをディレクトリ レベルで適用できます。 [ディレクトリ管理者は自分自身を昇格させて](../../role-based-access-control/elevate-access-global-admin.md)、最初にこのルート グループの所有者にする必要があります。 グループの所有者になった管理者は、任意の RBAC ロールを他のディレクトリ ユーザーまたはグループに割り当てて、階層を管理することができます。

### <a name="important-facts-about-the-root-management-group"></a>ルート管理グループに関する重要な事実

- ルート管理グループの名前と ID は、既定で設定されています。 表示名はいつでも更新でき、Azure Portal 内での表示を変えることができます。
  - 名前は "Tenant root group" になります。
  - ID は Azure Active Directory ID になります。
- 他の管理グループと異なり、ルート管理グループを移動または削除することはできません。  
- すべてのサブスクリプションと管理グループは、ディレクトリ内の 1 つのルート管理グループにまとめられます。
  - ディレクトリ内のすべてのリソースは、グローバル管理用のルート管理グループにまとめられます。
  - 既定では、新しいサブスクリプションは作成時に自動的にルート管理グループに設定されます。
- すべての Azure ユーザーはルート管理グループを表示できますが、すべてのユーザーがルート管理グループを管理するアクセス権を持つわけではありません。
  - サブスクリプションへのアクセス権を持つすべてのユーザーは、階層内にそのサブスクリプションが存在するコンテキストを参照できます。  
  - 既定では、ルート管理グループには誰もアクセスできません。 ディレクトリグローバル管理者は、自分自身を昇格させてアクセス権を取得できる唯一のユーザーです。  アクセス権を取得したディレクトリ管理者は、他のユーザーが管理できるように任意の RBAC ロールを割り当てることができます。  

> [!IMPORTANT]
> ルート管理グループでのユーザーのアクセス権またはポリシーの割り当ては、**ディレクトリ内のすべてのリソースに適用されます**。
> そのため、すべてのユーザーは、このスコープで定義されている項目を所有する必要性を評価する必要があります。
> ユーザーアクセスやポリシーの割り当ては、このスコープでのみ「必須」である必要があります。  

## <a name="initial-setup-of-management-groups"></a>管理グループの初期セットアップ

ユーザーが管理グループの使用を開始する際には、初期セットアップ プロセスが発生します。 最初の手順として、ディレクトリでルート管理グループが作成されます。 このグループが作成されると、ディレクトリ内に存在するすべての既存のサブスクリプションがルート管理グループの子になります。 このプロセスが実行される理由は、ディレクトリ内に管理グループ階層が 1 つだけ存在するようにするためです。 ディレクトリ内に 1 つの階層が存在することにより、ディレクトリ内の他のユーザーがバイパスできないグローバル アクセス権とポリシーを管理ユーザーが適用できるようになります。 ディレクトリ内に 1 つの階層が存在することにより、ルートに割り当てられている内容は、ディレクトリ内のすべての管理グループ、サブスクリプション、リソース グループ、およびリソースに適用されます。

## <a name="trouble-seeing-all-subscriptions"></a>サブスクリプションの表示の問題

(2018 年 6 月 25 日)より前のプレビューの管理グループを使用して開始された数個のディレクトリでは、一部のサブスクリプションが階層に適用されないという問題が発生していました。  これは、階層にサブスクリプションを適用するプロセスが、ディレクトリのルート管理グループでロールまたはポリシーの割り当てが行われた後に実装されることが原因でした。

### <a name="how-to-resolve-the-issue"></a>この問題を解決する方法

この問題の解決策として、セルフサービスのオプションが 2 つあります。

1. ルート管理グループからすべてのロールとポリシーの割り当てを削除します
    1. ルート管理グループからすべてのポリシーとロールの割り当てを削除することによって、一夜開けたときすべてのサブスクリプションが階層にバックフィルされています。  このチェックは、テナントのサブスクリプションに偶発的なアクセスやポリシーの割り当てが発生していないことを確認することが目的です。
    1. サービスに影響を与えずにこのプロセスを実行する最善の方法として、ルート管理グループの 1 つ下のレベルのロールまたはポリシー割り当てを適用します。 次に、ルート スコープからすべての割り当てを削除します。
1. API を直接呼び出して、バックフィル プロセスを開始します
    1. ディレクトリ内のすべての承認された顧客は、*TenantBackfillStatusRequest* または *StartTenantBackfillRequest* API を呼び出せます。 StartTenantBackfillRequest API が呼び出されると、すべてのサブスクリプションを階層に移動する初期セットアップ プロセスが開始されます。 このプロセスでは、すべての新しいサブスクリプションが強制的にルート管理グループの子になります。 このプロセスを実行するには、ルートのすべてのポリシーまたはアクセス許可の割り当てをすべてのサブスクリプションに適用できることを示すために、ルート レベルで割り当てを変更することが必要です。

このバックフィル プロセスについて質問等がございましたら、managementgroups@microsoft.com にお問い合わせください。  
  
## <a name="management-group-access"></a>管理グループ アクセス

Azure 管理グループは、すべてのリソース アクセスとロール定義について、[Azure のロールベースのアクセス制御 (RBAC)](../../role-based-access-control/overview.md) をサポートします。
これらのアクセス許可は、階層内に存在する子リソースに継承されます。 管理グループには任意の組み込み RBAC ロールを割り当てることができます。ロールは階層を継承し、各リソースに割り当てられます。
たとえば、VM 共同作成者の RBAC ロールは、管理グループに割り当てることができます。 このロールには、管理グループに対するアクションはありませんが、その管理グループに属するすべての VM に継承されます。

次の図に、管理グループのロールとサポートされているアクションの一覧を示します。

| RBAC ロール名             | Create | 名前の変更 | Move | 削除 | アクセス権の割り当て | ポリシーの割り当て | 読み取り  |
|:-------------------------- |:------:|:------:|:----:|:------:|:-------------:| :------------:|:-----:|
|Owner                       | ○      | ○      | ○    | ○      | ○             | ○             | ○     |
|Contributor                 | ○      | ○      | ○    | ○      |               |               | ○     |
|MG Contributor*             | ○      | ○      | ○    | ○      |               |               | ○     |
|Reader                      |        |        |      |        |               |               | ○     |
|MG Reader*                  |        |        |      |        |               |               | ○     |
|リソース ポリシー共同作成者 |        |        |      |        |               | ○             |       |
|User Access Administrator   |        |        |      |        | ○             |               |       |

*: MG Contributor と MG Reader は、管理グループのスコープ内でのみ、ユーザーによるそれらのアクションの実行を許可します。  

### <a name="custom-rbac-role-definition-and-assignment"></a>カスタム RBAC ロールの定義と割り当て

現時点では、管理グループのカスタム RBAC ロールはサポートされていません。 この項目の状態については、[管理グループ フィードバック フォーラム](https://aka.ms/mgfeedback)を参照してください。

## <a name="next-steps"></a>次の手順

管理グループについて詳しくは、以下をご覧ください。

- [管理グループを作成して Azure リソースを整理する](create.md)
- [管理グループを変更、削除、または管理する方法](manage.md)
- [Azure PowerShell モジュールのインストール](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups)
- [REST API 仕様の確認](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview)
- [Azure CLI 拡張機能のインストール](/cli/azure/extension?view=azure-cli-latest#az-extension-list-available)