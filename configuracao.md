# Primeiro Container

(nesse container foi instalado apenas o ssh)

docker run -d --name servidor ubuntu:24.04 bash -c "tail -f /dev/null" 

docker exec -it servidor /bin/bash

apt update \
&& apt install -y openssh-server \
&& mkdir /var/run/sshd

echo 'root:123456' | chpasswd

sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config

sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

apt install -y systemd systemd-sysv dbus dbus-user-session

apt install systemctl -y

systemctl status ssh.service

systemctl enable ssh.service 

systemctl start ssh.service

# Segundo Container

(nesse container foi instalado o ssh e o ufw)

docker run --privileged -d --name cliente ubuntu:24.04 bash -c "tail -f /dev/null" 

docker exec -it cliente /bin/bash

apt update \
&& apt install -y openssh-server \
&& mkdir /var/run/sshd

echo 'root:123456' | chpasswd

sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config

sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

apt install -y systemd systemd-sysv dbus dbus-user-session

apt install systemctl -y

systemctl status ssh.service

systemctl enable ssh.service 

systemctl start ssh.service

apt install ufw -y

systemctl status ufw

systemctl enable ufw

systemctl start ufw
