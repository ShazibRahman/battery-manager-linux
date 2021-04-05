# Battery Manager Linux (ASUS Laptops)

When the laptop is being constantly used with a charger plugged in it is better to limit the charging at 60% to 80% to improve the battery health.
Many laptop vendors like Asus provide software utility to set the battery max charge threshold but it works only in windows.

With Linux kernel 5.4 added the ability to set a battery charge threshold for many Asus laptops this script uses it to set the limit.

## Usage
Run the script limit.sh with max battery threshold as an argument

`eg: ./limit.sh 60`

*prompt to enter the password since it needs sudo permission*

Will set the battery threshold to 60% so even if the laptop is plugged in it won't charge beyond 60% helps to protect the battery health.

#### Set a systemd service

Set limit will be reset to 100% on system reboot.\
To apply the settings on reboot make a systemd service for that.

Create a battery-manager service by creating **battery-manager.service** file at **/etc/systemd/system/** 

`sudo nano /etc/systemd/system/battery-manager.service`

Paste the contents from [**battery-manager.service**](https://raw.githubusercontent.com/shazx06/battery-manager-linux/main/battery-manager.service) and save using ctrl+o (sets battery threshold to 60% change if needed)

Enable the service 

`sudo systemctl enable battery-manager.service`

Reboot the system and check if limit works

#### Renable full capacity 

Run the limit.sh script with 100%

`./limit.sh 100`

Note: make the script executable before running by executing 
`chmod +x limit.sh`

Disable the systemd service 

`sudo systemctl disable battery-manager.service`
#### Using cron
In case you don't use systemd, you can also do this by using cron,by running  `sudo crontab -e` and then pasting the following line (it's a single line):

```@reboot echo YOUR_PREFFERED_THRESHOLD(60%,80%,etc) > /sys/power_supply/BATTERY_NAME/charge_control_end_threshold```

## More info
* [ASUS Battery Information Center](https://www.asus.com/support/FAQ/1038475/)
* [Arch Wiki](https://wiki.archlinux.org/index.php/Laptop/ASUS#Battery_charge_threshold)


-----
>Tested in Asus Vivobook X412FJC with Intel i5-10210U running Linux mint 20.1 Kernal 5.4.0-58-generic
