# focal is ubuntu 20.04 LTS fossa focal
ARG VARIANT="focal"
# ms dev containers,  made to work with vscode remote containers extension
FROM mcr.microsoft.com/vscode/devcontainers/base:0-${VARIANT}
USER root
WORKDIR /home/app
RUN apt-get update
RUN apt-get -y install curl gnupg
RUN curl -sL https://deb.nodesource.com/setup_14.x  | bash -
RUN apt-get -y install nodejs
RUN npm install
RUN npm install -g truffle@5.3.4
