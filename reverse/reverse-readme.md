# Reverse API 說明

KM 針對特定客戶做 Reverse API 接入，每間客戶的API都不一樣，因此紀錄客戶各自的API資訊，以便快速查找問題及支援。

```mermaid
graph LR
    A[KM Game server] --> B[QM API server]
    B --> C[Seameless Client server]
```