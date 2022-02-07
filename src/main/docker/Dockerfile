FROM httpd

RUN apt-get update && apt install -y curl

COPY *.sh ./
RUN chmod +x *.sh

HEALTHCHECK --interval=3s --timeout=3s --retries=10 CMD curl -f http://localhost/ok || exit 1

ENTRYPOINT ["./startup.sh"]
