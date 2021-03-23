#第一步，找到一个基础centos镜像做底层镜像
FROM centos:7.4.1708
#解压jdk包到你这次打包的镜像目录/usr/local/下 
ADD jdk-8u221-linux-x64.tar.gz /usr/local/
#将镜像内老的的yum源备份
RUN mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.bak
#拷贝本地最新的yum源到你打包的镜像中，足够使用
ADD CentOS-Base.repo /etc/yum.repos.d/
#将镜像中的jdk环境重命名
RUN mv /usr/local/jdk1.8.0_221 /usr/local/jdk
#下载一些你需要的东西 RUN最好用一行写，每一个大写dockerfile命令就是一层基础镜像，所有RUN我们用&&链接
RUN yum update -y \
    && yum -y reinstall glibc-common \
    && yum install -y telnet net-tools wget curl kde-l10n-Chinese \
    && localedef -c -f UTF-8 -i zh_CN zh_CN.utf8 \
    && yum clean all \
    && rm -rf /tmp/* rm -rf /var/cache/yum/*
#定义字体格式
COPY i18n  /etc/sysconfig/i18n
#增加LANG变量，是镜像编码字符为zh_CN.UTF-8
RUN echo "LANG=”zh_CN.UTF-8”" > /etc/locale.conf && source /etc/locale.conf
#定义java环境变量，以及一些其他变量ENV 不可以用&&（我没找到说可以用）
ENV JAVA_HOME=/usr/local/jdk
ENV CLASS_PATH=$JAVA_HOME/lib:$JAVA_HOME/jre/lib
ENV PATH=$JAVA_HOME/bin:$PATH
ENV LC_ALL "zh_CN.UTF-8"
ENV LANG "zh_CN.UTF-8"
