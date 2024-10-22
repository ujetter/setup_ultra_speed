# Setup Ultra Speed
Contains tipps how to enhance the speed of my Asus Zenbook 14 UX3405MA with Intel Core Ultra 7 155H

It happens that I have aquired a Asus Zenbook 15 UX3405MA. After that I found out that the setting of Windows and the drivers are not as good as they are.

This page contains some tweaks and in the respective issues that I have with that zenbook and how I resolved them.
Especially speeding up the boot time and how to reduce the lag of Citrix VDI connections are hacks that iprove the performance dramatically and hence are recomended to a wider audience.

I have written this respository, because updates of the citrix software will result in deletion of the registry keys and revert the settings - so I need a space where to run them again from.

# Speed up windows startup 
In the original settings, windows startup is delayed.
The issue is, that Windows 11 is starting up all programs in serial order, The explorer waits until there is some idle time and after that starts the autostart programs one after the other.

As the Processor has some 24 cores, you only need to update the following registry keys to run the startup programs in parallel and decrease dramatically the startup time.
```
[HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\Serialize]
"WaitForIdleState"=dword:00000000
"StartupDelayInMSec"=dword:00000000
```

You can use this file as registry file: [windows_startup_serialize.reg]

# Speed up Citrix VDI performance
The issue with this is, that Citrix tries to use the hardware acceleration of the build-in Intel ARC graphic card. However, the actual Intel driver has the known issue to slow down in the described manner.

Intel is referring to the issue here: [https://www.intel.com/content/www/us/en/support/articles/000097751/graphics.html] referring to the grafics driver driver 31.0.101.5594. At the moment (23-Jul-2024) autoupdate updated to: Driver 31.0.101.5525 - which is not quite there....

You can advice Citrix to fall back to not use the graphics acceleration, resulting in a smoother result. However, Cirtix updates revert back to the old settings, so you need to apply the registry change every time, citrix receiver was updated:
```
[HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Citrix\ICA Client\Engine\Configuration\Advanced\Modules\GfxRender]
"UseD3DHybrid"=dword:00000000
```

You can use this file as registry file: [citrix_vdi_gfxRender.reg]


# Reconnect to VDI sessions
HKEY_LOCAL_MACHINE\SOFTWARE\Citrix\Reconnect "FastReconnect"=dword:00000000 
