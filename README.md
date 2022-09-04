# XiaomiCyberDog-k91
小米集团 XIaomi 铁蛋 CyberDog 仿生四足机器人 Bionic quadruped robot<br>

* [Archived Xiaomi CyberDog GitLab Project](#archived-xiaomi-cyberdog-gitlab-project)
* [Active GitHub Project](#active-github-project)
   * [English Wiki](#english-wiki)
   * [Discussion](#discussion)
* [OTA updates](#ota-updates)
* [CyberDog Engineering Exploration Edition forum](#cyberdog-engineering-exploration-edition-forum)
* [Xiaomi Phone Home](#xiaomi-phone-home)
   * [Home Assistant voice commands](#home-assistant-voice-commands)
* [STP file for accessory mounting](#stp-file-for-accessory-mounting)
* [GRPC remote control](#grpc-remote-control)
* [Wav files for dog sound effects](#wav-files-for-dog-sound-effects)
* [Bluetooth remote support](#bluetooth-remote-support)
   * [Usage](#usage)
   * [Services on dog for control](#services-on-dog-for-control)
* [Android App](#android-app)
* [Passwords and systems](#passwords-and-systems)
* [USB Download port](#usb-download-port)
* [LED control](#led-control)
* [Remote ROS connectivity.](#remote-ros-connectivity)
   * [Remote listener for ROS2](#remote-listener-for-ros2)
   * [Rviz + remote control plugin](#rviz--remote-control-plugin)

The smart robot dog has become a reality. With the new device by Xiaomi, everyone can have such an electronic pet.<br>
https://xiaomi-mi.com/appliances/yi-home-camera-3/<br>
https://www.mi.com/shop/buy/detail?product_id=14815<br>

# Archived Xiaomi CyberDog GitLab Project
https://partner-gitlab.mioffice.cn/groups/cyberdog/-/archived<br>

Clone all the repos with the following commands
```
git clone https://partner-gitlab.mioffice.cn/cyberdog/athena_tina_sdk.git
git clone https://partner-gitlab.mioffice.cn/cyberdog/athena_drivers_gd_bin.git
git clone https://partner-gitlab.mioffice.cn/cyberdog/athena_locomotion_bin.git
git clone https://partner-gitlab.mioffice.cn/cyberdog/athena_cyberdog.git
git clone https://partner-gitlab.mioffice.cn/cyberdog/athena_kernel.git
git clone https://partner-gitlab.mioffice.cn/cyberdog/athena_vision.git
git clone https://partner-gitlab.mioffice.cn/cyberdog/athena_assistant.git
git clone https://partner-gitlab.mioffice.cn/cyberdog/athena_automation.git
git clone https://partner-gitlab.mioffice.cn/cyberdog/athena_repos.git
git clone https://partner-gitlab.mioffice.cn/cyberdog/athena_lcm_type.git
git clone https://partner-gitlab.mioffice.cn/cyberdog/athena_drivers_st.git
git clone https://partner-gitlab.mioffice.cn/cyberdog/build_extra.git
```
# Active GitHub Project 
https://github.com/MiRoboticsLab/cyberdog_ros2<br>
https://github.com/MiRoboticsLab/cyberdog_motor_sdk<br>
https://github.com/MiRoboticsLab/cyberdog_tegra_kernel<br>
https://github.com/MiRoboticsLab/cyberdog_ros2/wiki<br>

## English Wiki
https://github.com/MiRoboticsLab/cyberdog_ros2/blob/main/README_EN.md<br>
https://github.com/MiRoboticsLab/cyberdog_ros2/wiki/铁蛋ROS-2软件架构-%7C-ROS-2-Software-Architecture-of-CyberDog#english<br>

## Discussion
Telegram group in Announcements: https://telegram.me/cyberdog_mi<br>

# OTA updates
https://github.com/MiRoboticsLab/cyberdog_ros2/discussions<br>
https://github.com/MiRoboticsLab/cyberdog_ros2/discussions/133<br>

# CyberDog Engineering Exploration Edition forum
https://www.xiaomi.cn/board/27817860<br>

# Xiaomi Phone Home
You can occasionally see the athena_audio process in the SYN_SENT state connecting to 47.241.92.41 on https port. 

This appears to be connection to access.speech.ai.xiaomi.com 

```
mi@lubuntu:/opt/ros2/cyberdog/data$ cat audio_debug.toml 
title = "audio_debug_config"
config_audio_force_enable = 0
config_audio_dump_raw = 0
config_audio_dump_asr = 0
config_audio_silence = 0
config_audio_testtoken = 0
testtoken_access = <snip>
testtoken_refresh = <snip>
testtoken_deviceid = <snip>
```

The token further supports this
```
$ cat token.json  | jq
{
  "censored_DO-TOKEN-V1_access.speech.ai.xiaomi.com-WIFI-mioffice-5g": 
"{\"dns\":[\"47.241.176.221:80\",\"47.241.176.221:443\"],\"expire_at\":1660787428}",
  "censored_DO-TOKEN-V1_aes_key_info": "{\"aes_key\":\"xxxxxx\"}",
  "censored_DO-TOKEN-V1_rsa_key_info": "{\"key_id\":\"xxxxx\",\"public_key\":\"-----BEGIN PUBLIC KEY-----\\ncensored
PUBLIC KEY-----\\n\",\"expire_at\":1672815028000}",
  "censored_DO-TOKEN-V1_token_info": 
"{\"access_token\":\"default\",\"expire_at\":1662774605,\"refresh_at\":1661753376,\"refresh_token\":\"default\"}",
  "censored_DO-TOKEN-V1_updated_at": "1660182605",
  "auto_ping_interval": null,
  "cloud_control_link_wss": "false",
  "cloud_control_version": "72",
  "next_cloud_control_interval": "120",
  "next_cloud_control_time": "1660186210",
  "next_hijack_detect_at": "1660441565",
  "wss_expire_at": null
}
```
 
This API is used to phone home for the Siri like audio "Home Assistant" functionality<br>
https://developers.xiaoai.mi.com/documents/Home?type=/api/doc/render_markdown/SkillAccess/skill/fulu/PrivacyInfo

The SDK in play is:  https://cdn.cnbj2m.fds.api.mi-img.com/cyberdog-package/packages/xiaoai_sdk_1.0.0.8.2.tar.xz


## Home Assistant voice commands
Uses Cuda Deep Neural Network cuDNN<br>
https://developer.nvidia.com/cudnn<br>
https://docs.nvidia.com/deeplearning/nemo/user-guide/docs/en/stable/asr/asr_language_modeling.html (ASR)<br>
https://docs.nvidia.com/deeplearning/nemo/user-guide/docs/en/stable/asr/speech_classification/models.html (VAD)<br>

https://docs.nvidia.com/deeplearning/nemo/user-guide/docs/en/stable/core/core.html<br>
NeMo comes with many pretrained models for each of our collections: ASR, NLP, and TTS.<br>

```
$ ls /opt/ros2/cyberdog/ai_conf/
dnn_vad_model  dnn_we_model  hot_query_robotcontroller.txt  k91.fst  model.bin  vad_config.json  xaudio_engine.conf  xaudio_vpm_ut

```

The audio_assistant source gives more clues about how things work.<br> 
https://partner-gitlab.mioffice.cn/cyberdog/athena_assistant/-/tree/devel/audio_assistant/src<br>

```
./audio_assistant/src/ai_asr_native.cpp:#define    ASR_MODEL_BIN  "/opt/ros2/cyberdog/ai_conf/model.bin"
./audio_assistant/src/ai_keyword.cpp:#define VPM_CONFIG_FILE_PATH "/opt/ros2/cyberdog/ai_conf/xaudio_engine.conf"
./audio_assistant/src/ai_asr_native.cpp:  std::string name = "k91";
```

The VPM config file also gives some hints<br>
```
mi@lubuntu:/opt/ros2/cyberdog/ai_conf$ cat xaudio_engine.conf
[engine opt]
version = K91_v1.0.0_8-20210810;
[wakeup opt]
model_path = /opt/ros2/cyberdog/ai_conf/wakeup_model.bin;
version = kws_tdtd_model_6.0-20210731;
[vad opt]
model_path = /opt/ros2/cyberdog/ai_conf/;
version = model-hires-0605-210709;
timeout_num = 600;
[dnn vad opt]
dnn_vad_model_path = /opt/ros2/cyberdog/ai_conf/dnn_vad_model;
dnn_we_model_path = /opt/ros2/cyberdog/ai_conf/dnn_we_model;
```

hot_query_robotcontroller.txt contains the gramatical structure for the recognized voice commands.<br>

If you split each line in the file by ":" you can get an idea of the voice commands. They line up with what is in the manual

```
(^(旺财旺财|请|麻烦|给我)?(站)?起来(啊)?$|^(给我)?起立$)	旺财，旺财，站起来	robotControl	"resp"

^(请|旺财|麻烦|给我|表演一下)?趴下去(旺财)?$	旺财，旺财，趴下去	robotControl	"resp"

^(旺财|麻烦|给我)?(过来)?过来$	旺财，旺财，过来过来	robotControl	"resp"

^(旺财|麻烦|请|给我)?后退一步$	旺财，旺财，后退一步	robotControl	"resp"

^(旺财来|麻烦|请|给我|表扬一下)?原地转圈$	旺财，旺财，原地转圈	robotControl	"resp"

(^(旺财来|麻烦|给我|表演一下|请)?(握个手|击个掌)$|^击个掌吧击个掌$)	旺财，旺财，握个手/击个掌	robotControl	"resp"

^(旺财来|麻烦|请|给我|表演一下)?后空翻$	旺财，旺财，后空翻	robotControl	"resp"

^(旺财来)?(麻烦|请|给我|表演一下)?跳个舞(吧|呗)?$	旺财，旺财，跳个舞	robotControl	"resp"
```

The rough English translation is below:<br>

```
(^(Wang Cai Wang Cai|Please|Trouble|Give me)?(Stand)?Get up(Ah)?$|^(Give me)?Stand up $) Prosperity, Prosperity, stand up robotControl "resp"

^(Please|prosperity|trouble|give me|perform)? Get down (prosperity)?$ Prosperity, prosperous, get down robotControl "resp"

^(Wangcai|Trouble|Give me)?(Come over)?Come over $ Wangcai,Wangcai, come here robotControl "resp"

^(Wangcai|trouble|please|give me)? One step back $ Wangcai, Wangcai, one step back robotControl "resp"

^(Wangcailai|trouble|please|give me|praise)? Circling in place $ Prosperity, prosperity, circling robotControl "resp"

(^(Prosperity Come|Trouble|Give me|Show me|Please)?(Shake a hand|Clap)$|^Clap, give a slap$) Prosperity, prosperity, shake hands/hit A palm robotControl "resp"

^(Wangcailai|trouble|please|give me|show it)? Backflip $ Prosperity, Prosperity, Backflip robotControl "resp"

^(Prosperity is coming)?(Trouble|please|show me|perform)? Dance (bar|chan)?$ Prosperity, prosperity, dance robotControl "resp"
```

"Wang Cai" is supposed to be "Iron Egg" which you can tell from the "CyberDog voice commands" section of the manual. 

Wake word: "iron egg iron egg"
instruction: "take a step back", "stand up", "get down", "dance", "shake hands", "turn around"

The "backflip" instruction seems to be disabled, even though shown in marketing.<br>
https://www.youtube.com/watch?v=Jj95byxQCeU 

This non CyberDog specific discussion on adding custom Xiaomi wakeup words has some overlap with the above info<br>
https://bbs.hassbian.com/archiver/?tid-5649.html&page=1


# STP file for accessory mounting
https://cdn.cnbj2m.fds.api.mi-img.com/cyberdog-package/packages/doc_materials/cyber_dog_body.stp<br>

# GRPC remote control 
https://github.com/Karlsx/CyberDog_Ctrl<br>

# Wav files for dog sound effects
https://cdn.cnbj2m.fds.api.mi-img.com/cyberdog-package/packages/CyberDog_wavs.tar.xz<br>

# Bluetooth remote support

The CyberDog appears to use a custom Bluetooth T8S by RadioLink. It includes a cell phone holder as the main difference between the T8S(BT)<br>
http://www.radiolink.com.cn/docc/index.html<br>
http://radiolink.com.cn/doce/product-detail-167.html<br> 
The "FPV Monitor Holder" is also included<br>
https://www.aliexpress.com/item/2255800731743329.html<br>
https://www.amazon.com/ATA-HOBBY-Transmitter-Controller-Accessories/dp/B09Z5F798W<br>
https://www.amazon.com/Radiolink-Controller-Receiver-Transmitter-Gamepad/dp/B07WR9Y1HG<br>
If you buy your own, you'll need to center the throttle stick<br>
https://www.youtube.com/watch?v=4_FrGGzbxiE<br>

## Usage
Open the App... go to settings, go to Remote Control Connection. <br>

```
action gait, send combination key from CH5 and CH7, when Key CH6 pressed
gait range: 1~4, 7~11, when ch6 pressed
```
https://github.com/MiRoboticsLab/cyberdog_ros2/blob/main/cyberdog_interaction/cyberdog_wireless/bluetooth/bluetooth/remote.py

```
-------------------------------------
Roller - Ch8 (height)
-------------------------------------
Left - Ch7  | Right - Ch5  | Mode
-------------------------------------
Center      | Center       | Trot
Center      | Down         | Slow Trot
Center      | Up           | Fly Trot
Down        | Center       | Kneel
Down        | Down         | Down
Down        | Up           | Recover 
Up          | Center       | Bound
Up          | Down         | Recover
Up          | Up           | Show Atti
--------------------------------------
```

## Services on dog for control 

The services are enabled by default:<br>
```
/opt/ros2/cyberdog/lib/python3.6/site-packages/bluetooth/gattserver.py:enableAutoRemoteControl: bool = True  # for bluetooth remote control handle auto connect<br>
```

The following binaries support the remote control<br>
```
mi@lubuntu:/opt$ grep btRemoteCommand . -r
Binary file ./ros2/cyberdog/lib/athena_cyberdog_app/app_server matches
Binary file ./ros2/cyberdog/lib/libapp_server_core.a matches
Binary file ./ros2/cyberdog/lib/python3.6/site-packages/bluetooth/__pycache__/remote.cpython-36.pyc matches
```

https://github.com/MiRoboticsLab/cyberdog_ros2/blob/011025c2ee43a144f32f42440b631019124a731b/cyberdog_interaction/cyberdog_wireless/bluetooth/bluetooth/remote.py
https://github.com/MiRoboticsLab/cyberdog_ros2/blob/011025c2ee43a144f32f42440b631019124a731b/cyberdog_interaction/cyberdog_wireless/bluetooth/bluetooth/gattserver.py#L36

Launched by ROS2 gattserver
```
root@lubuntu:/home/mi# ps -ax | grep gat
 4628 ?        Ss     0:01 /usr/bin/python3 /opt/ros2/foxy/bin/ros2 run bluetooth gattserver
 5755 ?        Sl     0:07 /usr/bin/python3 /opt/ros2/cyberdog/lib/bluetooth/gattserver
```

When the service starts the following log entries are created. 
```
2022-07-30 12:44:01,744 - gattserver.py[line:1006] - INFO: main: init
2022-07-31 06:39:29,957 - gattserver.py[line:253] - INFO: add service: <dbus._dbus.SystemBus (system) at 0x7f9dde60a0>
2022-07-31 06:39:29,958 - gattserver.py[line:263] - INFO: add_service: <bluetooth.gattserver.RobotService at /org/bluez/example/service0 at 0x7f9dde2c50>
2022-07-31 06:39:31,537 - gattserver.py[line:232] - INFO: get psn is:b'P21511906GR00152ZM\n'
2022-07-31 06:39:31,541 - gattserver.py[line:1046] - DEBUG: main: set bleStatus BLE_STATUS_NONE -> BLE_STATUS_ADV
2022-07-31 06:39:31,542 - gattserver.py[line:930] - INFO: RegADV: RegisterAdvertisement
2022-07-31 06:39:31,543 - gattserver.py[line:942] - INFO: RegATT: RegisterApplication
2022-07-31 06:39:31,543 - gattserver.py[line:258] - INFO: get_path: <bluetooth.gattserver.RobotApplication at / at 0x7f9dde2c88>
2022-07-31 06:39:31,548 - gattserver.py[line:1050] - DEBUG: main init and send audio cmd BOOT_COMPLETE
2022-07-31 06:39:31,564 - gattserver.py[line:1053] - DEBUG: RegADV: BT enable adv, set LED_BT_ADV
2022-07-31 06:39:31,607 - gattserver.py[line:760] - INFO: register_app_cb: GATT application registered!!!
2022-07-31 06:39:31,609 - gattserver.py[line:204] - INFO: GetAll
2022-07-31 06:39:31,609 - gattserver.py[line:207] - INFO: returning props
2022-07-31 06:39:31,618 - gattserver.py[line:771] - INFO: register_ad_cb: Advertisement registered!!!


2022-07-30 12:08:34,895 - remote.py[line:863] - INFO: startAutoConnect: get autoConThreadRunning is 0
2022-07-30 12:08:34,899 - remote.py[line:872] - INFO: startAutoConnect: waiting 10 seconds for start
2022-07-30 12:08:34,902 - gattserver.py[line:978] - DEBUG: disable ADV and ATT, led send LED_BT_ADV OFF
2022-07-30 12:08:44,897 - remote.py[line:914] - INFO: autoConnection: THREAD WAITING-> ENTER
2022-07-30 12:08:44,898 - remote.py[line:920] - WARNING: auto device is null
2022-07-30 12:08:44,898 - remote.py[line:1000] - DEBUG: autoConnection: autoConThreadRunning is 0 and quit
```

# Android App

Download link: http://cdn.cnbj1.fds.api.mi-img.com/ota-packages/apk/cyberdog_app.apk<br>

```
1|gtowifi:/ # ps -ef | grep athena                                                                                                                                                                   
u0_a131      20839   433 7 18:18:44 ?     00:00:02 com.xiaomi.athena_remocons

gtowifi:/ # logcat --pid 21131                                                                                                                                                                       
--------- beginning of main
07-30 18:20:09.481 21131 21131 E athena_remocon: Not starting debugger since process cannot load the jdwp agent.
```

The app leaves a GRPC port open for remote control 

```
gtowifi:/data/user/0/com.xiaomi.athena_remocons # netstat -anp  | grep athen                                                                       
tcp        0      0 10.0.0.64:46038         10.0.0.65:1935          ESTABLISHED 21131/com.xiaomi.athena_remocons
tcp6       0      0 :::8980                 :::*                    LISTEN      21131/com.xiaomi.athena_remocons
tcp6       0      0 ::ffff:10.0.0.64:8980   ::ffff:10.0.0.65:44306  ESTABLISHED 21131/com.xiaomi.athena_remocons

```

Files left in /data/data/com.xiaomi.athena_remocons/files/mmkv/spUtils seemingly ties the current dog mode and settings to a Xiaomi account session 
/data/data/com.xiaomi.athena_remocons/files/mmkv/<serialnumber> seems to tie the serial to the dogs settings, including the "room map"  

The app forces the end user to login before being able to control the dog. The needs to be neutered.<br>
You are also forced to read the manual, privacy policy, legal statement, etc. 

# Passwords and systems

Tegra: pi / 123
       root / 123

Tina: root / no pass

# USB Download port

L4T provides a USB bridge on the Jetson inside the dog. You can use it for IP communications, or serial <br>
https://forums.developer.nvidia.com/t/ip-address-192-168-55-1/184273/4

```
$ ls -al /dev/ttyACM0 
crw-rw-rw- 1 root dialout 166, 0 Aug 16 01:20 /dev/ttyACM0
$ screen /dev/ttyACM0 115200
[screen is terminating]
$ sudo ifconfig usb0 192.168.55.100
$ ping 192.168.55.1
PING 192.168.55.1 (192.168.55.1) 56(84) bytes of data.
64 bytes from 192.168.55.1: icmp_seq=1 ttl=64 time=0.350 ms
64 bytes from 192.168.55.1: icmp_seq=2 ttl=64 time=0.321 ms
^C
--- 192.168.55.1 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1019ms
rtt min/avg/max/mdev = 0.321/0.335/0.350/0.014 ms
```

Alternately if you plug a USB adapter into the extension port, you need to 
edit th e/etc/netplan/01-netcfg.yaml file to get DHCP

```
mi@lubuntu:/etc/netplan$ sudo pico 01-netcfg.yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    eth1:
      dhcp4: true
      optional: true

mi@lubuntu:/etc/netplan$ sudo netplan try 
Do you want to keep these settings?


Press ENTER before the timeout to accept the new configuration


Changes will revert in 118 seconds
Configuration accepted.
mi@lubuntu:/etc/netplan$ ifconfig eth1
eth1: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.1.147  netmask 255.255.255.0  broadcast 192.168.1.255
        inet6 fe80::205:1bff:fec1:c8be  prefixlen 64  scopeid 0x20<link>
        ether 00:05:1b:c1:c8:be  txqueuelen 1000  (Ethernet)
        RX packets 93  bytes 5476 (5.4 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 87  bytes 9430 (9.4 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```


# LED control

```
import rclcomm
import rclpy

https://raw.githubusercontent.com/MiRoboticsLab/cyberdog_ros2/main/cyberdog_interaction/cyberdog_led/README.md

rclpy.init(args=None)
ledClient = rclcomm.LedClientAsync()

if ledClient.check_ready():
	if ledClient.get_last_always():
		ledClient.send_request(ledClient.LED_BT_ADV,ledClient.TYPE_FUNCTION,ledClient.COMMAND_OFF)  # clean adv 
led
	else:
	#	ledClient.send_request(ledClient.LED_WIFI_CONNECT_SUCCESS,ledClient.TYPE_FUNCTION, 1000000000)
		ledClient.send_request(25,ledClient.TYPE_ALARM, ledClient.COMMAND_ALWAYSON) # SkyBlue Breath == 25
```

# Backdoor wifi connections
```
mi@lubuntu:/etc/init.d$ nmcli connection show
NAME                UUID                                  TYPE      DEVICE 
DataPipe            defae6a4-944f-45b4-8d19-1a364b4f70e8  wifi      wlan0  
eth0                ee15f340-6d55-47ed-b794-39c9d63586c7  ethernet  eth0   
l4tbr0              d91c2d61-76a2-4ef1-9514-cd2d9695bbde  bridge    l4tbr0 
1234                f3ad1b24-3707-4c1a-b347-8327164b9bd8  wifi      --     
Dog2                9b9423fe-ce84-40ae-9757-9ffd15824299  wifi      --     
Dreame-test         0acd8a29-8c8c-4b9e-81cd-2d53404bf7e2  wifi      --     
Dreame-test 1       2a28484c-8323-4719-a888-4c8dc7db2e54  wifi      --     
Dreame-test 2       650c03e9-aef0-48be-b7de-1b22b863b105  wifi      --     
Dreame-test 3       39d2f37b-a38d-4cd6-9b3a-0b9249a7cec9  wifi      --     
Dreame-test 4       42370f0a-8270-42d4-8c5c-c93b26d7dc2a  wifi      --     
Dreame-test 5       14cd5bf9-6416-4744-80b1-a89693feea90  wifi      --     
Redmi               e7161e00-13d9-44f5-975b-312b53f80eec  wifi      --     
Wired connection 1  86d27518-92c1-3b91-9a7e-3b3b3cd31351  ethernet  --     
ddog1               2e3920ce-b3d3-4589-8dee-a7f18d4b6f60  wifi      --     
dog4                4fece0fb-dc5c-416f-a4c7-cde54368164c  wifi      --     
mi@lubuntu:/etc/init.d$ sudo grep -r '^psk=' /etc/NetworkManager/system-connections/
/etc/NetworkManager/system-connections/Dreame-test 2:psk=12345678
/etc/NetworkManager/system-connections/Redmi:psk=peterwu123
/etc/NetworkManager/system-connections/dog4:psk=12345678
/etc/NetworkManager/system-connections/DataPipe:psk=givemeinternet!now
/etc/NetworkManager/system-connections/Dreame-test:psk=12345678
/etc/NetworkManager/system-connections/Dreame-test 1:psk=12345678
/etc/NetworkManager/system-connections/Dog2:psk=123456789
/etc/NetworkManager/system-connections/Dreame-test 4:psk=12345678
/etc/NetworkManager/system-connections/1234:psk=123456789
/etc/NetworkManager/system-connections/ddog1:psk=123456789
/etc/NetworkManager/system-connections/Dreame-test 3:psk=12345678
/etc/NetworkManager/system-connections/Dreame-test 5:psk=12345678
```

# Remote ROS connectivity.

Install Debinan binary version of foxy
https://docs.ros.org/en/rolling/Installation/Alternatives/Ubuntu-Install-Binary.html
```
$ sudo apt-get install libzmq3-dev
$ sudo apt install python3-colcon-common-extensions
$ sudo apt-get install ros-foxy-rmw-cyclonedds-cpp
$ source /opt/ros/foxy/setup.bash
```

The dog uses Cyclone DDS & Dos Domain 42

```
$ ps -ax | grep dds -i
23620 pts/2    Sl     0:01 /usr/bin/python3 /opt/ros2/foxy/bin/_ros2_daemon --rmw-implementation rmw_cyclonedds_cpp --ros-domain-id 42
```

## Remote listener for ROS2
The github wiki gives us more detail
https://github.com/MiRoboticsLab/cyberdog_ros2/wiki/CyberDog-DDS本地及多播设置

modify the config as follows:
```
root@lubuntu:/home/mi# grep -E 'CYCLONEDDS_URI|ROS_LOCALHOST_ONLY' /etc -r

/etc/systemd/system/cyberdog_automation.service:#Environment="ROS_LOCALHOST_ONLY=1"
/etc/systemd/system/cyberdog_automation.service:#Environment="CYCLONEDDS_URI=file:///etc/systemd/system/cyclonedds.xml"
/etc/systemd/system/cyberdog_ros2.service:#Environment="ROS_LOCALHOST_ONLY=1"
/etc/systemd/system/cyberdog_ros2.service:#Environment="CYCLONEDDS_URI=file:///etc/systemd/system/cyclonedds.xml"
/etc/systemd/system/bluetooth_ros2.service:#Environment="ROS_LOCALHOST_ONLY=1"
/etc/systemd/system/bluetooth_ros2.service:#Environment="CYCLONEDDS_URI=file:///etc/systemd/system/cyclonedds.xml"
/etc/mi/mi_config:#export ROS_LOCALHOST_ONLY=1
/etc/mi/mi_config:#export CYCLONEDDS_URI=file:///etc/systemd/system/cyclonedds.xml

root@lubuntu:/home/mi# grep -E 'NetworkInterfaceAddress|AllowMulticast' /etc/systemd/system/cyclonedds.xml
            <NetworkInterfaceAddress>wlan0</NetworkInterfaceAddress>
            <AllowMulticast>true</AllowMulticast>
```


## Rviz + remote control plugin
https://www.bilibili.com/video/BV1ML4y1b78d
```
$ mkdir ~/cyberdog_ws/src -p
$ cd ~/cyberdog_ws/src/
$ git clone https://github.com/linzhibo/Cyberdog_rviz2_plugin.git
```
Set dog namespace in ./Cyberdog_rviz2_plugin/src/mission_panel.cpp
```
$ cd ~/cyberdog_ws/src/
$ git clone https://github.com/MiRoboticsLab/cyberdog_ros2.git
$ cd ~/cyberdog_ws/
$ colcon build
$ source install/setup.sh
```
To talk to the dog remotely several conditions are necessary on the client PC
```
export ROS_DOMAIN_ID=42
export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp
```

You'll know it works when you can enable the cam from the remote PC
```
kfinisterre@dev0:~$ ros2 service call /mi1036871/camera/enable std_srvs/SetBool "{data : True}"
waiting for service to become available...
requester: making request: std_srvs.srv.SetBool_Request(data=True)

response:
std_srvs.srv.SetBool_Response(success=False, message='Device is already ON.')

```

Next launch rviz
```
$ rviz2
```
