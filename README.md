Steps taken to test

1) Created docker images locally on my machine using docker compose
    - Dockerfile uses the following line: ```RUN iris start IRIS && iris session IRIS < /tmp/iris.script && iris stop IRIS quietly```
    - iris.script will then change IRISLIB to RW, delete %DeepSee.Report package, make IRISLIB RO
2) Ran this twice while testing, commented out ```do $System.OBJ.DeletePackage("%DeepSee.Report")``` in iris.script on 2nd run to compare behavior
3) Exported images (testing durable sys on Windows doesn't work)
    - ```docker save -o wrc1000670Original.tar wrc1000670:Report```
    - ```docker save -o wrc1000670NoReport.tar wrc1000670:NoReport```
4) Imported to Ubuntu VM
    - ```sudo docker load -i wrc1000670Original.tar```
    - ```sudo docker load -i wrc1000670NoReport.tar```
5) Set up directory for durable sys
    - ```mkdir install```
    - ```sudo chown 51773:51773 install```
6) Run containers and check for %DeepSee.Report package
    - ```sudo docker run --name wrc1000670-Report --detach --publish 52774:52773 --volume ./install:/install --env ISC_DATA_DIRECTORY=/install wrc1000670:Report```
    - ```sudo docker run --name wrc1000670-NoReport --detach --publish 52773:52773 --volume ./install:/install --env ISC_DATA_DIRECTORY=/install wrc1000670:NoReport```