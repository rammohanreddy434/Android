# ?? Android Treble Compliance: CTS, VTS, GTS and SELinux Policy Guidelines

---

## ?? What is Android Treble?

**Android Treble** is an architectural separation of the Android OS framework from the vendor implementation (such as HALs and drivers). Introduced in Android 8.0 (Oreo), it enables faster and easier updates by:

- Separating system and vendor code.
- Allowing OEMs to update the Android OS without touching vendor-specific code.

**Treble compliance** means a device follows this separation and passes relevant test suites.

---

## ?? Understanding CTS, VTS, and GTS

| Test Suite | Focus Area | Related to Treble? | Purpose |
|------------|------------|--------------------|---------|
| **CTS (Compatibility Test Suite)** | Android Framework | ? Yes (Upper Layer) | Verifies framework and app compatibility |
| **VTS (Vendor Test Suite)** | Vendor Implementation (HALs, Kernel) | ? Yes (Lower Layer) | Validates VINTF and vendor interface integrity |
| **GTS (Google Test Suite)** | Google Mobile Services (GMS) | ? No (but needed for GMS) | Verifies Google services and apps like Play Store |

### ?? Summary

- **CTS**: Ensures the framework layer is consistent with Android API expectations.
- **VTS**: Confirms that the vendor partition is isolated and stable.
- **GTS**: Ensures proper integration of Google services (not directly tied to Treble compliance).

---

## ? Can You Claim Treble Compliance Without Passing CTS and VTS?

**No**, a device **cannot** be considered Treble compliant if it **fails CTS or VTS**.

- Failing **VTS** means the vendor interface is not stable or correctly implemented.
- Failing **CTS** means the framework does not meet Android API expectations.
- GTS is optional unless you want to ship **Google apps**.

| Test | Required for Treble Compliance? | Why |
|------|-------------------------------|-----|
| **CTS** | ? Yes | Ensures Android framework compatibility |
| **VTS** | ? Yes | Ensures vendor interface is correct and stable |
| **GTS** | ? No | Only required for Google Mobile Services (GMS) |

---



## ? Can You Claim Treble Compliance Without Passing CTS and VTS?

**No**, a device **cannot** be considered Treble compliant if it **fails CTS or VTS**.

- Failing **VTS** means the vendor interface is not stable or correctly implemented.
- Failing **CTS** means the framework does not meet Android API expectations.
- GTS is optional unless you want to ship **Google apps**.

| Test | Required for Treble Compliance? | Why |
|------|-------------------------------|-----|
| **CTS** | ? Yes | Ensures Android framework compatibility |
| **VTS** | ? Yes | Ensures vendor interface is correct and stable |
| **GTS** | ? No | Only required for Google Mobile Services (GMS) |

---

## ?? SELinux Policy Guidelines to Pass CTS & VTS (Treble Compliance)

---

### ?? 1. Vendor Binary & File Location Rules

- ? Vendor binaries, libraries, HALs, and services **must reside in** `/vendor` or `/odm`.
- ? **Do NOT place** vendor components in `/system` or `/product`.
- ? Do not execute vendor binaries from system partitions.
- ? Label vendor files using `vendor_*` types (e.g., `vendor_exec_type`, `vendor_data_file_type`).

---

### ?? 2. SELinux Domain & Labeling Rules

- ? Each vendor HAL or service must have its own domain using `vendor_*`.
  ```bash
  type vendor_myhal, domain;
  init_daemon_domain(vendor_myhal)
```
? Do NOT assign system domains (e.g., system_server, init) to vendor processes.


? Use only vendor-specific SELinux types, not system types like system_file.

3. Vendor-to-System Access Rules
? Vendor components must NOT directly access system-side data, files, or services.


- ? Communication between vendor and system components must occur through stable AIDL/HIDL HALs or public Android APIs.


- ? Access to system services must be via defined interfaces in manifest.xml.



?? 4. Policy Modification Rules
? Do NOT modify AOSP system/sepolicy.
- 

? All custom SELinux policy must be added under:



```
device/<vendor>/<device>/sepolicy/vendor/
```
?? 5. Avoiding Over-Permissive Rules

- ? Never use wildcard or overly permissive rules:
```
allow * * * *;
```
- ? Avoid overly broad or generic allow rules without strong justification.


- ? Apply the Principle of Least Privilege: allow only what is required.

7. VINTF Manifest and HAL Alignment

- ? Every HAL listed in manifest.xml must:

Have a defined SELinux domain.

Be isolated from system domains.


- ? Do not allow vendor domains to modify or access system files unless permitted via defined HAL interfaces.


## ?? Summary Table

| Rule Category       | Key Rule                                              | Required for CTS/VTS |
|---------------------|--------------------------------------------------------|------------------------|
| Binary Location     | Use `/vendor` or `/odm` only                          | ?                     |
| Domain Usage        | Use only `vendor_*` SELinux domains                   | ?                     |
| System Access       | Access system only through HALs/APIs                  | ?                     |
| Policy Modification | Do not alter system SELinux policy                    | ?                     |
| Permissions         | Avoid wildcards and over-permissive `allow` rules     | ?                     |
| Testing             | Must pass VTS SELinux tests and analyze `neverallow`s | ?                     |
