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

## 📡 Modem Compatibility (Powered by wwand)

`wwand` is highly mature and has been production-tested across a wide variety of hardware, spanning MBIM, QMI, NCM, and PCIe/MHI protocols. It handles complex features like 5G SA/NSA (ENDC), eUICC (eSIM), and remote SIMs.

**Specifically Tested Modems Include:**
*   **Fibocom:** FM350-GL *(Our primary target on the H29K. Tested natively via NCM & eUICC)*
*   **Quectel:** RG650E, RM520N, RG502Q, EG06, RG520N-EB, BG96 *(LTE Cat-M)*
*   **MeiG:** SLM770A
*   **Huawei:** E392
*   **Foxconn (Snapdragon X62):** T99W373 *(Tested via PCIe/MHI on x86_64)*

**Tested Hardware & Routers:**
*   MikroTik Chateau 5G
*   Zyxel NR7101 & LTE3301-M209
*   Cudy LT300
*   GL.iNet X3000
*   Dell DW5931e / DW5932e (in HP EliteBook laptops)

*Note: Because `wwand` uses standard AT commands and native Linux datapaths, it is highly likely to work with many other unlisted modems. However, the hardware listed above is what the developer has explicitly verified in production environments.*

---

## 🛠️ Key Features of This Build

*   **eMMC Boot Fix:** Resolves a critical kernel bug where the H29K would bootloop on the eMMC unless an SD card was inserted. This build safely disables HS200 mode for absolute stability.
*   **wwand Pre-integrated:** Fully compiled with `luci-app-wwand` and `luci-proto-wwand`.
*   **Trusted Feed & Key:** The `wwand` repository and its public signature key are securely baked into the image, allowing for safe future updates via `apk`.
*   **Proven Base:** Built on top of the I-agree scripts, ensuring hardware support for the screen, fan, and Wi-Fi.

---

## 🏗️ System Base (OpenWrt Snapshot)

This firmware is built on the **OpenWrt SNAPSHOT** branch (Kernel 6.18). It uses the bleeding-edge `apk` package manager (replacing the older `opkg`).

**Why Snapshot?**
*   **Latest Hardware Support:** Needed for the RK3528 chip and the latest H29K kernel patches.
*   **Modern Package Manager:** `apk` is the future of OpenWrt and is required for the latest `wwand` builds.
*   **Note on Stability:** Because this tracks the OpenWrt development branch, it receives the newest features but may occasionally experience minor upstream bugs compared to a "Stable" release.

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

## 📸 Screenshots

<p align="center">
  <img src="https://github.com/user-attachments/assets/46aa49aa-c2f7-4056-9a7d-549dcc5a5e9d" width="32%" />
  <img src="https://github.com/user-attachments/assets/9a2431c2-4f9e-4507-a2f6-c9ac1f6fac7b" width="32%" />
  <img src="https://github.com/user-attachments/assets/d4a9c70c-7b94-4011-ad4e-60e1aa85cf60" width="32%" />
</p>
<p align="center">
  <img src="https://github.com/user-attachments/assets/7c81648a-edd8-48cf-9647-af9ec34f8caa" width="32%" />
  <img src="https://github.com/user-attachments/assets/4e694517-2fcf-4af6-834b-10804db5c1f4" width="32%" />
  <img src="https://github.com/user-attachments/assets/45249187-5333-449a-b331-8b5758673883" width="32%" />
</p>
<p align="center">
  <img src="https://github.com/user-attachments/assets/9c76cdff-058d-44d8-9625-001661dfd4cc" width="32%" />
  <img src="https://github.com/user-attachments/assets/e6d77321-d43e-4ea2-8c80-218e00125725" width="32%" />
  <img src="https://github.com/user-attachments/assets/3a1522d5-da53-4ad0-beb1-372342ee303b" width="32%" />
</p>

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
