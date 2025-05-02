# 2025cloud
雲原生作業 4

##  描述如何透過 docker build 與  docker run 達到作業要求
### 建立 Docker Image

請在此專案資料夾中執行以下指令：

```bash
docker build -t cyjginny/2025cloud:v1 .
docker build -t cyjginny/2025cloud:v2 .
```
### 執行 Container Image（docker run）

建構完成後，請使用以下指令來執行容器：
```bash
docker run -d -p 8080:80 cyjginny/2025cloud:v1
docker run -d -p 8081:80 cyjginny/2025cloud:v2
```
### 確認是否執行成功
1. 查看目前執行中的容器：
```bash
docker ps
```
2. 開啟瀏覽器：
- http://localhost:8080 （對應 v1）
- http://localhost:8081 （對應 v2）


## 圖文說明目前專案自動化產生 Container Image 以及 Tag 的選擇的邏輯
1. 自動化產生 Container Image 的邏輯 :採用 GitHub Actions 作為 CI 工具，當下列觸發條件發生時，會自動執行建置流程
- 觸發條件 1：主分支有任何變更（例如修改 Dockerfile 或 app.py），會觸發自動建置與推送流程
- 觸發條件 2：若有人對 main 發 PR，也會執行 Build 檢查流程，驗證是否能成功建構 Image
2. 自動化流程內容
```bash
    A[Push to main branch] --> B[Trigger GitHub Action]
    B --> C[Checkout code]
    C --> D[Login to DockerHub]
    D --> E[Build Docker Image]
    E --> F{是否設定 push?}
    F -- 是 --> G[Push to DockerHub]
    F -- 否 --> H[僅本地建置，不推送]
```
  
3. 建置流程（build-and-push）
GitHub Actions 中的 docker-build.yml 指定以下步驟：
- checkout：抓取最新的程式碼
- docker login：使用 Secrets 中帳密登入 Docker Hub
- docker build：建置 Docker Image
- docker push（可選）：推送 Image 至 Docker Hub


##  圖文說明目前 Image Tag 設計邏輯
1. Tag 名稱與用途
- gh-auto : 表示為 GitHub Action 自動產生的版本
- v1, v2 : 表示為手動建立的版本
