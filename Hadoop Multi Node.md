#Hadoop Multi Node
在伺服器上使用虛擬機模擬叢集     
安裝完畢後更改虛擬網路卡設定
每一台節點的路徑和使用者都必須一樣
  
每一台電腦均要做
0.將原先的網路卡從 NAT 改為 橋接
sudo vim /etc/network/interfaces

auto eth0
iface eth0 inet static
address 140.119.164.78
netmast 255.255.255.0
gateway 140.119.164.254
dns-nameservers 140.119.1.110
設定完網路必須重新開機

1.$sudo apt-get update  
2.$sudo apt-get upgrade  
3.$sudo apt-get install vim  
4.$sudo apt-get install openjdk-7-jdk (http://openjdk.java.net/install)  
5.$sudo apt-get install openssh-server  
6.$ssh-keygen -t rsa -P "" (and just press enter)  
7.$cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys  
8.$ssh localhost (type "yes")  login without key  
9.$exit    

僅master做，全不設定完成在複製到其他節點
