FROM manjarolinux/base:latest

RUN pacman --noconfirm --sync --refresh --sysupgrade;\
    pacman --noconfirm --sync  yay;\
    yay --noconfirm --sync --refresh;\
    yay --noconfirm --sync  git wget sed tar gzip aur/android-sdk-platform-tools ;\
    yay --noconfirm --sync --refresh

COPY ./scripts /scripts
RUN chmod +x /scripts/*

RUN /scripts/install-go 1.21.0;\
    ln -s /opt/go/1.21.0/bin/go /usr/local/bin/go
