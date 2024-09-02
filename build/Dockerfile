FROM ubuntu:24.04

RUN apt-get update \
	&& apt-get install -y \
		ca-certificates \
		curl \
		git \
		keychain \
		mycli \
		neovim \
		php \
		sudo \
		wget \
		zsh \
		&& rm -rf /var/lib/apt/lists/*

COPY --from=composer/composer:latest-bin /composer /usr/bin/composer

# Install Docker
RUN install -m 0755 -d /etc/apt/keyrings \
	&& curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc \
	&& chmod a+r /etc/apt/keyrings/docker.asc \
	&& echo \
  		"deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  		$(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  		tee /etc/apt/sources.list.d/docker.list > /dev/null \	
	&& apt-get update \
	&& apt-get install -y \ 
		containerd.io \
		docker-buildx-plugin \
		docker-ce \
		docker-ce-cli \ 
		docker-compose-plugin

ARG USER=coder
RUN useradd --groups sudo --no-create-home --shell /bin/bash ${USER} \
	&& echo "${USER} ALL=(ALL) NOPASSWD:ALL" >/etc/sudoers.d/${USER} \
	&& chmod 0440 /etc/sudoers.d/${USER}
USER ${USER}
WORKDIR /home/${USER}

# Add user to docker group.
RUN sudo usermod -aG docker ${USER}

