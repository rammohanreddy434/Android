
# 🔐 Android App Signing and Key Usage

Understanding Android app signing and how different signing keys (platform, media, testkey, etc.) are used is essential for building secure and trusted Android system images and apps.

---

## 📦 How Android App Signing Works

### 1. Why Sign an APK?

- Every Android app **must be signed** with a digital certificate before it can be installed or run.
- Android uses the app’s **signing certificate** to:
  - Verify **authenticity**
  - Enforce **signature-level permissions**
  - Enable **shared UID** or allow **app updates**

### 2. Signature Verification

- At **install time**, Android checks the APK's digital signature.
- For app **updates**, the new APK must be signed with the **same key** as the existing one.
- Apps signed with the **same certificate** can:
  - Share data
  - Share a `sharedUserId`
  - Access `signature` or `signatureOrSystem` permissions

---

## 🗝️ Types of Signing Keys in Android

Android builds use various signing keys for different purposes. These are found in:

```bash
build/target/product/security/
```

| Key Name        | Usage Area                    | Purpose                                                                 |
|------------------|-------------------------------|-------------------------------------------------------------------------|
| `platform`       | Core system apps and services | Grants system-level privileges, shared UID with `android.uid.system`    |
| `media`          | Media components               | Grants access to media framework features                               |
| `shared`         | Shared UID apps                | Used for apps sharing a UID (e.g., dialer, messaging)                   |
| `networkstack`   | Network stack modules          | Used to sign network-related components                                 |
| `testkey`        | Generic test key               | Default debug key — **insecure for production**                         |
| `releasekey`     | OEM production apps            | OEM-provided key used to sign production apps                           |
| `veritykey`      | Verified boot key              | Used for dm-verity metadata signing                                     |

---

## ⚙️ How Keys Are Used in the Build

During the build process, each app is signed with a key specified in its `Android.mk` or `Android.bp` file:

```make
LOCAL_CERTIFICATE := platform
```

- If not explicitly specified, it may default to `testkey`.
- This affects:
  - Installation privilege
  - Runtime permissions
  - Access to `signature` permissions

---

## 🛑 Importance of Using the Correct Key

- Apps signed with the **platform key** get access to **signature-level permissions**, e.g.,

```xml
<permission android:name="android.permission.MANAGE_USERS"
            android:protectionLevel="signature" />
```

- If a system app isn't signed with the right key, it may:
  - **Fail to install**
  - **Lose access to privileged permissions**
- **Never** sign production builds with the `testkey` — it’s widely known and **easily exploitable**.

---

## 📦 Example: `Settings.apk`

- Signed with: **platform key**
- UID: `android.uid.system`
- Access to signature-only permissions like `MANAGE_USERS`, `DEVICE_POWER`, etc.

---

## 🏁 Summary Table

| Item                     | Description                                                   |
|--------------------------|---------------------------------------------------------------|
| App Signing              | Ensures integrity, authenticity, and permission enforcement    |
| Platform Key             | For core system apps with system UID and signature-level perms |
| Media Key                | For media-related components                                   |
| Test Key                 | For development only, insecure for production use             |
| Release Key              | OEM-specific production signing key                           |
| Signature Verification   | Happens at install/update time                                |
| Shared User ID           | Requires all apps be signed with the same key                 |

---

## ✅ Best Practices

- Always use **release keys** for production builds.
- Use `platform`, `media`, `shared` keys only for their intended purposes.
- Ensure all privileged apps are signed with **appropriate keys** to avoid permission denial or install failures.
