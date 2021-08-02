FROM ubuntu:20.04

ARG DEBIAN_FRONTEND=noninteractive

# Install dependencies
WORKDIR /install
COPY ./apt-list.txt .
RUN apt-get update && \
    cat apt-list.txt | \
    sed 's/#.*$//g' | \
    sed '/^$/d' | \
    tr '\n' ' ' | \
    xargs apt-get install -y

# Links for python, pip, and python debugger
RUN ln -sf /usr/bin/python3 /usr/bin/python && \
    ln -sf /usr/bin/pip3 /usr/bin/pip && \
    ln -sf /usr/bin/pdb3 /usr/bin/pdb

# Python/Flask dev dependencies
COPY ./dev_requirements.txt /opt/
RUN pip install --upgrade pip
# Installing first pip-licenses so we can track what licenses/versions of packages
RUN pip install -U pip-licenses
RUN pip install --no-cache-dir -r /opt/dev_requirements.txt

# Clean up
RUN apt-get autoremove -y && \
    apt-get clean -y && \
    rm -rf /var/lib/apt/lists/*

CMD [ "/bin/sh" ]