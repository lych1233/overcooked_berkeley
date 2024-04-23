# FROM python:3.7-stretch
FROM python:3.10

ARG BUILD_ENV
ARG OVERCOOKED_BRANCH
ARG GRAPHICS

WORKDIR /app

# Install non-chai dependencies
# COPY ./requirements_new.txt ./requirements_new.txt
# RUN pip install -r requirements_new.txt

# Install eventlet production server if production build
RUN if [ "$BUILD_ENV" = "production" ] ; then pip install eventlet ; fi

# Clone chai code
# RUN git clone --recursive https://github.com/HumanCompatibleAI/overcooked_ai.git --branch $OVERCOOKED_BRANCH --single-branch /overcooked_ai
RUN git clone --recursive https://github.com/lych1233/overcooked_berkeley.git --branch $OVERCOOKED_BRANCH --single-branch /overcooked_ai

# Dummy data_dir so things don't break
RUN echo "import os; DATA_DIR=os.path.abspath('.')" >> /overcooked_ai/src/human_aware_rl/data_dir.py

# Install chai dependencies
RUN pip install -e '/overcooked_ai[harl]'

# Added custom pip installs
RUN pip install eventlet flask flask_socketio torch
# torchvision torchaudio

RUN apt-get -y update
RUN apt-get install -y libgl1-mesa-dev

# Copy over remaining files
COPY ./static ./static
COPY ./*.py ./
COPY ./graphics/$GRAPHICS ./static/js/graphics.js
COPY ./config.json ./config.json

# Add new layouts
# COPY ././././data/layouts ././././data/layouts

# Add new checkpoint files from custom trained policy (VAE FCP)
# Already done in ./static -> ./static (where all checkpoint files are located)

# Set environment variables that will be used by app.py
ENV HOST 0.0.0.0
ENV PORT 5000
ENV CONF_PATH config.json

# Do the thing
EXPOSE 5000
CMD ["python", "-u", "app.py"]
# CMD /bin/bash

