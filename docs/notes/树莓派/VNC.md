# 修改VNC的分辨率

```bash
cd /boot/
sudo nano config.txt
```

## 修改配置文件

```bash
# 树莓派4B必须把这行注释掉
dtoverlay=vc4-fkms-v3d –> must be #dtoverlay=vc4-fkms-v3d
framebuffer_width=1920
framebuffer_height=1080
#hdmi_force_hotplug=1 –> must be hdmi_force_hotplug=1
```

## 重启

```bash
sudo reboot
```
