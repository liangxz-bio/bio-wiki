---
title: 用DeepSeek V4做R语言数据可视化进阶 — SCI论文高频7种图
type: raw-article
created: 2026-05-07
domain: bioinfo-pipeline
author: RAI王博士
source_type: wechat
source_url: https://mp.weixin.qq.com/  # placeholder
sha256: 49e2a1cd0b9732fee9f9cb2ed3138bad06b041dd2664d1c4a383e6d3822a92f0
ingested: 2026-05-07
tags: [r, visualization, sci-paper, tutorial, bioinfo-pipeline, clinical-prediction-model]
---

# 用DeepSeek V4做R语言数据可视化进阶：SCI论文高频使用的7种图，代码直接用

**作者**：RAI王博士

---

## 01 准备工作

```r
# 智能安装函数：已安装则跳过，未安装才安装
install_if_missing <- function(pkgs, bioc = FALSE) {
  missing_pkgs <- pkgs[!pkgs %in% rownames(installed.packages())]
  if (length(missing_pkgs) == 0) {
    message("✅ 所有包已安装，无需重复安装")
    return(invisible(NULL))
  }
  message("📦 待安装包（", length(missing_pkgs), "个）：",
          paste(missing_pkgs, collapse = ", "))
  if (bioc) {
    if (!requireNamespace("BiocManager", quietly = TRUE))
      install.packages("BiocManager")
    BiocManager::install(missing_pkgs, ask = FALSE, update = FALSE)
  } else {
    install.packages(missing_pkgs)
  }
  message("✅ 安装完成")
}

# CRAN包
install_if_missing(c("tidyverse", "pROC", "rms", "ggalluvial","patchwork", "shapviz", "ggrepel", "here", "rmda"))

# Bioconductor包
install_if_missing("maftools", bioc = TRUE)

library(tidyverse)
library(pROC)
library(here)

set.seed(2026)

dir.create(here("outputs", "figures"), recursive = TRUE,
           showWarnings = FALSE)

# 生成贯穿全文的模拟数据
n <- 300
true_label <- factor(
  rbinom(n, 1, 0.4),
  levels = c(0, 1),
  labels = c("Negative", "Positive"))

pred_xgb <- plogis(rnorm(n,  ifelse(true_label=="Positive",  0.6, -0.6), 1.2))
pred_rf  <- plogis(rnorm(n,  ifelse(true_label=="Positive",  0.5, -0.5), 1.3))
pred_lr  <- plogis(rnorm(n,  ifelse(true_label=="Positive",  0.4, -0.4), 1.4))
```

---

## 02 图一：ROC曲线——多模型对比版

**论文场景：** 预测模型类论文，比较不同模型的判别能力。

**审稿人要求：** AUC要有置信区间，多个模型之间要做DeLong检验。

很多人只画了自己模型的ROC曲线，被审稿人要求：「请与传统逻辑回归做比较」。正确做法是从一开始就把多条曲线画在同一张图里。

```r
library(pROC)

# 计算ROC对象
roc_lr  <- roc(true_label, pred_lr,  quiet = TRUE)
roc_rf  <- roc(true_label, pred_rf,  quiet = TRUE)
roc_xgb <- roc(true_label, pred_xgb, quiet = TRUE)

# AUC + 95%CI标签
get_label <- function(roc_obj, name) {
  ci_val <- ci.auc(roc_obj)
  sprintf("%s\nAUC=%.3f (%.3f–%.3f)", name,
          ci_val[2], ci_val[1], ci_val[3])
}

# 整理绘图数据
make_roc_df <- function(roc_obj, label) {
  data.frame(fpr   = 1 - roc_obj$specificities,
             tpr   = roc_obj$sensitivities,
             model = label)
}

roc_df <- bind_rows(
  make_roc_df(roc_lr,  get_label(roc_lr,  "Logistic Regression")),
  make_roc_df(roc_rf,  get_label(roc_rf,  "Random Forest")),
  make_roc_df(roc_xgb, get_label(roc_xgb, "XGBoost"))
)

# DeLong检验（模型间AUC差异）
p_delong_lr_rf  <- roc.test(roc_lr,  roc_rf)$p.value
p_delong_rf_xgb <- roc.test(roc_rf, roc_xgb)$p.value

p_roc <- ggplot(roc_df,
                aes(x=fpr, y=tpr, color=model, linetype=model)) +
  geom_line(linewidth = 0.9) +
  geom_abline(slope=1, intercept=0,
              linetype="dashed", color="grey50", linewidth=0.5) +
  scale_color_manual(values = setNames(
    c("#f41919","#0bed13","#000000"),
    unique(roc_df$model)
  )) +
  scale_linetype_manual(values = setNames(
    c("dotted","dashed","solid"),
    unique(roc_df$model)
  )) +
  annotate("text", x=0.62, y=0.08,
           label=paste0("DeLong test:\n",
                        "LR vs RF: P=",   round(p_delong_lr_rf, 3), "\n",
                        "RF vs XGB: P=",  round(p_delong_rf_xgb, 3)),
           size=2.8, hjust=0, color="grey30") +
  theme_classic() +
  theme(text              = element_text(family="Arial", size=11),
        legend.position   = c(0.60, 0.18),
        legend.background = element_rect(fill="white", color="grey80",
                                         linewidth=0.3),
        legend.title      = element_blank(),
        legend.text       = element_text(size=9),
        legend.key.width  = unit(1.5, "cm")) +
  labs(x="1 - Specificity", y="Sensitivity") +
  coord_equal()

print(p_roc)

ggsave(here("outputs","figures","Fig1_ROC_multimodel.tiff"),
       plot=p_roc, dpi=300, width=6.5, height=6.5, units="in")

message("✅ ROC曲线已保存")
```

