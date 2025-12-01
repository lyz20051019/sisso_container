FROM ubuntu:22.04
ENV I_MPI_F90=ifx
ENV PATH=/root/bin:$PATH
WORKDIR /sisso_tmp/
#RUN sh -c "apt update -y && apt install vim wget git -y"
RUN apt-get update && apt-get install -y --no-install-recommends \
        build-essential \
        gcc \
        g++ \
        gfortran \
        libc6-dev && \
    rm -rf /var/lib/apt/lists/*
COPY ./SISSO /sisso_tmp/SISSO
COPY ./intel-fortran-compiler-2025.3.1.16_offline.sh ./intel-mpi-2021.17.0.377_offline.sh ./
RUN sh -c "chmod +x intel-fortran-compiler-2025.3.1.16_offline.sh intel-mpi-2021.17.0.377_offline.sh"
ENV TERM=linux
RUN sh -c "./intel-mpi-2021.17.0.377_offline.sh -a -s --eula accept && ./intel-fortran-compiler-2025.3.1.16_offline.sh -a -s --eula accept"
RUN sh -c "mkdir /root/bin"
WORKDIR /sisso_tmp/SISSO/src
RUN sh -c ". /opt/intel/oneapi/setvars.sh --force && mpiifort -fp-model precise var_global.f90 libsisso.f90 DI.f90 FC.f90 FCse.f90 SISSO.f90 -o ~/bin/SISSO"
ENV LD_LIBRARY_PATH=/opt/intel/oneapi/compiler/2025.3/lib:$LD_LIBRARY_PATH
ENV PATH=/opt/intel/oneapi/mpi/2021.17/bin:$PATH
ENV LD_LIBRARY_PATH=/opt/intel/oneapi/mpi/2021.17/lib:$LD_LIBRARY_PATH
WORKDIR /sisso_workspace
RUN sh -c "rm -rf /sisso_tmp"