# 🚀 H29K Custom OpenWrt Build (wwand + eMMC Boot Fix)

This repository provides a custom OpenWrt build for the **HinLink H29K** router (RK3528). 

It is based on the excellent **I-agree** project, but introduces crucial stability fixes and replaces the default modem manager with **wwand** for a significantly better 5G experience.

---

## ✨ Why wwand? (User Experience & Features)

If you are using a 5G module (like the FM350-GL) on the H29K, **wwand** is a massive upgrade over the default QModem. Based on real-world testing, here is why you should use this build:

*   📱 **Mature eSIM Management:** Seamlessly switch between physical SIMs and eSIM profiles without ever failing.
*   🧠 **Smart Per-SIM APN Profiles:** Automatically sets the correct APN based on the SIM's ICCID. No more manual reconfiguration when swapping profiles.
*   ⚡ **Blazing Fast Reconnects:** From the moment you hit "Reboot" in LuCI, your internet is back up in just **40–50 seconds**. It is noticeably faster than QModem.
*   🛡️ **Incredibly Stable Setup:** Unlike QModem—which sometimes fails to dial or randomly creates duplicate dial profiles that cause conflicts—`wwand` behaves exactly like standard OpenWrt protocols (e.g., `proto atc` or WireGuard). Once set up, it never duplicates or randomly changes itself unless you modify the hardware.

---

## 📡 A Note on Modem Compatibility

`wwand` is a relatively new project compared to long-standing tools like ModemManager or QModem. Because it is under active development, its hardware support list is currently focused on specific, well-tested modules rather than being universally compatible with everything on the market.

Currently supported and tested modules include:
*   **Fibocom FM350-GL** (This build is heavily tested with this module)
*   Quectel RG650E
*   Quectel EG06
*   MeiG SLM770A
*   Zyxel NR7101

If you are using a different modem, it may work, but it has not been explicitly verified by the developer. The FM350-GL is the primary target for this H29K build.

---

## 🛠️ Key Features of This Build

*   **eMMC Boot Fix:** Resolves a critical kernel bug where the H29K would bootloop on the eMMC unless an SD card was inserted. This build safely disables HS200 mode for absolute stability.
*   **wwand Pre-integrated:** Fully compiled with `luci-app-wwand` and `luci-proto-wwand`.
*   **Trusted Feed & Key:** The `wwand` repository and its public signature key are securely baked into the image, allowing for safe future updates via `apk`.
*   **Proven Base:** Built on top of the I-agree scripts, ensuring hardware support for the screen, fan, and Wi-Fi.

---

## ⚠️ 🚨 IMPORTANT: First Boot Instructions

Because this is a custom build, OpenWrt's package manager will show a warning during `apk update` regarding a broken `wwand` feed URL. 

**Don't worry, this is expected and easy to fix.** After your first boot, run these two commands via SSH:

1. Remove the auto-generated, invalid wwand feed URL:
`sed -i '/wwand/d' /etc/apk/repositories.d/distfeeds.list`

2. Update the package lists:
`apk update`

*Note: The actual, secure wwand repository and key are already baked into the system. These commands simply delete the build system's auto-generated duplicate and refresh the package list.*

---

## 📥 Download & Flash

1. Go to the **Releases** tab on this repository.
2. Download the latest `openwrt-rockchip-armv8-hinlink_h29k-squashfs-sysupgrade.img.gz` file.
3. Log into your H29K's LuCI web interface (or use RKDevTool).
4. Flash the image. **Uncheck "Keep Settings"** to ensure a clean transition to `wwand`.

---

## 🙏 Credits & Acknowledgements

This project wouldn't exist without the hard work of the open-source community:

**Project Specific:**
*   **[I-agree](https://github.com/I-agree/H29K)** - For the original H29K build scripts, kernel patches, and hardware support.
*   **[ddimension](https://github.com/ddimension)** - For the incredible `wwand` modem manager and its eSIM capabilities.
*   **DeepSeek AI** - For debugging assistance, kernel DTS fixes (eMMC boot), and build environment configuration.

**Original I-agree / P3TERX Base:**
- [Microsoft Azure](https://azure.microsoft.com)
- [GitHub Actions](https://github.com/features/actions)
- [OpenWrt](https://github.com/openwrt/openwrt)
- [coolsnowwolf/lede](https://github.com/coolsnowwolf/lede)
- [Mikubill/transfer](https://github.com/Mikubill/transfer)
- [softprops/action-gh-release](https://github.com/softprops/action-gh-release)
- [Mattraks/delete-workflow-runs](https://github.com/Mattraks/delete-workflow-runs)
- [dev-drprasad/delete-older-releases](https://github.com/dev-drprasad/delete-older-releases)
- [peter-evans/repository-dispatch](https://github.com/peter-evans/repository-dispatch)

---

## ⚠️ Disclaimer

*Flash at your own risk.* It is highly recommended to keep a working SD card recovery method ready before flashing custom firmware. The author is not responsible for bricked devices.

---

## License

[MIT](https://github.com/P3TERX/Actions-OpenWrt/blob/main/LICENSE) © [**P3TERX**](https://p3terx.com)
