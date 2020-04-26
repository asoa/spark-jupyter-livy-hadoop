FROM jupyter/base-notebook

USER root
RUN conda update -n base -y conda && \
    conda install -y requests-kerberos && \
        conda install -y -c conda-forge plotly "jupyterlab>=1.0" jupyterlab-dash=0.1.0a3 widgetsnbextension nodejs ipywidgets

USER $NB_USER

RUN pip install --upgrade requests-kerberos pyspark

USER root

RUN apt update && apt install -y git && \
    rm -rf /var/lib/apt/lists/*

RUN cd /opt/conda/lib/python3.7/site-packages && \
    fix-permissions /home/$NB_USER

COPY ./jdk-8u221-linux-x64.tar.gz /usr/lib/jvm/

USER root

RUN cd /usr/lib/jvm && \
    tar -xzvf /usr/lib/jvm/jdk-8u221-linux-x64.tar.gz && \
    rm jdk-8u221-linux-x64.tar.gz


WORKDIR /home/$NB_USER

USER root
CMD ["start-notebook.sh", "--NotebookApp.iopub_data_rate_limit=1000000000"]

USER $NB_USER
ENV PATH="/usr/lib/jvm/jdk1.8.0_221/bin:$PATH"
