# com.citrix.ICAClient
Build and install the Citrix Workspace app (ICAClient) as a Flatpak application for Linux.

## Disclaimer
This project and I are not affiliated with Citrix. This repository does not contain any Citrix software. When the user builds the Flatpak application using this template, the required packages are obtained from Citrix's website, where Citrix has made the installers available for download. By downloading Citrix software, you agree to and accept the [Citrix End-User License Agreement](https://www.cloud.com/content/dam/cloud/documents/legal/end-user-agreement.pdf).

This repository will be supported and updated for as long as I require this for my work. As I am a consultant and change projects fairly frequently, this will not be for that long.

## Requirements
flatpak, flatpak-builder, elfutils, pulseaudio
You should be able to install all of these through your distro's package manager.

This flatpak is used by me on the latest version of Bluefin, a variant of Fedora Silverblue. It is completely untested on any other distribution.

## Instructions
Perform the [flatpak setup](https://flatpak.org/setup/).
Add the flathub remote, and install the Gnome SDK and runtime:
*flatpak remote-add --user --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
flatpak install --user flathub org.gnome.Platform//46
flatpak install --user flathub org.gnome.Sdk//46*

Clone/download this repo. Open a terminal in the folder where you downloaded this repo, and run the following:
*flatpak-builder --user --install --force-clean icaclient com.citrix.ICAClient.yml*
(If your distro uses musl libc rather than the tyipcal GNU libc, you may have to add the flag "--disable-rofiles-fuse" due to [this bug](https://github.com/flatpak/flatpak-builder/issues/329))

It will take some time to download sources and build. Note that libwebkit2gtk, one the the dependencies in the build, requires a significant amount of memory and CPU time to compile. >=32GB system memory is recommended.

Once it is finished, it should be automatially added to your application launcher (may need to log out and back in for it to show up), if not you can launch it via:
*~/.local/share/flatpak/exports/share/applications/com.citrix.ICAClient.desktop*
Or launch it via command line:
*flatpak run com.citrix.ICAClient*

## Updating
When you build the app, it will automatically grab the most recent versions of Workspace. Therefore, if Citrix has published new verions you can update your flatpak app by simply re-running the *flatpak-builder* command listed above. However, you may need to empty the .flatpack-builder folder (located in the folder where you previously built the app), as Flatpak may see the build manifest hasn't changed and therefore use the cached files from the previous build, rather than re-downloading and grabbing the latest installers.

## Notes
So far the only thing I can confirm is that the application starts. I've not actually logged into anything and made sure it works.
