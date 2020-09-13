# Begin using Anchore

Start using the anchore-engine service to analyze images - a short example follows which demonstrates a basic workflow of adding a container image for analysis, waiting for the analysis to complete, then running content reports, vulnerability scans and policy evaluations against the analyzed image.

`docker-compose exec api anchore-cli image add docker.io/library/openjdk:14-jdk-alpine3.10`{{execute}}

    ...
    ...

`docker-compose exec api anchore-cli image wait docker.io/library/openjdk:14-jdk-alpine3.10`{{execute}}

    Status: analyzing
    Waiting 5.0 seconds for next retry.
    Status: analyzing
    Waiting 5.0 seconds for next retry.
    ...
    ...

`docker-compose exec api anchore-cli image content docker.io/library/openjdk:14-jdk-alpine3.10 os`{{execute}}

    Package                       Version            Licenses
    alpine-baselayout             3.1.2-r0           GPL-2.0-only
    alpine-keys                   2.1-r2             MIT
    apk-tools                     2.10.4-r2          GPL2
    busybox                       1.30.1-r3          GPL-2.0
    ca-certificates-cacert        20190108-r0        MPL-2.0 GPL-2.0-or-later
    libc-utils                    0.7.1-r0           BSD
    libcrypto1.1                  1.1.1d-r2          OpenSSL
    libssl1.1                     1.1.1d-r2          OpenSSL
    libtls-standalone             2.9.1-r0           ISC
    musl                          1.1.22-r3          MIT
    musl-utils                    1.1.22-r3          MIT BSD GPL2+
    scanelf                       1.2.3-r0           GPL-2.0
    ssl_client                    1.30.1-r3          GPL-2.0
    zlib                          1.2.11-r1          zlib

`docker-compose exec api anchore-cli image vuln docker.io/library/openjdk:14-jdk-alpine3.10 all`{{execute}}

    Vulnerability ID        Package                       Severity        Fix              CVE Refs        Vulnerability URL                                                  Type        Feed Group         Package Path
    CVE-2020-1967           libcrypto1.1-1.1.1d-r2        Medium          1.1.1g-r0                        http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1967        APKG        alpine:3.10        pkgdb
    CVE-2020-1967           libssl1.1-1.1.1d-r2           Medium          1.1.1g-r0                        http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1967        APKG        alpine:3.10        pkgdb

`docker-compose exec api anchore-cli evaluate check docker.io/library/openjdk:14-jdk-alpine3.10`{{execute}}

    docker.io/library/openjdk:14-jdk-alpine3.10
    Image Digest: sha256:7c29ddf86e7fc5ea5fe01e1ad3e3439422fc50dc2c568b00d6bd79bdb026bfdf
    Full Tag: docker.io/library/openjdk:14-jdk-alpine3.10
    Status: pass
    Last Eval: 2020-09-13T13:12:28Z
    Policy ID: 2c53a13c-1765-11e8-82ef-23527761d060
