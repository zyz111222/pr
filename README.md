# PR Visual Walkthroughs (`zyz111222/pr`)

基于纯静态、零外部依赖（Zero-CDN）交互式 HTML + Inline SVG 构建的 Pull Request 全景拆解与性能优化归档库。

A zero-CDN static HTML & inline-SVG archive of technical Pull Request visual walkthroughs, sequence/dataflow diagrams, hardware ablations, and annotated code diffs.

- **GitHub Pages Portal**: https://zyz111222.github.io/pr/

## PR Index / 拆解报告索引

| PR | Target Repo | Commit | Highlights / 核心指标 | Visual Report (GitHub Pages) | Upstream PR |
| --- | --- | --- | --- | --- | --- |
| **#22872** | `vllm-project/vllm-ascend` | `c8b90b9e8` | DeepSeek-V4 (W4A8MXFP / PCP=4) Prefill 优化：`20K -9.24%`、`131K -8.50%`、`0.000e+00` Bit-Exact（`DynamicMXQuantScaleAlg.CEIL`）、950DT 四方案消融、284 UTs Passed（5 源码 + 5 UT） | [在线阅读 PR #22872 全景拆解](https://zyz111222.github.io/pr/pr-22872/) | [vllm-ascend#22872](https://github.com/vllm-project/vllm-ascend/pull/22872) |

### PR #22872 Summary

1. **提前发起 O-Proj 权重异步 AllGather**：在计算流执行期间提前异步拉取权重，掩盖通信气泡。
2. **消除 `o_proj_input` 冗余分配与零拷贝直通**：去除入口 `new_zeros` 预分配与多余拷贝路径。
3. **MoE PCP 通信前 MXFP8 (`CEIL`) 预量化 + 本地 TopK + 3 路原生 AllGather**：压缩通信载荷并基于 950DT 四方案消融锁定 `Variant B + DynamicMXQuantScaleAlg.CEIL`，实现严格位级一致（`0.000e+00`）并兼容 `#22230`。
