# KY10 内核 BTF 文件获取与仓库更新指南

本仓库用于存储 KY10 操作系统不同版本的内核 BTF (BPF Type Format) 文件。

## BTF 文件获取方法

### 1. 从官方源获取调试包

KY10 官方源地址:
- V10SP2: https://update.cs2c.com.cn/NS/V10/V10SP2/os/adv/lic/updates/aarch64/debug/
- V10SP1: https://update.cs2c.com.cn/NS/V10/V10SP1/os/adv/lic/updates/aarch64/debug/

示例（以 V10SP2 4.19.90-25.41 版本为例）:

```bash
# 1. 下载对应版本的 kernel-debuginfo 包
wget https://update.cs2c.com.cn/NS/V10/V10SP2/os/adv/lic/updates/aarch64/debug/kernel-debuginfo-4.19.90-25.41.v2101.ky10.aarch64.rpm

# 2. 解压 RPM 包
rpm2cpio kernel-debuginfo-4.19.90-25.41.v2101.ky10.aarch64.rpm | cpio -idmv

# 3. 使用 pahole 工具生成 BTF 文件
pahole --btf_encode_detached 4.19.90-25.41.v2101.ky10.aarch64.btf ./usr/lib/debug/lib/modules/4.19.90-25.41.v2101.ky10.aarch64/vmlinux

# 4. 压缩 BTF 文件
tar -cJf 4.19.90-25.41.v2101.ky10.aarch64.btf.tar.xz 4.19.90-25.41.v2101.ky10.aarch64.btf
```

## 仓库更新流程

1. 将生成的 BTF 文件放入对应的目录结构中：
```
btfhub-archive/
└── ky10
    └── <版本号>
        └── <架构>
            └── <内核版本>.btf.tar.xz
```

2. 提交更新：
```bash
git add .
git commit -m "feat: add BTF for KY10 kernel <版本号>"
git push origin main
```

## 注意事项

- 确保使用正确版本的 pahole 工具
- BTF 文件生成前请验证 kernel-debuginfo 包的完整性
- 提交前请确认 BTF 文件的正确性

