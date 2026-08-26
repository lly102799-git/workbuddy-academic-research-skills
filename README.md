# workbuddy-academic-research-skills

WorkBuddy 移植版 **academic-research-skills**（源自 GitHub 原版 [`Imbad0202/academic-research-skills`](https://github.com/Imbad0202/academic-research-skills)，Claude Code / Codex 插件）。

本仓库把原版移植为 WorkBuddy 可直接加载的 4 个 skill + 1 个共享库，并把所有机器专属的绝对路径改写为**相对路径**，使其可在任意电脑上同步使用。

## 包含的 skills

| 目录 | 作用 |
| --- | --- |
| `academic-pipeline/` | 10 阶段学术流水线编排器（研究 → 写作 → 完整性校验 → 评审 → 修订） |
| `academic-paper/` | 12-agent 论文写作 agent 团队 |
| `academic-paper-reviewer/` | 5 席 + Devil's Advocate 多视角论文评审 |
| `deep-research/` | 13-agent 深度研究流水线 |
| `_ars_shared/` | 共享知识库（`.md`）+ 597 个 Python 辅助脚本（`scripts/`） |

> `_ars_shared` 以**下划线开头**，是纯文件库，不会被 WorkBuddy 当作独立 skill 加载。

## 与原版的关键差异（移植说明）

1. **路径全部相对化**：`.md` 中对 `_ars_shared` / `scripts/` / `references/` 的引用改写为相对路径（skill 内用 `../_ars_shared/...`，`_ars_shared` 内部用 `../scripts/` 等）。`.py` 脚本本就用 `Path(__file__).resolve()` 运行时解析，天然可移植。
2. **Claude Code 专有机制降级为软约束**：`/ars-*` slash 命令、`PreToolUse` 写权限守卫、`ARS_*` 环境变量、`resume_from_passport` 在 WorkBuddy 无对应运行时，已转写为 SKILL.md 中的文本说明，不影响核心流程。
3. **Python 依赖**：`ars_apply_revision_patch.py` 依赖第三方库 `jsonschema`（确定性补丁应用高级特性）。其余脚本仅用标准库。

## 安装 / 跨机同步

仓库根目录的 5 个目录，需同步到本机 WorkBuddy 的 skills 根目录 `~/.workbuddy/skills/`：

```bash
# 在目标机器上，将仓库内容合并进 skills 根目录
git clone git@github.com:lly102799-git/workbuddy-academic-research-skills.git /tmp/ars
cp -r /tmp/ars/* "$HOME/.workbuddy/skills/"
```

同步后无需重跑移植脚本——相对路径会按目标机器的 `~/.workbuddy/skills/` 基址自动解析。

### 解锁确定性补丁应用特性（可选）

```bash
python -m pip install jsonschema
```

## 验证状态

- 419 个 `_ars_shared/scripts/*.py` 全部 `py_compile` 通过；关键模块（`arxiv_client` / `_text_similarity` / `claim_audit_pipeline` / `ars_apply_revision_patch`）导入正常。
- 已部署 skills 源码含机器专属绝对路径数 = 0。

---

*本仓库为个人私有备份 / 跨机同步用途。*
