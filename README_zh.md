# AI Hedge Fund（中文说明）

本项目是一个 **AI 驱动对冲基金团队** 的概念验证，用于探索如何用大语言模型辅助交易决策。**仅供教育与科研用途**，不应用于真实交易或投资决策。

系统由多类智能体协同工作：

1. **Aswath Damodaran** — 估值视角：叙事、数字与严谨估值  
2. **Ben Graham** — 价值投资：安全边际与「隐藏宝石」  
3. **Bill Ackman** — 积极股东：大胆仓位与推动变革  
4. **Cathie Wood** — 成长投资：创新与颠覆  
5. **Charlie Munger** — 优质企业、合理价格  
6. **Michael Burry** — 逆向与深度价值  
7. **Mohnish Pabrai** — Dhandho：低风险下的翻倍机会  
8. **Nassim Taleb** — 黑天鹅与尾部风险、反脆弱与非对称收益  
9. **Peter Lynch** — 从日常生活中寻找「十倍股」  
10. **Phil Fisher** — 成长投资与深度「小道消息」式调研  
11. **Rakesh Jhunjhunwala** — 印度市场的「大牛市」视角  
12. **Stanley Druckenmiller** — 宏观与成长中的非对称机会  
13. **Warren Buffett** — 优质公司、合理价格  
14. **估值智能体** — 内在价值估算与交易信号  
15. **情绪智能体** — 市场情绪与信号  
16. **基本面智能体** — 基本面数据与信号  
17. **技术面智能体** — 技术指标与信号  
18. **风险管理** — 风险指标与仓位上限  
19. **组合经理** — 最终交易决策与「订单」输出  

<img width="1042" alt="界面示意" src="https://github.com/user-attachments/assets/cbae3dcf-b571-490d-b0ad-3f0f035ac0d4" />

**说明**：系统 **不会** 向真实券商下单，不会产生真实成交。

[![Twitter Follow](https://img.shields.io/twitter/follow/virattt?style=social)](https://twitter.com/virattt)

---

## 免责声明

本项目 **仅用于教育与科研**。

- 不构成真实交易或投资建议  
- 不提供任何收益保证  
- 作者不对因使用本软件造成的财务损失承担责任  
- 投资决策请咨询持牌专业人士  
- 历史表现不代表未来结果  

使用本软件即表示您同意 **仅出于学习目的** 使用。

---

## 目录

- [安装](#安装)
- [运行](#运行)
  - [命令行](#命令行)
  - [Web 应用](#web-应用)
- [参与贡献](#参与贡献)
- [功能建议](#功能建议)
- [许可证](#许可证)

---

## 安装

在运行 AI Hedge Fund 之前，需要完成安装并配置 API 密钥。以下步骤对 **全栈 Web 应用** 与 **命令行** 均适用。

### 1. 克隆仓库

本说明以当前维护的 fork 为例（亦兼容上游仓库）：

```bash
git clone https://github.com/YeLuo45/ai-hedge-fund.git
cd ai-hedge-fund
```

上游参考：[virattt/ai-hedge-fund](https://github.com/virattt/ai-hedge-fund)。

### 2. 配置 API 密钥

在项目根目录从示例文件生成 `.env`：

**Bash / macOS / Linux**

```bash
cp .env.example .env
```

**Windows PowerShell**

```powershell
Copy-Item .env.example .env
```

编辑 `.env`，至少填写：

- **至少一种 LLM 服务** 的 API Key（例如 `OPENAI_API_KEY`、`GROQ_API_KEY`、`ANTHROPIC_API_KEY`、`DEEPSEEK_API_KEY` 等，完整列表见 `.env.example`）。  
- **金融数据**：`FINANCIAL_DATASETS_API_KEY`（见 [.env.example](.env.example) 内注释与 [Financial Datasets](https://financialdatasets.ai/)）。

其他可选提供商（Gemini、xAI、Moonshot、Azure OpenAI、OpenRouter 等）均在 `.env.example` 中有说明。

---

## 运行

### 命令行

适合自动化、脚本集成与细粒度控制。

<img width="992" alt="CLI 示意" src="https://github.com/user-attachments/assets/e8ca04bf-9989-4a7d-a8b4-34e04666663b" />

#### 快速开始

1. **安装 [Poetry](https://python-poetry.org/)**（若尚未安装）

   **Linux / macOS**

   ```bash
   curl -sSL https://install.python-poetry.org | python3 -
   ```

   **Windows（PowerShell，示例：用 pip 安装到当前 Python）**

   ```powershell
   python -m pip install poetry -i https://pypi.tuna.tsinghua.edu.cn/simple
   ```

2. **安装依赖**

   ```bash
   poetry install
   ```

   若在国内网络环境下 `poetry install` 频繁超时，可在 `pyproject.toml` 中配置国内 PyPI 镜像源后执行 `poetry lock` 再 `poetry install`，或使用系统级 pip 镜像。

#### 运行主程序

**必选参数为股票代码列表 `--tickers`**（逗号分隔，无空格或按需去掉空格）：

```bash
poetry run python src/main.py --tickers AAPL,MSFT,NVDA
```

使用本地 **Ollama** 推理：

```bash
poetry run python src/main.py --tickers AAPL,MSFT,NVDA --ollama
```

指定回测/决策区间（可选）：

```bash
poetry run python src/main.py --tickers AAPL,MSFT,NVDA --start-date 2024-01-01 --end-date 2024-03-01
```

其他常用参数（详见 `poetry run python src/main.py --help`）：

- `--analysts`：逗号分隔的分析师 key  
- `--analysts-all`：启用全部分析师  
- `--model`：指定模型名称  
- `--initial-cash` / `--initial-capital`：初始资金  
- `--margin-requirement`：做空保证金比例  
- `--show-reasoning`：打印各智能体推理过程  
- `--show-agent-graph`：展示智能体图  

#### 运行回测

```bash
poetry run python src/backtester.py --tickers AAPL,MSFT,NVDA
```

回测同样支持 `--ollama`、`--start-date`、`--end-date` 等参数（与 CLI 解析逻辑一致）。

**示例输出：**

<img width="941" alt="回测输出示意" src="https://github.com/user-attachments/assets/00e794ea-8628-44e6-9a84-8f8a31ad3b47" />

---

### Web 应用

通过 Web 界面操作更直观，适合不习惯命令行的用户。

安装与启动说明见仓库内 **[app](https://github.com/YeLuo45/ai-hedge-fund/tree/main/app)** 目录中的文档。

<img width="1721" alt="Web 示意" src="https://github.com/user-attachments/assets/b95ab696-c9f4-416c-9ad1-51feb1f5374b" />

---

## 参与贡献

1. Fork 本仓库  
2. 新建功能分支  
3. 提交变更  
4. 推送到远程分支  
5. 发起 Pull Request  

**建议**：单次 PR 尽量小而聚焦，便于审阅与合并。

---

## 功能建议

若有新功能想法，请创建 Issue，并打上 **`enhancement`** 标签。

- 本 fork：[Issues · YeLuo45/ai-hedge-fund](https://github.com/YeLuo45/ai-hedge-fund/issues)  
- 上游：[Issues · virattt/ai-hedge-fund](https://github.com/virattt/ai-hedge-fund/issues)  

---

## 许可证

本项目采用 **MIT License**，详见仓库中的 [LICENSE](LICENSE) 文件。

---

**英文原版说明**见 [README.md](README.md)。
