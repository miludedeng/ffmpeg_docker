##RUN ffmpeg in a container

----------

把ffmpeg脚本放到PATH中，就可以像普通的ffmpeg一样执行

####使用说明

1、安装并docker
	
centos7 安装docker

`yum -y install docker`

`systemctl start docker`

2、pull ffmpeg镜像

`docker pull cafetime/ffmpeg`

3、下载ffmpeg脚本，并放入/usr/local/bin
