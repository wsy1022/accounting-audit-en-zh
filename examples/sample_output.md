# 示例：合并利润表（中英对照）

下面这段是 `app/translator.py` 用内置术语库（750 条）翻译的真实输出，未做人工修改。

| English | 中文 |
|---|---|
| CONSOLIDATED STATEMENTS OF OPERATIONS | 合并利润表 |
| (in thousands, except per share data) | （单位：千元，每股数据除外） |
|                                           Year ended Dec 31, | 截至12月31日止年度 |
|                                         2024            2023 | 2024            2023 |
| Revenue                              $ 1,284,500     $ 1,102,340 | 收入                              $ 1,284,500     $ 1,102,340 |
| Cost of revenue                         (742,180)       (668,900) | 营业成本                             (742,180)       (668,900) |
| Gross profit                             542,320         433,440 | 毛利                                  542,320         433,440 |
| Research and development expenses        (96,400)        (84,120) | 研发费用                              (96,400)        (84,120) |
| Selling, general and administrative     (188,750)       (172,300) | 销售、一般及管理费用                 (188,750)       (172,300) |
| Operating income                         257,170         177,020 | 营业利润                              257,170         177,020 |
| Interest expense, net                    (18,300)        (21,450) | 利息费用，净额                        (18,300)        (21,450) |
| Other income, net                          3,240           1,980 | 其他收益，净额                          3,240           1,980 |
| Income before income taxes               242,110         157,550 | 利润总额                              242,110         157,550 |
| Provision for income taxes               (58,106)        (37,812) | 所得税费用                            (58,106)        (37,812) |
| Net income                             $ 184,004       $ 119,738 | 净利润                             $ 184,004       $ 119,738 |
| Earnings per share - diluted           $    1.84       $    1.21 | 每股收益 - 稀释                       $    1.84       $    1.21 |
| Weighted average shares outstanding      100,000          99,000 | 加权平均流通在外股份                  100,000          99,000 |
| NOTE 2 — ACCOUNTS RECEIVABLE | 附注2 — 应收账款 |
| Trade receivables are stated at net realisable value after deducting an allowance for doubtful | 应收账款按可变现净值列示，已扣除截至2024年12月31日的坏账准备$12,400（2023年：$9,870）。该准备根据历史损失经验和前瞻性信息估计。 |

---

观察点：

- `cost of revenue` → 营业成本、`operating income` → 营业利润、`net income` → 净利润 —— 这三处是最常见的错译点，术语库直接锁定。
- 括号负数、千分位、`$` 全部原样保留，单位写在表头。
- 段末附注保留「按……列示」「依据……估计」的中文财务句式。
