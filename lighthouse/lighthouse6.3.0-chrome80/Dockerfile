FROM debian:buster-slim

# Install deps
RUN apt-get update && apt-get install -y \
  apt-transport-https \
  ca-certificates \
  curl \
  gnupg \
  --no-install-recommends \
  && curl -sSL https://deb.nodesource.com/setup_12.x | bash - \
  && apt-get update && apt-get install -y \
  fontconfig \
  fonts-ipafont-gothic \
  fonts-wqy-zenhei \
  fonts-thai-tlwg \
  fonts-kacst \
  fonts-symbola \
  fonts-noto \
  fonts-freefont-ttf \
  nodejs \
  --no-install-recommends \
  && apt-get purge --auto-remove -y curl gnupg \
  && rm -rf /var/lib/apt/lists/*

# Install chrome Stable
RUN apt-get update && apt-get install -y snapd
COPY google-chrome-stable_current_amd64_80.0.3987.149.deb /google-chrome-stable_current_amd64_80.0.3987.149.deb
RUN apt install -y /google-chrome-stable_current_amd64_80.0.3987.149.deb

# Install lighthouse
ARG CACHEBUST=1
RUN npm install -g lighthouse@6.3.0

# Add Chrome as a user
RUN groupadd -r chrome && useradd -r -g chrome -G audio,video chrome \
  && mkdir -p /home/chrome/reports && chown -R chrome:chrome /home/chrome

# some place we can mount and view lighthouse reports
VOLUME /home/chrome/reports
WORKDIR /home/chrome/reports

# Run Chrome non-privileged
USER chrome

# Drop to cli
CMD ["/bin/bash"]
