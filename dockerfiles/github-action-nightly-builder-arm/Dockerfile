# Image for building async-profiler release packages for arm64 on github actions

FROM public.ecr.aws/codebuild/amazonlinux2-aarch64-standard:3.0

#libicu is needed for the github actions setup script to run
RUN yum update -y && yum install -y libicu tar sudo patchelf make gcc-c++ libstdc++-static java-11-amazon-corretto

ARG musl_src=musl-1.2.5
ARG musl_sha256=a9a118bbe84d8764da0ea0d28b3ab3fae8477fc7e4085d90102b8596fc7c75e4

ADD https://musl.libc.org/releases/${musl_src}.tar.gz /
RUN echo ${musl_sha256} ${musl_src}.tar.gz | sha256sum -c

RUN ["/bin/bash", "-c", "\
    tar xfz ${musl_src}.tar.gz && \
    cd /${musl_src} && \
    ./configure --disable-shared --prefix=/usr/local/musl/aarch64 && \
    make -j`nproc` && make install && make clean && \
    ln -s /usr/include/{asm,asm-generic,linux} /usr/local/musl/aarch64/include/"]
