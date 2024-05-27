# HamDexMFAScreenlock

- Locks the Windows desktop, using a dynamic verification code that changes every minute, provided by an MFA authenticator (e.g., Google Authenticator) for unlocking, to prevent unauthorized remote desktop access.
- While it can also be used to protect local computers, due to the possibility of physical access, it can only serve as an auxiliary login protection on local machines.

## Installation

### Dependencies
- `Windows ≥ 7 SP1 (x86 and x64)`
- `Windows Server ≥ 2008 R2 SP1 (x64)`
- `.NET Framework ≥ 4.6.2`
  - Download: [https://support.microsoft.com/zh-cn/help/3151800/the-net-framework-4-6-2-offline-installer-for-windows](https://support.microsoft.com/zh-cn/help/3151800/the-net-framework-4-6-2-offline-installer-for-windows)

### If you have an older version installed:
- It is recommended to clear user settings in the old version (this will also unbind the authenticator) before uninstalling the old version. (This is optional for upgrades but mandatory for downgrades)
- During the upgrade process, user settings and binding status may be automatically cleared.

### Installation Steps
1. Install `install.exe`.
   - If you do not actively configure file permissions, it is recommended to install it in the user folder to prevent password files from being accessed by other users.
2. Run the shortcut created on the desktop or `MFAScreenLockApp.exe` in the installation directory.

## Configuration

1. Run the program directly. If no authenticator has been bound, a setup screen will appear. Click the bind button.
2. Ensure the displayed date and time are fully synchronized with your phone.
3. Use an MFA app to scan the QR code.
   - You can also manually enter the key displayed in the upper right into the authenticator.
   - **Note:** If this computer's name, user domain, and username are the same as another computer, the login information on the other computer will be overwritten! If this occurs, please change your computer name and restart before scanning the code.
4. Enter the code displayed on the authenticator and click to complete the binding.
5. Note down the recovery code displayed on the screen.
   - Clicking "Yes" will copy the recovery code to the clipboard; you can use this recovery code if the dynamic password cannot be used.
   - Clicking "No" will not generate a recovery code. This is more secure but will prevent login if your phone is damaged, lost, the authentication app is uninstalled, or its data is lost.
6. The binding information will display the computer name, user domain, and username.
   - **Note:** The computer name, user domain, and username are part of the binding. If you change any of these, you will be unable to log in. To make changes, unbind the authenticator first.
7. Close the setup window or proceed with "Lock Settings" and "Personalization" appearance settings.
8. Close all program windows and right-click the system tray icon to exit the program. You will need to verify your password once.
9. The next time the program starts, it will enter a locked state.

## Automatic Locking

### Method 1 (recommended)
1. Right-click the taskbar icon and select "Auto Lock Settings".
   - You can set it to run automatic locking at Windows startup.
   - You can set it to automatically lock after a certain number of minutes.
     - You can see the auto-lock timer below. If it does not work properly, use method two to configure automatic locking.
     - The settings window will not trigger auto-lock when open.

### Method 2
1. Set this program to start on boot in Task Scheduler.
   - You can add the `-e` parameter in the startup options. The program will exit immediately after unlocking (otherwise, the icon will remain in the system tray), allowing Task Scheduler's idle time feature to handle automatic locking.
   - Example: `MFAScreenLockApp.exe -e`

## Usage

- The next time you run the program, the computer screen will be locked. Enter the dynamic password displayed in the authenticator or the recovery code, and press Enter.
  - If correct, it will unlock directly.
  - If incorrect, it will lock for a few seconds (default is 3 seconds) before allowing another attempt.
  - The unlock screen's background image will be your primary display's desktop wallpaper and will automatically adjust (fit) the screen. If not set, the desktop wallpaper will be black.
    - Text color will automatically switch between black and white based on the desktop wallpaper.
    - Other non-primary displays will show a blank desktop wallpaper. If not set, the desktop wallpaper will be black.
    - You can change these wallpaper, color, and font settings in "Personalization".
- After unlocking, an icon will remain in the system tray. Right-clicking it will bring up a menu to lock immediately, modify bindings, change appearance, and exit.
  - These menu items will re-enter the password input state to verify identity before executing the selected command.
  - If there is no icon in the system tray, the `-e` parameter might have been added to the startup options or an error occurred.

## Unbinding

### Method 1
1. Without the `-e` parameter, enter the password or recovery code to unlock.
2. Right-click the system tray icon, select "Binding Management", choose "Unbind", and then exit the software.

### Method 2
1. With the program completely exited, start it with the `-r <recovery code>` parameter.
2. Confirm to directly clear all software settings, including the password. The software will automatically exit.
   - Example: `MFAScreenLockApp.exe -r Y8K2V-5J2ZL-F6QC8-ERFJ4-4HME8-POL50`

## Troubleshooting

### Cannot run, prompts missing .NET Framework
- Install the .NET Framework runtime.

### Dynamic password always shows as incorrect
- Enter your recovery code in the password box.
- Go to binding settings and revalidate your recovery code.
- Clear all user settings.

### Cannot auto-lock on boot
- Disable startup at boot in program settings.
- Fully exit the program, then "Run as Administrator".
- Enable startup at boot in program settings.
- If still not working: use "Task Scheduler" to set startup at boot or move the program shortcut to the "Startup" folder.

### Program error
- [Feedback](issues)

