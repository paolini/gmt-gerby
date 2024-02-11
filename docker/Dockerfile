
# docker build -t gmt-gerby .
# docker run -it --rm -p 8080:8080 gmt-gerby

FROM alpine:3.14
ENV PYTHONUNBUFFERED=1
RUN apk add --update --no-cache python3 && ln -sf python3 /usr/bin/python
RUN python3 -m ensurepip
RUN pip3 install --no-cache --upgrade pip setuptools

#FROM python:3.7.3-slim
RUN python -m pip install --upgrade pip
RUN pip3 install gmt-gerby==0.1.2
RUN pip3 install gunicorn
RUN apk add --no-cache git
RUN git clone https://github.com/paolini/mdx_bleach
RUN python -m pip install ./mdx_bleach

COPY . /app

WORKDIR /app
EXPOSE 8080
CMD ["gunicorn","--config", "gunicorn_config.py", "app:app"]