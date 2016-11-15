# http-proxy
A sample demonstrating how to build a docker image containing the RTVS HTTP proxy for message interception.

# Steps

1\. Clone the example
```
git clone https://github.com/ibm-rtvs/rtvs-docker.git
```
2\. Make changes as necessary

3\. Build the image
```
cd rtvs-docker/http-proxy
docker build -t rtvs-http-proxy .
```
4\. Execute the image
```
docker run -d -p 3128:3128 -e RULE_SERVER=http://$(hostname):7819/RTCP rtvs-http-proxy
```
Note: The **RULE_SERVER** must refer to an instance of RTCP.

You now have a running HTTP Proxy.

#Connection details
The connection details are:

* Host: ```$(docker-machine ip)```
* Port: ```3128```
