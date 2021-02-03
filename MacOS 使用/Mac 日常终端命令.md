
### 1. 立即关机

```shell
sudo halt
# 或者
sudo shutdown -h now
```

### 2. 定时关机

```shell
# 10分钟后关机
sudo shutdown -h +10

# 晚上8点关机
sudo shutdown -h 20:00
```

### 3. 立即重启

```shell
sudo reboot
# 或者
sudo shutdown -r now
```

### 4. 立即休眠

```shell
sudo shutdown -s now
```

### 5. 禁止开机音效

```shell
# 新机型尝试用该命令
sudo nvram StartupMute=%01

# 老机型尝试用下面两行命令
sudo nvram SystemAudioVolume=%80
# 或
sudo nvram SystemAudioVolume=%00
```









