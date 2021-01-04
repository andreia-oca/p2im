FROM ubuntu:16.04
ARG VERSION=1

LABEL author="andreia.ocanoaia@gmail.com" \
      name="P2IM Testing" \
      version="${VERSION}"

WORKDIR /root/

# Install missing packages and required dependencies
RUN apt-get update && \
    DEBIAN_FRONTEND="noninteractive" apt-get install -y wget vim ssh git python3 python3-pip \
    libboost-all-dev libz3-dev cmake

# Clone P2IM and it's submodules
RUN git clone --recursive https://github.com/andreia-oca/p2im.git p2im

# Compile AFL
RUN ["/bin/bash", "-c", "cd /root/p2im && make -C afl/"]

# Download and install GNU Arn Enbedded Toolchain
RUN wget "https://developer.arm.com/-/media/Files/downloads/gnu-rm/10-2020q4/gcc-arm-none-eabi-10-2020-q4-major-x86_64-linux.tar.bz2?revision=ca0cbf9c-9de2-491c-ac48-898b5bbc0443&la=en&hash=68760A8AE66026BCF99F05AC017A6A50C6FD832A" -O "gcc-arm-none-eabi-10-2020-q4-major.tar.bz2"
RUN tar xjf gcc-arm-none-eabi-10-2020-q4-major.tar.bz2
ENV PATH=$PATH:/root/gcc-arm-none-eabi-10-2020-q4-major/bin/

# Uncomment the following lines to ignore the 
# errors/warnings about crashes and cpu clock frequency coming from AFL. 
# After changing this file recompile it with `make build`.
# ENV AFL_I_DONT_CARE_ABOUT_MISSING_CRASHES=1
# ENV AFL_SKIP_CPUFREQ=1

# Fuzz with AFL on a precompiled, real world firmware 
# found in P2IM github repo.
ENV WORKING_DIR=/root/p2im/fuzzing/heat_press/01
RUN mkdir -p $WORKING_DIR
RUN cp /root/p2im/fuzzing/configs/Heat_Press.cfg $WORKING_DIR/fuzz.cfg
RUN cp -r /root/p2im/fuzzing/templates/seeds/ $WORKING_DIR/inputs
RUN cp /root/p2im/externals/p2im-real_firmware/binary/Heat_Press $WORKING_DIR/

# Run example test when container starts
CMD /root/p2im/model_instantiation/fuzz.py -c $WORKING_DIR/fuzz.cfg
