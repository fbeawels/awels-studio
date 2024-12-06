FROM python:3.10

WORKDIR /code

RUN apt-get update && \
    apt-get install -y --force-yes --no-install-recommends \
        npm rsync && \
    apt-get autoclean && \
    apt-get autoremove && \
    rm -rf /var/lib/apt/lists/*

COPY requirements.txt requirements.txt
COPY frontend frontend
COPY autogenstudio autogenstudio
COPY setup.py setup.
COPY pyproject.toml pyproject.toml 

RUN pip install --upgrade pip && \
    pip install -r requirements.txt

RUN pip install -e .

RUN cd frontend && \
    npm install -g gatsby-cli && \
    npm install --global yarn && \
    yarn install && \
    yarn build

RUN useradd -m -u 1001 awels && \
    mkdir -p /studio /home/awels/app/.autogenstudio && \
    chown -R awels.awels /studio /home/awels/.autogenstudio && \
    chmod -R 666 /studio /home/awels/.autogenstudio

USER awels
ENV HOME=/home/awels \
    PATH=/home/awels/.local/bin:$PATH \
    AUTOGENSTUDIO_APPDIR=/home/awels/app
    
WORKDIR $HOME/app

EXPOSE 8081

CMD ["autogenstudio", "ui", "--host", "0.0.0.0", "--port", "8081", "--appdir", "/studio"]