FROM yeak/baseimage-rockylinux:8

# Zimbra softwareonly

COPY keystrokes /installer/

RUN cd /installer \
  && cat keystrokes | sed -e 's/.*Use Zimbra/y Use Zimbra/' | cut -d' ' -f1 > /tmp/keystrokes \
  && curl -LO -C - https://files.zimbra.com/downloads/8.8.15_GA/zcs-8.8.15_GA_4362.RHEL8_64.20220721104405.tgz \
  && tar xf zcs-8.8.15_GA_4362.RHEL8_64.20220721104405.tgz \
  && cd zcs-8.8.15_GA_4362.RHEL8_64.20220721104405 \
  && sed -i '/^checkRequired/d' install.sh \
  && ./install.sh -s < /tmp/keystrokes \
  && cd / \
  && rm -rf /installer \
  && rm -f /tmp/keystrokes \
  && mv /tmp/install.log* /opt/zimbra/log/ \
  && install -m 644 -o zimbra -g zimbra /dev/null /var/log/zimbra-stats.log

# Supervisor

COPY --chmod=644 zimbra.ini /etc/supervisord.d/zimbra.ini
COPY --chmod=775 savedata.sh /etc/cron.daily/savedata.sh
COPY --chmod=775 entrypoint.sh start.sh /

#ENTRYPOINT ["/entrypoint.sh"]
#CMD ["/bin/bash"]

VOLUME /data

EXPOSE 22
EXPOSE 80
EXPOSE 443
EXPOSE 25
EXPOSE 465
EXPOSE 587
EXPOSE 110
EXPOSE 143
EXPOSE 993
EXPOSE 995
EXPOSE 7071
EXPOSE 9071
