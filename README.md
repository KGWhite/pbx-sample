# pbx-server-template

## 目錄結構
```
/pbx-server-template
├── README.md
├── docker-compose.yml          # Root docker-compose (if any)
├── hal                         # Hardware Abstraction Layer (硬體與宿主機抽象層)
│   └── os
│       ├── general             # 通用於各大 Linux 發行版的通用腳本與驅動
│       │   ├── drivers
│       │   ├── packages        # 存放通用的離線安裝包或原始碼檔 (如 .tar.gz)
│       │   └── scripts
│       └── rocky               # 專屬於 Rocky Linux 的腳本與驅動 (例如 dnf 或特製 rpm)
│           ├── drivers
│           ├── packages        # 存放 Rocky 專用的套件檔 (如 .rpm 檔)
│           └── scripts
├── services                    # 純軟體容器層 (無關硬體，隨處可跑)
│   ├── api
│   │   └── Dockerfile          # Golang + Gin API 服務
│   ├── db
│   │   └── Dockerfile          # PostgreSQL 資料庫服務
│   ├── docker-compose.yml      # 管理四個容器的設定檔
│   ├── pbx
│   │   ├── Dockerfile          # 編譯 Asterisk PBX 服務
│   │   ├── configs
│   │   ├── patches
│   │   └── source
│   │       └── asterisk
│   └── web
│       ├── Dockerfile          # React 前端服務
│       └── nginx.conf          # Nginx 反向代理設定
└── tests
```

## 服務管理

目前 `services` 目錄下已經配置了四個主要的 Docker 容器：
1. **api**: 基於 Golang 與 Gin 框架構建的後端 API。
2. **db**: 基於 PostgreSQL 的資料庫。
3. **pbx**: 從 `source/asterisk` 編譯生成的 Asterisk PBX 服務。
4. **web**: 基於 React 構建並使用 Nginx 部署的前端服務。

可以使用 `services/docker-compose.yml` 統一管理這四個容器：

```bash
cd services
docker-compose up -d --build
```