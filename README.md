# 🛡️⚔️👾 Slayin64

Personal arm64 rebuild of **Slayin** (FDG Entertainment, 2013) for a 64 bit only Pixel 8 Pro. The original Android APK (2.0.13) shipped only `armeabi-v7a` and cannot run on phones without 32 bit support.

Slayin is © FDG Entertainment / Pixelicious; this is a fan compatibility rebuild of the 2013 Android release for phones that dropped 32 bit support, not affiliated with or endorsed by FDG. The Unity project (an AssetRipper export of the original APK) and the game's decompiled scripts are kept locally. This repo has our build scripts and `PATCHES.md`, which describes every change so it can be applied again to a fresh export. The APKs are in Releases.

## Builds (see Releases)
The latest release, [v2.0.13-64](https://github.com/firestarter1996/Slayin64/releases/latest), has two APKs:

- `Slayin64-arm64.apk`: the faithful game.
- `Slayin64-Unlimited-arm64.apk`: same package and title (`com.heyitsj0n.slayin64` / "Slayin64") with the `SLAYIN_CHEAT` define: 999,999 fame points and no debit.

Both are debug signed by Unity; install with `pm install -r` (same signature, so either replaces the other).

## How it was made
1. AssetRipper 2.0 export of the original APK (Unity 4.5.5), opened in **Unity 2022.3.62f3**.
2. Fixes needed on a modern engine (details in `PATCHES.md`):
   - real tk2d shaders (the export only had placeholders): `Shader/`, with `Resources/BlendVertexColor.shader` overwritten in place to keep its GUID;
   - `Rb2DCompat.cs`: kinematic `Rigidbody2D` velocity semantics changed, so coins fell through the floor;
   - `AbstractBehavior` z tie break: Unity 2022 sorts sprites with equal z again every frame, so overlapping slimes flickered;
   - `ScrollMaskFix.cs` and a stencil `DepthMask.shader`: tk2d's depth masks drew solid black bars over the tavern;
   - AdColony JNI init stubbed and 169 plugin calls wrapped (JNI abort on launch);
   - IL2CPP, ARM64, targetSdk 34, original icon, `SlayinBuild.cs` batch build.
3. `scripts/build_both.bat` builds both variants: `Unity.exe -batchmode -nographics -executeMethod SlayinBuild.Android`, env `SLAYIN_VARIANT=unlimited` for the cheat. `scripts/build_unlimited.bat` builds only the Unlimited one.

Toolchain: JDK 11 + NDK r23b in `C:\Users\jonal\AndroidUnity`, SDK root `C:\Users\jonal\AndroidUnity\SDK`.

## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=firestarter1996/Slayin64&type=Date)](https://star-history.com/#firestarter1996/Slayin64&Date)
