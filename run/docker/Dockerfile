FROM python:3.9

ENV DEBUG=False
ENV HOST_NAME=*
# Keep the DB file in a separate directory, which can be shared as a volume.
ENV SQLITE_DB_FILE=/srv/db/db.sqlite3

WORKDIR /srv

COPY . .

RUN mkdir `dirname $SQLITE_DB_FILE`

RUN pip install --no-cache-dir -Ur requirements/base-full.txt

RUN python manage.py collectstatic --noinput

EXPOSE 8000

# Install dos2unix only if needed and convert line endings if necessary
RUN if file /srv/run/docker/start.sh | grep -q "CRLF"; then \
    apt-get update && apt-get install -y dos2unix && \
    dos2unix /srv/run/docker/start.sh && \
    apt-get remove -y dos2unix && \
    apt-get autoremove -y && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*; \
    fi

RUN chmod +x /srv/run/docker/start.sh

CMD ["/srv/run/docker/start.sh"]