FROM asmorkalov/riscv-fedora

COPY lib /new_lib
RUN echo "/new_lib" > /etc/ld.so.conf.d/modern_gcc_runtime.conf && ldconfig

RUN useradd ci -m -s /bin/bash -G users
USER ci

CMD bash
