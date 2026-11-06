# gRPC/Grpcio For Termux(完全体)

这是 `grpcio` 在 Termux 原生环境(Android AArch64, Python 3.14)下从源码编译的完全体备份仓库。

包含：
1. 经过Patch的 **全量源码树**
2. 编译产生的所有 **中间产物**
3. 切分后的 **核心 `.so` 二进制文件**
4. 打包好的 **`.whl` 预编译轮子**

## 🛠 核心Patch记录

由于Android使用Bionic libc，不存在独立的 `librt.so`，导致gRPC官方 `setup.py` 在Android上链接失败。通过修改 `setup.py` 的第415行，成功绕过该问题：

```python
# 修改前
if "linux" in sys.platform:
    EXTENSION_LIBRARIES += ("rt",)

# 修改后
if "linux" in sys.platform and "ANDROID_ROOT" not in os.environ:
    EXTENSION_LIBRARIES += ("rt",)
```
编译环境变量：
```bash
export TMPDIR=~/tmp-build
```
unset GRPC_PYTHON_BUILD_SYSTEM_OPENSSL GRPC_PYTHON_BUILD_SYSTEM_ZLIB GRPC_PYTHON_BUILD_SYSTEM_CARES GRPC_PYTHON_BUILD_SYSTEM_RE2 GRPC_PYTHON_BUILD_SYSTEM_ABSL
cat << 'EOF' > README.md
# gRPC/Grpcio For Termux(完全体)

这是 `grpcio` 在Termux原生环境(Android AArch64, Python 3.14)下从源码编译的完全体备份仓库。

包含：
1. 经过Patch的 **全量源码树**
2. 编译产生的所有 **中间产物**
3. 切分后的 **核心 `.so` 二进制文件**
4. 打包好的 **`.whl` 预编译轮子**

## 🛠 核心Patch记录

由于Android使用Bionic libc，不存在独立的 `librt.so`，导致 gRPC 官方 `setup.py` 在Android上链接失败。通过修改 `setup.py` 的第 415 行，成功绕过该问题：

# gRPC/Grpcio For Termux(完全体)

这是 `grpcio` 在Termux原生环境(Android AArch64, Python 3.14)下从源码编译的完全体备份仓库。

包含：
1. 经过Patch的 **全量源码树**
2. 编译产生的所有 **中间产物**
3. 切分后的 **核心 `.so` 二进制文件**
4. 打包好的 **`.whl` 预编译轮子**

## 🛠 核心Patch记录

由于Android使用Bionic libc，不存在独立的 `librt.so`，导致 gRPC 官方 `setup.py` 在Android上链接失败。通过修改 `setup.py` 的第 415 行，成功绕过该问题：

```python
# 修改前
if "linux" in sys.platform:
    EXTENSION_LIBRARIES += ("rt",)

# 修改后
if "linux" in sys.platform and "ANDROID_ROOT" not in os.environ:
    EXTENSION_LIBRARIES += ("rt",)
```
编译环境变量：
```bash
export TMPDIR=~/tmp-build
unset GRPC_PYTHON_BUILD_SYSTEM_OPENSSL GRPC_PYTHON_BUILD_SYSTEM_ZLIB GRPC_PYTHON_BUILD_SYSTEM_CARES GRPC_PYTHON_BUILD_SYSTEM_RE2 GRPC_PYTHON_BUILD_SYSTEM_ABSL
```
# ⚠️ 注意：.so文件已被切分

由于GitHub限制单文件不能超过100 MB，而编译生成的cygrpc.cpython-314-aarch64-linux-android.so高达243MB，因此被强行切分为了三个文件，存放在：
pyb/lib.android-24-arm64_v8a-cpython-314/grpc/_cython/

切分文件列表：

· cygrpc_part_aa (约90MB)
· cygrpc_part_ab (约90MB)
· cygrpc_part_ac (约63MB)
## 如何合并还原

如果你需要用到源码树中的这个.so文件，请先进入该目录并执行合并命令：
```bash
cd pyb/lib.android-24-arm64_v8a-cpython-314/grpc/_cython/
cat cygrpc_part_* > cygrpc.cpython-314-aarch64-linux-android.so
```

# 🚀 最简单的安装方式

如果你只是想使用gRPC，不需要管那一堆源码，直接使用仓库根目录下的预编译Wheel包即可(里面已经包含了合并好的完整.so)：
