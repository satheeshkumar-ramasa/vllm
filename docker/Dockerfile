FROM nvidia/cuda:12.2.2-cudnn8-devel-ubuntu22.04

ENV MODEL_ID facebook/opt-125m

# Any RUN, CMD, ADD, COPY, or ENTRYPOINT command will be executed 
# in the specified working directory.
WORKDIR /

RUN apt update && \
    apt install -y python3-pip

# Upgrade pip
RUN pip install --no-cache-dir --upgrade pip

# Install other packages
RUN apt install -y git

# Install ray_vllm_inference from Github
RUN pip install "ray_vllm_inference @ git+https://github.com/asprenger/ray_vllm_inference"

RUN pip install git+https://gitlab.dell.com/services/cto/digital-humans/digitalhuman-virtual-assistant/digitalhuman-vllm.git@branch_or_tag#egg=package_name

# Expose ports
EXPOSE 9000
EXPOSE 8265

# Start the serve application
# TODO '--host' is depricated and will be removed in a future Ray version
CMD ray start --head --disable-usage-stats --dashboard-host 0.0.0.0; serve run --host 0.0.0.0 ray_vllm_inference.vllm_serve:deployment model=${MODEL_ID}
