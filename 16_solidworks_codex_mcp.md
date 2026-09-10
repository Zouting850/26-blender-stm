# SolidWorks 接入 Codex

## 1. 目标

让 `Codex` 通过本地 `MCP` 服务调用电脑上的 `SolidWorks COM` 自动化接口。

本仓库已经提供两个文件：

- `D:\26stm_blender\pc\solidworks_bridge.ps1`
- `D:\26stm_blender\pc\solidworks_mcp_server.py`

它们的分工是：

- `solidworks_bridge.ps1`：直接调用本机 `SolidWorks COM`
- `solidworks_mcp_server.py`：把这些能力包装成 `Codex` 可识别的 `MCP` 工具

## 2. 当前实现的工具

当前先接入最小可用能力：

- `solidworks_status`
- `solidworks_open_document`
- `solidworks_get_active_document`
- `solidworks_close_active_document`

这样已经足够让 `Codex`：

- 检查 `SolidWorks` 是否可用
- 打开 `sldprt / sldasm / slddrw`
- 读取当前活动文档
- 关闭当前活动文档

## 3. 已确认的本机条件

当前机器上已确认：

- `SolidWorks` 安装目录：`D:\Program Files\SOLIDWORKS Corp`
- 主程序：`D:\Program Files\SOLIDWORKS Corp\SOLIDWORKS\SLDWORKS.exe`
- `COM ProgID`：`SldWorks.Application` 已注册

同时也确认当前 `Python` 环境没有这些包：

- `win32com`
- `pythoncom`
- `comtypes`

因此这里采用 `PowerShell COM` 桥接，避免额外安装依赖。

## 4. 手工测试

先在终端里测试桥接：

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File D:\26stm_blender\pc\solidworks_bridge.ps1 -Action status
```

如果要打开一个零件文件：

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File D:\26stm_blender\pc\solidworks_bridge.ps1 -Action open -Path "D:\your_model.sldprt" -Visible $true
```

## 5. 配置到 Codex

把下面这段加入 `C:\Users\25736\.codex\config.toml`：

```toml
[mcp_servers.solidworks]
command = "python"
args = ["D:\\26stm_blender\\pc\\solidworks_mcp_server.py"]
```

如果你的 `Codex` 没有直接找到 `python`，也可以改成绝对路径，例如：

```toml
[mcp_servers.solidworks]
command = "C:\\Users\\25736\\AppData\\Local\\Programs\\Python\\Python313\\python.exe"
args = ["D:\\26stm_blender\\pc\\solidworks_mcp_server.py"]
```

配置完成后，重启 `Codex`。

## 6. 接入后怎么用

接入成功后，可以直接对 `Codex` 说：

- “检查 SolidWorks 是否在线”
- “打开 `D:\xxx\part.sldprt`”
- “读取当前 SolidWorks 活动文档”
- “关闭当前活动文档”

## 7. 后续可扩展

如果这条链路稳定，下一步很适合继续加：

- 导出 `STEP / STL / PDF`
- 获取装配体层级
- 批量打开并导出
- 读取自定义属性
- 触发指定宏
