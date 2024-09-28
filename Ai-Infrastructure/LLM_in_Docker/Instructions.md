## 🧠 LLM Text Generation Web UI Docker Setup

This repository provides a Docker-based environment for running large language models (LLMs) with support 
for the **Text Generation Web UI**. The setup utilizes **NVIDIA CUDA** and **Ubuntu 22.04**, enabling accelerated AI workloads.

## 🛠 Features

- **CUDA 12.2 Runtime**: Optimized for running LLMs on NVIDIA GPUs.
- **Miniconda Environment**: Isolated Python environment for managing dependencies.
- **Text Generation Web UI**: Interface for interacting with various language models.
- **Support for PyTorch and Additional Libraries**: Includes essential libraries for model training and inference.

## 🚀 Getting Started

### Prerequisites

Before running this setup, ensure you have the following:

- **NVIDIA GPU** with CUDA support.
- **Docker** and **NVIDIA Container Toolkit** installed. You can find installation instructions [here](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html).

### Building the Docker Image

1. Clone this repository and navigate to Ai-Infrastructure/LLM_in_Docker containing the `Dockerfile`.

   ```bash
   git clone <repository-url>
   cd <repository-directory>
   ```

2. Build the Docker image using the following command:

   ```bash
   docker build -t llm-text-gen-webui .
   ```
   Or download the image from our Docker repository usinng Docker CLI :
   =>>> docker pull pinkelephants/rivalscope:latest <===

### Running the Docker Container

To run the container and expose the necessary ports, use:

```bash
sudo docker run -d --name text-gen-webui \
  --gpus all \
  -p 7860:7860 \
  -p 5000:5000 \
  -v /LLM_models:/app/models \
  -v ~/loras:/app/loras \
  -v ~/training:/app/training \
  -e NVIDIA_VISIBLE_DEVICES=all \
  -m 98304m \
  --memory-reservation 65536m \
  --cpus 28 \
  --shm-size 16384m \
  text-gen-webui:latest
```

- **Port 7860**: Access the Text Generation Web UI.
- **Port 5000**: Access the API for programmatic interaction.

### Accessing the Application

- The **Text Generation Web UI** will be available at: [http://localhost:7860](http://localhost:7860)
- The **API** will be available at: [http://localhost:5000](http://localhost:5000)

## 📚 Additional Resources

- **NVIDIA CUDA Documentation**: [Link](https://docs.nvidia.com/cuda/)
- **PyTorch Documentation**: [Link](https://pytorch.org/)
- **Oobabooga's Text Generation Web UI GitHub Repository**: [Link](https://github.com/oobabooga/text-generation-webui)

## 📝 License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more details.

## 🤝 Contribution

We welcome contributions to enhance this project. Feel free to open issues or submit pull requests to improve functionality, add features, or update documentation.

---

Thank you for using the LLM Text Generation Web UI Docker setup! Happy coding!
```