---

## 03 图二：校准曲线——预测概率准不准

**论文场景：** 所有预测模型论文，ROC之后的必备伴侣。

**审稿人要求：** 实线靠近对角线（完美校准线），报告Hosmer-Lemeshow检验P值。

ROC曲线回答：能不能区分高危低危？校准曲线回答：预测「70%风险」，真实发生率是不是也接近70%？

两个问题都要回答，只有ROC是不够的。

```r
# 校准曲线-预测概率是否准确

# 等频分箱绘制校准曲线（比等宽分箱更稳定）
n_bins <- 10

cal_df <- data.frame(
  obs  = as.integer(true_label == "Positive"),
  pred = pred_xgb) |>
  mutate(
    bin = cut(pred,
              breaks = quantile(pred, probs=seq(0, 1, 1/n_bins)),
              include.lowest = TRUE,
              labels = FALSE)
  ) |>
  group_by(bin) |>
  summarise(
    mean_pred = mean(pred),
    obs_rate  = mean(obs),
    n         = n(),
    se        = sqrt(obs_rate * (1 - obs_rate) / n),
    .groups   = "drop"
  )

# 在校准曲线上添加LOESS平滑线和误差条，点的大小表示每个bin的样本量
p_cal_smooth <- ggplot(cal_df,
                        aes(x = mean_pred, y = obs_rate)) +
  geom_abline(slope=1, intercept=0,
              linetype="dashed", color="grey50",
              linewidth=0.6) +
  geom_smooth(method = "loess", se = TRUE,
              color = "#2166AC", fill = "#BDD7E7",
              linewidth = 0.9, alpha = 0.3) +  # LOESS平滑
  geom_errorbar(aes(ymin = obs_rate - 1.96*se,
                    ymax = obs_rate + 1.96*se),
                width=0.02, color="#2166AC",
                linewidth=0.5, alpha=0.6) +
  geom_point(aes(size=n), color="#2166AC", alpha=0.85) +
  scale_size_continuous(name="Sample size", range=c(2,7)) +
  theme_classic() +
  theme(text=element_text(family="Arial", size=11),
        legend.position=c(0.85, 0.2)) +
  labs(x="Predicted Probability",
       y="Observed Frequency") +
  xlim(0,1) + ylim(0,1) + coord_equal()

print(p_cal_smooth)

ggsave(here("outputs","figures","Fig2_Calibration.tiff"),
       plot=p_cal_smooth, dpi=300, width=6, height=6, units="in")

message("✅ 校准曲线已保存")
```

---

## 04 图三：决策曲线——模型有没有临床价值

**论文场景：** 临床预测模型，证明你的模型比「全部治疗」或「全部不治疗」更有价值。

**审稿人要求：** 三条基准线都要有（你的模型、Treat All、Treat None），x轴范围只展示临床合理区间。

