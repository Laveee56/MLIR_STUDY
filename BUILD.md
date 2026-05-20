# 构建与开发

克隆本仓库后，按以下步骤配置本地环境并开始开发。

> 项目概览见 [README.md](README.md)

---

## 1. 前置依赖

| 依赖 | 说明 |
|------|------|
| Git | 拉取代码与 `llvm-project` submodule |
| CMake ≥ 3.13.4 | 建议 3.20+ |
| Ninja | 推荐构建后端 |
| GCC / Clang（C++17） | 编译 LLVM/MLIR 与 out-of-tree 目标 |
| Python 3 | LLVM 部分脚本依赖 |

Ubuntu / Debian：

```bash
sudo apt update
sudo apt install -y git cmake ninja-build build-essential python3
```

---

## 2. 克隆仓库并初始化 submodule

```bash
git clone <repo-url> mlir
cd mlir
git submodule update --init --recursive
```

---

## 3. 编译并安装 LLVM / MLIR

MLIR 工具链安装到仓库根目录的 `install/`，供 `mlir-toy` 等 out-of-tree 项目使用。

```bash
cd llvm-project
mkdir -p build && cd build

cmake -G Ninja ../llvm \
  -DCMAKE_BUILD_TYPE=Release                    # Release 优化构建
  \
  -DLLVM_ENABLE_PROJECTS=mlir                   # 启用 MLIR 子项目
  \
  -DLLVM_BUILD_EXAMPLES=ON                      # 编译 LLVM/MLIR 示例
  \
  -DLLVM_ENABLE_ASSERTIONS=ON                   # 开启断言，便于开发调试
  \
  -DLLVM_TARGETS_TO_BUILD="Native"              # 仅生成本机架构后端，缩短编译时间
  \
  -DCMAKE_INSTALL_PREFIX="$PWD/../../install"   # 安装到仓库根目录 install/

cmake --build . --target install
```

首次全量编译耗时较长，可用 `ninja -j<N>` 控制并行度。`llvm-project/build/` 与 `install/` 合计约需 **20 GB** 磁盘空间。

安装完成后确认：

```bash
ls ../../install/lib/cmake/mlir/MLIRConfig.cmake
```

将 `install/bin` 加入 `PATH`，以便直接使用 `mlir-opt`、`mlir-translate` 等工具：

```bash
export PATH="$PWD/../../install/bin:$PATH"
```

若需长期生效，可将上述 `export` 写入 `~/.bashrc`（路径替换为仓库的绝对路径，例如 `/home/user/mlir/install/bin`）。

---

## 4. 编译 `mlir-toy`

```bash
cd ../../mlir-toy
mkdir -p build && cd build

cmake -G Ninja .. \
  -DMLIR_DIR="$PWD/../../install/lib/cmake/mlir"

cmake --build .
```

修改源码后，在 `mlir-toy/build/` 下重新执行 `cmake --build .` 即可增量编译。

---

## 5. 运行

```bash
./mlir-toy /path/to/test.mlir
```

示例输入 `test.mlir`：

```mlir
func.func @main() {
  return
}
```

---

## 6. 日常开发

**增量编译 `mlir-toy`**

```bash
cd mlir-toy/build && cmake --build .
```

**更新 LLVM 版本后**（submodule 指向变更时），需重新执行第 3 步，再重新配置并编译 `mlir-toy`。

**clangd 支持**：`mlir-toy` 构建后会生成 `mlir-toy/build/compile_commands.json`，可链接到项目根目录供 IDE 使用。

---

## 常见问题

**`find_package(MLIR)` 失败** — 确认第 3 步 `install` 已完成，且配置时传入正确的 `-DMLIR_DIR=.../install/lib/cmake/mlir`。

**链接错误** — 检查 `mlir-toy/CMakeLists.txt` 的 `LIBS` 是否包含所用 dialect 库，并在代码中 `loadDialect` 注册对应 dialect。

**清理重建**

```bash
rm -rf llvm-project/build install mlir-toy/build
```

然后重新执行第 3、4 步。
