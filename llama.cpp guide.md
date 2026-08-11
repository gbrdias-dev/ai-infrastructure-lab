# Guia de instalação do Llama.cpp

TESTADO NO UBUNTU SERVER 26.04 lts

- ATUALIZAR REPOSITÓRIOS E INSTALAR COMPILADOR E GIT
  	```
	sudo apt update && sudo apt install -y git build-essential cmake
	```
- CLONAR PROJETO NA PASTA "/opt":
    ```
	cd /opt
	git clone https://github.com/ggml-org/llama.cpp
	cd llama.cpp
	```

- COMPILAR (escolha o tipo de processamento para modelo de IA)
PARA CPU:
    ```
	cmake -B build -DCMAKE_BUILD_TYPE=Release
	cmake --build build --config Release -j$(nproc)
	```
PARA NVIDIA
	cmake -B build -DGGML_CUDA=ON -DCMAKE_BUILD_TYPE=Release
	cmake --build build --config Release -j$(nproc)

PARA AMD
	bibliotecas adicionais precisam ser instaladas
	sudo apt install -y libclblast-dev rocminfo rocm-smi libhipblas-dev hipcc

	caso a placa esteja listada a abaixo usar comando mais específico.
	```
	sudo cmake -B build -DGGML_HIP=ON -DAMDGPU_TARGETS=gfx1100 -DCMAKE_BUILD_TYPE=Release

	 - gfx1101 / gfx1102: RX 7800 XT / RX 7700 XT / RX 7600
	 - gfx1030: RX 6900 XT / RX 6800 XT / RX 6700 XT
	```
	caso falhe usar:
	```
	sudo apt install -y clang llvm libomp-dev
	cmake -DGGML_HIP=ON -DAMDGPU_TARGETS=gfx1102 -DCMAKE_C_COMPILER=clang -DCMAKE_CXX_COMPILER=clang++
	```
	
	finalize compilação
	sudo cmake --build build --config Release -j$(nproc)


- INSTALAR CLIENTE HUGGINGFACE
  	```
	sudo snap install astral-uv
	uv tool install huggingface_hub
  	 ```

	Caso falhe:
	```
	pip3 install huggingface_hub --break-system-packages
	```

- CRIAR PASTA PARA MODELOS
  	```
	cd /opt
	sudo mkdir modelos-ia
	sudo chown -R $USER:$USER /opt/modelos-ia
	```
   
- BAIXAR MODELOS
  	```
	hf download Qwen/Qwen3-4B-GGUF     --local-dir /opt/modelos-ia
	hf download ggml-org/gemma-3-1b-it-GGUF --local-dir /opt/modelos-ia
	hf download hf://ibm-granite/granite-4.1-3b-GGUF/granite-4.1-3b-Q4_K_M.gguf  --local-dir /opt/modelos-ia
	```

- ABRIR PORTA 8080 no firewall
  	```
	sudo ufw allow 8080/tcp
   ```

- EXECUTAR MODELO
	Comando para forçar o modelo da GPU (se necessário):
	```
	export HSA_OVERRIDE_GFX_VERSION=11.0.0
 	```
	Via processador:
	```
	/opt/llama.cpp/build/bin/llama-server --model /opt/modelos-ia/"nome-do-modelo" --host 0.0.0.0 --port 8080 --ctx-size 8192
	```
	Se for usar GPU:
	```
	/opt/llama.cpp/build/bin/llama-server --model /opt/modelos-ia/"nome-do-modelo" --host 0.0.0.0 --port 8080 --ctx-size 8192 --n-gpu-layers 99
 	```