```r
# 决策曲线分析（DCA）：评估模型在不同阈值下的临床净收益
# 安装dcurves
install_if_missing("dcurves")

library(dcurves)
library(tidyverse)

# 数据准备（结局必须是逻辑型TRUE/FALSE）
dca_df <- data.frame(
  outcome  = as.logical(as.integer(true_label == "Positive")),
  pred_xgb = pred_xgb,
  pred_rf  = pred_rf,
  pred_lr  = pred_lr
)

# 运行DCA
dca_result <- dca(
  outcome ~ pred_xgb + pred_rf + pred_lr,
  data        = dca_df,
  thresholds  = seq(0.05, 0.75, by = 0.01),
  label       = list(
    pred_xgb = "XGBoost",
    pred_rf  = "Random Forest",
    pred_lr  = "Logistic Regression"
  ))

p_dca_final <- dca_result |>
  plot(smooth = TRUE) +

  # 颜色：模型用黑白灰，基准线用彩色
  scale_color_manual(
    values = c(
      "Treat All"           = "#D73027",
      "Treat None"          = "#2166AC",
      "XGBoost"             = "#000000",
      "Random Forest"       = "#555555",
      "Logistic Regression"  = "#999999"
    )
  ) +

  # 线型：双重区分（黑白打印友好）
  scale_linetype_manual(
    values = c(
      "Treat All"           = "longdash",
      "Treat None"          = "longdash",
      "XGBoost"             = "solid",
      "Random Forest"       = "dashed",
      "Logistic Regression"  = "dotted"
    )
  ) +

  # 线宽：模型线稍粗
  scale_linewidth_manual(
    values = c(
      "Treat All"           = 0.7,
      "Treat None"          = 0.7,
      "XGBoost"             = 1.1,
      "Random Forest"       = 1.0,
      "Logistic Regression"  = 1.0
    )
  ) +

  # x轴范围 + 去掉百分号，改成小数
  coord_cartesian(
    xlim = c(0.05, 0.70),
    ylim = c(-0.05, 0.35)
  ) +
  scale_x_continuous(
    labels = scales::percent_format(accuracy = 1),
    breaks = seq(0.1, 0.7, by = 0.1)
  ) +

  # 参考线：y=0
  geom_hline(yintercept = 0,
             color = "grey60", linewidth = 0.4,
             linetype = "solid") +

  # 投稿主题
  theme_classic() +
  theme(
    text              = element_text(family = "Arial", size = 11),
    axis.title        = element_text(size = 11, face = "bold"),
    axis.text         = element_text(color = "black", size = 10),
    # 图例移到图内左上角
    legend.position   = c(0.78, 0.82),
    legend.title      = element_blank(),
    legend.text       = element_text(size = 9.5),
    legend.background = element_rect(fill      = "white",
                                     color      = "grey80",
                                     linewidth  = 0.3),
    legend.key.width  = unit(1.2, "cm"),
    plot.background   = element_rect(fill = "white", color = NA),
    plot.margin       = margin(10, 15, 10, 10)
  ) +
  labs(
    x = "Threshold Probability",
    y = "Net Benefit"
  )

print(p_dca_final)

# 导出投稿格式
ggsave(
  here("outputs", "figures", "Fig3_DCA_final.tiff"),
  plot   = p_dca_final,
  dpi    = 300,
  width  = 7,
  height = 5.5,
  units  = "in")

message("✅ 投稿格式决策曲线已保存")
```

---

## 05 图四：列线图（Nomogram）——临床预测模型标配

**论文场景：** 临床预测模型，让医生在床旁计算患者个体风险。

**审稿人要求：** 同时展示1/3/5年生存概率，校准曲线配套。

