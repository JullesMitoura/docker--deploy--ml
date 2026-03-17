# ── Imagem base ──────────────────────────────────────────────────────────────
# python:3.13-slim é o Python oficial num Debian mínimo (~50 MB vs ~1 GB full)
FROM python:3.13-slim

# ── Diretório de trabalho ─────────────────────────────────────────────────────
# Todos os caminhos relativos dentro do container partem daqui
WORKDIR /app

# ── Dependências ──────────────────────────────────────────────────────────────
# Copiamos o requirements.txt ANTES do código-fonte.
# Motivo: o Docker cacheia cada instrução. Se o código mudar mas as dependências
# não mudarem, o "pip install" não será re-executado — o build fica mais rápido.
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# ── Código-fonte e dados ───────────────────────────────────────────────────────
COPY src/  src/
COPY data/ data/

# ── Variáveis de ambiente ──────────────────────────────────────────────────────
# Valores padrão. Podem ser sobrescritos em runtime com -e ou --env-file.
ENV DB_PATH=data/heat_exchanger.db \
    MODEL_DIR=models \
    LOG_LEVEL=INFO

# ── Entrypoint padrão ─────────────────────────────────────────────────────────
# "docker run <imagem>" executa o treino.
# Para inferência, sobrescreva o CMD: "docker run <imagem> python src/inference.py ..."
CMD ["python", "src/train.py"]
