# Supplementary Data 7 — Human validation forms and agreement statistics

## 1. Design

- Sample: 120 records, stratified by crop (wheat / rice / maize) and journal (Cell / Nature / Science), drawn at random from the 616-record corpus.
- Raters: two independent reviewers with crop-science backgrounds, blinded to the LLM labels; a third reviewer arbitrated residual disagreements.
- Instrument: coding form below (identical theme definitions and decision rules as in the LLM prompts, Supplementary Data 3).

## 2. Coding form (per record)

| Field | Entry |
|---|---|
| Record ID / DOI | (pre-filled) |
| Primary theme | (free choice from the theme list, Supplementary Data 5) |
| Secondary theme(s) | (optional) |
| Entity normalization correct? (authors / affiliations / countries) | Yes / No; if No, list wrong items |
| Boundary case? | Yes / No (adjacent or overlapping themes) |
| Comments | |

## 3. Agreement statistics

### 3.1 Human–human baseline (theme coding, Rater 1 vs Rater 2)
- Status: IN PROGRESS — the coding forms (Section 2) are in use; statistics will be reported separately when the review is complete. No value is estimated here.

### 3.2 Human–LLM agreement (theme coding)
- Status: IN PROGRESS — to be reported with the expert review; the benchmarking criterion (Susnjak et al., 2026: LLM–human κ comparable to expert–expert κ) will be applied once both ratings exist.

### 3.3 Entity normalization (vs adjudicated reference)
- Precision = 【待填】; recall = 【待填】; F1 = 【待填】
- Error source: 【待填：别名列举 / 语境误判】
- Supporting deterministic audit (computable from the export; not a substitute for the adjudicated reference): a case/punctuation/spacing-insensitive duplicate scan of the normalized tables found 19 candidate residual duplicate pairs among 6,842 author names (e.g., "Gershenzon, J" vs "Gershenzon, J."; "Wang, Jia-Wei" vs "Wang, Jiawei"), 5 among 1,142 affiliation names (e.g., "Bayer Crop Sci" vs "Bayer Cropsci"; "Shanghai Tech Univ" vs "Shanghaitech Univ") and 0 among 56 country names. All 24 pairs were adjudicated as missed merges of spelling variants (18 certain, e.g. "Bayer Crop Sci"/"Bayer Cropsci", "Jiang, Yu-Ying"/"Jiang, Yuying"; 6 probable, all single-initial variants, e.g. "Li, C"/"Li, C."). These pairs upper-bound the missed-merge component of recall; false-merge (precision) and context-dependent errors still require the adjudicated reference.

### 3.4 All-coder agreement (experts + repeat runs)
- Executed portion (production labels + three prompt-variant runs of the alternative model, n = 118 records labelled by the production model): Fleiss' κ = 0.847; across the three alternative-model runs alone: Fleiss' κ = 0.933; pairwise exact agreement 91.7–95.0% (median 93.3%); median pairwise theme-set Jaccard 1.0 (mean 0.93).
- All-coder κ including the two experts: pending (Section 3.1).

### 3.5 Convergence with conventional co-word clustering — COMPUTED FROM THE DATA SOURCE
Because author keywords were available for only 3 of the 616 records, LLM theme assignments were compared with title-term co-word clusters instead (TF-IDF over the exported title-term lists; KMeans, k = number of themes in the analysis set; 20 restarts with n_init = 10).

- Analysis set: 322 records spanning the 96 themes assigned to at least two records (of 610 records with theme assignments and 577 records with both terms and themes).
- **NMI = 0.740 ± 0.005 (range 0.734–0.749); ARI = 0.101 ± 0.010** (k = 96).
- Cluster-number sensitivity: k = 10 → NMI 0.340 / ARI 0.008; k = 20 → 0.467 / 0.018; k = 50 → 0.627 / 0.045; k = 96 → 0.740 / 0.101.
- Interpretation: substantial shared information but low exact pairwise agreement — the partitions answer different questions, and partial correspondence is the expected outcome (Section 3.6).

