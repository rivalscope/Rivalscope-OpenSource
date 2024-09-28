FROM nvidia/cuda:12.2.0-runtime-ubuntu22.04

ENV DEBIAN_FRONTEND=noninteractive
ENV PATH="/root/miniconda3/bin:${PATH}"

RUN apt-get update && apt-get install -y \
    wget \
    bzip2 \
    ca-certificates \
    git \
    curl \
    vim \
    && rm -rf /var/lib/apt/lists/*

RUN wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda.sh && \
    /bin/bash ~/miniconda.sh -b -p /root/miniconda3 && \
    rm ~/miniconda.sh

RUN conda create -n llm python=3.10 -y

RUN echo "source activate llm" > ~/.bashrc
ENV PATH=/root/miniconda3/envs/llm/bin:$PATH
SHELL ["/bin/bash", "--login", "-c"]

RUN git clone https://github.com/oobabooga/text-generation-webui.git /app

WORKDIR /app

# Install PyTorch with CUDA 12.x support
RUN pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121

# Install other dependencies
RUN pip install -r requirements.txt

RUN pip install \
    git+https://github.com/oobabooga/torch-grammar.git \
    accelerate \
    bitsandbytes \
    exllamav2 \
    optimum \
    auto-gptq \
    transformers \
    peft \
    scipy \
    datasets

# Create a Python script to comment out the problematic line
RUN echo "import fileinput" > fix_torchvision.py && \
    echo "import sys" >> fix_torchvision.py && \
    echo "file_path = '/root/miniconda3/envs/llm/lib/python3.10/site-packages/torchvision/_meta_registrations.py'" >> fix_torchvision.py && \
    echo "with fileinput.FileInput(file_path, inplace=True) as file:" >> fix_torchvision.py && \
    echo "    for line in file:" >> fix_torchvision.py && \
    echo "        if '@torch.library.register_fake(\"torchvision::nms\")' in line:" >> fix_torchvision.py && \
    echo "            print('# ' + line, end='')" >> fix_torchvision.py && \
    echo "        else:" >> fix_torchvision.py && \
    echo "            print(line, end='')" >> fix_torchvision.py

# Run the fix script
RUN python fix_torchvision.py

EXPOSE 7860
EXPOSE 5000

CMD ["python", "server.py", "--listen", "--api", "--extensions", "api", "training"]
