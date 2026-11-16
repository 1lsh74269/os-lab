# 实验三：构建独立的不依赖 Rust 标准库的可执行程序

## 📁 项目文件结构

```
实验三独立可执行程序/
├── .cargo/
│   └── config          # Rust 构建配置（target = riscv64）
├── src/
│   └── main.rs         # 主程序代码
├── Cargo.toml          # Rust 项目配置
├── 实验报告.md          # 实验报告模板
└── README.md           # 本说明文件
```

## 🚀 实验步骤

### 步骤 1：进入容器环境

进入已配置好 Rust 环境的容器，并进入 `/mnt` 目录：

```bash
# 假设你已经在容器中
cd /mnt
```

### 步骤 2：复制项目文件到工作目录

```bash
# 将本项目复制到 /mnt 目录
cp -r /path/to/实验三独立可执行程序 /mnt/os-experiment
cd /mnt/os-experiment
```

### 步骤 3：编译项目

```bash
# 首次编译前，确保安装了必要的组件
rustup target add riscv64gc-unknown-none-elf
cargo install cargo-binutils
rustup component add llvm-tools-preview
rustup component add rust-src

# 编译项目
cargo build
```

### 步骤 4：分析生成的二进制文件

```bash
# 查看文件类型
file target/riscv64gc-unknown-none-elf/debug/os

# 查看文件头信息
rust-readobj -h target/riscv64gc-unknown-none-elf/debug/os

# 反汇编查看代码
rust-objdump -S target/riscv64gc-unknown-none-elf/debug/os
```

**📸 请截图命令行窗口，插入到实验报告中**

### 步骤 5：运行程序

```bash
# 使用 QEMU 运行程序
qemu-riscv64 target/riscv64gc-unknown-none-elf/debug/os
```

如果看到输出 `Hello, world!` 并正常退出，说明实验成功！

**📸 请截图 QEMU 运行结果，插入到实验报告中**

## 📝 实验报告填写说明

1. 打开 `实验报告.md` 文件
2. 填写你的 **姓名** 和 **学号**
3. 在相应的 **截图位置** 插入命令行截图
4. 检查思考问题的答案，可以根据自己的理解修改完善
5. 在 **Git 提交截图** 位置插入 Git 提交记录截图

## 🔧 Git 提交

```bash
# 初始化 git（如果需要）
git init

# 添加所有文件
git add .

# 提交代码（记得在注释中加入你的学号）
git commit -m "feat: 实验三 - 独立可执行程序 - 学号：[你的学号]"

# 推送到远程仓库（如果需要）
git push
```

## ⚠️ 常见问题

### 问题 1：编译时报错找不到 target

**解决方案：**
```bash
rustup target add riscv64gc-unknown-none-elf
```

### 问题 2：cargo-binutils 安装失败

**解决方案：**
```bash
cargo install cargo-binutils
```

### 问题 3：编译后出现其他错误

**解决方案：**
```bash
# 清理构建缓存后重新编译
cargo clean
cargo build
```

### 问题 4：QEMU 运行时报错

**解决方案：**
确保已安装 QEMU：
```bash
# Ubuntu/Debian
sudo apt install qemu-system-riscv64

# 或使用其他包管理器安装
```

## 📚 核心知识点

1. **`#![no_std]`**：禁用标准库，只使用 core crate
2. **`#![no_main]`**：禁用默认的 main 入口点
3. **`#[panic_handler]`**：自定义 panic 处理函数
4. **`#[no_mangle]`**：防止函数名被编译器修改
5. **`_start`**：Rust 编译器寻找的入口函数
6. **系统调用**：通过 `ecall` 指令与内核通信

## ✅ 实验检查清单

- [ ] 创建了 `.cargo/config` 配置文件
- [ ] `main.rs` 中添加了 `#![no_std]` 和 `#![no_main]`
- [ ] 实现了 `#[panic_handler]`
- [ ] 实现了 `_start` 入口函数
- [ ] 实现了系统退出机制（`sys_exit`）
- [ ] 实现了输出支持（`sys_write`、`print!`、`println!`）
- [ ] 成功编译并运行程序
- [ ] 截取并保存了分析结果的截图
- [ ] 截取了 QEMU 运行结果的截图
- [ ] 完成了实验报告
- [ ] 提交了 Git 代码（commit 中包含学号）

---

**祝你实验顺利！🎉**