```r
library(rms)
library(survival)

set.seed(2026)
n_nom <- 400

nom_df <- data.frame(
  time       = rgamma(n_nom, shape=2, scale=20),
  event      = rbinom(n_nom, 1, 0.55),
  age        = rnorm(n_nom, 60, 12) |> round(),
  stage      = factor(sample(c("I","II","III","IV"), n_nom,
                             replace=TRUE, prob=c(0.2,0.3,0.3,0.2)),
                     levels=c("I","II","III","IV")),
  biomarker  = rnorm(n_nom, 5, 2),
  risk_score = rnorm(n_nom, 0, 1))

dd <- datadist(nom_df)
options(datadist="dd")

cox_nom <- cph(
  Surv(time, event) ~ age + stage + biomarker + risk_score,
  data=nom_df, surv=TRUE, x=TRUE, y=TRUE)

surv_fit <- Survival(cox_nom)
lp_vals  <- cox_nom$linear.predictors

cat("1年生存率范围：", round(range(surv_fit(12,  lp_vals), na.rm=TRUE), 3), "\n")
cat("3年生存率范围：", round(range(surv_fit(36,  lp_vals), na.rm=TRUE), 3), "\n")
cat("5年生存率范围：", round(range(surv_fit(60,  lp_vals), na.rm=TRUE), 3), "\n")

nom_fixed <- nomogram(
  cox_nom,
  fun = list(
    function(lp) surv_fit(12, lp),
    function(lp) surv_fit(36, lp),
    function(lp) surv_fit(60, lp)
  ),
  funlabel = c("1-year OS", "3-year OS", "5-year OS"),
  lp       = FALSE,

  # 根据实际范围设置刻度，每个时间点只放4—5个刻度值
  fun.at = list(
    c(0.93, 0.94, 0.95),             # 1年：窄范围，只放3个刻度
    c(0.65, 0.68, 0.70, 0.72),       # 3年
    c(0.35, 0.38, 0.42, 0.46)       # 5年
  ))

# 导出（加大高度，给各行更多空间）
tiff(here("outputs", "figures", "Fig4_Nomogram.tiff"),
     width = 12, height = 8, units = "in", res = 300)

par(
  mar    = c(2, 2, 2, 2),
  family = "Arial",
  cex    = 0.85        # 整体字号缩小一点，避免重叠
)

plot(
  nom_fixed,
  xfrac      = 0.30,   # 变量名区域宽度
  cex.axis   = 0.75,   # 刻度字号
  cex.lab    = 0.85,   # 轴标签字号
  lplabel    = "",     # 不显示lp轴
  col.conf   = "grey80")

dev.off()

message("✅ 列线图修复版已保存")
```

---

## 06 图五：瀑布图（OncoPrint）——肿瘤突变全景图

**论文场景：** 肿瘤基因组学，展示样本基因突变景观。

**审稿人要求：** 基因按突变频率排序，每种突变类型颜色区分，右侧标注突变频率百分比。

```r
library(maftools)

# 使用maftools内置示例数据
laml_maf  <- system.file("extdata","tcga_laml.maf.gz", package="maftools")
laml_clin <- system.file("extdata","tcga_laml_annot.tsv", package="maftools")
maf_obj <- read.maf(maf=laml_maf, clinicalData=laml_clin, verbose=FALSE)

# 基本统计
cat("样本数：",  as.numeric(maf_obj@summary[ID=="Samples", summary]), "\n")
cat("基因数：",  nrow(getGeneSummary(maf_obj)), "\n")
cat("中位TMB：", median(getSampleSummary(maf_obj)$total), "\n")

# OncoPrint瀑布图
tiff(here("outputs","figures","Fig5_OncoPrint.tiff"),
     width=14, height=8, units="in", res=300)

oncoplot(
  maf               = maf_obj,
  top               = 20,
  fontSize          = 0.8,
  titleFontSize     = 1.2,
  legendFontSize    = 1.0,
  clinicalFeatures  = "FAB_classification",
  annotationColor   = list(
    FAB_classification = RColorBrewer::brewer.pal(
      length(unique(getClinicalData(maf_obj)$FAB_classification)),
      "Set1"
    ) |> setNames(
      sort(unique(getClinicalData(maf_obj)$FAB_classification))
    )
  ),
  showTumorSampleBarcodes = FALSE,
  SampleNamefontSize      = 0.5)

dev.off()

message("✅ 瀑布图已保存")
```

---

## 07 图六：桑基图——患者流向可视化

**论文场景：** 展示患者从治疗方案到缓解状态到生存结局的流向，或多级分类关系。

**审稿人要求：** 流量带宽度与样本量成比例，节点标签清晰。

