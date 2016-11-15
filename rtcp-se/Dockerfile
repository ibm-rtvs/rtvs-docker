FROM tatsushid/tinycore:7.2-x86_64

ENV BIN_SERVER=ftp://public.dhe.ibm.com/software/spcn/continuoustest

# fix image to allow 64bit executables to launch
RUN ln -s /lib /lib64 \
# download the latest starter edition
  && wget -O /tmp/redirect.txt $BIN_SERVER/IntegrationTesterSE.props \
  && wget -O /tmp/dist.zip $BIN_SERVER/ritse.linux.x86_64_$( \
       sed -n 's/CurrentVersion=\(.*\)/\1/p' /tmp/redirect.txt \
       ).v$( \
       sed -n 's/Build=I\(.*\)/\1/p' /tmp/redirect.txt \
       ).zip \
# move RTCP to the /opt directory
  && mkdir /tmp/dist \
  && unzip -q /tmp/dist.zip -d /tmp/dist \
  && mv /tmp/dist/IntegrationTester/Apps/RTCP /opt \
  && mv /tmp/dist/IntegrationTester/Apps/jre /opt \
# prep logs directory
  && ln -s /var/log /opt/logs \
# clean-up
  && rm -fr /tmp/dist /tmp/dist.zip /tmp/redirect.txt \
# run on all network interfaces in the container
  && sed -i 's/host=".*"/host="*"/' /opt/RTCP/usr/servers/RTCPServer/server.xml \
# run RTCP without detach so that container is kept alive
  && sed -i 's/start RTCP/run RTCP/' /opt/RTCP/bin/startup.sh \
# stop the container on request by getting 'server' script to handle the signal
  && sed -i 's/\(.*server.*run\)/exec \1/' /opt/RTCP/bin/startup.sh \
# stop the container on request by getting jvm to handle the signal
  && sed -i 's/\("${JAVA_CMD}"\)/exec \1/' /opt/RTCP/bin/server

EXPOSE 7819
EXPOSE 9443

CMD ["/opt/RTCP/bin/startup.sh"]
