# RockyLinux 9.8 minimal install
nmtui
dnf update
dnf install epel-release -y
dnf update
curl https://raw.githubusercontent.com/NethServer/ns8-core/ns8-stable/core/install.sh | bash

default 
account:  root or admin
password: Nethesis,1234

# 準備 win11 要 pro 版或以上，預先安裝 RSAT 相關程式
# windows update 要啟用
Get-WindowsCapability -Name RSAT* -Online | Add-WindowsCapability -Online

# 1. 檢視 samba1 內部真正的容器狀態
runagent -m samba1 podman ps -a

# 2. 直接一鍵將網域功能等級提高到 2016
runagent -m samba1 samba-tool domain level raise --domain-level=2016

# 3. 直接一鍵將樹系功能等級提高到 2016
runagent -m samba1 samba-tool domain level raise --forest-level=2016


[root@localhost ~]# runagent -m samba1 podman exec -it samba-dc /bin/sh -c "echo 'ad dc functional level = 2016' >> /etc/samba/smb.conf"
[root@localhost ~]# systemctl restart srv-samba1
Failed to restart srv-samba1.service: Unit srv-samba1.service not found.
[root@localhost ~]# runagent -m samba1 systemctl --user restart samba-dc
[root@localhost ~]#



domain level raise --domain-level=2012_R2
runagent -m samba1 podman exec -it samba-dc


ls -al /home

runagent -m samba1 podman exec -it samba-dc

runagent -m traefik1 podman ps

runagent -m samba1 podman exec -it samba-dc samba-tool domain level show
runagent -m samba1 podman exec -it samba-dc samba-tool domain level raise --domain-level=2012_R2 --forest-level=2012_R2

[global]
... 其他原本的設定 ...
ad dc functional level = 2016



dsa.msc：開啟 Active Directory 使用者和電腦
gpmc.msc：開啟 群組原則管理
dnsmgmt.msc：開啟 DNS 管理員
dhcpmgmt.msc：開啟 DHCP 管理員
compmgmt.msc：開啟 電腦管理（可用於遠端連線其他伺服器）

runagent -m samba1 podman exec -it samba-dc samba-tool domain schemaupgrade --schema=2016

runagent -m samba1 podman exec -it samba-dc samba-tool domain functionalprep --function-level=2016