```r
# 桑基图

# 安装ggsankey
install_if_missing("ggsankey",  bioc = FALSE)

# ggsankey不在CRAN，从GitHub安装
if (!requireNamespace("ggsankey", quietly = TRUE)) {
  if (!requireNamespace("remotes", quietly = TRUE))
    install.packages("remotes")
  remotes::install_github("davidsjoberg/ggsankey")
}

library(ggsankey)
library(tidyverse)

# 重新准备数据
set.seed(2026)
n_san <- 300

raw_df <- data.frame(
  Treatment = sample(c("Surgery","Chemotherapy","Combined"),
                     n_san, replace = TRUE,
                     prob = c(0.3, 0.4, 0.3)),
  Response  = sample(c("CR","PR","SD","PD"),
                     n_san, replace = TRUE,
                     prob = c(0.25, 0.35, 0.25, 0.15)),
  Outcome   = sample(c("Alive","Dead"),
                     n_san, replace = TRUE,
                     prob = c(0.60, 0.40)))

# ggsankey 通过在数据里预排序来控制节点位置
# 关键：make_long之前，确保factor levels正确
# 然后在geom_sankey里加 node.order 参数

raw_df_ordered <- raw_df |>
  mutate(
    # ggplot从下到上，想CR在最上就放levels最后
    Response  = factor(Response,
                       levels = c("PD","SD","PR","CR")),
    Outcome   = factor(Outcome,
                       levels = c("Dead","Alive")),
    Treatment = factor(Treatment,
                       levels = c("Chemotherapy",
                                  "Combined",
                                  "Surgery"))
  )

sankey_df2 <- raw_df_ordered |>
  make_long(Treatment, Response, Outcome)

# 关键修复：在make_long之后，
# 把node也转成有序因子
node_levels <- c("Chemotherapy","Combined","Surgery",    # Treatment列（下到上）
                 "PD","SD","PR","CR",                     # Response列（下到上）
                 "Dead","Alive"                           # Outcome列（下到上）
)

sankey_df2 <- sankey_df2 |>
  mutate(
    node      = factor(node,      levels = node_levels),
    next_node = factor(next_node, levels = node_levels)
  )

p_v3 <- ggplot(
  sankey_df2,
  aes(x = x, next_x = next_x,
      node = node, next_node = next_node,
      fill = factor(node), label = node)) +
  geom_sankey(
    flow.alpha = 0.55,
    node.color = "grey25",
    smooth     = 8,
    linewidth  = 0.35
  ) +
  geom_sankey_label(
    size          = 3.5,
    color         = "black",
    fill          = "white",
    fontface      = "bold",
    label.padding = unit(0.15, "lines")
  ) +
  scale_fill_manual(
    values = c(
      "Chemotherapy" = "#6BAED6",
      "Combined"     = "#2166AC",
      "Surgery"      = "#4393C3",
      "PD"           = "#D73027",
      "SD"           = "#FEE08B",
      "PR"           = "#91CF60",
      "CR"           = "#1A9850",
      "Dead"         = "#FB6A4A",
      "Alive"        = "#74C476"
    )
  ) +
  scale_x_discrete(
    labels = c(
      "Treatment" = "Treatment",
      "Response"  = "Best Response",
      "Outcome"   = "Outcome"
    )
  ) +
  theme_sankey(base_size = 11) +
  theme(
    text        = element_text(family = "Arial", size = 11),
    axis.text.x = element_text(size  = 11, face  = "bold",
                               color = "black"),
    axis.title       = element_blank(),
    legend.position  = "none",
    plot.background  = element_rect(fill = "white", color = NA),
    panel.background = element_rect(fill = "white", color = NA),
    plot.margin      = margin(15, 15, 15, 15)
  )

print(p_v3)

ggsave(here("outputs", "figures", "Fig6_Sankey_final.tiff"),
       plot = p_v3, dpi = 300,
       width = 9, height = 6, units = "in")

message("✅ 桑基图最终版已保存")
```

---

## 08 图七：SHAP蜂群图——机器学习模型可解释性

**论文场景：** 机器学习预测模型，回答「哪些特征最重要，影响方向是什么」。

**审稿人要求：** 蜂群图（beeswarm）优于条形图，因为同时展示了方向性和分布。

