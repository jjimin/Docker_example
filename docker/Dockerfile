# for pyTorch-1.3 with docker

FROM pytorch/pytorch:1.3-cuda10.1-cudnn7-devel

USER root
RUN apt-get update
RUN apt-get install -y wget gedit eog


##### Install Sublime Text
# RUN apt-get update
# RUN wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | apt-key add -
# RUN echo "deb https://download.sublimetext.com/ apt/stable/" | tee /etc/apt/sources.list.d/sublime-text.list
# RUN apt-get update
# RUN apt-get install -y sublime-text


RUN pip install protobuf \
				matplotlib \
				future \
				scipy


##### Set the system configuration
USER torch
ENV HOME /home/torch

USER root
RUN apt-get -y install sudo
RUN adduser torch
RUN echo "torch:torch" | chpasswd && adduser torch sudo

USER torch
WORKDIR /home/torch

ENV SDL_VIDEODRIVER=x11

CMD /bin/bash
