#-*- coding: utf-8 -*-
#
# This file is part of Invenio.
#
# Copyright (C) 2019-2020 CERN.
# Copyright (C) 2019-2020 Northwestern University.
#
# Invenio is free software; you can redistribute it and/or modify it
# under the terms of the MIT License; see LICENSE file for more details.
#
# Dockerfile that builds a fully functional image of your app.
#
# This image installs all Python dependencies for your application. It's based
# on CentOS 7 with Python 3 (https://github.com/inveniosoftware/docker-invenio)
# and includes Pip, Pipenv, Node.js, NPM and some few standard libraries
# Invenio usually needs.
#
# Note: It is important to keep the commands in this file in sync with your
# bootstrap script located in ./scripts/bootstrap.

FROM inveniosoftware/centos7-python:3.6

ARG include_assets

COPY Pipfile Pipfile.lock ./
RUN pipenv install --deploy --system

COPY ./docker/uwsgi/ ${INVENIO_INSTANCE_PATH}
COPY ./invenio.cfg ${INVENIO_INSTANCE_PATH}
COPY ./templates/ ${INVENIO_INSTANCE_PATH}/templates/
COPY ./app_data/ ${INVENIO_INSTANCE_PATH}/app_data/
COPY ./ .

RUN if [ "$include_assets" = "true" ]; \
    then \
        cp -r ./static/. ${INVENIO_INSTANCE_PATH}/static/ && \
        cp -r ./assets/. ${INVENIO_INSTANCE_PATH}/assets/ && \
        invenio collect --verbose  && \
        invenio webpack buildall \
    ; fi

ENTRYPOINT [ "bash", "-c"]
