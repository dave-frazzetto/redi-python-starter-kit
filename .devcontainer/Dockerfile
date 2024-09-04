# install Python
FROM python:3.11


# install OS packages: Core utils, Kerberos client, unixODBC, Java
RUN apt-get -y update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends inetutils-ping vim build-essential sl \
    && apt-get -y install --no-install-recommends default-jre
ENV JAVA_HOME /usr/lib/jvm/default-java/


# VSCODE: Create the non-root user and give it sudo capabilities, to avoid file permissions issues between host & container
ARG VSCODE_USERNAME=vscode
ARG VSCODE_USER_UID=1000
ARG VSCODE_USER_GID=1000
RUN groupadd --gid ${VSCODE_USER_GID} ${VSCODE_USERNAME} \
    && useradd --uid ${VSCODE_USER_UID} --gid ${VSCODE_USER_GID} -m ${VSCODE_USERNAME} \
    && apt-get update \
    && apt-get install -y sudo \
    && echo $VSCODE_USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$VSCODE_USERNAME \
    && chmod 0440 /etc/sudoers.d/$VSCODE_USERNAME

# Additional Python packages - installed as ROOT in "/usr/local/bin/python"
COPY .devcontainer/requirements.txt /tmp/pip-tmp/
RUN pip install --no-cache-dir --upgrade pip && \
    pip install --no-cache-dir -r /tmp/pip-tmp/requirements.txt

# start the container as USERNAME
USER $VSCODE_USERNAME
