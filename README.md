Cherish TV Launcher
A minimal, offline Android TV home screen with three layouts, light/dark colour themes, frosted glass, a configurable clock, a favourites dock, and built-in gradient and nature wallpapers.

Current build: Beta 2.0.2610 (org.stan.calmtv, versionCode 3). This is a beta tested on one TCL television. There is no in-app update checker yet, and it is not a Google Play release.

Requirements
An Android TV / Google TV device with Android 6.0+ (the APK declares API 23 as its minimum), landscape display, and a remote. Compatibility and blur performance have only been tried by me, the creator, on the TCL setup below; other firmware is unverified.
A way to install an APK: a TV file manager that supports sideloading and permission to install unknown apps, or a computer with Android SDK Platform Tools and authorized ADB.
For ADB over Wi-Fi, the TV and computer must be on the same network. Wireless debugging pairing is offered only on supported TV firmware. A USB-debugging switch by itself does not mean adb connect IP:5555 will work.
At least a few megabytes of storage. The APK is about 1.2 MB; wallpapers, exported settings and cached blur use more space and memory at runtime.
This APK cannot run on non-Android televisions or intercom displays. It does not require root, debloat the manufacturer system, or request internet access.

Install
Download CherishTVLauncher-Beta-2.0.2610.apk from this repository's GitHub Release Assets, not the source-code ZIP. Optionally verify its SHA-256 against the adjacent .sha256 asset.
On the TV: copy the APK via USB/LocalSend, open it in a file manager, and allow that file manager to install unknown apps if Android asks. Select Install. If your TV does not offer an APK installer, use ADB below.
Open Cherish from the apps list first. Check that your remote can navigate, Options opens, and you can return to the TV's existing home. Do this before changing the default Home.
ADB alternative: after installing official Platform Tools and authorizing your computer, run these commands in the directory containing the APK (Windows Command Prompt, macOS Terminal, or Linux shell):

adb devices
adb install -r CherishTVLauncher-Beta-2.0.2610.apk
adb shell am start -n org.stan.calmtv/.HomeActivity
-r updates an existing copy signed with the same key while retaining its app data. If you get INSTALL_FAILED_UPDATE_INCOMPATIBLE, stop: it is signed with a different key. Do not clear or remove the installed app just to force this update.

To connect with Wireless debugging when your TV exposes it, follow Android's pairing instructions: adb pair TV_IP:PAIRING_PORT, enter the displayed code, then adb connect TV_IP:CONNECT_PORT. These ports are different. On some devices ADB over network must instead be enabled through an initial physical USB connection (adb tcpip 5555 then adb connect TV_IP:5555), and some TVs provide neither method. Do not expose ADB to the public internet.

Set as default Home
On the TV, look for Settings → Apps → Default apps → Home app (names vary), choose Cherish TV Launcher, then press Home. If a Home chooser appears, select Cherish and Always.
If your firmware has no Home chooser but ADB is connected, record the current Home before changing it:
adb shell pm resolve-activity --brief --user 0 -a android.intent.action.MAIN -c android.intent.category.HOME
Save the component shown at the end, such as com.example.launcher/.MainActivity. Then try:

adb shell pm set-home-activity --user 0 org.stan.calmtv/.HomeActivity
adb shell pm resolve-activity --brief --user 0 -a android.intent.action.MAIN -c android.intent.category.HOME
Confirm the resolved Home is org.stan.calmtv/.HomeActivity, press Home and test after a normal restart. A Success message alone does not prove that a TV firmware honoured the choice. Some manufacturers override Home; if it still resolves to the stock launcher, stop and open a device-specific issue. Do not blindly disable a stock launcher, system UI, Play Store or TV input components.
Restore the previous Home
Select the original launcher in the TV's Home-app settings. If you used ADB, use the exact original component you recorded:

adb shell pm set-home-activity --user 0 ORIGINAL_PACKAGE/.OriginalActivity
adb shell pm resolve-activity --brief --user 0 -a android.intent.action.MAIN -c android.intent.category.HOME
Replace ORIGINAL_PACKAGE/.OriginalActivity with your saved result; it is an example, not a literal command. Verify the original is selected and Home works before considering any other app changes. This changes the Home preference; it does not delete your Cherish settings. Export a copy from Options → Your setup before trying future beta builds. On an existing personal Cherish installation, use its matching backed-up updater for APK rollback rather than these general first-install steps.

Tested setup
Item	Observed on the creator's TV
Brand	TCL
ADB-reported product	C06_2K_GB
ADB-reported model / device	Smart_TV / C06
Android version	Android 11 reported during the personal TV audit
Captured launcher image	1280 × 720 pixels (screenshot dimensions; not a verified panel resolution)
ADB connection during testing	Local Wi-Fi over TCP port 5555 on this firmware; this is not a universal setup
The retail TCL model number, SoC, CPU, RAM, and physical panel resolution have not been verified. To inspect your own TV, use adb shell getprop ro.product.model, adb shell getprop ro.build.version.release, and adb shell wm size (which reports display configuration, not necessarily native panel pixels).

What is included
Central, Simple, and Side layouts; light and dark Sky, Orchid, and Mint themes.
Nature or gradient wallpapers, rotation controls, a local image picker, and wallpaper-derived accent colours.
Adjustable frosted effect and optional artistic clock blur. Smooth gradients can make the blur visually subtle; nature photos show it more clearly. Blur uses memory and processing time even though the APK is small.
Dock editing, app list, settings, clock format/size, idle fade, and setup export/import.
No ads, telemetry SDK, or network permission. The TV's installed apps (Netflix, Play Store, etc.) remain separate and are not bundled with Cherish.
The earlier concept previews are design renders, not screenshots from the TV. Use real on-device screenshots for the GitHub release and issue reports, together with Android version, TV model, what happened, and steps to reproduce.

Source and assets
The Java source is in launcher/; it builds with JDK 17+, Android platform 35, and Build Tools 35 via build.py. The private signing keystore is not included. The script requires CHERISH_KEYSTORE_PASS and CHERISH_KEY_PASS environment variables and the original keystore at signing/calmtv-preview.jks to produce a compatible update. Keep the keystore and credentials out of Git history and GitHub Releases.

Bundled Poppins, Manrope, Outfit and Barlow Condensed font licences are in launcher/assets/fonts/. The nature photo credits and licence links are in PHOTO-CREDITS.md. A licence for the Cherish source code has not yet been chosen; the publisher should add one explicitly before inviting public contributions.
