FROM python:3.12-slim

WORKDIR /api

COPY api.py ./

RUN pip install Flask

HEALTHCHECK --interval=2s --timeout=5s --retries=3 CMD pwd || exit 1

EXPOSE 5010

ENV FLASK_APP=api.py

CMD ["flask", "run", "--host=0.0.0.0", "--port=5000"] 