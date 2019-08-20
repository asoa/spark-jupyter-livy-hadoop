FROM jupyter/base-notebook

USER root
RUN conda install requests-kerberos -y 

USER $NB_USER
# install spark magic and kernels
RUN pip install --upgrade requests-kerberos pyspark sparkmagic && \
    jupyter nbextension enable --py --sys-prefix widgetsnbextension && \
    jupyter labextension install @jupyter-widgets/jupyterlab-manager

USER root
#RUN apt update && apt install -y default-jdk && \
#    rm -rf /var/lib/apt/lists/*

 
RUN cd /opt/conda/lib/python3.7/site-packages && \
    jupyter-kernelspec install sparkmagic/kernels/sparkkernel && \
    jupyter-kernelspec install sparkmagic/kernels/pysparkkernel && \
    jupyter-kernelspec install sparkmagic/kernels/sparkrkernel && \
    jupyter serverextension enable --py sparkmagic && \
    fix-permissions /home/$NB_USER

COPY ./jdk-8u221-linux-x64.tar.gz /usr/lib/jvm/

USER root
RUN cd /usr/lib/jvm && \
    tar -xzvf /usr/lib/jvm/jdk-8u221-linux-x64.tar.gz && \
    rm jdk-8u221-linux-x64.tar.gz

WORKDIR /home/$NB_USER

USER $NB_USER

RUN mkdir /home/$NB_USER/.sparkmagic

COPY ./example_config.json /home/$NB_USER/.sparkmagic/config.json

USER root
RUN chown $NB_USER /home/$NB_USER/.sparkmagic/config.json

CMD ["start-notebook.sh", "--NotebookApp.iopub_data_rate_limit=1000000000"]

USER $NB_USER
ENV PATH="/usr/lib/jvm/jdk1.8.0_221/bin:$PATH"
ENV GRANT_SUDO="yes"
