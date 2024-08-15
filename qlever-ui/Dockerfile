FROM ubuntu:latest
MAINTAINER Patrick Westphal

RUN apt update
RUN apt -y upgrade
RUN apt -y install git python3-pip python3-virtualenv

USER ubuntu
WORKDIR /home/ubuntu/qlever-ui

RUN git clone https://github.com/ad-freiburg/qlever-ui.git .

ENV VIRTUAL_ENV=qlever-ui-venv
RUN virtualenv $VIRTUAL_ENV
ENV PATH="$VIRTUAL_ENV/bin:$PATH"
RUN pip install -r requirements.txt

ENV SECRET_KEY topsecret

RUN echo "SECRET_KEY = '$SECRET_KEY'" > qlever/settings_secret.py
RUN echo "ALLOWED_HOSTS = ['*']" >> qlever/settings_secret.py

RUN python manage.py makemigrations --merge && python manage.py migrate

ENV DJANGO_SUPERUSER_USERNAME qlever
ENV DJANGO_SUPERUSER_PASSWORD secret

# dummy e-mail address!!!
ENV DJANGO_SUPERUSER_EMAIL foo@bar.com

RUN ./manage.py createsuperuser --username $DJANGO_SUPERUSER_USERNAME --email $DJANGO_SUPERUSER_EMAIL --noinput
RUN ./manage.py collectstatic

EXPOSE 7000

ENTRYPOINT ./start.sh

