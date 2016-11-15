# rtcp-se
A sample demonstrating how to build a docker image containing the RTVS Rational Test Control Panel (starter edition).
The starter edition does not have all the capabilities of full edition, for example no usage metrics capability is present,
however recording and intercept mediation is included which is sufficient to get started using Virtual Services in docker.

# Steps

1\. Clone the example
```
git clone https://github.com/ibm-rtvs/rtvs-docker.git
```
2\. Make changes as necessary

3\. Build the image
```
cd rtvs-docker/rtcp-se
docker build -t rtcp-se .
```
4\. Execute the image
```
docker run -d -p 7819:7819 rtcp-se
```

You now have a running instance of RTCP (starter edition).

#Connection details
The connection details are:

URL for rule mediation:
* ```RULE_SERVER```: ```http://$(docker-machine ip):7819/RTCP```

Note: This assumes you are using Docker Machine, if you're not, use the hostname or ip address of the docker host instead.
