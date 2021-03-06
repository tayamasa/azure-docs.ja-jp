---
title: B2B メッセージの監視とログの設定 - Azure Logic Apps | Microsoft Docs
description: AS2、X12、EDIFACT の各メッセージを監視します。 Azure Logic Apps の統合アカウントに対して診断ログを設定します。
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: bb7d9432-b697-44db-aa88-bd16ddfad23f
ms.date: 07/21/2017
ms.openlocfilehash: fd48dfaf7b863ab026da1758751509d9911ae01e
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/24/2018
ms.locfileid: "46948011"
---
# <a name="monitor-b2b-messages-and-set-up-logging-for-integration-accounts-in-azure-logic-apps"></a>B2B メッセージの監視と Azure Logic Apps の統合アカウントに対するログの設定

統合アカウント経由で 2 つの実行中のビジネス プロセスまたはアプリケーション間で B2B 通信を設定した後、これらのエンティティは互いにメッセージを交換できます。 この通信が予想通りに動作していることを確認するために、[Azure Log Analytics](../log-analytics/log-analytics-overview.md) サービスを使用して、AS2、X12、および EDIFACT メッセージの監視と統合アカウントの診断ログを設定できます。 このサービスは、クラウド環境とオンプレミス環境を監視して、その可用性とパフォーマンスの管理を支援することに加え、深いデバッグを行うためのランタイムの詳細とイベントの収集も実行します。 [診断データを他のサービスで使用する](#extend-diagnostic-data)こともできます (Azure Storage や Azure Event Hubs などで使用できます)。

## <a name="requirements"></a>必要条件

* 診断ログが設定されているロジック アプリ。 [ロジック アプリのログの設定方法](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics)に関する記事を参照してください。

  > [!NOTE]
  > この要件を満たした後、Log Analytics 内にワークスペースを用意する必要があります。 統合アカウントのログを設定したときと同じ Log Analytics ワークスペースを使用する必要があります。 Log Analytics ワークスペースがない場合は、[Log Analytics ワークスペースの作成方法](../log-analytics/log-analytics-quick-create-workspace.md)に関するページを参照してください。

* ロジック アプリにリンクされた統合アカウント。 [ロジック アプリにリンクする統合アカウントを作成する方法](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md)に関する記事を参照してください。

## <a name="turn-on-diagnostics-logging-for-your-integration-account"></a>統合アカウントの診断ログを有効にする

ログは、統合アカウントから直接有効にすることも、[Azure Monitor サービスを通じて](#azure-monitor-service)有効にすることもできます。 Azure Monitor は、インフラストラクチャ レベルのデータの基本的な監視を提供します。 詳細については、「[Azure Monitor の概要](../azure-monitor/overview.md)」を参照してください。

### <a name="turn-on-diagnostics-logging-directly-from-your-integration-account"></a>診断ログを統合アカウントから直接有効にする

1. [Azure ポータル](https://portal.azure.com)で、統合アカウントを検索して選択します。 次に示すように、**[監視]** の下の **[診断ログ]** を選択します。

   ![統合アカウントを検索して選択し、[診断ログ] を選択する](media/logic-apps-monitor-b2b-message/integration-account-diagnostics.png)

2. 統合アカウントを選択すると、次の値が自動的に選択されます。 これらの値が正しければ、**[診断を有効にする]** を選択します。 それ以外の場合は、使用する値を選択します。

   1. **[サブスクリプション]** で、統合アカウントで使用する Azure サブスクリプションを選択します。
   2. **[リソース グループ]** で、統合アカウントで使用するリソース グループを選択します。
   3. **[リソースの種類]** で **[統合アカウント]** を選択します。 
   4. **[リソース]** で、使用する統合アカウントを選択します。 
   5. **[診断を有効にする]** を選択します。

   ![統合アカウントで診断を設定する](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account.png)

3. **[診断設定]** で、**[ステータス]**、**[オン]** の順に選択します。

   ![Azure 診断を有効にする](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account-2.png)

4. 次に、ログで使用する Log Analytics ワークスペースとデータを次に示すように選択します。

   1. **[Log Analytics への送信]** を選択します。 
   2. **[Log Analytics]** で、**[構成]** を選択します。 
   3. **[OMS ワークスペース]** で、ログに使用する Log Analytics ワークスペースを選択します。
   4. **[ログ]** で、**[IntegrationAccountTrackingEvents]** カテゴリを選択します。
   5. **[保存]** を選択します。

   ![診断データをログに送信できるように Log Analytics を設定する](media/logic-apps-monitor-b2b-message/send-diagnostics-data-log-analytics-workspace.png)

5. 次に、[Log Analytics で B2B メッセージを追跡するための設定](../logic-apps/logic-apps-track-b2b-messages-omsportal.md)を行います。

<a name="azure-monitor-service"></a>

### <a name="turn-on-diagnostics-logging-through-azure-monitor"></a>Azure Monitor を通じて診断ログ記録を有効にする

1. [Azure ポータル](https://portal.azure.com)で、Azure のメイン メニューの **[監視]** を選択し、**[診断ログ]** を選択します。 次に、統合アカウントを次のように選択します。

   ![[監視]、[診断ログ] の順に選択し、統合アカウントを選択する](media/logic-apps-monitor-b2b-message/monitor-service-diagnostics-logs.png)

2. 統合アカウントを選択すると、次の値が自動的に選択されます。 これらの値が正しければ、**[診断を有効にする]** を選択します。 それ以外の場合は、使用する値を選択します。

   1. **[サブスクリプション]** で、統合アカウントで使用する Azure サブスクリプションを選択します。
   2. **[リソース グループ]** で、統合アカウントで使用するリソース グループを選択します。
   3. **[リソースの種類]** で **[統合アカウント]** を選択します。
   4. **[リソース]** で、使用する統合アカウントを選択します。
   5. **[診断を有効にする]** を選択します。

   ![統合アカウントで診断を設定する](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account.png)

3. **[診断設定]** で、[ステータス] の **[オン]** を選択します。

   ![Azure 診断を有効にする](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account-2.png)

4. ここで、次のように、ログ用に Log Analytics ワークスペースとイベント カテゴリを選択します。

   1. **[Log Analytics への送信]** を選択します。 
   2. **[Log Analytics]** で、**[構成]** を選択します。 
   3. **[OMS ワークスペース]** で、ログに使用する Log Analytics ワークスペースを選択します。
   4. **[ログ]** で、**[IntegrationAccountTrackingEvents]** カテゴリを選択します。
   5. 完了したら、**[保存]** を選択します。

   ![診断データをログに送信できるように Log Analytics を設定する](media/logic-apps-monitor-b2b-message/send-diagnostics-data-log-analytics-workspace.png)

5. 次に、[Log Analytics で B2B メッセージを追跡するための設定](../logic-apps/logic-apps-track-b2b-messages-omsportal.md)を行います。

## <a name="extend-how-and-where-you-use-diagnostic-data-with-other-services"></a>診断データを他のサービスで使用する方法と場所を拡張する

Azure Log Analytics と併せて、ロジック アプリの診断データを他の Azure サービスで使用する方法を次のように拡張できます。 

* [Azure 診断ログを Azure Storage にアーカイブする](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md)
* [Azure 診断ログを Azure Event Hubs にストリーミングする](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md) 

これにより、[Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md) や [Power BI](../log-analytics/log-analytics-powerbi.md) などの他のサービスのテレメトリと分析を使用したリアルタイム監視が可能になります。 例: 

* [Event Hubs からStream Analytics にデータをストリーミングする](../stream-analytics/stream-analytics-define-inputs.md)
* [ストリーミング データを Stream Analytics で分析し、Power BI でリアルタイム分析ダッシュボードを作成する](../stream-analytics/stream-analytics-power-bi-dashboard.md)

設定するオプションに基づいて、[Azure ストレージ アカウントの作成](../storage/common/storage-create-storage-account.md)または[Azure イベント ハブの作成](../event-hubs/event-hubs-create.md)を最初に行うようにしてください。 その後、診断データの送信先のオプションを選択してください。

![データを Azure ストレージ アカウントまたはイベント ハブに送信する](./media/logic-apps-monitor-b2b-message/storage-account-event-hubs.png)

> [!NOTE]
> リテンション期間は、ストレージ アカウントの使用を選択した場合にのみ適用されます。

## <a name="supported-tracking-schemas"></a>サポートされる追跡スキーマ

Azure では、次の種類の追跡スキーマをサポートしています。カスタム以外はすべて固定スキーマです。

* [AS2 の追跡スキーマ](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [X12 の追跡スキーマ](../logic-apps/logic-apps-track-integration-account-x12-tracking-schema.md)
* [カスタム追跡スキーマ](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)

## <a name="next-steps"></a>次の手順

* [Log Analytics で B2B メッセージを追跡する](../logic-apps/logic-apps-track-b2b-messages-omsportal.md "OMS で B2B メッセージを追跡する")
* [Enterprise Integration Pack についての詳細情報](../logic-apps/logic-apps-enterprise-integration-overview.md "Enterprise Integration Pack についての詳細情報")

