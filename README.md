# Assay 数据包

Assay 试金的公开数据包分发点。
**这个仓库只放数据与索引，不放代码。**

## 里面是什么

一份 19 MB / 19 个 parquet 的美股基本面数据包：SEC XBRL 财报锚、披露日、Form 4 内部人交易、
13F 机构持股、众议院 PTR、分部、10-K 叙事、体检结论与处方、预报及其结算，
以及一份逐条可在 EDGAR 复核的**崩塌 / 执法标签库**。

**不含行情。** 日线没有再分发许可，所以行情与一切由行情衍生的结论都不在包里
（许可状态取最弱的那一环）。

## 许可

SEC 申报是美国政府作品，属公有领域（17 U.S.C. §105），**可自由再分发**。
每个数据集在 `MANIFEST.json` 里标着来源层级（`DISCLOSED` / `DERIVED` / `REFERENCE` / `FORECAST`），
标为 `STRUCTURAL` 的合成明细不参与判定、也不随包分发。

## 怎么用

客户端读 `data-latest.json`，它是一份约定好的索引：

```json
{
  "version": "2026-08-27",
  "file": "assay-public-2026-08-27.zip",
  "bytes": 19566010,
  "sha256": "42906df6…f080c5",
  "url": "https://github.com/…/releases/download/data-2026-08-27/assay-public-2026-08-27.zip"
}
```

**它不绑任何一家的 releases API** —— 只是一个裸 JSON，`url` 指哪儿都行。
换渠道只需要改这一个字段，客户端一行不用动。

手工用也一样：下 zip，比对下面这行哈希，解压。

## 各版哈希

发布页面上这一行是**唯一的信任锚点** —— 包里的 `MANIFEST.json` 存着各文件的哈希，
却没法把自己的哈希写进自己；能替换 parquet 的人也能顺手替换清单。

| 版本 | 文件 | SHA-256 |
|---|---|---|
| 2026-08-27 | `assay-public-2026-08-27.zip` | `42906df68e8c9c049bb434804c9cc5670ecf8eb63c9b2ceb0638e82aadf080c5` |

## 发一版新的

1. 在有全量库的机器上跑 `release-public-data.ps1`，得到 zip 与哈希
2. 建一个 tag 为 `data-<版本>` 的 release，把 zip 传成附件
3. 改这个仓库根目录的 `data-latest.json`（五个字段）并 push
4. 把哈希补进上面那张表

客户端下次启动就会看见。**顺序不能反** —— 索引先更新、附件还没传上去，
所有客户端会同时开始下一个 404。
