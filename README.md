# How to install a new printer on Ubuntu (Kubuntu 18.04 in my case)

I have acquired a new HP printer (HP Laser MFP 135a) a few days ago and I was trying to install it on Ubuntu.
As it turned out it wasn't as easy as I was expecting it to be.

First of all the printer I got is connected only via USB cable so these instructions only are applicable here.

I have found some instructions over a gist file and really like to upgrade them to be more readable. To not take the credit thought here is the actual [gist link](https://gist.github.com/taniwallach/f1f6c81ce19b7d68f74d4b71d1db57a2).

## Actions I tried but not succesfully solved my problem

- I read some blogs where you just connect your new printer and it's being detected by Kubuntu (I am not sure if original Ubuntu behaves better here so I won't generalize this). My printer wasn't detected by the system.

- Install HPLIP because at some answer it was suggested. Did not help either as the program (run from bash):
$hp-setup

could not recognize the my printer:
>HP Linux Imaging and Printing System (ver. 3.20.3)  
>Printer/Fax Setup Utility ver. 9.0  
>
>Copyright (c) 2001-18 HP Development Company, LP  
>This software comes with ABSOLUTELY NO WARRANTY.  
>This is free software, and you are welcome to distribute it  
>under certain conditions. See COPYING file for more details.  
>  
>Searching... (bus=usb, search=(None), desc=0)  
>error: No devices found on bus: usb  

## Actions that resolved my problem

- I searched and downloaded the original provided driver from HP. From [here](https://support.hp.com/us-en/drivers/selfservice/closure/hp-laser-mfp-130-printer-series/24494378/model/24494379?ssfFlag=true&sku=). Well, the driver was found (it's a generic driver as I get it but I am satisfied with that) but no other instructions were given.

- The downloaded file was _ULDLINUX_V1.00.39_00.12.zip_ which of course had to unzip to get the actual driver. Inside the zipped file there is another folder _uld_ where the interesting files reside: _install-printer.sh_ (and I guess _install-scanner.sh_ although not tested)

- Execute with root priviledges the former script:
`sudo ./install-printer.sh`

The menu need some comfirmations:
  * "q" to leave "more" of the license
  * "y" to accept the license
  * "n" (not installing network device)
  
Summary from install:

>Do you agree ? [y/n] : y  
>Are you going to use network devices ? If yes, it is recommended to configure your firewall.  
>If you want to configure firewall automatically, enter 'y' or just press 'Enter'. To skip, enter 'n'. : n  
>Registering CUPS backend ...  
>CUPS restart OK.  
>Print driver has been installed successfully.  
>Install finished.  

If everything goes OK this will install the driver (not the __printer!__)

- The latter step installs some files to _/opt/hp/printer/share/ppd/_
`ls -l /opt/hp/printer/share/ppd/`
>drwxr-xr-x 2 root root   4096 Απρ   8 14:49 cms  
>-rwxr-xr-x 1 root root  37069 Απρ   8 14:49 HP_Color_Laser_15x_Series.ppd  
>-rwxr-xr-x 1 root root  37065 Απρ   8 14:49 HP_Color_Laser_MFP_17x_Series.ppd  
>-rwxr-xr-x 1 root root  35391 Απρ   8 14:49 HP_Laser_10x_Series.ppd  
>-rwxr-xr-x 1 root root  81576 Απρ   8 14:49 HP_LaserJet_MFP_M433.ppd  
>-rwxr-xr-x 1 root root  88555 Απρ   8 14:49 HP_LaserJet_MFP_M436.ppd  
>-rwxr-xr-x 1 root root 183424 Απρ   8 14:49 HP_LaserJet_MFP_M72625_72630.ppd  
>-rwxr-xr-x 1 root root  35399 Απρ   8 14:49 HP_Laser_MFP_13x_Series.ppd  

- I used the CUPS web interface (I found it more easy to use than the other options) by invoking:
http://localhost:631 on the browser

- I added a new printer by choosing [Adding Printers and Classes](http://localhost:631/admin) and providing my credentials

- Then I choose _Local printer_ and my (recognized) printer.

- The intersting part is when I should choose a driver and I point to the PPD file by choose the previously installed file. In my case of course this meant choosing _HP_Laser_MFP_13x_Series.ppd_.

Hope this be helpful to someone frustated user.

Good luck!