### 3.6 Repeat coding (three prompt variants; three random seeds at non-zero temperature; one alternative model, Kimi K3)
- Alternative-model check — EXECUTED (Kimi K3, Moonshot AI): blinded recoding of the same 120-record sample under the identical 400-theme taxonomy, titles only (a conservative variant of the production title-and-abstract workflow; the analysis export retains titles, not abstracts). Results: Cohen's κ = 0.760 (95% bootstrap CI 0.681–0.836, 10 000 resamples, seed 42; n = 118 records labelled by both models); exact primary-theme agreement 90/118 = 76.3%; alternative label contained in the production theme set for 100/120 = 83.3% of records; of 28 discordant pairs, 23 (82.1%) were boundary cases within the same higher-order community and only 2 (records wos:a1987k781000034, wos:000183443400045) reassigned a record between the three principal communities of Section 3.2. The full audit table is given in Section 4 below.
- Prompt-variant and non-zero-temperature seed runs of the production model: Fleiss' κ across runs = 【待填】; median pairwise Jaccard of entity mappings / theme sets = 【待填】. Requires re-running the production LLM API; not computable from the exported data alone.

### 3.7 Boundary-case review (n = 50) — EXECUTED (alternative-model adjudication)
- Design: boundary-prone records = production theme set contains at least one same-community theme pair (111 of 610 labelled records); 50 drawn at random (seed 2026); adjudicated by Kimi K3 under the merge–split rules of Supplementary Data 5, blinded to all agreement statistics. Human re-adjudication remains pending.
- Results: accepted without change 27/50 = 54%; merge corrections (a redundant same-community theme pair) 23/50 = 46%; split corrections 0/50. No corrected case altered the assignment of any record to the three principal communities of Section 3.2.
- The full adjudication table is in Section 6 below.

---

## 4. Alternative-model repeat coding (Kimi K3): design and audit table

**Sampling.** 120 records from the 616-record corpus, stratified by journal (Cell / Nature / Science) and crop stratum from title keywords (wheat / rice / maize, including the Latin names Triticum / Oryza / Zea mays / teosinte; titles naming several crops or none form separate strata), proportional allocation with largest-remainder rounding, `numpy.random.default_rng(2026)` (code in Supplementary Data 10). Stratum sizes and allocations: maize|Cell 17→3; maize|Nature 43→8; maize|Science 69→13; multiple|Nature 1→0; multiple|Science 2→0; none-named|Cell 54→11; none-named|Nature 137→27; none-named|Science 141→28; rice|Cell 14→3; rice|Nature 47→9; rice|Science 39→8; wheat|Cell 3→1; wheat|Nature 21→4; wheat|Science 28→5.

**Coding protocol.** Kimi K3 (Moonshot AI) received the record titles only — never the production (DeepSeek) labels — and assigned one primary theme per record from the identical 400-theme taxonomy (Supplementary Data 5). Entity normalization was not re-run; this check targets theme coding. Two sampled records carry no production primary theme and enter the containment analysis but not Cohen's κ.

**Higher-order communities** (operationalization of Section 3.2, applied to Chinese theme labels, in precedence order): Disease & immunity = label matches 抗病|免疫|瘟病|病抗|侵染|致病|效应子|病原; Genomics = 基因组|测序|转座子|内含子|染色体|减数分裂|突变率|表观遗传|甲基化|印记|自私遗传|基因复制|DNA重排|多倍体|泛基因组; Development & regulation = 发育|调控|信号|开花|分生组织|细胞命运|细胞分化|气孔|向性|糖信号; otherwise 'outside'. Discordant pairs in the same community are boundary cases; otherwise direct.

**Theme labels are given in Chinese exactly as in the data-source export; English translations are in Supplementary Data 5.**

