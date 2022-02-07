# Example for docker-maven-plugin healthy issue

[Issue](https://github.com/fabric8io/docker-maven-plugin/issues/1101)

The docker image in `src/main/docker` is a modified httpd image that becomes healthy after 15 seconds.

## Profile 1

    mvn docker:run -Phttpd

In this case the plugin waits only on port (httpd default image doesn't provide an healthcheck)

## Profile 2

    mvn docker:run -Ptest1

In this case the plugin says

> [test1:latest]: Waited on tcp port '[/172.17.0.2:80]' **and** on healthcheck '"CMD-SHELL", "curl -f http://localhost/ok || exit 1"' 4 ms

But it waited only on the port and the container is on `health: starting` status when the waiting is finished.

Commenting out the `<tcp>` tag, the plugin waits 15 seconds, as expected:

    [INFO] DOCKER> [test1:latest]: Waited on healthcheck '"CMD-SHELL", "curl -f http://localhost/ok || exit 1"' 15969 ms
