### [Windows 10 64Bit](source/HowToInstallSEMA.md#windows-10-64bit)
### Ubuntu 20.04 LTS / Ubuntu 18.04 LTS
  * [how to install SEMA 4.0](source/HowToInstallSEMA.md#how-to-build-amp-install-sema-40)
  * [how to install GPIO Expander (PCA9535)](source/HowToInstallSEMA.md#how-to-install-io-expander-pca9535)



<br> 

### Windows 10 64Bit

Please go to [here](https://hq0epm0west0us0storage.blob.core.windows.net/public/SEMA%204.0.0_20200215.rar)  to download the Installer which contains:

* SEMA EAPI and SMBus Driver
* Command Line Interface Application

Running the installer which will be automatically installed SMBus driver, EAPI library and command line utility.

1. After download, please execute the installer file and click "Next" button

    ![install1](HowToInstallSEMA.assets/install1-1582257539251.png)


2. Click "Next" Button to start to install 

    ![capture2](HowToInstallSEMA.assets/capture2.png)

3. Until you see "Finish" button for the successful installation

    ![capture3](HowToInstallSEMA.assets/capture3-1582261215293.png)

4. You can see the installed files/folders under "**c:\Program Files\Adlink**"

  * **Application** folder: includes EAPI.dll, EAPI.lib, semauti.exe, example codes.
  * **SEMA_SMBus_4** folder: includes Windows SMBus drivers (sys, inf files)

<br />

<br>

### Ubuntu 20.04 LTS / Ubuntu 18.04 LTS

#### Prerequisites

Install build-essential package to install all tools used along with make. Install git, hexer and i2c-tools.

```
sudo apt install build-essential git hexer i2c-tools
```

<br>

#### How to Build & Install SEMA 4.0

#### Build and Install

1. Download the source code from ADLINK git repository

```
git clone https://github.com/ADLINK/sema-linux.git
```

2. Change directory to sema-linux and run make.

```
cd sema-linux
```

3. Run make

```
sudo make
```

4. To install driver modules, dynamic library and utilities into root file system

```
sudo make install
```


5. To load all of drivers

```
sudo modprobe -a adl-bmc adl-bmc-boardinfo adl-bmc-vm adl-bmc-wdt adl-bmc-hwmon adl-bmc-nvmem adl-bmc-bklight adl-bmc-i2c 
```

   **Note:** after installed, these files will be located at the following path

| File       | Description                                            |
| ---------- | ------------------------------------------------------ |
| libsema.so | EAPI Library is located under `/usr/lib/`              |
| semautil   | SEMA Command Line utility is located under `/usr/bin/` |


6. After loading kernel driver, configure the GPIO device with the following command.

```
echo pca9535 0x20 > /sys/bus/i2c/devices/i2c-12/new_device
```

**Note**: here i2c-12 is used, since SMBus is located at bus 12. `0x20` is GPIO device slave address. Please refer the below screenshot to know the i2c bus number and GPIO device address.



<br>

#### How to Install I/O Expander (PCA9535)

1. To load PCA9535 driver

   ```
   $ sudo modprobe -a gpio-pca953x
   ```

2. Please check the i2c bus number of SMBus CMI adapter driver with the below command:

   ```
   $ i2cdetect -l
   ```

   ![image-20210323103456374](HowToInstallSEMA.assets/image-20210323103456374.png)

   **Note:** 

   * In our case, the bus number is **i2c-0** 
   * please contact ADLINK FAE if there is no CMI adapter driver

   

2. Run the below command to add PAC9535 under **i2c-0** bus

```
$ sudo echo pca9535 0x20 > /sys/bus/i2c/devices/i2c-0/new_device
```

3. Once successfully,  you can see the PAC9535 device is loaded as the below

   ![image-20210323104505689](HowToInstallSEMA.assets/image-20210323104505689.png)

