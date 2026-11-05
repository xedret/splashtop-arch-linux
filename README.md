# splashtop-arch-linux installation guide
## 1.- Download and uncompress the Splashtop installer
Until today (11/04/26), the latest download link is:
```
wget https://download.splashtop.com/linuxclient/splashtop-business_Ubuntu_v3.8.6.0_amd64.tar.gz
tar -xvzf splashtop-business_Ubuntu_v3.8.6.0_amd64.tar.gz
```
### 1a.- Scenario with working debtap (best case scenario)
Once uncompressed we should have a `splashtop-business_Ubuntu_amd64.deb` in our current working directory. If you have debtap installed try:.
```
debtap splashtop-business_Ubuntu_amd64.deb
sudo pacman -Syu
sudo pacman -U splashtop-business-3.8.6.0-1-x86_64.pkg.tar.zst
```
If you encounter no errors you should be able to run splashtop-business by executing:
```
splashtop-business
``` 
If you encountered errors during this stage, proceed to 1b 


### 1b.- debtap hell scenario
The debtap installation may require a bit of tweaking as it has a known bug that stems from an invalid regular expression in the script's database check logic, specifically `grep -E '*.files?(.[[:digit:]]{3})'`, which fails to match current pkgfile cache filenames like `core.files` or `extra.files`.

We will require to remove debtap and it's cache:
```
sudo pacman -R debtap
sudo rm -rf /var/cache/debtap
```
Then reinstall it from the AUR by running:
```
yay -S debtap-git
```
Once installed make sure to update the debtap database:
```
sudo debtap -u
```
At this point we should still have problems with the debtap's line that checks the database logic, to overcome this I have fixed the expression, please proceed to manually replace your `/usr/bin/debtap` with the variant file in this repo.

With the edited debtap file in place, the database logic check should be compliant and we should be able to run:
```
debtap splashtop-business_Ubuntu_amd64.deb
```
If executed successfully, we should have a new file named `splashtop-business-3.8.6.0-1-x86_64.pkg.tar.zst` in our current working directory. Install it on Arch Linux with:
```
sudo pacman -Syu
sudo pacman -U splashtop-business-3.8.6.0-1-x86_64.pkg.tar.zst
```
And to finally run `splashtop-business` just run the command:
```
splashtop-business
```

## 2.- Fixing `libxdo` errors after installing `splashtop-business`:
If you successfully installed the zst file but get an error about `libxdo` on the terminal. Proceed to install xdotool, create a symlink to the new library and executing `splashtop-business`:
```
sudo pacman -S xdotool
sudo ln -s /usr/lib/libxdo.so.4 /usr/lib/libxdo.so.3
splashtop-business
```

## 3.- Enjoy!!
That's it :)