| # | RecordId | Journal | Year | Crop | DeepSeek primary | Kimi K3 primary | Exact match | Contained | Discord type |
|---|----------|---------|------|------|------------------|-----------------|-------------|-----------|--------------|
| 1 | wos:a1988m922100015 | Cell | 1988 | none-named | 拟南芥端粒 | 拟南芥端粒 | yes | yes | — |
| 2 | wos:a1991ge46000009 | Cell | 1991 | maize | 玉米发育基因 | 玉米发育基因 | yes | yes | — |
| 3 | wos:a1991er41800048 | Nature | 1991 | none-named | 大气CO2同位素重建 | 大气CO2同位素重建 | yes | yes | — |
| 4 | wos:000230964500047 | Nature | 2005 | maize | 玉米驯化起源 | 玉米驯化起源 | yes | yes | — |
| 5 | wos:a1996un47200045 | Science | 1996 | maize | 玉米雄性不育 | 玉米雄性不育 | yes | yes | — |
| 6 | wos:a1993lu59200018 | Cell | 1993 | maize | 玉米花发育 | 玉米花发育 | yes | yes | — |
| 7 | wos:000230938200041 | Science | 2005 | rice | 水稻细胞分裂素与产量 | 水稻细胞分裂素与产量 | yes | yes | — |
| 8 | wos:000920432900010 | Science | 2023 | maize | 玉米抗寄生植物 | 独脚金寄生抗性 | no | no | boundary |
| 9 | wos:000084769600039 | Science | 2000 | rice | 水稻β-胡萝卜素生物合成 | 水稻β-胡萝卜素生物合成 | yes | yes | — |
| 10 | wos:000513111200001 | Nature | 2020 | rice | 水稻基因表达自然选择 | 水稻基因表达自然选择 | yes | yes | — |
| 11 | wos:000231116500034 | Nature | 2005 | rice | 水稻基因组 | 水稻基因组 | yes | yes | — |
| 12 | wos:000730550300007 | Nature | 2022 | none-named | 植物免疫 | 植物免疫 | yes | yes | — |
| 13 | wos:000258228000044 | Nature | 2008 | none-named | 花青素途径基因进化 | 基因复制与适应性冲突 | no | yes | direct |
| 14 | wos:001468217600001 | Nature | 2025 | rice | 水稻泛基因组 | 水稻泛基因组 | yes | yes | — |
| 15 | wos:000396119500035 | Nature | 2017 | rice | 磷利用效率 | 磷利用效率 | yes | yes | — |
| 16 | wos:000343420300004 | Science | 2014 | wheat | 小麦基因组 | 小麦基因组 | yes | yes | — |
| 17 | wos:a1985asw2500037 | Science | 1985 | none-named | 臭氧与植物光合作用 | 臭氧与植物光合作用 | yes | yes | — |
| 18 | wos:001045155200027 | Nature | 2023 | wheat | 小麦驯化起源 | 小麦驯化起源 | yes | yes | — |
| 19 | wos:000345770600042 | Nature | 2014 | none-named | 农田生产力与大气CO2季节性 | 农田生产力与大气CO2季节性 | yes | yes | — |
| 20 | wos:000282644600042 | Science | 2010 | maize | Bt玉米害虫抗性 | Bt玉米害虫抗性 | yes | yes | — |
| 21 | wos:000386869800053 | Science | 2016 | none-named | 植物抗虫性 | 植物抗虫性 | yes | yes | — |
| 22 | wos:000228160700032 | Nature | 2005 | maize | 植物挥发物信号 | 植物挥发物信号 | yes | yes | — |
| 23 | wos:000274394300030 | Nature | 2010 | none-named | 短柄草基因组 | 短柄草基因组 | yes | yes | — |
| 24 | wos:000865667300003 | Cell | 2022 | wheat | 小麦抗锈病 | 小麦抗锈病 | yes | yes | — |
| 25 | wos:000396351200046 | Science | 2017 | none-named | 气孔发育 | 气孔发育 | yes | yes | — |
| 26 | wos:000079228400052 | Nature | 1999 | maize | 玉米驯化选择 | 玉米驯化选择 | yes | yes | — |
| 27 | wos:000416909500006 | Science | 2017 | wheat | 小麦瘟病真菌进化 | 小麦瘟病真菌进化 | yes | yes | — |
| 28 | wos:000242215800043 | Science | 2006 | wheat | 小麦籽粒营养品质 | 小麦籽粒营养品质 | yes | yes | — |
| 29 | wos:000239154300050 | Science | 2006 | rice | 作物根际微生物组 | 稻田甲烷排放 | no | no | boundary |
| 30 | wos:000404358500014 | Cell | 2017 | rice | 水稻免疫 | 水稻稻瘟病抗性 | no | no | boundary |
| 31 | wos:000228810900048 | Science | 2005 | rice | 植物抗虫性 | 植物抗虫性 | yes | yes | — |
| 32 | wos:000313871400034 | Nature | 2013 | none-named | 边际土地生物能源 | 边际土地生物能源 | yes | yes | — |
| 33 | wos:001365199300001 | Nature | 2025 | wheat | 小麦泛基因组 | 小麦泛基因组 | yes | yes | — |
| 34 | wos:a1988p120800036 | Science | 1988 | maize | 抗病性 | 抗病性 | yes | yes | — |
| 35 | wos:000271951000031 | Science | 2009 | none-named | 玉米基因组 | 古基因组学 | no | no | boundary |
| 36 | wos:000187385200043 | Science | 2003 | maize | 玉米基因组 | 玉米基因组 | yes | yes | — |
| 37 | wos:a1987j666500038 | Science | 1987 | maize | 植物转座子 | 玉米转座子 | no | no | boundary |
| 38 | wos:a1986axu6400064 | Nature | 1986 | maize | DNA甲基化 | DNA甲基化 | yes | yes | — |
| 39 | wos:000351951800019 | Cell | 2015 | rice | 水稻耐冷性 | 水稻耐冷性 | yes | yes | — |
| 40 | wos:a1987h602200065 | Nature | 1987 | maize | 抗病性 | 植物抗病性 | no | no | boundary |
| 41 | wos:000262862800047 | Science | 2009 | maize | 作物细胞分化 | 作物细胞分化 | yes | yes | — |
| 42 | wos:000271951000046 | Science | 2009 | maize | 玉米基因表达调控 | 玉米基因表达调控 | yes | yes | — |
| 43 | wos:a1989u111600023 | Science | 1989 | maize | 植物转座子 | 玉米转座子 | no | no | boundary |
| 44 | wos:a1992hh74800014 | Cell | 1992 | none-named | 花青素途径基因进化 | 花青素途径基因进化 | yes | yes | — |
| 45 | wos:001216996700004 | Science | 2024 | rice | 水稻穗型调控 | 油菜素甾醇信号 | no | yes | boundary |
| 46 | wos:a1989am93300024 | Science | 1989 | wheat | 转录调控 | 转录调控 | yes | yes | — |
| 47 | wos:001461192500026 | Science | 2025 | wheat | 植物免疫 | 抗病性 | no | yes | boundary |
| 48 | wos:a1990dk40800034 | Science | 1990 | maize | 玉米转座子表观调控 | 转座子活性调控 | no | no | boundary |
| 49 | wos:000181669700047 | Science | 2003 | none-named | 赤霉素信号 | 赤霉素信号 | yes | yes | — |
| 50 | wos:000232157900044 | Nature | 2005 | none-named | 赤霉素信号 | 赤霉素信号 | yes | yes | — |
| 51 | wos:000223514900044 | Nature | 2004 | none-named | 花发育调控基因 | 细胞分裂素信号 | no | yes | boundary |
| 52 | wos:000272623600048 | Science | 2009 | none-named | 转录调控 | DNA识别机制 | no | yes | direct |
| 53 | wos:001443572500001 | Nature | 2025 | rice | 水稻免疫 | 水稻免疫 | yes | yes | — |
| 54 | wos:a1986d125500015 | Cell | 1986 | maize | 内含子进化 | 内含子进化 | yes | yes | — |
| 55 | wos:a1992kb96400033 | Science | 1992 | maize | 花青素途径基因工程 | 花青素途径基因工程 | yes | yes | — |
| 56 | wos:000859793500006 | Nature | 2022 | wheat | 植物免疫 | 植物免疫 | yes | yes | — |
| 57 | wos:a1996vf61000048 | Science | 1996 | maize | 受体蛋白激酶 | 受体蛋白激酶 | yes | yes | — |
| 58 | wos:000228273700055 | Science | 2005 | none-named | 昆虫宿主种族分化 | 昆虫宿主种族分化 | yes | yes | — |
| 59 | wos:000512382900023 | Science | 2020 | rice | 氮利用效率 | 氮利用效率 | yes | yes | — |
| 60 | wos:000464956600053 | Science | 2019 | rice | 水稻基因组编辑 | 水稻基因组编辑 | yes | yes | — |
| 61 | wos:000285153500067 | Science | 2010 | none-named | 黑粉菌致病机制 | 黑粉菌致病机制 | yes | yes | — |
| 62 | wos:000228524600029 | Nature | 2005 | rice | 稻瘟菌基因组 | 稻瘟菌基因组 | yes | yes | — |
| 63 | wos:a1997xn90700044 | Science | 1997 | none-named | 禾本科化学防御 | 禾本科化学防御 | yes | yes | — |
| 64 | wos:000325988400055 | Nature | 2013 | maize | 玉米发育基因 | 分生组织调控 | no | no | boundary |
| 65 | wos:a1994nh01000033 | Science | 1994 | none-named | 植物向性信号 | 植物向性信号 | yes | yes | — |
| 66 | wos:000085204400011 | Cell | 2000 | none-named | 基因组测序 | 基因组测序 | yes | yes | — |
| 67 | wos:000223085400043 | Nature | 2004 | none-named | 旧石器时代谷物加工 | 旧石器时代谷物加工 | yes | yes | — |
| 68 | wos:000172405900046 | Nature | 2001 | maize | 转基因玉米基因流 | 转基因玉米基因流 | yes | yes | — |
| 69 | wos:001408515800001 | Nature | 2025 | rice | 赤霉素信号 | 作物碱敏感性调控 | no | yes | boundary |
| 70 | wos:a1990el84300038 | Science | 1990 | none-named | 光敏色素基因转录调控 | 光敏色素基因转录调控 | yes | yes | — |
| 71 | wos:a1987k781000034 | Science | 1987 | maize | 玉米胚乳基因调控 | 转座子标签 | no | yes | direct (cross-community) |
| 72 | wos:000073619900047 | Nature | 1998 | maize | 玉米叶发育 | 玉米叶发育 | yes | yes | — |
| 73 | wos:001396215400001 | Nature | 2025 | wheat | 根内共生信号 | 根内共生信号 | yes | yes | — |
| 74 | wos:000815052900040 | Science | 2022 | rice | 作物耐热性 | 作物耐热性 | yes | yes | — |
| 75 | wos:001479565600001 | Nature | 2025 | none-named | 根组织单细胞转录组 | 根组织单细胞转录组 | yes | yes | — |
| 76 | wos:000174901900042 | Nature | 2002 | none-named | 生态系统气候信号提取 | 生态系统气候信号提取 | yes | yes | — |
| 77 | wos:000183443400045 | Nature | 2003 | none-named | 植物免疫 | DNA重排 | no | yes | direct (cross-community) |
| 78 | wos:a1996tn21600049 | Nature | 1996 | rice | 颗粒物质雪崩动力学 | 颗粒物质雪崩动力学 | yes | yes | — |
| 79 | wos:001412025800001 | Cell | 2024 | rice | 独脚金内酯信号 | 水稻独脚金内酯信号 | no | no | boundary |
| 80 | wos:000261559900034 | Nature | 2008 | none-named | 植物遗传学 | 综述 | no | yes | boundary |
| 81 | wos:000284584200034 | Nature | 2010 | none-named | 糖转运蛋白与病原菌营养 | 糖转运蛋白与病原菌营养 | yes | yes | — |
| 82 | wos:a1995qv41000010 | Cell | 1995 | none-named | 核质转运 | 核质转运 | yes | yes | — |
| 83 | wos:000401906500054 | Nature | 2017 | none-named | 植物免疫 | 植物抗病性 | no | no | boundary |
| 84 | wos:a1995tl42000038 | Science | 1995 | none-named | 胞间连丝运输 | 胞间连丝运输 | yes | yes | — |
| 85 | wos:000221524500041 | Science | 2004 | none-named | 植物体内定向进化 | 除草剂抗性 | no | no | boundary |
| 86 | wos:a1988q391900047 | Science | 1988 | none-named | 古代农业起源 | 中美洲古代农业起源 | no | no | boundary |
| 87 | wos:a1993kx80000035 | Science | 1993 | none-named | 拟南芥转座子 | 拟南芥转座子 | yes | yes | — |
| 88 | wos:000244039400045 | Nature | 2007 | none-named | — | 细胞分裂素信号 | — | no | — |
| 89 | wos:000280141200035 | Nature | 2010 | none-named | — | 昆虫性信息素调控 | — | no | — |
| 90 | wos:a1997xk41800044 | Science | 1997 | none-named | 木质素生物合成 | 木质素生物合成 | yes | yes | — |
| 91 | wos:000073532900046 | Science | 1998 | none-named | 植物钾营养 | 植物钾营养 | yes | yes | — |
| 92 | wos:000177653200012 | Cell | 2002 | none-named | 植物microRNA靶标预测 | 植物microRNA靶标预测 | yes | yes | — |
| 93 | wos:a1994pn27200035 | Science | 1994 | none-named | 植物发育调控 | 细胞命运决定 | no | no | boundary |
| 94 | wos:000231230100039 | Science | 2005 | none-named | 植物开花调控 | 植物开花调控 | yes | yes | — |
| 95 | wos:000452972600049 | Nature | 2018 | none-named | 土地利用与气候变化减缓 | 土地利用与气候变化减缓 | yes | yes | — |
| 96 | wos:000347915300029 | Science | 2015 | none-named | 农业转型与人口迁移 | 农业转型与人口迁移 | yes | yes | — |
| 97 | wos:a1995qc27300033 | Science | 1995 | none-named | 真菌致病性进化 | 真菌致病机制 | no | no | boundary |
| 98 | wos:001174849800004 | Science | 2024 | none-named | 减数分裂重组调控 | 减数分裂重组调控 | yes | yes | — |
| 99 | wos:a1991fp51600013 | Cell | 1991 | none-named | 微管动力学 | 微管动力学 | yes | yes | — |
| 100 | wos:000074150100050 | Nature | 1998 | none-named | 昆虫-植物化学通讯 | 昆虫-植物化学通讯 | yes | yes | — |
| 101 | wos:000698977800053 | Science | 2021 | none-named | 无细胞化学酶法淀粉合成 | 无细胞化学酶法淀粉合成 | yes | yes | — |
| 102 | wos:000233343400048 | Science | 2005 | none-named | 植物开花调控 | 植物开花调控 | yes | yes | — |
| 103 | wos:000185003500040 | Science | 2003 | none-named | 植物抗病性 | 植物免疫 | no | no | boundary |
| 104 | wos:a1993kg95500012 | Cell | 1993 | none-named | 转座子与花同源异型调控 | 转座子与花同源异型调控 | yes | yes | — |
| 105 | wos:a1990cy82700039 | Science | 1990 | none-named | 朊蛋白生物发生 | 朊蛋白生物发生 | yes | yes | — |
| 106 | wos:a1990cp23700003 | Cell | 1990 | none-named | 植物机械刺激响应 | 植物机械刺激响应 | yes | yes | — |
| 107 | wos:000271468000043 | Science | 2009 | none-named | 受体蛋白激酶 | 水稻免疫 | no | no | direct |
| 108 | wos:000429805400037 | Science | 2018 | none-named | 细胞事件记录系统 | 细胞事件记录系统 | yes | yes | — |
| 109 | wos:a1985aaz2900011 | Cell | 1985 | none-named | 内含子进化 | 内含子进化 | yes | yes | — |
| 110 | wos:000087991200053 | Nature | 2000 | none-named | 植物UV-B辐射与基因组稳定性 | 植物UV-B辐射与基因组稳定性 | yes | yes | — |
| 111 | wos:000292690500047 | Nature | 2011 | none-named | 土壤温室气体排放 | 土壤温室气体排放 | yes | yes | — |
| 112 | wos:001047823000001 | Cell | 2023 | none-named | 脱氨酶功能发现 | 脱氨酶功能发现 | yes | yes | — |
| 113 | wos:000714665100005 | Cell | 2021 | none-named | 植物磷酸盐信号 | 根内共生信号 | no | yes | boundary |
| 114 | wos:000391190500050 | Nature | 2016 | none-named | 植物糖信号 | 植物糖信号 | yes | yes | — |
| 115 | wos:000490988300069 | Nature | 2019 | rice | 稻瘟病菌侵染机制 | 稻瘟菌侵染机制 | no | no | boundary |
| 116 | wos:000416043700037 | Nature | 2017 | none-named | DNA碱基编辑 | DNA碱基编辑 | yes | yes | — |
| 117 | wos:a1991fd83800088 | Nature | 1991 | none-named | 土壤温室气体排放 | 土壤温室气体排放 | yes | yes | — |
| 118 | wos:000343801500048 | Nature | 2014 | none-named | 基因组印记与种子发育 | 基因组印记与种子发育 | yes | yes | — |
| 119 | wos:000286886400038 | Nature | 2011 | none-named | 古气候甲烷排放 | 古气候甲烷排放 | yes | yes | — |
| 120 | wos:000325106000044 | Nature | 2013 | none-named | 聚酮合酶催化机制 | 聚酮合酶催化机制 | yes | yes | — |

