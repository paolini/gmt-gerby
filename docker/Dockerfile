## BUILD: 
# docker build -t gmt-gerby .
#
## RUN: 
# docker run -it --rm -p 8080:8080 gmt-gerby
#
## PUBLISH:
# VERSION=$( grep -i version ../setup.py | cut -f2 -d\")
# docker tag gmt-gerby paolini/gmt-gerby:latest
# docker tag gmt-gerby paolini/gmt-gerby:${VERSION}
# docker push paolini/gmt-gerby:latest 
# docker push paolini/gmt-gerby:${VERSION}

FROM alpine:3.14
ENV PYTHONUNBUFFERED=1
RUN apk add --update --no-cache python3 && ln -sf python3 /usr/bin/python
RUN python3 -m ensurepip
RUN pip3 install --no-cache --upgrade pip setuptools

#FROM python:3.7.3-slim
RUN python -m pip install --upgrade pip
RUN pip3 install gunicorn
RUN apk add --no-cache git
RUN git clone https://github.com/paolini/mdx_bleach
RUN python -m pip install ./mdx_bleach

RUN pip3 install gmt-gerby

COPY . /app

WORKDIR /app
EXPOSE 8080
CMD ["gunicorn","--config", "gunicorn_config.py", "app:app"]