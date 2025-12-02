FROM docker.1ms.run/ubuntu:22.04
ENV I_MPI_F90=ifx
ENV PATH=/root/bin:$PATH
WORKDIR /sisso_tmp/
RUN sh -c "apt update -y && apt install vim wget git -y"
RUN apt-get install -y --no-install-recommends \
        build-essential \
        gcc \
        g++ \
        gfortran \
        libc6-dev && \
    rm -rf /var/lib/apt/lists/*
RUN sh -c "git clone https://bgithub.xyz/rouyang2017/SISSO.git && wget https://registrationcenter-download.intel.com/akdlm/IRC_NAS/724303ca-6927-4327-a560-e0aabb55b010/intel-fortran-compiler-2025.3.1.16_offline.sh && wget https://registrationcenter-download.intel.com/akdlm/IRC_NAS/0a3cb474-49c7-45a9-9dea-104122592a63/intel-mpi-2021.17.0.377_offline.sh"
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