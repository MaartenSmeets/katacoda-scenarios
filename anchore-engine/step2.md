# Verify service availability

After a few moments (depending on system speed), your Anchore Engine services should be up and running, ready to use. You can verify the containers are running with docker-compose:

    $ docker-compose ps
                    Name                               Command                        State           Ports
    -------------------------------------------------------------------------------------------------------
    anchorequickstart_anchore-db_1                   docker-entrypoint.sh postgres    Up      5432/tcp
    anchorequickstart_analyzer_1              /docker-entrypoint.sh anch ...   Up      8228/tcp
    anchorequickstart_api_1                   /docker-entrypoint.sh anch ...   Up      0.0.0.0:8228->8228/tcp
    anchorequickstart_catalog_1               /docker-entrypoint.sh anch ...   Up      8228/tcp
    anchorequickstart_policy-engine_1         /docker-entrypoint.sh anch ...   Up      8228/tcp
    anchorequickstart_simpleq_1               /docker-entrypoint.sh anch ...   Up      8228/tcp
    
You can run a command to get the status of the Anchore Engine services:

    $ docker-compose exec api anchore-cli system status
    Service policy_engine (anchore-quickstart, http://policy-engine:8228): up
    Service simplequeue (anchore-quickstart, http://simpleq:8228): up
    Service catalog (anchore-quickstart, http://catalog:8228): up
    Service analyzer (anchore-quickstart, http://analyzer:8228): up
    Service apiext (anchore-quickstart, http://api:8228): up

    Engine DB Version: 0.0.13
    Engine Code Version: 0.8.1

**Note:** The first time you run Anchore Engine, it will take some time (10+ minutes, depending on network speed) for the vulnerability data to get synced into the engine. For the best experience, wait until the core vulnerability data feeds have completed before proceeding. You can check the status of your feed sync using the CLI:

    $ docker-compose exec api anchore-cli system feeds list
    Feed                   Group                  LastSync                          RecordCount        
    vulnerabilities        alpine:3.10            2020-04-27T19:49:45.186409        1725               
    vulnerabilities        alpine:3.11            2020-04-27T19:49:59.993730        1904               
    vulnerabilities        alpine:3.3             2020-04-27T19:50:16.213013        457                
    vulnerabilities        alpine:3.4             2020-04-27T19:50:20.128136        681                
    vulnerabilities        alpine:3.5             2020-04-27T19:50:25.876762        875                
    vulnerabilities        alpine:3.6             2020-04-27T19:50:33.361682        1051               
    vulnerabilities        alpine:3.7             2020-04-27T19:50:42.354798        1395               
    vulnerabilities        alpine:3.8             2020-04-27T19:50:54.311199        1486               
    vulnerabilities        alpine:3.9             2020-04-27T19:51:07.340326        1558               
    vulnerabilities        amzn:2                 2020-04-27T19:51:20.726861        327                
    vulnerabilities        centos:5               2020-04-27T19:51:31.586422        1347               
    vulnerabilities        centos:6               2020-04-27T19:51:57.345700        1403               
    vulnerabilities        centos:7               2020-04-27T19:52:26.350592        1063               
    vulnerabilities        centos:8               2020-04-27T19:52:59.187517        215                
    vulnerabilities        debian:10              2020-04-27T19:53:08.194067        22580              
    vulnerabilities        debian:11              2020-04-27T19:56:03.833415        19681              
    vulnerabilities        debian:7               2020-04-27T19:58:44.907852        20455              
    vulnerabilities        debian:8               pending                           12500              
    vulnerabilities        debian:9               pending                           None               
    vulnerabilities        debian:unstable        pending                           None               
    vulnerabilities        ol:5                   pending                           None               
    vulnerabilities        ol:6                   pending                           None               
    vulnerabilities        ol:7                   pending                           None               
    vulnerabilities        ol:8                   pending                           None               
    vulnerabilities        rhel:5                 pending                           None               
    vulnerabilities        rhel:6                 pending                           None               
    vulnerabilities        rhel:7                 pending                           None               
    vulnerabilities        rhel:8                 pending                           None               
    vulnerabilities        ubuntu:12.04           pending                           None               
    vulnerabilities        ubuntu:12.10           pending                           None               
    vulnerabilities        ubuntu:13.04           pending                           None               
    vulnerabilities        ubuntu:14.04           pending                           None               
    vulnerabilities        ubuntu:14.10           pending                           None               
    vulnerabilities        ubuntu:15.04           pending                           None               
    vulnerabilities        ubuntu:15.10           pending                           None               
    vulnerabilities        ubuntu:16.04           pending                           None               
    vulnerabilities        ubuntu:16.10           pending                           None               
    vulnerabilities        ubuntu:17.04           pending                           None               
    vulnerabilities        ubuntu:17.10           pending                           None               
    vulnerabilities        ubuntu:18.04           pending                           None               
    vulnerabilities        ubuntu:18.10           pending                           None               
    vulnerabilities        ubuntu:19.04           pending                           None               
    vulnerabilities        ubuntu:19.10           pending                           None               
    vulnerabilities        ubuntu:20.04           pending                           None

As soon as you see RecordCount values > 0 for all vulnerability groups, the system is fully populated and ready to present vulnerability results. Note that feed syncs are incremental, so the next time you start up Anchore Engine it will be ready immediately. The CLI tool includes a useful utility that will block until the feeds have completed a successful sync:

    $ docker-compose exec api anchore-cli system wait
    Starting checks to wait for anchore-engine to be available timeout=-1.0 interval=5.0
    API availability: Checking anchore-engine URL (http://localhost:8228)...
    API availability: Success.
    Service availability: Checking for service set (catalog,apiext,policy_engine,simplequeue,analyzer)...
    Service availability: Success.
    Feed sync: Checking sync completion for feed set (vulnerabilities)...
    Feed sync: Checking sync completion for feed set (vulnerabilities)...
    ...
    ...
    Feed sync: Success.
