# Azahar for EmuDEN

This fork (Denarmo/emuden-azahar, branch `emuden`) builds Azahar's own libretro core
(`src/citra_libretro`, `-DENABLE_LIBRETRO=ON`) for EmuDEN's Nintendo 3DS console plugin
(`emuden.n3ds`). EmuDEN loads it as a `libretro-core`, so it gets save states, fast-forward, shaders
and the player's controls. No source change was needed to run it: the fork exists to build and pin
the core locally (no GitHub Actions) and to carry any EmuDEN-specific change in one place.

Azahar is GPL-2.0-or-later (see LICENSE.txt). Its AI policy (AI-POLICY.md) governs contributions to
the upstream project; changes in this fork are not submitted upstream.

## Building (what the release zips are made with)

Submodules: `git submodule update --init --recursive --depth 1`.

- Android arm64: NDK 29.0.14206865, `cmake -G Ninja -DENABLE_LIBRETRO=ON -DCMAKE_BUILD_TYPE=Release
  -DANDROID_PLATFORM=android-21 -DANDROID_STL=c++_static -DANDROID_ABI=arm64-v8a
  -DCMAKE_TOOLCHAIN_FILE=$NDK/build/cmake/android.toolchain.cmake`, target `azahar_libretro`, then
  `llvm-strip -s`. Output name in the zip: `azahar_libretro_android.so`.
- Linux x86_64 (WSL Debian, gcc 14): `-DENABLE_LIBRETRO=ON -DENABLE_LTO=OFF -DCMAKE_BUILD_TYPE=Release`.
- Windows x64: MSVC 2022 build tools, Ninja, `-DENABLE_LIBRETRO=ON -DENABLE_LTO=OFF
  -DCMAKE_C_COMPILER=cl -DCMAKE_CXX_COMPILER=cl`.

## What EmuDEN sets (option hints in the plugin)

`citra_layout_option=default` (top screen over the touch screen, a 400x480 picture),
`citra_swap_screen=Top`, `citra_render_3d=off`, and `citra_resolution_factor` by device tier.
No BIOS, keys or firmware are needed for games that run on Azahar's built-in system software.
