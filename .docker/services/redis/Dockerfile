FROM redis:6.0.16
LABEL authors="statuscu"

ARG MAGENTO_USER_WEB
ARG MAGENTO_UID_WEB

RUN apt-get update && apt-get install -y sudo
RUN useradd -m -u ${MAGENTO_UID_WEB} -s /bin/bash ${MAGENTO_USER_WEB} && \
    echo "${MAGENTO_USER_WEB} ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

RUN apt-get install -y mc

USER ${MAGENTO_USER_WEB}
