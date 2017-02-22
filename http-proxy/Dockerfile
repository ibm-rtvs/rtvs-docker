FROM tatsushid/tinycore:7.2-x86_64

USER root

# we expect the user to either set the RTCP RULE_SERVER location at container creation 
# time or at build time.
ENV BIN_SERVER=ftp://public.dhe.ibm.com/software/spcn/continuoustest \
    RULE_SERVER=start\ container\ with\ -e\ RULE_SERVER=http://rtcp.local:7819/RTCP

# fix image to allow 64bit executables to launch
RUN ln -s /lib /lib64 \
# download the latest starter edition
  && wget -O /tmp/redirect.txt $BIN_SERVER/IntegrationTesterSE.props \
  && wget -O /tmp/dist.zip $BIN_SERVER/ritse.linux.x86_64_$( \
       sed -n 's/CurrentVersion=\(.*\)/\1/p' /tmp/redirect.txt \
       ).v$( \
       sed -n 's/Build=I\(.*\)/\1/p' /tmp/redirect.txt \
       ).zip \
# move the proxy to the /opt directory
  && mkdir /tmp/dist \
  && unzip -q /tmp/dist.zip -d /tmp/dist \
  && mv /tmp/dist/IntegrationTester/Apps/HttpTcpProxy /opt \
  && mv /tmp/dist/IntegrationTester/Apps/jre /opt \
# prep logs directory
  && ln -s /var/log /opt/logs \
# clean-up
  && rm -fr /tmp/dist /tmp/dist.zip /tmp/redirect.txt \
# hook up RTCP in registration.xml to the RULE_SERVER env variable
  && sed -i 's#.*\#!.*#&\nsed -i "s!http://localhost:7819/RTCP!\$RULE_SERVER!" /opt/HttpTcpProxy/registration.xml#' /opt/HttpTcpProxy/startup.sh \
# stop the container on request by getting the jvm to handle the signal
  && sed -i 's#^\("\$_RUNJAVA\)#exec \1#' /opt/HttpTcpProxy/startup.sh \
# bind to all container addresses
  && sed -i 's/bind-address=".*"/bind-address=""/' /opt/HttpTcpProxy/registration.xml

# http proxy port
EXPOSE 3128
# dedicated https only proxy port
EXPOSE 3129

CMD ["/opt/HttpTcpProxy/startup.sh"]
