# std

## build
● 打包成 Windows EXE 可执行文件，请按以下步骤操作：

  📋 先决条件

  1. Rust 工具链
  # 安装 Rust (如果尚未安装)
  curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
  # 或从 https://rustup.rs/ 下载安装
  2. Bun 或 Node.js (推荐 Bun，因为项目使用 bun.lock)
  # 安装 Bun (推荐)
  powershell -c "irm bun.sh/install.ps1 | iex"
  # 或使用 Node.js 18+
  3. Windows 构建工具 (Tauri 必需)
    - 方法 A: 安装 https://visualstudio.microsoft.com/zh-hans/downloads/
        - 选择 "使用 C++ 的桌面开发" 工作负载
      - 包含 Windows 10/11 SDK
    - 方法 B: 只安装构建工具
        - https://visualstudio.microsoft.com/zh-hans/downloads/#build-tools-for-
  visual-studio-2022
      - 勾选 "C++ 生成工具" 和 "Windows 10/11 SDK"
  4. WebView2 (Windows 自带，但确保已安装)
    - Windows 10/11 通常已包含
    - 可手动安装：https://developer.microsoft.com/zh-cn/microsoft-edge/webview2/

  🚀 打包步骤

  1. 克隆仓库 (如未克隆)
  git clone https://github.com/codeErrorSleep/dbpaw.git
  cd dbpaw
  2. 安装依赖
  bun install
  3. 构建前端
  bun run build
  # 这会生成 dist/ 目录
  4. 打包为 EXE
  bun tauri build
    - 第一次运行会下载 Rust 依赖和 Tauri 工具链
    - 构建过程可能需要 5-15 分钟

  📁 输出文件

  构建完成后，可执行文件位于：
  - 安装程序: src-tauri/target/release/bundle/nsis/ 目录下的 .exe 安装程序
  - 便携版: src-tauri/target/release/bundle/portable/ 目录下的 .exe 便携版
  - 单个 EXE: src-tauri/target/release/dbpaw.exe

  🛠️ 构建选项

  仅构建 Rust 后端 (不打包)

  cargo build --release --manifest-path src-tauri/Cargo.toml
  # 输出: src-tauri/target/release/dbpaw.exe

  开发模式运行

  bun tauri dev
  # 启动带热重载的开发版应用

  特定平台构建

  # Windows (默认)
  bun tauri build --target x86_64-pc-windows-msvc

  # 其他平台交叉编译需要额外配置

  ⚠️ 常见问题

  1. SSL 相关错误

  如果出现 OpenSSL 错误：
  # 安装 vcpkg 和 OpenSSL
  git clone https://github.com/Microsoft/vcpkg.git
  cd vcpkg
  .\bootstrap-vcpkg.bat
  .\vcpkg install openssl:x64-windows

  2. 内存不足

  增加虚拟内存或使用：
  # 清理缓存
  cargo clean
  bun tauri build

  3. 防病毒软件误报

  部分防病毒软件可能误报，可将输出目录添加到白名单。

  4. 图标缺失

  确保 src-tauri/icons/ 目录包含所需图标文件。

  📦 发布准备

  1. 版本号更新
    - package.json 中的 version
    - src-tauri/Cargo.toml 中的 version
    - src-tauri/tauri.conf.json 中的 version
  2. 代码签名 (可选，用于消除安全警告)
    - 需要代码签名证书
    - 配置 tauri.conf.json 中的 signingIdentity
  3. 测试安装
    - 在不同 Windows 版本上测试安装程序
    - 测试数据库连接功能

  🎯 快速开始脚本

  创建 build-windows.ps1：
  # 检查依赖
  rustc --version
  bun --version

  # 清理并构建
  bun install
  bun run build
  bun tauri build

  Write-Host "构建完成！EXE 文件位于: src-tauri/target/release/"

  运行：powershell -ExecutionPolicy Bypass -File build-windows.ps1

  ---
  提示：首次构建可能较慢，后续构建会利用缓存。确保网络通畅以下载 Rust crates。

● 根据项目文档，将 DbPaw 打包为 Windows EXE 的步骤如下：

  📋 先决条件

  1. Rust - 最新稳定版
  # 安装 Rust (如果尚未安装)
  curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
  # 或使用 rustup 安装
  rustup update stable
  2. Bun (推荐) 或 Node.js 18+
  # 安装 Bun
  curl -fsSL https://bun.sh/install | bash
  3. Windows 平台工具链 (Tauri 要求)
    - Microsoft Visual Studio 2022 或 Visual Studio Build Tools
        - 安装时选择 "Desktop development with C++" 工作负载
    - Windows 10/11 SDK (通常随 VS 一起安装)
    - Rust 目标 (如果构建 64 位)
    rustup target add x86_64-pc-windows-msvc

  🚀 打包步骤

  1. 克隆并进入项目目录

  git clone https://github.com/codeErrorSleep/dbpaw.git
  cd dbpaw

  2. 安装依赖

  bun install

  3. 构建应用

  bun tauri build

  📁 输出文件位置

  构建完成后，可执行文件位于：
  src-tauri/target/release/bundle/msi/dbpaw_0.2.8_x64_en-US.msi  # Windows
  安装包
  src-tauri/target/release/bundle/nsis/dbpaw_0.2.8_x64-setup.exe  # NSIS
  安装程序
  src-tauri/target/release/bundle/portable/dbpaw_0.2.8_x64-portable.exe  #
  便携版

  直接的可执行文件（未打包）在：
  src-tauri/target/release/dbpaw.exe

  ⚙️ 构建选项

  - 仅构建前端: bun run build
  - 开发模式运行: bun tauri dev
  - 指定目标平台 (交叉编译):
  bun tauri build --target x86_64-pc-windows-msvc

  🔧 常见问题解决

  1. "error: linker link.exe not found"

     安装 Visual Studio Build Tools 并确保 link.exe 在 PATH 中。

  2. "Windows SDK not found"

     通过 Visual Studio Installer 安装 Windows 10/11 SDK。

  3. 构建过程卡住或内存不足

     尝试增加 Node.js 内存限制：
  export NODE_OPTIONS="--max-old-space-size=4096"
  # 然后重新运行 bun tauri build

  4. SSL/TLS 相关错误

     确保 OpenSSL 可用，或使用系统 TLS：
  # 在 src-tauri/Cargo.toml 中确保 reqwest 使用 "rustls-tls" 特性

  📝 注意事项

  1. 首次构建较慢 - 需要下载 Rust 依赖和编译 Tauri 运行时
  2. 防病毒软件可能误报 - 构建的 EXE 可能被误判，需添加到白名单
  3. 图标资源 - 应用图标位于 src-tauri/icons/ 目录
  4. 版本号 - 在 src-tauri/tauri.conf.json 和 package.json 中配置