```r
library(xgboost)
library(shapviz)

# 模拟特征数据（含与标签相关的信号）
X_mat <- data.frame(
  EGFR_expr  = rnorm(n) + as.integer(true_label=="Positive") * 1.5,
  TP53_expr  = rnorm(n) - as.integer(true_label=="Positive") * 1.2,
  KRAS_expr  = rnorm(n) + as.integer(true_label=="Positive") * 0.8,
  Age        = rnorm(n, 60, 12) - as.integer(true_label=="Positive") * 5,
  Stage      = rbinom(n, 3, 0.5),
  Biomarker1 = rnorm(n) + as.integer(true_label=="Positive") * 0.6,
  Biomarker2 = rnorm(n) - as.integer(true_label=="Positive") * 0.5,
  Biomarker3 = rnorm(n)) |> as.matrix()

y_vec   <- as.integer(true_label == "Positive")
dtrain  <- xgb.DMatrix(X_mat, label=y_vec)
xgb_fit <- xgb.train(
  params   = list(objective="binary:logistic",
                  max_depth=4, eta=0.1, nthread=2),
  data     = dtrain,
  nrounds  = 100,
  verbose  = 0)

# SHAP值计算
shap_obj <- shapviz(xgb_fit, X_pred=X_mat,
                    X=as.data.frame(X_mat))

# 蜂群图
p_shap <- sv_importance(
  shap_obj,
  kind        = "beeswarm",
  max_display = 8,
  alpha       = 0.45,
  size        = 1.1) +
  theme_classic() +
  theme(text=element_text(family="Arial", size=11)) +
  labs(x="SHAP Value (Impact on Model Output)", title=NULL)

ggsave(here("outputs","figures","Fig7_SHAP_beeswarm.tiff"),
       plot=p_shap, dpi=300, width=8, height=6, units="in")

message("✅ SHAP蜂群图已保存")
```

---

## 09 多图拼接：期刊要求的Figure格式

```r
library(patchwork)

# 预测模型三件套拼图（ROC + 校准曲线 + 决策曲线）
prediction_triad <- (p_roc | p_cal_smooth) / p_dca_final +
  plot_annotation(
    tag_levels = "A",
    theme = theme(
      plot.tag = element_text(size=14, face="bold", family="Arial")
    )
  )

prediction_triad

ggsave(here("outputs","figures","Fig_PredictionTriad.tiff"),
       plot=prediction_triad, dpi=300,
       width=13, height=10, units="in")

# 两行两列四图拼接
quad_fig <- (p_roc + p_cal_smooth) / (p_dca_final + p_shap) +
  plot_annotation(
    tag_levels = "A",
    theme = theme(
      plot.tag = element_text(size=14, face="bold", family="Arial")
    )
  )

ggsave(here("outputs","figures","Fig_Quad.tiff"),
       plot=quad_fig, dpi=300,
       width=14, height=11, units="in")

message("✅ 组合图已保存")
```

---

## 10 选图决策速查——根据研究类型选图

### 临床预测模型

- **必选：** ROC曲线对比 + 校准曲线 + 决策曲线
- **推荐加：** 列线图（临床应用价值展示）

### 机器学习预测模型

- **必选：** ROC曲线对比 + SHAP蜂群图
- **推荐加：** 校准曲线 + 决策曲线

### 肿瘤基因组学

- **必选：** 瀑布图（OncoPrint）
- **推荐加：** 突变签名图（maftools::plotSignatures）

### 队列研究/临床试验

- **必选：** 桑基图（患者流向）
- **推荐加：** KM生存曲线

### 所有类型的论文

- **多结果展示：** 多图拼接（patchwork）

---

## 11 审稿人最常提的意见——对照预防

| 图表 | 高频审稿意见 | 预防方法 |
|------|-------------|----------|
| ROC曲线 | 缺少AUC置信区间 | `ci.auc(roc_obj)` 标注在图例里 |
| ROC曲线 | 缺少模型间比较 | `roc.test(roc1, roc2, method="delong")` |
| 校准曲线 | 没有H-L检验P值 | `ResourceSelection::hoslem.test()` |
| 校准曲线 | 分箱太少不稳定 | 等频分箱，至少8—10个区间 |
| 决策曲线 | 阈值范围包含不合理区间 | `thresholds = seq(0.05, 0.75)` |
| 列线图 | 只有总分无生存概率 | `fun=list()` 设置多个时间点 |
| 瀑布图 | 突变类型颜色含义不清 | 保留默认图例，字号≥8pt |
| SHAP图 | 只有重要性无方向 | 蜂群图代替条形图 |
| 所有图 | 分辨率<300DPI | `dpi=300` |
| 所有图 | 字体不是Arial | `element_text(family="Arial")` |

---

## 12 写在最后

今天7种图的完整代码，加微信发**进阶图表**，发给你。

上周有个做临床预测模型的博士，文章被拒了两次，审稿人每次都提同样的意见：请补充校准曲线和决策曲线。他第二次补的决策曲线x轴范围画到了1.0，又被要求限制在合理的阈值范围内。两次小问题，来回折腾了1个月。

这些问题不是难题，是不知道审稿人在意什么。
