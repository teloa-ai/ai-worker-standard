# AI Worker 工作與協作應用規範

[Languages / 语言](../docs/languages.md)

**Max Luo、Morgan Chen｜Teloa 共同創辦人**

> 本文是[簡體中文權威版本](work-profile.zh-CN.md)的完整繁體中文譯文，尚未經母語業界專家完整複核。如有歧義或衝突，以簡體中文版為準；歡迎回報翻譯問題。JSON／YAML 範例保留原始中文值，以確保各語言的範例相同。

日期：2026-10-18。版本：`1.0`。屬於 [AI Worker、AI Team 與 AI Organization 開放定義標準 1.0](core.zh-Hant.md)的規範性附錄。它定義工作語義與適配要求，不定義新的執行引擎。

## 1. 用一份契約連結 Worker、Team 和 Organization

員工提供工作入口；團隊提供自己的工作入口，並說明成員如何分擔；組織繼續組合團隊。三者都明確輸入、輸出、依賴、責任與驗收。組織不會自動暴露成員所有工作，加入組織也不會自動分享個人記憶。

工作應用規範作為一個必需擴展出現：`id = urn:teloa:profile:ai-work`、`version = "1.0"`、`required = true`。這是本規範的固定識別碼，不是身分認證或線上服務入口；任何宿主均可實現，不要求 Teloa 帳號。使用者不得改成 optional 來繞過關鍵語義。只有核心 agent 欄位的檔案只能聲明核心交換支援，不能聲明完整 AI Worker 定義符合性。

核心規範中的非空字串、LocalId、固定引用、無副作用匯入與未知擴展規則繼續適用。下列結構均只允許列出的欄位；除註明選用外均必需。沒有適用內容時使用明確的空陣列，不能用 null 代替。各類 id 只在所屬陣列唯一，引用位置按本附錄明確解釋。

## 2. 擴展 data 的結構

| 欄位 | 類型 | 用途與預設行為 |
| --- | --- | --- |
| responsibility | Responsibility | 職務或組合體的判斷規則 |
| resources | Resource[] | 方法、資料、應用和策略目錄；允許為空，列出不等於整體必須啟用 |
| works | 非空 Work[] | 對外承諾的工作入口 |
| requires | LocalId[]，選用 | 所有工作共同需要的 resources；省略視為空 |
| routines | Routine[]，選用 | 例行安排；省略視為空，不自動啟用 |
| authorityRoles | AuthorityRole[]，選用 | 外部核准、驗收或責任角色；省略視為空 |
| memoryPolicyRef | LocalId，選用 | 指向 purpose=policy 的資源；存在時計入整體依賴；省略表示不聲明跨任務記憶能力 |
| collaboration | Collaboration | organization 必需，agent 不得使用 |
| governance | Governance，選用 | 僅 organization 可用；聲稱支援本版 AI org 定義時必須提供 |

**Responsibility** 含五個必需字串陣列：`triggers、autonomousActions、confirmationPoints、escalationRules、deliveryChecks`。分別說明何時接手、自主動作、確認點、升級規則和交付檢查。triggers、autonomousActions、deliveryChecks 必須非空；另兩項在確實無額外業務確認或升級規則時可為空。空確認陣列不免除宿主授權，空升級陣列不允許在缺少輸入或權限時繼續執行。描述不授予工具權限。核心 instructions 說明持續職責，工作細節由 Work 和資源提供；文字是否有實質含義仍需內容評審，不能僅憑字串非空判斷。

**Resource** 含 `id:LocalId、title:非空字串、purpose、binding`，以及選用 `requires:LocalId[]`。purpose 為 `method | knowledge | procedure | application | runtime | policy | trigger | template-reference`。這些是資源用途標籤，不是必須新增的產品資源類型。binding 含 `kind、config`，重用核心 Capability 的四類綁定及其 config 規則，不重複定義協定。

Resource.requires 只引用同一定義的 resources，省略視為空。固定資料優先帶 digest；可變來源須在實際執行時固定讀取版本。方法可引用完整技能套件，SOP 可引用方法或流程資源。不能將僅可讀的應用說明、模型設定或政策檔案標為應用已連線、執行環境已滿足或政策已執行。

對於 runtime、policy、trigger 或以資源形式描述的 application，適配報告必須記錄其原生格式、精確版本和明確處理者。處理者可以是支援該格式的宿主功能或經明確交辦的人工環節；只有文字說明且無人承擔執行時，不滿足必要條件。此約定保留現有原生格式，不另造通用權限或排程語言。

