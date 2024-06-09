FROM rockylinux:8

RUN yum install epel-release -y \
  && yum groupinstall "Minimal Install" --allowerasing -y \
  && yum install -y \
  glibc-all-langpacks \
  net-tools \
  nmap-ncat \
  dnsutils \
  perl \
  spax \
  sqlite \
  libidn \
  libaio \
  libicu \
  perl-Socket6 \
  sysstat \
  tmux \
  supervisor \
  && yum update -y \
  && yum clean all \
  && rm -f /var/log/dnf.* \
  && rm -f /var/log/*log

COPY --chmod=644 supervisord.conf /etc/supervisord.conf
COPY --chmod=644 default.ini /etc/supervisord.d/default.ini
COPY --chmod=775 supervisor/ /supervisor/
COPY --chmod=644 sysstat.cron /etc/cron.d/sysstat

ENTRYPOINT ["/usr/bin/supervisord", "-c", "/etc/supervisord.conf"]
