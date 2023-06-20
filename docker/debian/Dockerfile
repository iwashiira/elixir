FROM debian:latest

#RUN \
#    : "${GIT_REPO_URL:?set GIT_REPO_URL to the repo git url}"

#RUN \
#    : "${PROJECT:?set PROJECT to set the project name}"

RUN \
    echo "repo url to index: ${GIT_REPO_URL}"

RUN \
  apt-get update && \
  apt-get -y install \
    python3 \
    python3-pip \
    python3-jinja2 \
    python3-bsddb3 \
    python3-pytest \
    perl \
    git \
    apache2 \
    libapache2-mod-wsgi-py3 \
    libjansson4 \
    libyaml-0-2 \
    wget

RUN \
  python3 -m pip install falcon --break-system-packages

#RUN \
#  ln -s /usr/bin/pytest-3 /usr/bin/pytest

RUN \
  wget https://bootlin.com/pub/elixir/universal-ctags_0+git20200526-0ubuntu1_amd64.deb

RUN \
  dpkg -i universal-ctags_0+git20200526-0ubuntu1_amd64.deb

RUN \
  wget https://bootlin.com/pub/elixir/Pygments-2.6.1.elixir-py3-none-any.whl

RUN \
  python3 -m pip install Pygments-2.6.1.elixir-py3-none-any.whl --break-system-packages

RUN \
  git config --global user.email 'elixir@dummy.com' && \
  git config --global user.name 'elixir'

RUN \
  git clone https://github.com/bootlin/elixir.git /usr/local/elixir/

RUN \
  mkdir -p /srv/elixir-data/

ENV MY_LOCAL_IP=localhost

#glibc
ENV GIT_REPO_URL=git://sourceware.org/git/glibc.git
ENV PROJECT=glibc

RUN \
  mkdir -p /srv/elixir-data/$PROJECT/repo && \
  mkdir -p /srv/elixir-data/$PROJECT/data && \
  git clone --bare "${GIT_REPO_URL}" /srv/elixir-data/$PROJECT/repo/

ENV LXR_REPO_DIR /srv/elixir-data/$PROJECT/repo
ENV LXR_DATA_DIR /srv/elixir-data/$PROJECT/data

RUN \
  cd /usr/local/elixir/ && \
  ./script.sh list-tags && \
  python3 -u ./update.py

#QEMU
ENV GIT_REPO_URL=https://gitlab.com/qemu-project/qemu.git
ENV PROJECT=qemu

RUN \
  mkdir -p /srv/elixir-data/$PROJECT/repo && \
  mkdir -p /srv/elixir-data/$PROJECT/data && \
  git clone --bare "${GIT_REPO_URL}" /srv/elixir-data/$PROJECT/repo/

ENV LXR_REPO_DIR /srv/elixir-data/$PROJECT/repo
ENV LXR_DATA_DIR /srv/elixir-data/$PROJECT/data

RUN \
  cd /usr/local/elixir/ && \
  ./script.sh list-tags && \
  python3 -u ./update.py

#linux
ENV GIT_REPO_URL=git://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git
ENV PROJECT=linux

RUN \
  mkdir -p /srv/elixir-data/$PROJECT/repo && \
  mkdir -p /srv/elixir-data/$PROJECT/data && \
  git clone --bare "${GIT_REPO_URL}" /srv/elixir-data/$PROJECT/repo/

ENV LXR_REPO_DIR /srv/elixir-data/$PROJECT/repo
ENV LXR_DATA_DIR /srv/elixir-data/$PROJECT/data

RUN \
  cd /usr/local/elixir/ && \
  ./script.sh list-tags && \
  python3 -u ./update.py
# apache elixir config, see elixir README
# make apache less stricter about cgitb spam headers

RUN \
  chown -R www-data /srv/elixir-data/

COPY ./000-default.conf /etc/apache2/sites-available/000-default.conf

RUN \
  #echo PERpcmVjdG9yeSAvdXNyL2xvY2FsL2VsaXhpci9odHRwLz4KICAgIE9wdGlvbnMgK0V4ZWNDR0kKICAgIEFsbG93T3ZlcnJpZGUgTm9uZQogICAgUmVxdWlyZSBhbGwgZ3JhbnRlZAogICAgU2V0RW52IFBZVEhPTklPRU5DT0RJTkcgdXRmLTgKICAgIFNldEVudiBMWFJfUFJPSl9ESVIgL3Nydi9lbGl4aXItZGF0YQo8L0RpcmVjdG9yeT4KPERpcmVjdG9yeSAvdXNyL2xvY2FsL2VsaXhpci9hcGkvPgogICAgU2V0SGFuZGxlciB3c2dpLXNjcmlwdAogICAgUmVxdWlyZSBhbGwgZ3JhbnRlZAogICAgU2V0RW52IFBZVEhPTklPRU5DT0RJTkcgdXRmLTgKICAgIFNldEVudiBMWFJfUFJPSl9ESVIgL3Nydi9lbGl4aXItZGF0YQo8L0RpcmVjdG9yeT4KQWRkSGFuZGxlciBjZ2ktc2NyaXB0IC5weQo8VmlydHVhbEhvc3QgKjo4MD4KICAgIFNlcnZlck5hbWUgbG9jYWxob3N0CiAgICBEb2N1bWVudFJvb3QgL3Vzci9sb2NhbC9lbGl4aXIvaHR0cAogICAgV1NHSVNjcmlwdEFsaWFzIC9hcGkgL3Vzci9sb2NhbC9lbGl4aXIvYXBpL2FwaS5weQogICAgQWxsb3dFbmNvZGVkU2xhc2hlcyBPbgogICAgUmV3cml0ZUVuZ2luZSBvbgogICAgUmV3cml0ZVJ1bGUgIl4vJCIgIi9saW51eC9sYXRlc3Qvc291cmNlIiBbUl0KICAgIFJld3JpdGVSdWxlICJeLyg/IWFwaXxhY3ApLiovKHNvdXJjZXxpZGVudHxzZWFyY2gpIiAiL3dlYi5weSIgW1BUXQogICAgUmV3cml0ZVJ1bGUgIl4vYWNwIiAiL2F1dG9jb21wbGV0ZS5weSIgW1BUXQo8L1ZpcnR1YWxIb3N0Pgo= | base64 -d > /etc/apache2/sites-available/000-default.conf && \
  echo -e "\nHttpProtocolOptions Unsafe" >> /etc/apache2/apache.conf && \
  a2enmod cgi rewrite

EXPOSE 80

ENTRYPOINT ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