**AuthorityRole** 含 `id:LocalId、name:非空字串、kind:human | service`。它是職責槽位，不是帳號。宿主必須綁定可信主體，service 表示宿主確認的外部服務主體，不代表模型可以自己成為核准人。需要人工核准時必須聲明 human；角色的作用由引用它的驗收、升級或治理契約確定。

## 3. 每項工作的輸入輸出與交付

| Work 欄位 | 類型 | 含義 |
| --- | --- | --- |
| id、title、instructions | LocalId、非空字串、非空字串 | 工作入口、名稱及目標/方法要求 |
| inputs、outputs | JSON Schema 物件 | 分別描述完整輸入物件和完整輸出物件 |
| requires | LocalId[] | 本項工作需要的 resources；允許為空 |
| responsible | ParticipantRef | 一個主要負責位置 |
| acceptance | Acceptance | 完成檢查與驗收責任 |

`ParticipantRef` 是 `self`、`member/<LocalId>` 或 `authority/<LocalId>`。self 指當前定義對應的實例；member 只指當前 organization 的直接 Member 槽位，包括按任務參與的 consultant；authority 指當前 data.authorityRoles。agent 不得使用 member 引用。所有引用必須存在，不能按名稱猜測。responsible 必須是 self 或 member；外部人員可負責驗收和升級，不被偽裝成 AI 執行者。organization 使用 self 時，表示組合體負責對外交付，宿主仍須記錄實際執行者、輸入分發與輸出彙整的承擔者，不能以組織名義代替實際貢獻者。

**Acceptance** 含 `criteria:非空字串陣列、reviewer:ParticipantRef、independent:boolean`。criteria 為可檢查的完成要求；reviewer 是驗收職責位置。independent=true 時，宿主必須證明實際複核主體與被審成果的實際執行主體不同；複合成果須核對實際貢獻者，不能只比較主要負責者或團隊名義身分。兩個槽位、兩個工作階段或兩個名稱都不構成獨立性證明。獨立主體也不自動證明判斷品質。驗收記錄固定被審的成果版本；結構驗證通過、模型完成和業務採用分別記錄。

