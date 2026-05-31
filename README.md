# ImmortalWrt for 360 T7

基于 [padavanonly/immortalwrt-mt798x-6.6](https://github.com/padavanonly/immortalwrt-mt798x-6.6)（`openwrt-24.10-6.6` 分支）的 360 T7 路由器固件，GitHub Actions 自动编译。

## 固件特性

| 类别 | 内容 |
|------|------|
| 内核 | Linux 6.6.x |
| WiFi 驱动 | MTK 闭源驱动（信号/速率优于开源 mt76） |
| NAT 加速 | HNAT + WARP v2 硬件加速 |
| 基础 IP | `192.168.6.1`（无默认密码） |
| opkg 源 | 中科大 USTC 镜像 |
| 时区 | Asia/Shanghai |

### 预装软件

| 功能 | 软件包 |
|------|--------|
| 科学上网 | **PassWall2** + sing-box 核心 |
| 端口转发 | UPnP |
| 系统工具 | htop, tcpdump |
| 网络优化 | BBR 拥塞控制 |

## 下载

**最新版本**: [v1.0-8731f16](https://github.com/itoywh/t7-firmware-builder/releases/latest)

| 文件 | 说明 |
|------|------|
| `squashfs-sysupgrade.bin` | 升级固件（从现有 OpenWrt/ImmortalWrt 升级） |
| `manifest` | 已安装软件包清单 |
| `sha256sums` | 校验和 |

### 刷入方法

1. 下载 `squashfs-sysupgrade.bin`
2. 进入路由器管理页面 (`http://192.168.6.1`)
3. 系统 → 备份/升级 → 刷写新的固件
4. **强烈建议不保留配置**，刷完后重新设置

## 自定义编译

### 快速开始

Fork 本仓库，推送任意 commit 到 `main` 分支即可自动触发编译。约 1.5~2 小时后，GitHub Actions 会生成固件 artifact 并上传到 Release。

### 修改插件

编辑 `.github/workflows/build.yml` 中的「Customize packages」步骤：

```yaml
# 删除插件（注释掉对应行）
sed -i 's^CONFIG_PACKAGE_xxx=y$^# CONFIG_PACKAGE_xxx is not set^' .config

# 添加插件
echo "CONFIG_PACKAGE_yyy=y" >> .config
```

### 修改 opkg 源

编辑 `files/etc/opkg/distfeeds.conf`，换用你偏好的镜像站。

## 仓库结构

```
├── .github/workflows/build.yml   # GitHub Actions 编译流水线
├── feeds.conf.default             # 自定义 feeds（原版 + PassWall2）
├── files/
│   └── etc/
│       ├── opkg.conf              # 禁用签名检查
│       └── opkg/
│           └── distfeeds.conf     # USTC 中科大镜像
```

## 源码上游

- **固件源码**: [padavanonly/immortalwrt-mt798x-6.6](https://github.com/padavanonly/immortalwrt-mt798x-6.6)
- **PassWall2**: [Openwrt-Passwall/openwrt-passwall2](https://github.com/Openwrt-Passwall/openwrt-passwall2)

## 与原生 ImmortalWrt 的区别

| 方面 | 原生 ImmortalWrt | padavanonly/本项目 |
|------|-------------------|---------------------|
| WiFi 驱动 | 开源 mt76 | MTK 闭源驱动 |
| 内核 | 5.15 | 6.6 |
| NAT 加速 | 软件 offload | HNAT + WARP v2 硬件加速 |
| 默认 IP | 192.168.1.1 | **192.168.6.1** |
| 时区 | UTC | Asia/Shanghai |

## License

固件源码遵循各上游项目许可证。本仓库仅提供编译流水线和配置文件。
