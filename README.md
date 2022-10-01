## Welcome to 9810 AOSP Building guide!
## All of the Instructions lines are from my experience while building AOSP for 9810 devices. If I messed something up below, please send me a message for me to fix.

1. Requirements
To build AOSP on 9810 devices, you must prepare:
+ A Linux environment (Ubuntu 18.04 or newer is recommended!).
+ A 6-cores CPU and 16GB RAM or higher.
+ A good network! (Because you will need to sync 100GB+ of sources)
+ 300GB or more storage.
+ Basic knowledge about git.

2.Steps
+ Setting up building environment:
  - First, you will need the building environment before building. Luckily, we have Akhilnarang's scripts, which is more easier for us to setting up.
  - We need to clone his scripts on Github by doing this:
     ```bash
     git clone https://github.com/akhilnarang/scripts
     ```
 
  - This should copy all the scripts into /home/username/scripts
  
  - Then we need to navigate into that folder by doing this:
     ```bash
     cd scripts/setup
     ```
  - Then we need to lists out the directory:
     ```bash
     ls
     ```

  - Find the file that corresponds to our Linux build. Since we are using Ubuntu it will be android_build_env.sh.
    For other Distros refer to the readme that has also been cloned.
 
  - Run the scripts: 
    ```bash
     . android_build_env.sh
    ```

+ After all of the scripts are synced, we will need to setup Github by doing this:
    ```bash
     git config --global user.name "your name"
     git config --global user.email "youremail@example"
    ```

+ Syncing the sources
  - This is one of the worse step, you have to wait for a long time, depends on your network xD
  - Check the ROM you want to build, then we will make a directory for the sources
    (at here, I'll take PixelExperience for an example, you can change the name of the directory as you like):
    ```bash
     mkdir "ROMNAME"
      (example: mkdir pexp)
    ```

  - Then we will navigate to that directory:
    ```bash
     cd "ROMNAME" 
     (example: cd pexp)
    ```

  - You will have to check the command line to sync all the sources by looking on their Github.
    Usually The repository initiation command can be found on the GitHub page in manifest repo,
    then scroll down to repo initialisation and copy command.
  
  - It should look like this: 
    ```bash
     repo init -u https://github.com/PixelExperience/manifest -b thirteen
    ```

+ Cloning devices trees:
  - For 9810: you may have a look into our organization for all trees at here: https://github.com/SamsungExynos9810
  - Tip: https://github.com/SamsungExynos9810/local_manifests (All trees that we used to build is at here)
 ```bash
    while in rom folder do
    git clone https://github.com/SamsungExynos9810/local_manifests .repo/local_manifests
 ```

  - Then download all the sources:
    ```bash
     repo sync -c -j$(nproc --all) --force-sync --no-clone-bundle --no-tags
    ```

  - Now wait!

+ Modifying the trees for ROMs:
  - There are 3 files that you need to edit here: romname_device.mk, romname.dependencies, AndroidProducts.mk.
  - So how to modify it to build the ROM you want?
  - Easy, let's take Lineage 19.1 with beyond1lte as example
	 (https://github.com/LineageOS/android_device_samsung_beyond1lte/tree/lineage-19.1)

	+ In AndroidProducts.mk, you can see all has been changed into lineage_device.mk, so do the same for our device.
	+ lineage.dependencies, just rename the file and don't touch anything in it.
	+ lineage_beyond1lte.mk: you will need to change (necessary) PRODUCT_NAME into our 9810 devices (Example: PRODUCT_NAME := lineage_crownlte)
  + then search for this line about vendor and change ROMNAME according to the rom you will build 
    $(call inherit-product, vendor/ROMNAME/config/common_full_phone.mk) 
    depending on ROM it can be $(call inherit-product, vendor/ROMNAME/config/common.mk) too.
  - Now return to your rom repo, resync.

3. Build
    ```bash
   # Set up environment
     . build/envsetup.sh

   # Choose a target
     lunch romname_$device-userdebug

   # Build the code
     mka bacon  
   (check ROM manifest on github for exact commands)      
    ```
