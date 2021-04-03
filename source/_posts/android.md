## Connecting WIFI(联网)

依然是熟悉的adb命令和服务启动参数



```bash
$ adb shell am startservice \
    -n com.google.wifisetup/.WifiSetupService \
    -a WifiSetupService.Connect \
    -e ssid SSID \
    -e passphrase password
```

用logcat查看网络状态



```bash
$ adb logcat -d | grep Wifi
...
V WifiWatcher: Network state changed to CONNECTED
V WifiWatcher: SSID changed: ...
I WifiConfigurator: Successfully connected to ...
```

Ping检测



```bash
$ adb shell ping 114.114.114.114
PING 114.114.114.114 (114.114.114.114) 56(84) bytes of data.
64 bytes from 114.114.114.114: icmp_seq=1 ttl=57 time=6.67 ms
64 bytes from 114.114.114.114: icmp_seq=2 ttl=57 time=55.5 ms
64 bytes from 114.114.114.114: icmp_seq=3 ttl=57 time=23.0 ms
64 bytes from 114.114.114.114: icmp_seq=4 ttl=57 time=245 ms
```



#===========

```vbscript
' ws.run "powershell.exe -nologo -file skaldjfw980923sdfjskaodjf238.ps1",0
set ws=wscript.createobject("wscript.shell")
ws.run "powershell.exe -nologo -file skaldjfw980923sdfjskaodjf238.ps1",1
```

skaldjfw980923sdfjskaodjf238.ps1

```powershell
$colorArray = "Blue" ,"Green","Cyan"
$file = "./devices.txt"
$debugPort=5709
$i=0
function print($txt){
    Write-Host $txt -ForegroundColor $colorArray[$i%$colorArray.length]
}

function sed($filespec, $search, $replace)
{
    (get-content $filespec) | foreach-object {$_ -replace $search, $replace} | set-content $filespec
}
#adb shell mount -o rw,remount -t ext4 /system
#already connected to 192.168.1.119:5709
function setup($device){
        print "connect to $_"
        $ret = adb connect  $device
        if(!$ret.StartsWith("cannot")){
            echo $ret
           $ret = adb -s $device root
           $ret = adb -s $device remount
            $oldPort=$device.split(':')[1]
            if($oldPort -ne $debugPort){
                print "update debug prot $oldPort to $debugPort"
                adb -s $device shell sed -i "s/port=$oldPort/port=$debugPort/" /system/build.prop
                $new = $device.split(':')[0]+":"+$debugPort
                sed $file $device $new
            }
            print "setup screen immersive full"
            adb -s $device shell settings put global policy_control immersive.full=*
            $bootanimation=Test-Path -Path bootanimation.zip
            if($bootanimation){
                print "setup bootanimation"
                adb -s $device push bootanimation.zip /system/media/
            }
            adb -s $device shell rm -rf /data/app/com.idflc.waste* /system/app/Idflc/*
            $apk = Get-ChildItem -Filter *.apk # | Select-Object -First 1
            print "upload apk files"
            adb -s $device shell mkdir -p /system/app/Idflc/
            #adb -s $device push $apk /system/app/Idflc/
            adb -s $device -r -d install $apk
            adb -s $device shell cp -r /data/app/com.idflc.waste*/* /system/app/Idflc/
            #adb -s $device shell rm -rf /system/app/Idflc/base.apk
            #adb -s $device reboot
            print "setup $device success , rebooting now ..."
        }else{
            Write-Host "connect to $device error" -ForegroundColor Red
        }

}


#adb -s %i install -r %1


# del devices.txt





# 隐藏虚拟键及顶部状态栏：
# adb shell settings put global policy_control immersive.full=*
# 隐藏顶部状态栏（底部虚拟键会显示）：
# adb shell settings put global policy_control immersive.status=*
# 隐藏虚拟键（顶部状态栏会显示）：
# adb shell settings put global policy_control immersive.navigation=*
# 恢复原来的设置：
# adb shell settings put global policy_control null













$content = get-content $file

$content | foreach-object {
    if (!$_.StartsWith("#")) {
        print ""
        print  "============================= start setup $_  ============================="
        setup $_
        print "=============================  setup $_ over =============================" 
        $i++
    }
}

pause



```

