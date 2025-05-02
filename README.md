# 2025cloud
雲原生作業4

## 建立 Docker Image

請在此專案資料夾中執行以下指令：

```bash
docker build -t cyjginny/2025cloud:v1 .
docker build -t cyjginny/2025cloud:v2 .
```
## 執行 Container Image（docker run）

建構完成後，請使用以下指令來執行容器：
```bash
docker run -d -p 8080:80 cyjginny/2025cloud:v1
docker run -d -p 8080:80 cyjginny/2025cloud:v2
```

