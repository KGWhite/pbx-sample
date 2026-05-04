FROM postgres:18.3-alpine

# Default environment variables for Postgres
ENV POSTGRES_USER=postgres
ENV POSTGRES_PASSWORD=postgres
ENV POSTGRES_DB=pbx

# Copy initialization scripts if they exist
# COPY ./init.sql /docker-entrypoint-initdb.d/

EXPOSE 5432
