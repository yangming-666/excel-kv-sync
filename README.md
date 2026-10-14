# Excel ↔ KV Sync

面向 Dota 2 配置工作的 Python / Tkinter 桌面工具，支持 Excel → Valve KeyValues 文本和 KV → 现有 Excel 工作簿同步。

**状态：源码工具。** 仓库提供可运行源码，转换前请备份工作簿与 KV 目录。当前没有发布经签名的安装程序。

## 能做什么

- 选择单个 Excel 文件或文件夹，转换为同名 `.txt` KV 文件。
- 在选定 KV 目录中搜索已有同名文件，按现有实现同步内容。
- 把 KV 同步回现有 Excel，保留现有顺序规则，并用 `#DELETED` 标记消失的主键。
- 按项目提供的 `utils/backend_config_specs.txt` 清单导出后端 JSON。

```text
Excel 工作簿 ←→ 同名 KV 文本
                    ↓ 项目导出清单
               后端配置 JSON
```

## 从源码运行

推荐 Python 3.11+，Windows Python 安装需包含 Tcl/Tk。

```powershell
git clone https://github.com/yangming-666/excel-kv-sync.git
cd excel-kv-sync
py -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe excel_to_kv_gui.py
```

打开窗口后选择 Excel 文件/文件夹和 KV 目录，再点击转换方向。先使用工作数据的副本，检查结果差异后再应用。

## 输入与副作用

- 使用 `.xlsx`。虽然旧文件选择器列出了 `.xls`，底层 `openpyxl` 不支持传统二进制 `.xls`。
- 输入需符合现有项目表头/主键约定，工具并非任意 Excel 表格的通用转换器。
- `config.json` 记录上次路径；它由程序生成，不随源码共享。
- “Excel 转 KV”成功后还会尝试导出后端配置，并启动程序目录下的 `dzsj-kv-sync.exe`（若存在）。该外部程序不由此仓库提供，其行为需要独立核对。
- 缺少后端清单或后续程序时，弹窗可能提示警告；检查转换、JSON 导出和后续同步各自的结果。
- “KV 转 Excel”会写入所选工作簿，不是只读预览。

## 项目文件

| 文件 | 职责 |
|---|---|
| `excel_to_kv_gui.py` | Tkinter 界面、转换与 JSON 导出 |
| `kv_to_excel_idempotent_sync.py` | KV 解析与 Excel 同步 |
| `config.example.json` | 本地路径配置示例 |
| `excel_to_kv_gui.spec` | 历史 PyInstaller 配置；不代表已验证的发布流程 |

## 验证与限制

```powershell
.\.venv\Scripts\python.exe -m py_compile excel_to_kv_gui.py kv_to_excel_idempotent_sync.py
```

语法检查不证明真实工作簿转换正确。提交变更时附脱敏样例、转换前后差异和重复同步结果。首次使用应检查注释、嵌套字段、删除主键和字段顺序。

[贡献指南](CONTRIBUTING.md) · [安全说明](SECURITY.md)

当前没有仓库级开源许可证；不包含用户业务配置和构建产物。
