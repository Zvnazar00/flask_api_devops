
FROM python:3.9-slim


WORKDIR /tests

COPY requirements.txt /tests/requirements.txt
RUN pip install --no-cache-dir -r requirements.txt

COPY . /tests

CMD ["sh", "-c", "sleep 5 && python uat.py"]