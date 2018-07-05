# Linux 用命令查看 CPU 相关信息

```
// 获得CPU ID
dmidecode -t 4 | grep ID |sort -u |awk -F': ' '{print $2}'

// 获得磁盘ID
fdisk -l |grep "Disk identifier" |awk {'print $3'}

查看CPU信息
cat /proc/cpuinfo

显示当前硬件信息
sudo lshw

获取CPU序列号或者主板序列号
#CPU ID
sudo dmidecode -t 4 | grep ID

#Serial Number
sudo dmidecode  | grep  Serial

#CPU
sudo dmidecode -t 4

#BIOS
sudo dmidecode -t 0

#主板：
sudo dmidecode -t 2

#OEM:
sudo dmidecode -t 11

显示当前内存大小
free -m |grep "Mem" | awk '{print $2}'

查看硬盘温度
sudo apt-get install hddtemp
sudo hddtemp /dev/sda
```

copy from http://sheng.iteye.com/blog/2155138