FROM archlinux:latest

# Update package database, keys and current packages
RUN echo "Server = http://mirror.digitalnova.at/archlinux/\$repo/os/\$arch" | tee /etc/pacman.d/mirrorlist

RUN pacman -Sy --noconfirm
RUN pacman-key --init
RUN pacman-key --populate archlinux
RUN pacman -S pacman openssl archlinux-keyring --noconfirm
RUN pacman-db-upgrade
# RUN pacman-key --keyserver keys.openpgp.org --refresh-keys

# System Upgrade
RUN pacman -Su --noconfirm

# Install base packages
RUN \
  pacman -S --noconfirm \
    base-devel \
    sudo \
    go \
    git

# Install AUR helper
RUN useradd -r -m docker
RUN echo "docker ALL=(ALL) NOPASSWD: ALL" | tee /etc/sudoers
USER docker

RUN mkdir -p /tmp/aur
WORKDIR /tmp/aur
RUN curl https://aur.archlinux.org/cgit/aur.git/snapshot/yay.tar.gz | tar xz
WORKDIR /tmp/aur/yay
RUN makepkg --noconfirm

USER root
RUN pacman -U --noconfirm *.tar.zst
USER docker

# Install pwmt dependencies
RUN \
  yay -S --noconfirm \
    appstream \
    cairo \
    djvulibre \
    doxygen \
    file \
    gettext \
    glib2 \
    gtk3 \
    json-glib \
    libmupdf \
    libnotify \
    librsvg \
    libseccomp \
    libspectre \
    libsynctex \
    meson \
    mime-types \
    ninja \
    poppler-glib \
    python \
    python-pip \
    shared-mime-info \
    sqlite \
    weston \
    xorg-server-xvfb \
    xorg-xauth

USER root
RUN pip3 install --break-system-packages \
      sphinx \
      sphinx-rtd-theme \
      breathe \
      git+https://github.com/jnikula/hawkmoth@master

# Cleanup
RUN pacman -Rscn yay go --noconfirm
RUN pacman -Scc --noconfirm

WORKDIR /
CMD ["/bin/bash"]
