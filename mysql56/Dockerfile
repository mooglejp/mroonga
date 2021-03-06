FROM centos:7
MAINTAINER groonga

RUN yum upgrade -y && \
    yum install -y \
      which \
      http://packages.groonga.org/centos/groonga-release-latest.noarch.rpm \
      http://dev.mysql.com/get/mysql80-community-release-el7-3.noarch.rpm && \
    yum clean all

ENV groonga_version=10.0.1 \
    mroonga_version=10.01 \
    mysql_version=5.6.47

RUN yum install -y \
      --enablerepo=mysql56-community \
      --disablerepo=mysql57-community \
      --disablerepo=mysql80-community \
      mysql-community-client-${mysql_version} \
      mysql-community-server-${mysql_version} \
      mysql-community-mroonga-${mroonga_version} \
      groonga-${groonga_version} \
      groonga-tokenizer-mecab-${groonga_version} \
      groonga-normalizer-mysql && \
    yum clean all

COPY my.cnf /etc/my.cnf

RUN mysql-systemd-start pre && \
    sh -c "/usr/sbin/mysqld --basedir=/usr --user=mysql &" && \
    mysql-systemd-start post && \
    mysql -e "GRANT ALL ON *.* TO root@'%' WITH GRANT OPTION" && \
    mysql < /usr/share/mroonga/install.sql && \
    mysqladmin shutdown && \
    rm /var/lib/mysql/auto.cnf /var/lib/mysql/groonga.log
COPY entrypoint.sh /root/entrypoint.sh

EXPOSE 3306
ENTRYPOINT ["/root/entrypoint.sh"]
