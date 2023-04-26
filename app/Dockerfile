FROM python:3.11-alpine3.17

WORKDIR /app
COPY . .
RUN pip3 install -r requirements.txt

ENV FLASK_APP userapi.py
ENV FLASK_RUN_PORT 8000
ENV FLASK_RUN_HOST 0.0.0.0

EXPOSE 8000

CMD ["flask", "run"]
