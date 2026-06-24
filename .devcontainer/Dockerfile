
FROM kalilinux/kali-rolling

ENV DEBIAN_FRONTEND=noninteractive

RUN apt update && apt upgrade -y

# Install essential tools and desktop environment (XFCE) for GUI
RUN apt install -y \
    git \
    curl \
    wget \
    unzip \
    python3 \
    python3-pip \
    nmap \
    amass \
    wfuzz \
    whatweb \
    kali-desktop-xfce \
    tigervnc-standalone-server \
    xterm \
    sudo \
    openssh-server \
    && apt clean

# Install Go (for Go-based tools)
RUN curl -LO https://go.dev/dl/go1.21.1.linux-amd64.tar.gz && \
    tar -C /usr/local -xzf go1.21.1.linux-amd64.tar.gz && \
    rm go1.21.1.linux-amd64.tar.gz
ENV PATH="/usr/local/go/bin:$PATH"

# Install some Go-based pentest tools
RUN go install -v github.com/projectdiscovery/subfinder/v2/cmd/subfinder@latest && \
    mv $HOME/go/bin/subfinder /usr/local/bin/ && \
    go install -v github.com/projectdiscovery/httpx/cmd/httpx@latest && \
    mv $HOME/go/bin/httpx /usr/local/bin/ && \
    go install -v github.com/projectdiscovery/nuclei/v3/cmd/nuclei@latest && \
    mv $HOME/go/bin/nuclei /usr/local/bin/

# Configure VNC server
RUN mkdir -p /root/.vnc
COPY vnc_startup.sh /root/.vnc/xstartup
RUN chmod +x /root/.vnc/xstartup

# Set VNC password (for demonstration, in a real scenario, this should be set securely by the user)
RUN echo "password" | vncpasswd -f > /root/.vnc/passwd
RUN chmod 600 /root/.vnc/passwd

# Expose VNC port
EXPOSE 5901

# Start VNC server and SSH server when container starts
CMD service ssh start && vncserver :1 -geometry 1280x800 -depth 24 && tail -F /root/.vnc/*.log
