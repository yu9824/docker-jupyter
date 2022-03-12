# Base image
FROM continuumio/anaconda3:2021.11

# Set timezone
ENV TZ="Asia/Tokyo"

# Add conda-forge channel for optuna and boruta_py
RUN conda config --add channels conda-forge

# Create myenv
ARG env_name="myenv"
RUN conda create -yn ${env_name} python=3.8.8 jupyter

# Activate environment
ENV CONDA_DEFAULT_ENV ${env_name}

# Switch default environment
RUN echo "conda activate ${env_name}" >> ~/.bash_profile
ENV PATH /opt/conda/envs/${env_name}/bin:$PATH

# Add user
ARG user_name
RUN useradd -s /bin/bash -m ${user_name}
USER ${user_name}

# Move to working directory
ARG dir_name
ARG user_home_dir=/home/${user_name}
WORKDIR ${user_home_dir}/${dir_name}/

# jupyter config
RUN jupyter lab --generate-config
RUN ipython kernel install --user --name=${env_name} --display-name=${env_name}
ARG fpath_jupyter_config=${user_home_dir}/.jupyter/jupyter_lab_config.py
RUN echo "c.NotebookApp.ip = '0.0.0.0'" >> ${fpath_jupyter_config}
RUN echo "c.NotebookApp.port = 8888" >> ${fpath_jupyter_config}
