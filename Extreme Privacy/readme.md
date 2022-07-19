# Extreme Privacy: What it Takes to Disappear

### Introduction - Extreme Privacy

**Doxing**: The act of publishing complete personal details about a person online.

### Chapter One - Computers

Types of information Apple and Microsoft collect from devices:
- Approximate location
- IP Address history
- Search history
- Typed text
- Programs downloaded
- Programs opened

Use Linux (especially Ubuntu for new Linux users) over Mac or Windows

**Linux:**

System76 (https://system76.com/) sells Linux computers with Intel Management Engine disabled.

When installing Linux, choosing LVM with encryption increases security.
Run the command below to remove crash reporting and usage statistics on Ubuntu:

```
sudo apt purge -y apport
sudo apt remove -y popularity-contest
sudo apt autoremove -y
```

1. Launch "Settings" from the Applications Menu
2. Click "Notifications" and disable both options.
3. Click "Privacy", then "File History & Trash", and disable all options
4. Click "Diagnostics", then change to "Never".

Although rare, malwares for Linux do exist. Download ClamAV using the following commands:

```
sudo apt update
sudo apt install -y clamav clamav-daemon
sudo systemctl stop clamav-freshclam
sudo fleshclam
sudo systemctl start clamav-freshclam
```

To run the actual scan, run the following commands:

```
clamscan -r -i
clamscan -r -i --remove==yes
```

For system cleaner on Linux, use BleachBit, which can be installed through the command `sudo apt install bleachbit`.

Applications should be regularly updated via the following commands:

```
sudo apt update
sudo apt upgrade
```

Backup the home directory often in case of a disaster.

There is no way to achieve privacy with Google Chromebook.

It is better to start with a completely new device instead of sanitizing an existing computer, as Apple and Microsoft can record the serial numbers, hardware details, and IP address to identify the owner of the machine.


**Apple:**

More secure than Windows by default.

Although Apple ID is required for software and system updates, it can be bypassed, so Apple ID should never be attached to the device.

FDE can be enabled via FileVault

1. Open "System Preferences" and select "Security & Privacy"
2. Choose "FileVault"
3. Choose the second recovery option to store the recovery key manually
4. Copy and paste the key to a password manager

Do the following as well to increase security and privacy:

1. Open "Privacy & Security" in "System Preferences"
2. Under "General", change "Require password" to "Immediately"
3. Choose "Firewall" and enable it

Any of the Apple stock applications should not be used.

**Brew**: Package manager for MacOS

Brew can be installed by running the following command: `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`

Although malware on MacOS is extremely rare, there are cases when an Antivirus is benefitial. In these cases, use ClamAV
- Prevent bad files to spread to Windows users
- Government and private organizations may require possession of antivirus
- Court may ask for antivirus when doing an online investigations

Install ClamAV on MacOS by running the following commands:

```
brew analytics off
brew install clamav
sudo mkdir /usr/local/sbin
sudo chown -R `whoami`:admin /usr/local/sbin
brew link clamav
cd /usr/local/etc/clamav
cp freshclam.conf.sample freshclam.conf
sed -ie 's/^Example/#Example/g' freshclam.conf
```

Then update the database and conduct scans by running:

```
freshclam -v
clamscan -r -i
```

Some other Antimalwares include OverSight and Onyx.

**OverSight**: Notifies when a microphone or a webcam is enabled via an application. Can be installed by `brew install oversight`

**Onyx**: Maintenance program the can be installed by `brew install onyx`

**VirtualBox**: Virtualization software. can be installed by `brew install virtualbox`

To update software, run the following series of commands:

```
brew update
brew upgrade
brew cleanup -s
brew autoremove
```

**Privacy.sexy (https://privacy.sexy/)**: Creates custom privacy scripts for MacOS and Windows.

**Little Snitch**: Block network connections for specific applications. 

1. Modify default system setting within the "Rules" menu
2 Disable iCloud and macOS Services

**Windows:**

When creating an account on Windows 10 or 11, disable Wi-Fi to create a local account instead of Microsoft account.

Microsoft's Telemetry service collects the following:
- Typed text on keyboard
- Microphone transmissions
- Index of all media files on computer
- Webcam data
- Browsing history
- Search history
- Location activity
- Health activity collected by HealthVault, Microsoft Band, and other trackers
- Privacy settings across Microsoft applications

Choose "Customize Settings" and disable all options.

After inital boot, apply all system updates.

For Windows Pro, Bitlocker can be used for FDE. For Home users, VeraCrypt (https://www.veracrypt.fr/en/Home.html) provides FDE.

1. Install VeraCrypt from homepage and launch the program
2. Click "System" > "Encrypt System Partition/Drive"
3. For system encryption choose "Normal"
4. Select "Encrypt the whole drive" and choose "Single-boot".
5. Choose the default encryption standard
6. Enter a password
7. Create a Rescue Disk which will be used in case of corrupted filesystem.
8. Choose none when prompted for "wipe mode"

Antivirus is a necessity for Windows machine. Although Microsoft Defender collects many datas, because the OS does it anyways, it is preferred over other AV products.

**BleachBit**: System cleaner similar to CCleaner

**O&O ShutUp10++ (https://www.oo-software.com/en/shutup10)**: Utilities to block Microsoft's data collection. Selecting "Recommended and somewhat recommended" under "Actions" will block everything except Windows updates, Windows Defender, and OneDrive. 

**Glass Wire** or **Portmaster** works similar to Little Snitch, but not as intensive.

To remove Windows stock applications:

1. Execute PowerShell as Administrator
2. `set-executionpolicy remotesigned`
3. `Get-AppxProvisionedPackage -Online | Format-Table DisplayName, PackageName`
4. 
```
$ProvisionedAppPackageNames = @(
“Microsoft.3DBuilder”
‘“Microsoft.BingFinance”’
‘“Microsoft.BingNews”
“Microsoft.BingSports”’
“Microsoft.BingWeather”’
“Microsoft.ConnectivityStore”
““Microsoft.Getstarted”
““Microsoft.Messaging”’
““Microsoft.Microsoft3D Viewer”’
‘“Microsoft.MicrosoftOfficeHub”
“Microsoft.MicrosoftSolitaireCollection’
“Microsoft.MicrosoftStickyNotes”
‘“Microsoft.MSPaint”
‘“Microsoft.Office. OneNote”
“Microsoft.People”
“Microsoft.Print3D”
‘“Microsoft.skypeApp”’
“Microsoft.StorePurchaseApp”’
“microsoft.windowscommunicationsapps” # Mail,Calendar
“Microsoft. WindowsFeedbackHub”
“Microsoft.WindowsPhone”’
“Microsoft.WindowsStore”’
“Microsoft. Xbx. TCUI”
“Microsoft.XboxApp”
“Microsoft.XboxGameOverlay”
“Microsoft. XboxIdentityProvider”’
“Microsoft.XboxSpeechToTextOverlay”
“Microsoft.ZuneMusic”’
“Microsoft.ZuneVideo”’
“Microsoft. YourPhone’’)
foreach ($ProvisionedAppName in $ProvisionedAppPackageNames) {
Get-AppxPackage -Name $ProvisionedAppName -AllUsers | Remove-AppxPackage
Get-AppXProvisionedPackage -Online | Where-Object DisplayName -EQ $ProvisionedAppName |
Remove-AppxProvisionedPackage -Online}
exit 
```

Windows 10/11 Enterprise LTSC versions omits Edge, Microsoft Store, and Cortona.

Even after the hardening, Linux is preferred over Mac, and Mac is preferred over Windows.


### Chapter Two - Mobile Devices

If an extreme privacy is required, a completely new mobile device is required.
Used mobile device is also ill-advised, as it could have used by a criminal before.

The following is in order of high security and privacy to low:
- **GrapheneOS**: Open Source software that converts Google Pixel device into pure Android device without Google services. Most secure and private options.
- **LineageOS**: Custom ROM similar to GrapheneOS, but the bootloader is unlocked, which is a vulnerability. Still better than any stock device.
- **AOSP (Android Open Source Project)**: Create custom Android OS without any Google services. Used for devices that does not support GrapheneOS or LineageOS.
- You can manually remove Google services from phone to make it more secure.
- iPhone can be tweaked to be made more secure and private, but it is still the worst options. 

**GrapheneOS:**

GrapheneOS, on top of removing all Google data collections, provides layers of security by locking the bootloader. This helps the Operation System identify any changes made to compromise the device. It only works on Google Pixel.

GrapheneOS can be installed through the web installer or a Linux computer.

1. Turn the device on without entering any Google accounts.
2. 