# Windows 操作系统安装 WSL 和 Linux 的操作指南

## 前提条件
- Windows 10 版本 2004 或更高版本，或 Windows 11
- 系统管理员权限
- 互联网连接用于下载和更新

## 步骤 1：启用 WSL 和所需功能
1. 以管理员身份打开 PowerShell：
   - 开始菜单 > 输入 "PowerShell" > 右键单击 > 选择“以管理员身份运行”
2. 启用 Windows 子系统 Linux：
   ```
   dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
   ```
3. 启用虚拟机平台：
   ```
   dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
   ```
4. 重启计算机以应用更改。

## 步骤 2：升级到 WSL 2
1. 下载并安装 WSL 2 更新包：
   - 下载地址：https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi
   - 运行安装程序并按照提示操作。
2. 打开 PowerShell 并将 WSL 2 设置为默认版本：
   ```
   wsl --set-default-version 2
   ```
3. 更新 WSL 及其内核：
   ```
   wsl --update
   wsl --shutdown
   ```
4. 验证 WSL 版本：
   ```
   wsl --version
   ```
   - 确保输出显示 WSL 2.x。

## 步骤 3：启用自动更新
- **Windows 11**：
  - 转到 设置 > Windows 更新 > 高级选项
  - 启用“接收其他 Microsoft 产品的更新”
- **Windows 10**：
  - 转到 设置 > 更新与安全 > 高级选项
  - 启用“更新 Windows 时接收其他 Microsoft 产品的更新”

## 步骤 4：安装 Windows Terminal
1. 打开 Microsoft Store。
2. 搜索“Windows Terminal”。
3. 点击“安装”以下载并安装应用程序。

## 步骤 5：安装 Linux 发行版
1. 打开 Windows Terminal（或 PowerShell）。
2. 列出可用的 Linux 发行版：
   ```
   wsl --list --online
   ```
3. 安装特定的发行版（例如 Ubuntu-24.04）：
   ```
   wsl --install Ubuntu-24.04
   ```
   - 指定安装位置：
     ```
     wsl --install Ubuntu-24.04 --location D:\WSL\Ubuntu-24.04
     ```
4. 设置默认的 Linux 发行版：
   ```
   wsl --set-default Ubuntu-24.04
   ```
5. 检查运行中或已安装的发行版：
   ```
   wsl --list --running
   wsl --list --verbose
   ```
6. 默认安装路径：`C:\Users\<您的用户名>\AppData\Local\Packages`
7. 注册表路径：`HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Lxss`

## 步骤 6：安装和配置 Ubuntu-24.04
1. 创建安装目录：
   - 示例：`D:\WSL\Ubuntu-24.04`
2. 将 Ubuntu-24.04 安装到指定位置：
   ```
   wsl --install Ubuntu-24.04 --location D:\WSL\Ubuntu-24.04
   ```
3. 启动并配置发行版：
   ```
   wsl -d Ubuntu-24.04
   ```
   - 按提示输入用户名和密码。
4. 如需终止发行版：
   ```
   wsl --terminate -d Ubuntu-24.04
   ```

## 步骤 7：删除 Linux 发行版（例如 Ubuntu-24.04）
1. 注销发行版：
   ```
   wsl --unregister Ubuntu-24.04
   ```
2. 删除残留文件：
   - 导航到 `C:\Users\<您的用户名>\AppData\Local\Microsoft\WindowsApps`
   - 删除 `ubuntu2204.exe`（或类似的可执行文件，具体取决于版本）
3. 从 Windows Terminal 中移除：
   - 打开 Terminal > 设置
   - 在左侧边栏找到 Ubuntu-24.04 配置文件
   - 点击“删除配置文件” > 保存
   - 重启 Windows Terminal

## 步骤 8：配置 GPU 支持
1. 下载并安装适用于 Linux 的 GPU 驱动程序：
   - NVIDIA：https://www.nvidia.com/en-us/drivers/
   - AMD：https://www.amd.com/en/support/download/drivers.html
   - Intel：https://www.intel.com/content/www/us/en/download/19344/intel-graphics-windows-dch-drivers.html
2. 更新 WSL 以确保兼容性：
   ```
   wsl --update
   wsl --shutdown
   ```
3. 在 Linux 中安装并运行 GPU 加速应用程序（参考具体软件文档）。

## 步骤 9：运行 GUI 应用程序
1. 确保 WSL 已更新：
   ```
   wsl --update
   wsl --shutdown
   ```
2. 在 Linux 发行版中安装 GUI 应用程序（例如，在 Ubuntu 中通过 `apt` 安装）。
3. 从 Linux 终端启动 GUI 应用程序。

## 步骤 10：使用 .wslconfig 配置 WSL 设置
1. 在 Windows 用户目录（`C:\Users\<您的用户名>\.wslconfig`）中创建或编辑 `.wslconfig` 文件。
2. 示例配置，用于分配 CPU 核心数和内存：
   ```
   [wsl2]
   memory=4GB
   processors=2
   swap=2GB
   ```
3. 保存文件并重启 WSL：
   ```
   wsl --shutdown
   ```
4. 更多选项参考：https://learn.microsoft.com/en-us/windows/wsl/wsl-config

## 附加命令
- 移动已安装的发行版：
  ```
  wsl --manage <发行版名称> --move <新位置>
  ```
- 终止发行版：
  ```
  wsl --terminate <发行版名称>
  ```

## 资源
- 官方 WSL 安装指南：https://learn.microsoft.com/en-us/windows/wsl/install
- 手动安装：https://learn.microsoft.com/en-us/windows/wsl/install-manual
- Windows Terminal 设置：https://learn.microsoft.com/en-us/windows/terminal/install
- 环境设置：https://learn.microsoft.com/en-us/windows/wsl/setup/environment
- 基本命令：https://learn.microsoft.com/en-us/windows/wsl/basic-commands
- GUI 应用程序教程：https://learn.microsoft.com/en-us/windows/wsl/tutorials/gui-apps
- GPU 计算教程：https://learn.microsoft.com/en-us/windows/wsl/tutorials/gpu-compute
- WSL 配置：https://learn.microsoft.com/en-us/windows/wsl/wsl-config
- 社区指南：https://github.com/0xmoei/Install-Linux-on-Windows
