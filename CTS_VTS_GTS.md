**CTS , VTS & GTS**\
In the context of Android security and device management , CTS , GTS and VTS are three different sets of tests and certification process.\

### what is CTS (Compatibility Test Suite)?
* CTS is used to ensure that an Android device's software and hardware are compatible with the Android platform.
* CTS includes a suite of test cases that cover various aspects of Android such as app compatibility, security, and platform integrity.
* It verifies that the device's software, including the Android version and any modifications made by the device manufacturer, is consistent with the Android compatibility definition Document (CDD).
* Android device manufacturers must pass CTS to be able to ship their device with Google play services and access other Google apps and services.

### What is GTS ?
* GTS is a set of tests that are part of Google's effort to ensure the quality and compatibility of Android devices that include Google apps and services(such as Google Play, Youtube and Google Maps).
* While CTS focuses on the core compatibility of Android, GTS is specifically aimed at verifying the compatibility of Google Mobile services (GMS) and Google apps on Android devices.
* To pass GTS, a device must meet Google's specific requirements for GMS integration and compatibility.

### What is VTS ?
* VTS is a set of tests designed for device manufacturers and components suppliers to verify that their customizations and hardware components meet Android's compatibility requirements.
* VTS includes test cases for hardware compatibility , such as camera functionality , sensors , and other device specific features
* It is used to valiadate that the customizations and hardware added by the device manufacturer do not negatively impact the overall Android experience and are compatible with the Android platform.
* Passing VTS is necessary for device manufacturers to obtain Android Certification.

![Summery](https://github.com/rammohanreddy434/Android/blob/master/images/cts_vts/CTS_VTS.png)

# 📱 Android CTS vs VTS: Impact of Modifying System Components from Vendor

## ✅ Summary

If you modify **system components** from the **vendor side**, you risk **failing CTS**, while **VTS may still pass** — depending on the nature of the change.

---

## 🔍 What Are CTS and VTS?

- **CTS (Compatibility Test Suite)**  
  Tests the **system partition** and ensures the Android OS behaves as expected (AOSP standards, APIs, permissions, security).

- **VTS (Vendor Test Suite)**  
  Tests the **vendor partition** and ensures HALs, interfaces, and vendor extensions are compatible with the Android framework.

---

## ⚠️ Effect of Modifying System Components from Vendor

| Modification Type                              | CTS Risk | VTS Risk |
|------------------------------------------------|----------|----------|
| Replacing a system binary (`system/bin`)       | ✅ Yes   | ❌ Low    |
| Editing `system/sepolicy`                      | ✅ Yes   | ✅ Yes    |
| Modifying framework JARs (`system/framework/`) | ✅ Yes   | ❌ N/A    |
| Adding system services from vendor             | ✅ Yes   | ✅ Yes    |
| Adding privileged permissions to `system/`     | ✅ Yes   | ❌ Low    |
| Adding HALs to `vendor/`                       | ❌ No    | ✅ Must comply with VTS rules |

---

## 🧩 Treble Architecture Rule (Post Android 9+)

> **System** and **Vendor** partitions must be **independent**.  
> Vendors must **not modify system components**, but may extend functionality through `vendor/`, `product/`, or `system_ext/`.

---

## 🚫 Forbidden or Risky Vendor-Side Changes (For CTS)

1. **Modifying `system/lib`, `system/bin`, or `system/framework`**
2. **Overriding system services or permissions**
3. **Adding SELinux policies in `system/sepolicy` that grant excessive access**
4. **Changing AOSP behavior from the vendor side**

---


