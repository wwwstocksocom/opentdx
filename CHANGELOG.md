# Changelog

## 202606

OpenTDX 并不是调用 pytdx。它重新实现了TDX Binary Protocol的协议解析。
~~~
RainX
│
├── pytdx
│
│   （停止维护）
│
└──────────────┐
               │
        LisonEvf
               │
        想继续做 pytdx2
               │
      发现 PyPI 名称被占
               │
               ▼
          OpenTDX

~~~




## [0.2.1] - 2026-04-30

### Added

- 新增 bitmap 字段: `limit_up_count`/`limit_down_count` (涨停/跌停数), `up_count`/`down_count` (上涨/下跌家数), `vol_speed_pct` (量涨速%), `short_turnover_pct` (短换手%), `circulating_capital_z` (流通股本Z), `avg_price_copy` (均价备份), `annual_limit_up_days` (年涨停天数), `industry_sub` (行业二级分类), `auction_vol_ratio` (竞价量比), `constant_neg_one` (恒值-1)
- 新增预设: `PresetField.BOARD_STATS` (涨停/跌停/上涨/下跌家数)
- 新增命令 `opentdx mm` 支持主力监控查询, --search 指定

### Changed

- 澄清金额字段单位: amount/open_amount/amount_2m 返回**元**, TDX客户端显示为万元(需÷10000); total_market_cap_ab 返回元, CSV显示为亿元(需÷1亿)

## [0.2.0] - 2026-04-29

### Added

- 增加命令 `opentdx mm`  支持主力监控查询, --search 指定