---

## 5. Prompt-variant stability runs (alternative model, Kimi K3) — executed

**Design.** Three independent coding runs of the same 120-record sample by Kimi K3, blinded to the production labels, titles only, identical 400-theme taxonomy; the runs differ only in the decision rule:

- v1 (base): assign the single most appropriate primary theme.
- v2 (specificity-first): when several themes apply, choose the most specific (crop-specific over generic; mechanism-specific over broad category).
- v3 (process-first): choose the theme reflecting the paper's main biological process or agronomic objective rather than the molecular mechanism or data type.

**Results.** Fleiss' κ across the three runs = 0.933; pairwise exact agreement 95.0% / 93.3% / 91.7% (median 93.3%); median pairwise theme-set Jaccard 1.0 (mean 0.93). Fleiss' κ across the production labels plus the three variant runs (n = 118 records labelled by the production model) = 0.847.

**Variant flips (all other records identical to v1).**

| Record | v1 | v2 (specificity-first) | v3 (process-first) |
|---|---|---|---|
| wos:000920432900010 | 独脚金寄生抗性 | = v1 | 独脚金内酯生物合成 |
| wos:a1987j666500038 | 玉米转座子 | = v1 | RNA可变剪接 |
| wos:000262862800047 | 作物细胞分化 | 细胞命运决定 | = v1 |
| wos:001216996700004 | 油菜素甾醇信号 | 水稻穗型调控 | = v1 |
| wos:000223514900044 | 细胞分裂素信号 | = v1 | 分生组织调控 |
| wos:a1996vf61000048 | 受体蛋白激酶 | 作物细胞分化 | 作物细胞分化 |
| wos:000512382900023 | 氮利用效率 | = v1 | 水稻表观遗传 |
| wos:000325988400055 | 分生组织调控 | 玉米株型调控 | = v1 |
| wos:001408515800001 | 作物碱敏感性调控 | = v1 | 赤霉素信号 |
| wos:a1987k781000034 | 转座子标签 | 玉米胚乳基因调控 | 玉米胚乳基因调控 |
| wos:000244039400045 | 细胞分裂素信号 | = v1 | 分生组织调控 |
| wos:a1991fp51600013 | 微管动力学 | 蛋白质序列保守性 | = v1 |

