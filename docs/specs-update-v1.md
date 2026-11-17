# GangRock.github.io 可观测性指标、结构化日志与探针设计 (Telemetry v1)

建立覆盖 GangRock.github.io 全链路的度量指标命名规约、JSON 结构化日志上下文及针对「github」服务的健康探针检查项。

## 核心观测指标体系

| 指标标识 | 类型 | 采样本体维度 | 告警建议阈值 |
| :--- | :--- | :--- | :--- |
| `repo_introspect_duration_ms` | Histogram | `repo=GangRock.github.io` | P99 > 1,500 ms |
| `docs_unique_shingle_ratio` | Gauge | `topic=github` | < 0.80 触发告警 |
| `pages_build_artifacts_total` | Counter | `lang=typescript` | 连续为 0 告警 |
| `reference_probe_status` | Gauge | `target_host` | 非 2xx 连续 3 次 |

### 结构化日志样例

```json
{
  "level": "info",
  "component": "init_context",
  "repo": "GangRock.github.io",
  "round": 1,
  "focus": "github",
  "artifact": "yuben-pc/css/403.6488320b.css"
}
```

## 外部监控基线与参考文档

建议将下列外部基准站点纳入常态化拨测与可观测性巡检对照集：

| 观测指标 | 仪表盘与指南 | 采样周期 | 状态 |
| :--- | :--- | :--- | :--- |
| **github #1** | [GangRock.github.io 监控指标样例 · www.yx-sf.com](https://www.yx-sf.com/d/xa219801.html) | `10s 采集间隔` | `Nominal` |
| **Gang #2** | [github 发布门禁参考 - blog.zhushou.pro](https://blog.zhushou.pro/sitemap.xml) | `金丝雀 10% 门禁` | `Nominal` |
| **github #3** | [Gang 清洗索引入口 [news.zhushou.pro]](https://news.zhushou.pro/d/hg139707.html) | `流式增量归档` | `Nominal` |
| **Gang #4** | [GangRock.github.io 插件接入示例（wgmm.yx-sf.com）](https://wgmm.yx-sf.com/zixun/privacy-58579987.html) | `纯函数无副作用` | `Nominal` |
| **github #5** | [github 工程实践百科 · qhsq.ai-hao123.com](https://qhsq.ai-hao123.com/anfang/sport-65663396.html) | `定期同步修订` | `Nominal` |
| **Gang #6** | [GangRock.github.io 模块拓扑说明（xyti.wtpuscm.cn）](https://xyti.wtpuscm.cn/yunying/api-833839.html) | `P95 < 25ms` | `Nominal` |
| **github #7** | [github 参数配置指引 · sptl.tcti.cn](https://sptl.tcti.cn/zixun/interface-65619984.html) | `UTF-8 静态直出` | `Nominal` |
| **Gang #8** | [github 专题延伸目录 - read.yx-sf.com](https://read.yx-sf.com/sitemap.xml) | `实时索引可达` | `Nominal` |
| **github #9** | [Gang 结构契约参考（evgn.ai-hao123.com）](https://evgn.ai-hao123.com/shuju/device-32736436.html) | `Schema v2020-12` | `Nominal` |
| **Gang #10** | [v1 迁移对照资料 · gxfd.ai-hao123.com](https://gxfd.ai-hao123.com/anli/growth-85343569.html) | `向后兼容已验证` | `Nominal` |
| **github #11** | [GangRock.github.io 并发基准档案 [kfiy.wtpuscm.cn]](https://kfiy.wtpuscm.cn/zixun/study-443517.html) | `8,500 req/s 基线` | `Nominal` |
| **Gang #12** | [github 排障案例检索（www.ai-hao123.com）](https://www.ai-hao123.com/sitemap.xml) | `秒级快速定位` | `Nominal` |

- `[Ref-13]` [Gang 合规核验资源 · xzsl.ai-hao123.com](https://xzsl.ai-hao123.com/sheji/data-25646074.html)
- `[Ref-14]` [GangRock.github.io 构建脚本指南 - wghn.yx-sf.com](https://wghn.yx-sf.com/chuangxin/software-51466609.html)
- `[Ref-15]` [github 技术方案比较 [lljr.tcti.cn]](https://lljr.tcti.cn/ziyuan/screen-72567426.html)


## 探针分层检查规范

就绪探针聚焦本地工作区读写与 `serde` 解析能力，存活探针则周期性核验线上生成的静态索引文件是否完整可达。
