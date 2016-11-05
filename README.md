# docker-storage-setup
setup docker-thin-pool for docker-storage

# Install 
rpm -hiv docker-storage-setup-0.5-1.el7.centos.x86_64.rpm 

# Configuration
vim  /usr/lib/docker-storage-setup/docker-storage-setup

> DEVS=/dev/sdb1 要加入VG的分区

> DATA_SIZE=40%FREE docker-thin-pool的大小占整个VG的大小

> POOL_AUTOEXTEND_THRESHOLD=60 thin-pool自动扩容的阈值

> POOL_AUTOEXTEND_PERCENT=20 自动扩容的百分比

# Modify docker.service

`[Unit]`

`Description=Docker Application Container Engine`

`Documentation=http://docs.docker.com`

`After=network.target`

`[Service]`

`Type=notify`

`EnvironmentFile=-/etc/sysconfig/docker-storage`

`EnvironmentFile=-/opt/kubernetes/cfg/config`

`WorkingDirectory=/opt/kubernetes/bin`

`ExecStart=/opt/kubernetes/bin/docker daemon $DOCKER_OPT_BIP $DOCKER_OPT_MTU $DOCKER_OPTS $DOCKER_STORAGE_OPTIONS`

`LimitNOFILE=1048576`

`LimitNPROC=1048576`

`[Install]`

`WantedBy=multi-user.target `

#Setup

` systemctl enable docker-storage-setup`
