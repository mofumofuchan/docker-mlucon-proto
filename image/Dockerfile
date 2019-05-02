FROM debian:jessie

MAINTAINER you_oki1@yahoo.co.jp

RUN \
  apt-get update && \
  DEBIAN_FRONTEND=noninteractive apt-get install -y git gcc patch autoconf automake bison build-essential libssl-dev libyaml-dev libreadline6-dev zlib1g-dev libncurses5-dev libffi-dev libgdbm3 libgdbm-dev libmysqlclient-dev libpq-dev libcurl4-openssl-dev libpng12-0 libpng12-dev libmcrypt4 libmcrypt-dev libtidy-0.99-0 libtidy-dev libxml2-dev libxslt1-dev readline-common curl pkg-config mysql-server memcached nginx sudo && \
  cp -p /usr/share/zoneinfo/Japan /etc/localtime && \
  echo "deb http://packages.dotdeb.org jessie all" >> /etc/apt/sources.list && \
  echo "deb-src http://packages.dotdeb.org jessie all" >> /etc/apt/sources.list && \
  curl -sL https://www.dotdeb.org/dotdeb.gpg | apt-key add - && \
  apt-get update && \
  DEBIAN_FRONTEND=noninteractive apt-get install -y php7.0-cli php7.0-fpm php7.0-mysql php7.0-memcached && \
  apt-get clean && \
  groupadd wheel && \
  useradd -G wheel isucon -s /bin/bash && \
  git clone https://github.com/mofumofuchan/mlucon-proto.git /home/isucon/repos/mlucon_proto.git && \
  git clone https://github.com/mofumofuchan/mlucon-proto-python.git /home/isucon/repos/mlucon_proto.git/webapp/python && \
  cp /home/isucon/repos/mlucon_proto.git/provisioning/image/files/etc/profile.d/bashrc /home/isucon/.profile && \
  chown isucon:isucon /home/isucon/.profile && \
  chmod +x /home/isucon/.profile && \
  cp /home/isucon/repos/mlucon_proto.git/provisioning/image/files/home/isucon/env.sh /home/isucon/env.sh && \
  chmod +x /home/isucon/env.sh && \
  mkdir /home/isucon/.ssh && \
  cp /home/isucon/repos/mlucon_proto.git/provisioning/image/files/home/isucon/.ssh/authorized_keys /home/isucon/.ssh/authorized_keys && \
  chmod go-rwx -R /home/isucon/.ssh && \
  echo '%wheel ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers && \
  rm -r /etc/nginx/sites-*/default && \
  cp /home/isucon/repos/mlucon_proto.git/provisioning/image/files/etc/nginx/sites-available/isucon.conf /etc/nginx/sites-available/isucon.conf && \
  cp /home/isucon/repos/mlucon_proto.git/provisioning/image/files/etc/nginx/sites-available/isucon-php.conf /etc/nginx/sites-available/isucon-php.conf && \
  ln -s ../sites-available/isucon.conf /etc/nginx/sites-enabled/isucon.conf && \
  git clone https://github.com/tagomoris/xbuild.git /home/isucon/.xbuild && \
  cp /home/isucon/repos/mlucon_proto.git/provisioning/image/files/etc/php/7.0/fpm/pool.d/www.conf /etc/php/7.0/fpm/pool.d/www.conf && \
  cp /home/isucon/repos/mlucon_proto.git/provisioning/image/files/etc/systemd/system/* /etc/systemd/system/ && \
  systemctl enable isu-ruby && \
  rm -r \
    /home/isucon/repos/mlucon_proto.git/.git \
    /home/isucon/repos/mlucon_proto.git/ansible \
    /home/isucon/repos/mlucon_proto.git/benchmarker \
    /home/isucon/repos/mlucon_proto.git/e2etest \
    /home/isucon/repos/mlucon_proto.git/portal \
    /home/isucon/repos/mlucon_proto.git/provisioning \
    /home/isucon/repos/mlucon_proto.git/README.md \
    /home/isucon/repos/mlucon_proto.git/webapp/python/.git && \
  ( cd /home/isucon/repos/mlucon_proto.git/ && git init && git add . && git config --global user.email 'test@example.com' && git config --global user.name 'admin' && git commit -m 'Initial commit' ) && \
  git clone file:///home/isucon/repos/mlucon_proto.git /home/isucon/mlucon_proto && \
  mkdir /home/isucon/backup/ && \
  curl -sL https://github.com/mofumofuchan/mlucon-proto/releases/download/img/dump.sql.bz2 > /home/isucon/backup/mysqldump.sql.bz2 && \
  service mysql start && \
  bzcat /home/isucon/backup/mysqldump.sql.bz2 | mysql -u root && \
  service mysql stop && \
  chown -R isucon:isucon /home/isucon && \
  sudo -u isucon -i /home/isucon/.xbuild/ruby-install 2.3.0 /home/isucon/.local/ruby && \
  sudo -u isucon -i /home/isucon/.local/ruby/bin/gem install bundler && \
  sudo -u isucon -i sh -c "cd /home/isucon/mlucon_proto/webapp/ruby && bundle install" && \
  sudo -u isucon -i /home/isucon/.xbuild/node-install v4.4.3 /home/isucon/.local/node && \
  sudo -u isucon -i sh -c "cd /home/isucon/mlucon_proto/webapp/node && npm install" && \
  sudo -u isucon -i /home/isucon/.xbuild/go-install 1.9.7 /home/isucon/.local/go && \
  sudo -u isucon -i sh -c "cd /home/isucon/mlucon_proto/webapp/golang && ./setup.sh" && \
  sudo -u isucon -i curl -sL -o /home/isucon/mlucon_proto/webapp/php/composer.phar http://getcomposer.org/composer.phar && \
  chmod +x /home/isucon/mlucon_proto/webapp/php/composer.phar && \
  sudo -u isucon -i sh -c "cd /home/isucon/mlucon_proto/webapp/php && ./composer.phar install" && \
  sudo -u isucon -i /home/isucon/.xbuild/python-install 3.5.2 /home/isucon/.local/python3 && \
  sudo -u isucon -i sh -c "cd /home/isucon/mlucon_proto/webapp/python && pip install -r requirements.freeze" && \
  rm -r /var/lib/apt/lists/* /home/isucon/backup

COPY isu-python.service /etc/systemd/system/

EXPOSE 80

ENTRYPOINT ["/sbin/init"]