inputs、outputs 的根必須為 `type:object`，並聲明 `$schema = https://json-schema.org/draft/2020-12/schema`。重用 [JSON Schema 2020-12](https://json-schema.org/draft/2020-12) 的標準詞彙，不自建類型語言。Schema 可以包含說明及 `$defs`；所有引用（含 `$ref`、`$dynamicRef`）只能指向本 Schema 文件，不允許借巢狀 `$id` 改變引用的文件邊界。匯入不連線至網路解析引用。實現須報告不支援的詞彙或約束，不得忽略後稱驗證通過。format 如用於關鍵驗收，須顯式聲明並驗證相應格式檢查能力。

輸入缺項時報告待補充；輸出缺項或類型不符時不能聲明符合該成果契約。不隱式把字串轉數值、截斷小數或把 false 轉為 0。Schema default 是描述，不自動代表使用者提供或授權了該值。自由文字成果可以放在字串欄位中；業務證據與判斷品質仍由 acceptance 檢查。

**依賴檢查：** 先滿足核心 capabilities 及其 R05/R06 條件，再取 data.requires、memoryPolicyRef、governance.changePolicyRef（如存在）和所選 Work.requires 的並集，遞迴展開 Resource.requires。缺失引用或資源依賴循環是定義錯誤。缺少連線、權限或解釋器是相應工作未就緒；其他工作的專用資源未設定不阻塞當前工作。整體必要依賴缺失仍阻塞整體執行。解析支援、設定就緒與實際授權分別報告，不能將局部可用聲明為全部可執行。

## 4. 例行與經驗

**Routine** 含 `id:LocalId、work:LocalId、policyRef:LocalId`，以及選用 `triggerRef:LocalId`。work 引用當前 works；policyRef 指 purpose=policy 的資源，triggerRef 指 purpose=trigger 的資源。執行職務歸屬於當前定義綁定的實例；建立人、排程擁有者和實際授權主體由宿主另行綁定，不必與 Worker 身分相同。不得在匯入時另造員工、擴大例行可見性或把例行移給其他成員。

省略 triggerRef 表示待安排；即使存在，也只表示觸發定義，實例預設未啟用。觸發格式由宿主處理；Teloa 現有 daily/weekly 或本機事件只聲明實際支援範圍，任意 cron 或外部事件不能靜默近似。先驗證真實工作與實際觸發，再經部署者啟用。

例行 policy 必須明確輸入窗口/游標、無輸入與失效輸入行為、觸發去重及業務處理記帳、重試、併發/補跑、預算、通知和暫停條件。缺項時可以保留安排，但不得啟用。成功觸發不等於處理成功；只有約定的成果狀態達成才推進業務處理狀態，重試不得用新觸發編號繞過業務去重。沒有統一的固定重試數或「兩次無進展」產業規則。

memoryPolicyRef 只定義經驗保存、來源、範圍、審閱與撤回規則，不攜帶作者的私有記憶。template-reference 資源是帶來源的範本材料；歷史日誌不能冒充新實例親歷事實。共享員工的個人、業務、專案與群組上下文仍按宿主授權分隔。同一員工實例參與多個團隊不意味著團隊歷史互相可讀。

## 5. 團隊協作與組織治理

| Collaboration 欄位 | 類型 | 含義 |
| --- | --- | --- |
| roles | RoleAssignment[] | 覆蓋每個直接 Member，恰好一次 |
| coordinator | ParticipantRef，選用 | 如提供，只能指當前直接 Member 槽位；省略不自動選人 |
| handoffs | Handoff[] | 明確的責任和材料交接關係；允許為空 |
| sharing | Sharing[] | 共享資源的期望接收位置；允許為空 |
| stop | StopPolicy | 有限協作、無進展與升級規則 |

**RoleAssignment** 含 `member:LocalId、duty:非空字串、participation:member | consultant`。duty 說明在本組合中的分工，不改寫成員原職責；衝突時停止受影響工作。consultant 表示按任務參與的顧問，不自動加入群組或得到群組歷史。兩類槽位的固定定義引用都按核心規則解析；參與範圍在實例綁定和任務授權中落實。

coordinator 與 Work.responsible 均可指向 consultant 槽位，但必須明確交辦並取得當前工作的參與確認；它仍只按任務參與，不因此長期接管團隊或獲得完整群組歷史。`member/` 是引用 Member 槽位的語法，與 participation 中的 member 取值不是同一概念。

**Handoff** 含 `id:LocalId、from:WorkRef、to:WorkRef、when:非空字串、inputs:物件`。WorkRef 只允許 `member/<memberId>/<workId>`，引用當前組合體直接成員的已聲明工作，成員定義版本取自 Member.ref。它表示已產出成果的成員工作向接收成員工作交接；不支援 `self/<workId>`，不把組合體入口/出口混入同一引用語義。子團隊對外暴露自己的 works，不直接穿透其內部成員。

Handoff.inputs 的鍵是接收工作輸入物件的頂層欄位名，值是發送工作輸出物件的頂層欄位名，均按原字串解釋，不執行運算式、腳本或以點號分隔的路徑。欄位須在對應 Schema.properties 中聲明；複製值還須通過接收方 Schema 的實際驗證。接收工作所需的其他輸入必須明確補齊；未齊時回傳缺口，不偽造成功接收。when 是業務條件，只有被明確交辦或由已支援觸發機制確認時才進入交接，不憑一段文字宣稱監聽已實現。

組合體 Work 的輸入分發和輸出彙整由宿主的原生編排或明確交辦的協調者落實，本版不另造入口/出口映射語言。適配報告必須列明：哪個組合體輸入進入哪個成員工作、每個必需輸出由誰及哪個固定成果提供、由誰檢查完整性。定義靜態驗證不證明這些映射已執行；映射或處理者未確定時，相應組合體工作不得標為就緒。組合結果必須通過其 outputs Schema 與 acceptance，不能把成員任意輸出自動拼成團隊成果。

每次實際交接固定來源工作/Run、成果版本、發送與接收主體、映射後的材料、允許範圍和接收/拒收結果。接收方確認並建立工作歸屬後才算交接完成。只傳契約所需且已授權的材料，不隱式轉交整個工作階段、私人記憶或連線憑據。群組中發送訊息與交接成功分別記帳。

**Sharing** 含 `resource:LocalId、recipients:非空 ParticipantRef 陣列`。resource 引用當前 resources，接收者只能為當前組合體的直接 Member 槽位，即 `member/<LocalId>`，包括 consultant 槽位；是否可見及具體版本仍由宿主授權。sharing 不是權限授予，也不會把共享資源自動變成所有成員的必要依賴；工作需要時須由接收工作輸入或其依賴明確承接。

**StopPolicy** 含 `maxHops:正整數、maxNoProgress:正整數、escalateTo:ParticipantRef`。計數作用於同一次工作及其衍生交接鏈，不因換員工或重試歸零。maxHops 限制交接次數；maxNoProgress 限制連續沒有新增可驗證證據或沒有已聲明驗收項改善的輪次。達到任一限制時停止繼續派工並升級；恢復須有可信主體的明確處理和新記錄。具體數值由場景設定，不是員工能力評分。

**Governance** 含 `accountable:ParticipantRef、changePolicyRef:LocalId`。accountable 為組織範圍的責任位置；changePolicyRef 指 purpose=policy 的資源，並自動計入整體依賴。該策略必須明確成員任職/撤任、定義與約束變更、暫停及進行中的工作處理。組織級資料、預算和執行限制通過 data.requires 中的 policy/runtime 資源表達；需繼承的約束必須明確適用成員和作用域，並由宿主核實。約束不能自動擴大成員權限；衝突不得擇寬放行。

AI Team 使用協作契約；本版 AI Organization 定義還必須具備 governance。兩者使用相同 organization 核心類型，不按人數劃分。任意新增組織權限、實際成員管理或跨組織身分系統不在這份定義規範中自動成立。

聲稱完整 Team 或 Organization 定義符合性時，其成員引用閉包中的 agent 均須滿足 AI Worker 定義契約，organization 均須提供本附錄的協作契約；不是每個子團隊都必須聲明 governance。只有核心欄位的臨時 Agent 可由 Worker 的執行機制按授權呼叫，但不能冒充一個已符合本版的持續成員。外部人工或服務驗收位置使用 authorityRoles 表達。

## 6. 宿主綁定與實際執行

綁定記錄存放於部署環境，不強制所有宿主使用同一資料庫或 JSON 結構，但必須可以核對以下資訊：

| 記錄項 | 必須區分的內容 |
| --- | --- |
| 定義位置 | 根定義 id/revision、逐級 Member.id 組成的槽位路徑、路徑終點的固定定義 |
| 實例選擇 | 明確新建還是重用、實際實例身分與版本、所屬可信主體；匯入階段不建立 |
| 連續身分 | 實例身分可跨任務追蹤；模型、工作階段、臨時 Agent 和 Run 身分分別記錄；啟用、暫停、退休與恢復授權可核對 |
| 任務與授權主體 | 管理主體、任務發起人、例行擁有者、資源授權主體分別記錄；共享 Worker 不意味著共用一人的全部授權 |
| 重用關係 | 多個槽位是否綁定同一實例；本版共用須對應相同固定定義，不能按同名推斷 |
| 責任角色 | authorityRoles 對應的可信主體與 human/service 類型；核准人不能來自模型自報身分 |
| 執行條件 | 所選工作、資源和原生格式/版本、實際連線、有效授權、模型及資源限制 |
| 版本與變更 | 綁定版本、採用的成員分工、約束與材料版本；變更後的待辦處理方式 |

同一範本生成兩個實例與同一實例跨團隊任職必須有不同記錄。共用實例的群組可見性和執行授權按所屬工作核對，預算按實際適用的同一額度記帳，不能通過重複綁定獲取兩份原本只有一份的額度。

一次執行必須能追溯根定義、所選工作、實際執行主體、資料及資源版本、有效授權和成果版本。成員替換或範本升級不悄悄接管舊 Run；原執行恢復、終止或新建任務按明確變更策略處理。若宿主無法兌現關鍵約束，保留定義並報告阻塞，不將其降成備註繼續執行。

工作輸入、檢索資料、歷史訊息及工具輸出是帶來源的工作材料，不是更改 Worker 定義或擴大權限的可信指令。宿主必須區分這些材料與授權來源。Worker 可呼叫臨時 Agent，但對外交付仍歸於所選 Worker；臨時執行者不自動成為 Team 成員。獨立複核檢查實際貢獻者及代理關係，不能借新工作階段或臨時助手為自己的成果製造獨立身分。

執行記錄至少區分工作成功、失敗、部分完成、結果未知與暫停，允許宿主使用自己的狀態名稱。部分輸出或逾時回應不能自動標為工作成功；有外部副作用且結果未知時，須先核對原操作狀態再決定重試。任務要求、驗收條件、結果與限制構成品質評估範圍；不規定所有產業共用的分數或「通過一次即可上崗」。

## 7. OASF 與 Agent Spec 的實際吸收邊界

OASF discovery 作為獨立擴展承載，使用固定版本的原生 discovery Schema URI 作為 id、相同的 schema_version 作為 version，原生 payload 原樣置於 data。僅作發現資訊時 required=false；用於不可忽略的執行選擇條件時必須聲明 required=true 並驗證其語義，分類本身仍不授予權限。此處使用本規範的 Extension 包裝；它不同於 A2A Agent Card 中的 AgentExtension 包裝。兩種格式均可承載原生 OASF discovery payload，但其包裝欄位不能直接混用。

例如原生 payload 的 `schema_version、skills、domains` 保持原名和含義。skills 是能力類別，domains 是領域；只有固定目錄中真實存在的條目才可聲明 OASF 分類驗證通過。宿主可離線使用可信的固定 Schema/分類快照，不因匯入 URI 自動存取服務。沒有對應分類時保留未映射說明，不編造官方編號。[OASF discovery](https://docs.agntcy.org/oasf/discovery-extension/)

OASF 的 record 中繼資料和 locators 可作為匯出視圖，來源、版本及未映射欄位進入適配報告；它們不能取代本規範的固定修訂或實際實例身分。來源定義保持單一權威來源，派生視圖不成為第二份可獨立修改的執行設定。[Record Guide](https://docs.agntcy.org/oasf/agent-record-guide/)

Agent Spec 的引用、輸入輸出與組合思想用於本版 Work 和 Handoff。其 `$component_ref` 屬於設定內引用；匯出時須顯式建立它與本規範固定定義/資源的對應關係。其輸入輸出屬性列表與本版物件 Schema 需要轉換，不能只改欄位名。只在完整保留介面、約束和執行語義時聲明所選版本/子集的映射支援。[Agent Spec 26.1.2](https://oracle.github.io/agent-spec/26.1.2/agentspec/language_spec_26_1_2.html)

Swarm、ManagerWorkers 可以作為匹配團隊的適配目標；工作階段轉交、工作成員能否直接面對使用者以及訊息流必須逐項核對。當前契約不要求 Teloa 切換到其執行引擎，也不把全部團隊強制轉換為主管—工作者模式。

## 8. 對現有設計的覆蓋

| 現有要素 | 本規範的去向 |
| --- | --- |
| A01 職務身分與使命 | 核心 id/revision/name/instructions；頭像等展示資料以非關鍵擴展保留 |
| A02 責任與判斷 | responsibility 五組欄位；實際授權在宿主綁定 |
| A03 Skills | purpose=method 的 resources，引用完整方法套件或原生方法資源 |
| A04 知識 | purpose=knowledge，保留來源、版本或摘要，按工作引用 |
| A05 SOP | purpose=procedure 及其資源依賴；跨角色交接由 collaboration 表達 |
| A06 工作範本 | works 的目標、I/O、依賴、負責位置和驗收 |
| A07 例行任務 | routines、原生觸發和執行策略；未設定/未啟用狀態由宿主記錄 |
| A08 應用連線 | purpose=application、原生綁定與實際連線；目錄聲明不代表安裝成功 |
| A09 權限 | 必要 policy 資源、工作範圍、宿主可信授權；不匯出憑據 |
| A10 執行與空間 | runtime 資源要求與宿主模型、預算、檔案範圍綁定 |
| A11 經驗 | memoryPolicyRef 和來源參考；實際私有經驗留在實例 |
| A12 成果與品質 | outputs、acceptance 與執行證據；結構與業務品質分別檢查 |
| T01 共同目標 | organization.instructions 與對外 works |
| T02 職務責任 | Member、collaboration.roles、Work.responsible 和 authorityRoles |
| T03 共享上下文 | sharing、工作輸入以及宿主按範圍授權 |
| T04 協作交接 | handoffs、輸入映射、接收確認與 stop |
| T05 工作安排 | 成員或組合體的 routines、procedure 和協調分工 |
| T06 權限資源 | 整體/工作 requires、成員原權限和宿主實際額度 |
| T07 團隊驗收 | 組合體 outputs/acceptance、成員貢獻及固定成果版本 |

目前社群版儲存庫的 RoleDefinition、GroupDefinition、RoleWorkDelegation、RunRoleSnapshot 和資源套件關係作為適配對象，不改成新的產品物件。Teloa「AI 員工」對應 Worker 產品形態；RoleDefinition、固定職務版本及實際 Role 實例共同參與映射，單獨改名不能證明符合。role-coordination 只表示靜態協調關係；建立群組、派工和自動交接各自需要實際支援與授權。

Teloa `kind: employee | twin` 是產品角色類型，兩者只要滿足同一工作契約均可映射為 Worker；twin 不產生代表本人核准的權利。原 kind 在適配報告或非關鍵產品展示擴展中保留，不能直接寫入核心 kind。`scopes` 連同 dataScope/executionScope 映射為必要的範圍策略及宿主授權綁定；跨宿主無法對應業務範圍時，相關工作保持未就緒。它們屬於關鍵限制，不能只存入可忽略的展示擴展。此處欄位已對照社群版儲存庫 `packages/contract/src/roles.ts`，不代表轉換器已經實現。

Grok 公開 recipe 的 profile 映射職務說明，skills 映射方法，routines 映射工作與待安排的例行，plugins 映射待設定應用，memory 映射帶來源的範本材料；其中 profile 類工作準則與 log 類作者日誌分別保留。gettingStarted 僅作為入口提示。缺少職責、排程或驗收資訊時報告缺項，不從名字或原作者經驗猜造。完整原件及其摘要由適配報告關聯，關鍵語義不得藏入可忽略擴展。Grok Team Bot 映射共享 Worker，不按名稱自動生成多 Worker 的 Team。

Grok Team Bot 的公開文件將個人例行歸屬於建立者，連線也可能按當前使用者或 Bot 憑據工作。因此適配時必須保留例行擁有者、執行身分及資料可見範圍，不將所有人的例行合併成共享 Worker 的全員任務。這是來源行為的映射要求，不要求其他宿主複製其權限預設值。[Team Bots](https://docs.x.ai/grok-bot/team-bots)

**私人例行的落點：** 個人例行只映射為宿主側按擁有者與可見範圍隔離的安排，引用 Worker 已聲明的 Work；不得進入共享定義的 routines 或被共享匯出。適配報告標記「保留為私人實例安排，未進入定義」，報告本身也須服從其可見範圍。只有原作者明確發佈為範本並去除私有內容後，才可將範本放入定義的 routines；匯入仍預設不啟用。無法保留擁有者、可見性或工作引用時，報告無法映射，不得合併或靜默丟失。

適配報告至少列出來源格式/版本/摘要、目標定義修訂、欄位對應、保留/轉換/未支援項，以及未支援項影響的工作。原件留存不等於語義適配成功。轉換不能兌現權限、交接或驗收條件時必須阻止受影響工作。

## 9. 正反例與完成聲明

[完整組成範例](../examples/composition-fixture.json)使用固定的本機定義集合表達：兩名員工、兩個團隊重用這兩份員工定義、一個巢狀組織，以及宿主綁定與工作就緒的預期。集合外殼和說明是測試夾具格式，不是新增分發協定，也不是可直接匯入 Teloa 的資源套件。

| 檢查 | 必須得到的結論 |
| --- | --- |
| 零技能、零記憶、零例行的簡單職務 | 可定義；明確工作、I/O、責任和驗收仍必需 |
| 只有名稱、提示詞和 kind=agent | 可以符合核心交換；不得據此聲明完整 AI Worker 定義 |
| triggers、自主動作或交付檢查為空 | 不符合本版工作附錄；不能靠產品名稱補足 |
| 同一 Worker 啟動兩次任務、更換模型 | 必須能追蹤同一 Worker 實例及兩個獨立 Run；僅兩個工作階段不能證明身分連續性 |
| 同一員工定義在兩個團隊出現 | 只說明定義重用；是否同一實例由槽位綁定決定 |
| 一個工作缺專用應用，另一工作輸入齊全 | 前者未就緒；後者仍核對整體條件及自己的依賴，不自動被前者阻塞 |
| 子組織版本缺失、循環或未知必需 Profile | 停止裝配並報告具體引用，不換版或忽略 |
| 錯誤接收欄位、缺必要輸入或輸出類型不符 | 不能標交接成功或成果符合；不自動補值、轉換類型 |
| 兩個複核槽位綁定同一實際執行主體 | independent=true 不滿足 |
| 顧問只參與某項複核 | 不自動加入全部群組、讀取全部歷史或複製新員工 |
| 計劃缺觸發或策略不完整 | 保留為未就緒安排，不啟用 |
| OASF 分類未知或 Agent Spec 協作模式或介面約束不等價 | 報告不支援/映射差異，不宣稱全面相容 |

本規範聲明的是結構與行為契約。文稿檢查、範例檢查、真實連線/模型執行、獨立實現互通和產業採用分別報告。驗證範圍與實現報告要求見[符合性說明](../CONFORMANCE.md)。
