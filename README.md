# automated-effect-generation

This repo is a fork of [KDE-Rounded_Corners](https://github.com/matinlotfali/KDE-Rounded-Corners), which is a working KWin effect that rounds corners of windows and actually works with Plasma6 and Qt6. The goal of this repo is to modify the source code to align it with our use case, which is to:

**Create an automated pipeline that will apply fragment shaders to specific applications through KWin**.

# What needs to be changed
- Currently, the effect applies to all windows. It should only apply to a specific window and the user should be able to decide which application the effect should be applied to. This can maybe be done by assigning a keyboard shortcut to toggle the effect on/off.
- The Effect needs to be applied even if the window is maximized. Right now, the effect only applies if the window is not maximized.
- Figure out a pipeline for how the plugin will import fragment shaders. Currently, there is a .frag file in src/shaders/shapercorners_qt6 (qt6 since we are running qt6). This frag file runs the run() function in the shapecorners.glsl file in this directory. You can edit this function to change the effect/shader. But we will eventually need to figure out a way to apply any fragment shader to this plugin.

# How to build from source code

Install these dependencies:

```bash
sudo apt install git cmake g++ extra-cmake-modules kwin-dev qt6-base-private-dev qt6-base-dev-tools libkf6kcmutils-dev
```
If the build doesn't work, try installing these extra dependencies:
```bash
sudo apt install git cmake g++ extra-cmake-modules qt6-tools-dev kwin-dev libkf6configwidgets-dev gettext libkf6crash-dev libkf6globalaccel-dev libkf6kio-dev libkf6service-dev libkf6notifications-dev libkf6kcmutils-dev libkdecorations3-dev libxcb-composite0-dev libxcb-randr0-dev libxcb-shm0-dev
```

Then clone the source code and compile it:
```bash
git clone https://github.com/alanvillagrand-bip/automated-effect-generation
cd automated-effect-generation
mkdir build
cd build
cmake ../ -DCMAKE_INSTALL_PREFIX=/usr
make -j
sudo make install
```

# How to load or unload the effect

To activate the effect, you can now log out and log back in, or run the command below inside the `build` directory:

```bash
sh ../tools/load.sh
```

To fully uninstall the effect, run the following commands inside the `build` directory:

```bash
sh ../tools/unload.sh
sudo make uninstall
```

# How to recompile and reinstall after making changes
To reinstall and reload the effect, run the following commands inside the `build` directory:
```bash
sh ../tools/unload.sh
sudo make uninstall
make -j
sudo make install
sh ../tools/load.sh
```

# How to view debugging logs
Run:
```bash
journalctl -f /usr/bin/kwin_wayland
```
