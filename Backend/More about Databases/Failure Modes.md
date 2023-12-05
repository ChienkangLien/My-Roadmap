# Failure Modes
## 競爭contention
可以分為讀取Read 跟寫入Write 兩種競爭，
當多個客戶端或進程嘗試同時從資料庫中的相同位置讀取資料、或是將資料寫入資料庫中的相同位置，就會發生這種情況，這可能會導致延遲或錯誤。

## 驚群效應Thundering herd
當許多進程等待一個事件，事件發生後這些進程被喚醒，但只有一個進程能獲得CPU執行權，其他進程又得被阻塞，這造成了嚴重的系統上下文切換代價。

資料庫中也有同樣問題，當大量客戶端或進程嘗試同時存取相同資源時，就會發生這種情況。
## 級聯Cascade
這邊說的是故障的類型，當資料庫系統的一個部分發生故障導致連鎖反應，進而導致系統其他部分發生故障時，就會發生這種情況。

另外對於RDMS資料本身的操作，也有著級聯更新和級聯刪除是兩種常見的級聯操作。
## 死鎖Deadlock
當兩個或多個事務正在等待彼此釋放資源上的鎖時，就會發生這種情況，導致停滯。

關於鎖
### 悲觀鎖（Pessimistic Locking）
假設會發生並發沖突，因此在讀取或修改數據之前會進行鎖定，以防止其他事務對相同數據進行修改。
* 排他鎖（Exclusive Locks）：也稱為寫鎖，它會阻止其他事務對被鎖定資源進行任何讀寫操作，直到鎖被釋放。
* 共享鎖（Shared Locks）：也稱為讀鎖，多個事務可以同時持有共享鎖，但它們之間互斥於排他鎖。共享鎖允許多個事務並發地讀取同一資源，但會阻止其他事務獲取排他鎖。
* 意向鎖（Intention Locks）：在鎖住資源之前，會先請求意向鎖，通常是在行級或表級鎖定之前。記錄鎖（Record Locks）：悲觀鎖的一種形式，用於鎖定數據庫表中的單個記錄或行。
* 間隙鎖（Gap Locks）：在範圍查詢或者唯一索引掃描時，鎖定範圍內的間隙，防止新數據插入到已被讀取範圍內。

關於粒度
* 行級鎖（Row-level Locks）：鎖定數據庫中的單個行，允許事務獨立地操作不同行的數據。行級鎖通常用於悲觀鎖的實現。
* 表級鎖（Table-level Locks）：鎖定整個數據庫表，一旦被鎖定，其他事務無法對該表進行任何操作。表級鎖粒度較大，可能會對並發性造成較大影響。
### 樂觀鎖（Optimistic Locking）
不會直接鎖定資源，而是在更新時檢查數據是否發生了變化。
* 版本控制（Versioning）：使用版本號或時間戳來控制數據，從而實現樂觀鎖的機制。允許多個事務同時讀取同一資源，但在寫入時會檢查數據是否已被修改。
* CAS（Compare and Swap）：一種常見的樂觀鎖算法，比較當前值和預期值，如果相等則替換成新值，通常用於內存操作。

## 資料損壞Data corruption
當資料庫中的資料損壞時就會發生這種情況，這可能會導致在讀取或寫入資料庫時發生錯誤或意外結果。
## 拒絕服務 (DoS) 攻擊：
是一種網路攻擊手法，其目的在於使目標電腦的網路或系統資源耗盡，使服務暫時中斷或停止，導致其正常使用者無法訪問。

具體分成三種形式：頻寬消耗型、資源消耗型、漏洞觸發型。前兩者都是透過大量合法或偽造的請求占用大量網路以及器材資源，以達到癱瘓網路以及系統的目的。而漏洞觸發型，則是觸發漏洞導致系統崩潰癱瘓服務。
## 其他故障
* 硬體故障：
當磁碟機或記憶體等硬體組件發生故障時，就會發生這種情況，這可能會導致資料遺失或損壞。
* 軟體故障：
當軟體元件（例如資料庫管理系統或應用程式）發生故障時，就會發生這種情況，這可能會導致錯誤或意外結果。
* 網路故障：
當資料庫和用戶端之間的網路連線遺失時會發生這種情況，這可能會導致嘗試存取資料庫時發生錯誤或逾時。