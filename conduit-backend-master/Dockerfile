# -------- Stage 1: Builder --------
FROM python:3.6-slim AS builder
WORKDIR /app

RUN apt-get update && apt-get install -y gcc build-essential libpq-dev && apt-get clean

# requirements.txt is in the same folder as Dockerfile
COPY requirements.txt .
RUN pip install --user --no-cache-dir -r requirements.txt

# -------- Stage 2: Final --------
FROM python:3.6-slim AS final
WORKDIR /app

RUN apt-get update && apt-get install -y sqlite3 && apt-get clean

# Copy project files (requirements already installed from builder stage)
COPY . .
COPY --from=builder /root/.local /root/.local

ENV PATH="/root/.local/bin:${PATH}"

EXPOSE 8000

CMD ["gunicorn", "-w", "4", "-b", "0.0.0.0:8000", "conduit.wsgi:application"]