## 6. Boundary-case adjudication table (n = 50)

| # | RecordId | Production themes (primary first) | Verdict |
|---|----------|-----------------------------------|---------|
| 1 | wos:000263687600035 | 玉米副突变 \| RNA聚合酶IV | accept |
| 2 | wos:000665547300018 | 水稻泛基因组 \| 基因组变异 | accept |
| 3 | wos:a1994mx20700014 | 玉米花发育 \| 转录调控 | accept |
| 4 | wos:001783527300001 | 玉米种子蛋白含量 \| 氮利用效率 | accept |
| 5 | wos:000920432900010 | 玉米抗寄生植物 \| 独脚金内酯生物合成 | accept |
| 6 | wos:001449348400001 | 玉米耐冷性 \| 玉米高纬适应 | merge |
| 7 | wos:000416043700044 | 基因组测序 \| 小麦D基因组 | merge |
| 8 | wos:001699682300001 | 玉米耐冷性 \| 磷利用效率 | accept |
| 9 | wos:000730550300007 | 植物免疫 \| 抗病性 | merge |
| 10 | wos:000174858800039 | 水稻基因组 \| 基因组测序 | merge |
| 11 | wos:000311606000034 | 小麦基因组 \| 基因组测序 | merge |
| 12 | wos:000432242000060 | 小麦A亚基因组 \| 基因组测序 | merge |
| 13 | wos:000483195200019 | 作物秸秆燃烧替代 \| 农业可持续集约化 | accept |
| 14 | wos:000262852200034 | 高粱基因组 \| 禾本科基因组进化 \| 基因组测序 | merge |
| 15 | wos:000274394300030 | 短柄草基因组 \| 基因组测序 | merge |
| 16 | wos:000269242800047 | 水稻免疫 \| 抗病性 | merge |
| 17 | wos:000404358500014 | 水稻免疫 \| 抗病性 | merge |
| 18 | wos:000228810900048 | 植物抗虫性 \| 植物生物技术 | accept |
| 19 | wos:000403814100037 | 玉米基因组 \| 基因组测序 | merge |
| 20 | wos:000442818200033 | 多倍体小麦转录组 \| 多倍体基因组互作 | accept |
| 21 | wos:000537569500037 | 小麦赤霉病抗性 \| 抗病性 | merge |
| 22 | wos:a1988p120800036 | 抗病性 \| 植物免疫 | merge |
| 23 | wos:a1989ah16500061 | 南美古代农业起源 \| 古代玉米农业 | merge |
| 24 | wos:000172029100033 | 水体富营养化 \| 硝酸盐通量 | accept |
| 25 | wos:000187385200043 | 玉米基因组 \| 基因组测序 | merge |
| 26 | wos:000231230100049 | 作物根际微生物组 \| 稻田甲烷排放 | accept |
| 27 | wos:000084013200041 | 转基因玉米与生物多样性 \| 植物抗虫性 | accept |
| 28 | wos:000271951000046 | 玉米基因表达调控 \| 转录调控 | merge |
| 29 | wos:000681722700036 | 玉米基因组 \| 基因组变异 | accept |
| 30 | wos:a1986a911800046 | 玉米发育基因 \| 细胞命运决定 | accept |
| 31 | wos:000552814800002 | 赤霉素信号 \| 水稻株型调控 | accept |
| 32 | wos:000223514900044 | 花发育调控基因 \| 细胞分裂素信号 \| 转录调控 | merge |
| 33 | wos:a1989av75100075 | RNA编辑 \| 小麦线粒体基因组 \| 蛋白质序列保守性 | accept |
| 34 | wos:000247066400042 | 转基因作物与非靶标生物 \| 综述 | accept |
| 35 | wos:000306542600052 | 细胞命运决定 \| 细胞周期调控 | accept |
| 36 | wos:000079509000055 | 玉米发育基因 \| 转录调控 | accept |
| 37 | wos:000714065000017 | 水稻免疫 \| 植物免疫 | merge |
| 38 | wos:000325988400055 | 玉米发育基因 \| 玉米株型调控 | merge |
| 39 | wos:000229970400054 | 水稻免疫 \| 抗病性 | merge |
| 40 | wos:001408515800001 | 赤霉素信号 \| 作物碱敏感性调控 | accept |
| 41 | wos:000937133200003 | 作物水肥管理 \| 农业可持续集约化 | accept |
| 42 | wos:000261559900034 | 植物遗传学 \| 综述 | accept |
| 43 | wos:000296021100048 | 真菌效应子 \| 植物免疫 | accept |
| 44 | wos:a1993lx29200005 | 叶绿体发育信号转导 \| 植物发育调控 | merge |
| 45 | wos:000636049300058 | 农药毒性趋势与生物多样性保护 \| 转基因作物与非靶标生物 | accept |
| 46 | wos:000225695600051 | 植物-病原菌共进化 \| 植物抗病性 | accept |
| 47 | wos:000306506500042 | Bt棉花生物防治 \| 转基因作物与非靶标生物 | accept |
| 48 | wos:000632421900005 | 叶大小调控 \| 植物发育调控 | merge |
| 49 | wos:000490988300069 | 稻瘟病菌侵染机制 \| 真菌致病机制 | merge |
| 50 | wos:000307267000034 | 香蕉基因组 \| 被子植物基因组进化 | accept |
