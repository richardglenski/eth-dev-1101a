DEVONDKR / eth_dev_1101

Ethereum dev on docker containers with docker compose

See my google doc:  ETH-DEV-1101 Notes

Adapted from:
https://medium.com/coinmonks/ethereum-development-in-docker-a6bd11d00ef2



============    Git commands   ============

git init

git add README.md

git commit -m "initial setup commit"

git branch -M main

git remote add origin https://github.com/richardglenski/eth-dev-1101
git remote remove origin
git remote add origin  https://github.com/richardglenski/eth-dev-1101a.git
git push origin main

git pull origin main

git push -u origin main

git remote -v

git branch -M main

git push -u origin main



============    CREATE   ============


mkdir -p /Users/richardglenski/Desktop/Dev/DevOnDkr/eth_dev_1101
cd /Users/richardglenski/Desktop/Dev/DevOnDkr/eth_dev_1101


mkdir docker
cd docker
mkdir truffle-suite
cd truffle-suite
touch Dockerfile

# focal is ubuntu 20.04 LTS fossa focal
ARG VARIANT="focal"
FROM mcr.microsoft.com/vscode/devcontainers/base:0-${VARIANT}
USER root
WORKDIR /home/app
RUN apt-get update
RUN apt-get -y install curl gnupg
RUN curl -sL https://deb.nodesource.com/setup_14.x  | bash -
RUN apt-get -y install nodejs
RUN npm install
RUN npm install -g truffle@5.3.4


cd ..
mkdir ganache-cli
cd ganache-cli
touch Dockerfile


FROM trufflesuite/ganache-cli:latest
RUN /bin/sh -c "apk add --no-cache bash"
ENTRYPOINT node /app/ganache-core.docker.cli.js --quiet --networkId 57771 --verbose --host 0.0.0.0


cd ../..
touch docker-compose.yml


version: "3"
services:
    # start the ganache cli container
    ganache-cli:
        container_name: g1101-ganache-cli
        build:
            context: ./docker/ganache-cli
        # note host:container
        ports:
            - 5545:8545
    truffle_suite:
        container_name: t1101-truffle_suite
        build:
            context: ./docker/truffle-suite
                # volumes
        volumes:
            - ".:/workspace"
        # command: >
        #     bash
        stdin_open: true # docker run -i
        tty: true # docker run -t


============   BUILD   ============


open terminal ( in vscode )

$ pwd
/Users/richardglenski/Desktop/Dev/DevOnDkr/eth_dev_1101

$ docker compose up

verify no errors-- takes a while


============   TEST  DEV SPACE  ============


Open new terminal ( in vscode ), navigate to your project base dir

$ pwd
/Users/richardglenski/Desktop/Dev/DevOnDkr/eth_dev_1101
 
 and enter:

$ docker exec -it t1101-truffle_suite bash

We are in /home/app

root ➜ /home/app $ truffle version
Truffle v5.3.4 (core: 5.3.4)
Solidity v0.5.16 (solc-js)
Node v14.18.1
Web3.js v1.3.5
root ➜ /home/app $ 


============   EXPLORE  DEV SPACE  ============

(stay in container)

root ➜ /home/app $ pwd
/home/app

$ ls -all /home/app 
total 16
drwxr-xr-x 1 root root 4096 Nov  2 01:55 .
drwxr-xr-x 1 root root 4096 Nov  2 01:54 ..
-rw-r--r-- 1 root root   27 Nov  2 01:55 package-lock.json


Go to the Dev project in the container

cd /workspace

$ ls -all /workspace
total 12
drwxr-xr-x 5 root root  160 Nov  2 02:31 .
drwxr-xr-x 1 root root 4096 Nov  2 02:08 ..
drwxr-xr-x 4 root root  128 Nov  2 02:20 docker
-rw-r--r-- 1 root root  572 Nov  2 02:08 docker-compose.yml
-rw-r--r-- 1 root root 1852 Nov  2 02:31 readme_1101_setup.txt


============   DEV EXAMPLE   ============

use the  ‘metacoin’ example in the Docker terminal,

$ cd /workspace
$ truffle unbox metacoin

this buolds and should work:


take down docker compose set
in a 3rd (seperate) terminal

$ docker compose down

