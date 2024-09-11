# Guide on how to setup Adobe Acrobat to use the PIV module of your Yubikey

1. Install OpenSC from [this](https://github.com/OpenSC/OpenSC/releases) link (don't use pre-release)
2. Install Yubikey PIV tool from [this](https://developers.yubico.com/yubico-piv-tool/Releases/) link
3. Add the bin folder path of the Yubikey PIV tool to your path (Eg. path: `C:\Program Files\Yubico PIV Tool\bin`)
4. Open Adobe Acrobat
5. Goto Menu -> Prefrences -> Signatures -> Identities & Trusted Certificates -> PKCS#11 Modules and Tokens
6. Press the `Attach Module` button
7. Go to your Yubikey PIV tool bin path and select the `libykcs11.dll` file and press open
8. Expand the `PKCS#11 Modules and Tokens` list
9. Insert you Yubikey into your system/computer
10. Press the refresh button and select your Yubikey
11. Press the login button and enter your PIN
12. After login select your Yubikey in the list of devices on the left
13. Select your certificate and change its usage options to use it for signing
14. Close everything, you can now use a certificate stored on your Yubikey to sign PDF's