FROM node:lts-alpine3.12

LABEL AUTHOR="none" \
      VERSION=0.1.4

ARG KEY="-----BEGIN RSA PRIVATE KEY-----\nMIIEowIBAAKCAQEA5Hkz6pLTTYEDSXBOnFJYjQpvlJzzOkN7Z8vaQv6enZC7cHFC\n2cXK86wo7Q71sg/zCAX5KS6hB82DIgfjqlMO8XmlW32wKjEibEoYuAmlaGZdoGMU\nL1HcuzwMHiI0H1p0+ZYpo8QXPvIc5IviYN+QKP5+lPuXhlrXOrj3+S5dEDFWnCOw\n2M5pV2InCFl63i5Cgt0HjDReoqGnujwS0JF+bNIHuvi0C9SdOWn+ADXKyHi6h2sa\neJ2h0fBgww8R4m8HLngyc8v2oZz/I2rcpdrqOLUJB17bXW7V7EM1aFrcq2FQzrgz\npYnKojGrd1E9Qtr0bobRG7Yukd9k9BauN9lXHQIDAQABAoIBAH0a2RDDYSz5TgFy\nm7kBZjek1UB9SLpIpVRKzbMNlXEWBW0mt8JjiiQvyyEzpdng3eUw0CR6wfZx1DJY\nO5PSc0ZKo/8bLdGMohrF4A82SPqHwxDF8BGRM9raoJAHjp70e0UHsrlX8JqpHKUQ\n6967zEaGRG3z7h7Fs0EWc8JgXtk0AhXqXz4i/zOF9b3Ae1oec2xBKGJLUEIlXA5J\nmmWUkvI+qbP/0eoL4Zqtjmzg4FgUSqj/dvruQysYwT83qxfJHMqypDAx8/Vh5IwB\n5BruYG1XMUGhfKGQcJrXXB1WvA06zXmryU1CB9U5CXFkqSmZCdwLfxGl/PIQGncU\nIbqUFgECgYEA9/+VPPEBY42puDbsniiNGH/DjlXjVkwiNyjfQzWWej1t9I5KtaPu\naAWg30bQdd4pBA2GospgmG6F4E0/SW0NjAN7h2dp45hSG6N+B0fm8liPmLVDJKzC\ntneXe2C8gNtG4AkzBL3KbplpYiexqIwvlx5H0Yzw3s/wIJFGRT8y410CgYEA69hZ\nDk29YED81gfPnPyrGPUgqOIrZ84PtdMGCB/YjotneI3Qxz/Akqn/Ph+nwINhvdQ0\nR43JFbSnuIRbnZpnOg2HYkAHtBsSHoKpTheGkgXJT1QhAUXJoGyGPUjtvy+Ef9Wl\nzksRUWVqFpZcQ2qCMVG/rqrprMfZHs+90FkKxsECgYEA7p0iqaU18eSvBQkLt81K\nnOTpcr0e5LySO0J/NrqL04fhkOmL5qMZU0r/E3WeQ5qDGlwtS9qvAFtvQnbwyqQ7\n4ZjENW8CCjNX5TH0o+rBwjIF0Cevt8zl+AyvWs2j8dOJqSwQw6Ic8Td4lzGcFZD0\nhVFkRrJ2W9v0rlE5LI13WB0CgYA1f2PkHLpCZTt1CVyMzbu585Cu/m6+Iz3pDq8l\nlKO44eJctD6Rk+KI4krluqSAxMydFM+vE3hzlgEdl9h4TZfw0TTOkpusvR9FEHp7\nHlcGNI3PktPRyBTOcQ+aQzq6zXqOzNXJj7mj7w87hGv1VumlalfCtT5AVSzpElKK\nOkh5gQKBgDWYWHFdfdnvuFdaUq6RAhLuZ0Qi5MkE+yn7mcYAViuRmLNB2g45N5XJ\nHkaJF0W8qlFSfNxneTccaxoSVuSdJstGuOIXBSDpALeQYB+7vi2orpiniCIebeky\nvtTXtneO4YC2ZjpTZez++x+x/0AKJxHXbhUOXJLw3qss6xYBnvQc\n-----END RSA PRIVATE KEY-----"

ENV DEFAULT_LIST_FILE=crontab_list.sh \
    CUSTOM_LIST_MERGE_TYPE=append \
    COOKIES_LIST=/scripts/logs/cookies.list \
    REPO_URL=git@gitee.com:underlaw/jd_scripts.git \
    REPO_BRANCH=master

RUN set -ex \
    && apk update \
    && apk upgrade \
    && apk add --no-cache bash tzdata git moreutils curl jq openssh-client python3 py3-pip \
    && rm -rf /var/cache/apk/* \
    && ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && echo "Asia/Shanghai" > /etc/timezone \
    && mkdir -p /root/.ssh \
    && echo -e $KEY > /root/.ssh/id_rsa \
    && chmod 600 /root/.ssh/id_rsa \
    && ssh-keyscan gitee.com > /root/.ssh/known_hosts \
    && git clone -b $REPO_BRANCH $REPO_URL /scripts \
    && cd /scripts \
    && mkdir logs \
    && python3 -m pip install requests \
    && npm config set registry https://registry.npmjs.org \
    && npm install \
    && cp /scripts/docker/docker_entrypoint.sh /usr/local/bin \
    && chmod +x /usr/local/bin/docker_entrypoint.sh

WORKDIR /scripts

ENTRYPOINT ["docker_entrypoint.sh"]

CMD [ "crond" ]
