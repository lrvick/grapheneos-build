# GrapheneOS Build #

<http://github.com/lrvick/grapheneos-build>

## About ##

This project builds GrapheneOS using the sandboxed [AOSP-Build][1] system.

[1]: https://github.com/hashbang/aosp-build

## Devices ##

  | Device     | Codename   | Tested | Verifiable | Secure Boot |
  |------------|:----------:|:------:|:----------:|:-----------:|
  | Pixel 3 XL | Crosshatch | FALSE  | FALSE      | AVB 2.0     |
  | Pixel 3    | Blueline   | FALSE  | FALSE      | AVB 2.0     |
  | Pixel 2 XL | Taimen     | FALSE  | FALSE      | AVB 1.0     |
  | Pixel 2    | Walleye    | FALSE  | FALSE      | AVB 1.0     |
  | Pixel XL   | Marlin     | FALSE  | FALSE      | dm-verity   |
  | Pixel      | Sailfish   | FALSE  | FALSE      | dm-verity   |

## Build ##

### Requirements ###

 * Linux host system
 * Docker
 * x86_64 CPU
 * 10GB+ available memory
 * 60GB+ disk

### Generate Signing Keys ###

Each device needs its own set of keys:
```
make DEVICE=crosshatch keys
```

### Build Factory Image ###

Build flashable images for desired device:
```
make DEVICE=crosshatch clean build release
```

## Install ##

### Requirements ###

 * OSX/Linux host system
 * [Android Developer Tools][4]

[4]: https://developer.android.com/studio/releases/platform-tools

### Extract
```
unzip crosshatch-PQ1A.181205.006-factory-1947dcec.zip
cd crosshatch-PQ1A.181205.006/
```

### Flash

Unlock the bootloader.

NOTE: You'll have to be in developer mode and enable OEM unlocking

```
adb reboot bootloader
fastboot flashing unlock
```

Once the bootloader is unlocked it will wipe the phone and you'll have to do
basic setup to be able to drop into fastboot. You can skip everything since
you'll be starting from scratch again after flashing #!OS

Reboot phone in fastboot and flash

#### Pixel

```
adb reboot bootloader
./flash-all.sh
```

#### Pixel 2+

```
adb reboot fastboot
./flash-all.sh
```

## Develop ##

### clean ###

Do basic cleaning without deleting cached artifacts/sources:
```
make clean
```

Clean everything but keys
```
make mrproper
```

### Compare ###

Build a given device twice from scratch and compare with diffoscope:
```
make shell
```

### Edit ###

Create a shell inside the docker environment:
```
make shell
```

### Patch ###

Output all untracked changes in android sources to a patchfile:
```
make diff > patches/my-feature.patch
```

### Flash ###
```
adb reboot fastboot
make install
```

## Release ##

WIP

### Update ###

Build latest config from upstream sources:

```
make DEVICE=crosshatch config manifest
```
