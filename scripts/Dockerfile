FROM conda/miniconda3


MAINTAINER Ravin Kumar

ARG SRC_DIR

ARG PYTHON_VERSION
ARG PYSTAN_VERSION
ARG PYRO_VERSION

ENV PYTHON_VERSION=${PYTHON_VERSION}
ENV PYSTAN_VERSION=${PYSTAN_VERSION}
ENV PYRO_VERSION=${PYRO_VERSION}
ENV DOCKER_BUILD=true

# For Sphinx documentation builds
ENV LC_ALL=C.UTF-8
ENV LANG=C.UTF-8

RUN apt-get update && apt-get install -y git build-essential pandoc \
    && rm -rf /var/lib/apt/lists/*

# Copy requirements and environment installation scripts
COPY $SRC_DIR/requirements.txt  opt/arviz/
COPY $SRC_DIR/requirements-dev.txt  opt/arviz/
COPY $SRC_DIR/scripts/ opt/arviz/scripts

WORKDIR /opt/arviz


# Create conda environment. Defaults to Python 3.6
RUN ./scripts/create_testenv.sh

RUN conda clean --all

COPY $SRC_DIR /opt/arviz

# Clear any cached models from copied from host filesystem
RUN rm -f arviz/tests/saved_models/*.pkl
RUN find -type d -name __pycache__ -exec rm -rf {} +

CMD ["/bin/bash", "-c", "source /root/.bashrc && pytest -vv arviz/tests --cov=arviz/ && \
                         echo Finished Testing"]
