# XiaomiCyberDog-k91
小米集团 XIaomi 铁蛋 CyberDog 仿生四足机器人 Bionic quadruped robot<br>

* [XiaomiCyberDog-k91](#xiaomicyberdog-k91)
* [Archived Xiaomi CyberDog GitLab Project](#archived-xiaomi-cyberdog-gitlab-project)
* [Active GitHub Project](#active-github-project)
   * [English Wiki](#english-wiki)
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
   * [Tina Linux (Neptune, 5C1C9C53)](#tina-linux-neptune-5c1c9c53)
   * [请输入help指令,获取系统帮助](#请输入help指令获取系统帮助)
   * [please enter the help command to get system help](#please-enter-the-help-command-to-get-system-help)

The smart robot dog has become a reality. With the new device by Xiaomi, everyone can have such an electronic pet.<br>
https://xiaomi-mi.com/appliances/yi-home-camera-3/<br>
https://www.mi.com/shop/buy/detail?product_id=14815<br>

# Archived Xiaomi CyberDog GitLab Project
https://www.ithome.com/0/602/684.htm<br>

# Active GitHub Project 
https://github.com/MiRoboticsLab/cyberdog_ros2<br>
https://github.com/MiRoboticsLab/cyberdog_motor_sdk<br>
https://github.com/MiRoboticsLab/cyberdog_tegra_kernel<br>

## English Wiki
https://github.com/MiRoboticsLab/cyberdog_ros2/blob/main/README_EN.md<br>

# OTA updates
https://github.com/MiRoboticsLab/cyberdog_ros2/discussions/133<br>

# CyberDog Engineering Exploration Edition forum
https://www.xiaomi.cn/board/27817860<br>

# Xiaomi Phone Home
This API is used to phone home for the Siri like audio "Home Assistant" functionality<br>
https://developers.xiaoai.mi.com/documents/Home?type=/api/doc/render_markdown/SkillAccess/skill/fulu/PrivacyInfo

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
...

The app leaves a GRPC port open for remote control 

```
gtowifi:/data/user/0/com.xiaomi.athena_remocons # netstat -anp  | grep athen                                                                       
tcp        0      0 10.0.0.64:46038         10.0.0.65:1935          ESTABLISHED 21131/com.xiaomi.athena_remocons
tcp6       0      0 :::8980                 :::*                    LISTEN      21131/com.xiaomi.athena_remocons
tcp6       0      0 ::ffff:10.0.0.64:8980   ::ffff:10.0.0.65:44306  ESTABLISHED 21131/com.xiaomi.athena_remocons

```

Files left in files/mmkv/spUtils seemingly ties the current dog mode and settings to a Xiaomi account session 


# Passwords and systems

Tegra: pi / 123
       root / 123

Tina: root / no pass

