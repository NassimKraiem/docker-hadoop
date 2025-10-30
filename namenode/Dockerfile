FROM bde2020/hadoop-base:2.0.0-hadoop3.2.1-java8

MAINTAINER Ivan Ermilov <ivan.s.ermilov@gmail.com>

HEALTHCHECK CMD curl -f http://localhost:9870/ || exit 1

ENV HDFS_CONF_dfs_namenode_name_dir=file:///hadoop/dfs/name

# ---------------------------
# Install Python 3 + pip (Debian Stretch archived repos)
# ---------------------------
RUN set -eux; \
    # Backup old sources
    cp /etc/apt/sources.list /etc/apt/sources.list.bak; \
    \
    # Replace with archive sources (Stretch is EOL)
    echo 'deb http://archive.debian.org/debian stretch main contrib non-free' > /etc/apt/sources.list; \
    echo 'deb http://archive.debian.org/debian-security stretch/updates main contrib non-free' >> /etc/apt/sources.list; \
    \
    # Disable date validation
    echo 'Acquire::Check-Valid-Until "false";' > /etc/apt/apt.conf.d/99ignore-valid-until; \
    \
    # Update using insecure repos
    apt-get -o Acquire::AllowInsecureRepositories=true update; \
    \
    # Install Python packages
    apt-get install -y python3 python3-pip python3-venv; \
    \
    # Optional cleanup to keep the image lean
    apt-get clean; \
    rm -rf /var/lib/apt/lists/*; \
    \
    # (Optional) restore original apt config
    mv /etc/apt/sources.list.bak /etc/apt/sources.list || true; \
    rm -f /etc/apt/apt.conf.d/99ignore-valid-until

# ---------------------------
# Hadoop NameNode setup
# ---------------------------
RUN mkdir -p /hadoop/dfs/name
VOLUME /hadoop/dfs/name

ADD run.sh /run.sh
RUN chmod a+x /run.sh

EXPOSE 9870

CMD ["/run.sh"]
