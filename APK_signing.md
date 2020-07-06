Application Signing

Every application that is run on the Android platform must be signed by the developer.	Applications that attempt to install without being signed will be rejected by either Google Play or the package installer on the Android device.

On Android, application signing is the first step to placing an application in its Application Sandbox.

On Android, application signing is the first step to placing an application in its Application Sandbox. The signed application certificate defines which user ID is associated with which application; different applications run under different user IDs.Application signing ensures that one application cannot access any other application except through well-defined IPC.

When an application (APK file) is installed onto an Android device, the Package Manager verifies that the APK has been properly signed with the certificate included in that APK.

Applications can be signed by a third-party (OEM, operator, alternative market) or self-signed. Android provides code signing using self-signed certificates that developers can generate without external assistance or permission. Applications do not have to be signed by a central authority. Android currently does not perform CA verification for application certificates.
