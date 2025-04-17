# Use the Miniconda base image
FROM continuumio/miniconda3:latest

# Set the working directory
WORKDIR /workspace

# Create the Conda environment and install R and Python dependencies quietly
RUN conda create -n scalop_env r-essentials r-base python=3.11 -y -q && \
    conda clean -afy && \
    conda init bash

RUN apt-get update && apt-get install -y tmux && apt-get clean

# Activate the environment and install R packages quietly
RUN bash -c "source /opt/conda/etc/profile.d/conda.sh && \
    conda activate scalop_env && \
    conda install -c conda-forge r-curl r-textshaping r-ragg r-haven -y -q && \
    R -e \"options(repos = c(CRAN = 'https://cloud.r-project.org')); \
           if (!requireNamespace('BiocManager', quietly = TRUE)) install.packages('BiocManager', quiet = TRUE)\" && \
    R -e \"BiocManager::install('zellkonverter', ask = FALSE, quiet = TRUE)\" && \
    R -e \"BiocManager::install('SingleCellExperiment', ask = FALSE, quiet = TRUE)\" && \
    R -e \"install.packages('Matrix', repos='https://cloud.r-project.org', quiet = TRUE)\" && \
    R -e \"install.packages('dplyr', repos='https://cloud.r-project.org', quiet = TRUE)\" && \
    R -e \"install.packages('remotes', repos='https://cloud.r-project.org', quiet = TRUE)\" && \
    R -e \"remotes::install_github('JeanRadig/scalop')\" && \
    R -e \"install.packages('tidyverse', repos='https://cloud.r-project.org', quiet = TRUE)\""

# Install Python dependencies quietly
RUN bash -c "source /opt/conda/etc/profile.d/conda.sh && \
    conda activate scalop_env && \
    pip install --quiet --upgrade pip && \
    conda install -y -q ipykernel && \
    python -m ipykernel install --user --name=scalop_env --display-name 'Python (scalop_env)'"

# Set the default shell to Bash
CMD ["/bin/bash"]
