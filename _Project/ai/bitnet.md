
# Bitnet 설치하기

```shell
 git clone --recursive https://github.com/microsoft/BitNet.git
cd BitNet
'BitNet'에 복제합니다...
remote: Enumerating objects: 325, done.
remote: Counting objects: 100% (224/224), done.
remote: Compressing objects: 100% (143/143), done.
remote: Total 325 (delta 154), reused 81 (delta 81), pack-reused 101 (from 2)
오브젝트를 받는 중: 100% (325/325), 2.98 MiB | 10.39 MiB/s, 완료.
델타를 알아내는 중: 100% (159/159), 완료.
'3rdparty/llama.cpp' 경로에 대해 '3rdparty/llama.cpp' (https://github.com/Eddie-Wang1120/llama.cpp.git) 하위 모듈 등록
'/Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp'에 복제합니다...
remote: Enumerating objects: 25628, done.
remote: Total 25628 (delta 0), reused 0 (delta 0), pack-reused 25628 (from 1)
오브젝트를 받는 중: 100% (25628/25628), 59.52 MiB | 11.05 MiB/s, 완료.
델타를 알아내는 중: 100% (18216/18216), 완료.
Submodule path '3rdparty/llama.cpp': checked out '40ed0f290203a9a78540b8f7eb18bd828043fe21'
'3rdparty/llama.cpp/ggml/src/kompute' 경로에 대해 'kompute' (https://github.com/nomic-ai/kompute.git) 하위 모듈 등록
'/Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/ggml/src/kompute'에 복제합니다...
remote: Enumerating objects: 9122, done.
remote: Counting objects: 100% (155/155), done.
remote: Compressing objects: 100% (69/69), done.
remote: Total 9122 (delta 109), reused 86 (delta 86), pack-reused 8967 (from 3)
오브젝트를 받는 중: 100% (9122/9122), 17.59 MiB | 11.02 MiB/s, 완료.
델타를 알아내는 중: 100% (5728/5728), 완료.
Submodule path '3rdparty/llama.cpp/ggml/src/kompute': checked out '4565194ed7c32d1d2efa32ceab4d3c6cae006306'
(base)  lines@SEUNGHWAui-MacBookPro  ~/sources/97_ai/BitNet   main
 conda create -n bitnet-cpp python=3.9
Channels:
 - defaults
Platform: osx-arm64
Collecting package metadata (repodata.json): done
Solving environment: done

## Package Plan ##

  environment location: /opt/anaconda3/envs/bitnet-cpp

  added / updated specs:
    - python=3.9


The following packages will be downloaded:

    package                    |            build
    ---------------------------|-----------------
    ca-certificates-2025.9.9   |       hca03da5_0         127 KB
    expat-2.7.1                |       h313beb8_0         156 KB
    libcxx-20.1.8              |       h8869778_0         351 KB
    libzlib-1.3.1              |       h5f15de7_0          47 KB
    ncurses-6.5                |       hee39554_0         886 KB
    openssl-3.0.18             |       h9b4081a_0         3.1 MB
    pip-25.2                   |     pyhc872135_1         1.1 MB
    python-3.9.24              |       he5403cd_0        12.1 MB
    readline-8.3               |       h0b18652_0         464 KB
    setuptools-80.9.0          |   py39hca03da5_0         1.4 MB
    sqlite-3.50.2              |       h79febb2_1         1.0 MB
    tk-8.6.15                  |       hcd8a7d5_0         3.3 MB
    tzdata-2025b               |       h04d1e81_0         116 KB
    wheel-0.45.1               |   py39hca03da5_0         115 KB
    xz-5.6.4                   |       h80987f9_1         289 KB
    zlib-1.3.1                 |       h5f15de7_0          77 KB
    ------------------------------------------------------------
                                           Total:        24.6 MB

The following NEW packages will be INSTALLED:

  bzip2              pkgs/main/osx-arm64::bzip2-1.0.8-h80987f9_6
  ca-certificates    pkgs/main/osx-arm64::ca-certificates-2025.9.9-hca03da5_0
  expat              pkgs/main/osx-arm64::expat-2.7.1-h313beb8_0
  libcxx             pkgs/main/osx-arm64::libcxx-20.1.8-h8869778_0
  libffi             pkgs/main/osx-arm64::libffi-3.4.4-hca03da5_1
  libzlib            pkgs/main/osx-arm64::libzlib-1.3.1-h5f15de7_0
  ncurses            pkgs/main/osx-arm64::ncurses-6.5-hee39554_0
  openssl            pkgs/main/osx-arm64::openssl-3.0.18-h9b4081a_0
  pip                pkgs/main/noarch::pip-25.2-pyhc872135_1
  python             pkgs/main/osx-arm64::python-3.9.24-he5403cd_0
  readline           pkgs/main/osx-arm64::readline-8.3-h0b18652_0
  setuptools         pkgs/main/osx-arm64::setuptools-80.9.0-py39hca03da5_0
  sqlite             pkgs/main/osx-arm64::sqlite-3.50.2-h79febb2_1
  tk                 pkgs/main/osx-arm64::tk-8.6.15-hcd8a7d5_0
  tzdata             pkgs/main/noarch::tzdata-2025b-h04d1e81_0
  wheel              pkgs/main/osx-arm64::wheel-0.45.1-py39hca03da5_0
  xz                 pkgs/main/osx-arm64::xz-5.6.4-h80987f9_1
  zlib               pkgs/main/osx-arm64::zlib-1.3.1-h5f15de7_0


Proceed ([y]/n)? y


Downloading and Extracting Packages:

Preparing transaction: done
Verifying transaction: done
Executing transaction: done
#
# To activate this environment, use
#
#     $ conda activate bitnet-cpp
#
# To deactivate an active environment, use
#
#     $ conda deactivate

(base)  lines@SEUNGHWAui-MacBookPro  ~/sources/97_ai/BitNet   main
 conda activate bitnet-cpp
(bitnet-cpp)  lines@SEUNGHWAui-MacBookPro  ~/sources/97_ai/BitNet   main
 pip install -r requirements.txt
Looking in indexes: https://pypi.org/simple, https://download.pytorch.org/whl/cpu, https://download.pytorch.org/whl/cpu, https://download.pytorch.org/whl/cpu, https://download.pytorch.org/whl/cpu
Collecting numpy~=1.26.4 (from -r /Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/requirements/requirements-convert_legacy_llama.txt (line 1))
  Downloading numpy-1.26.4-cp39-cp39-macosx_11_0_arm64.whl.metadata (61 kB)
Collecting sentencepiece~=0.2.0 (from -r /Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/requirements/requirements-convert_legacy_llama.txt (line 2))
  Downloading sentencepiece-0.2.1-cp39-cp39-macosx_11_0_arm64.whl.metadata (10 kB)
Collecting transformers<5.0.0,>=4.46.3 (from -r /Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/requirements/requirements-convert_legacy_llama.txt (line 3))
  Downloading transformers-4.57.1-py3-none-any.whl.metadata (43 kB)
Collecting gguf>=0.1.0 (from -r /Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/requirements/requirements-convert_legacy_llama.txt (line 4))
  Downloading gguf-0.17.1-py3-none-any.whl.metadata (4.3 kB)
Collecting protobuf<5.0.0,>=4.21.0 (from -r /Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/requirements/requirements-convert_legacy_llama.txt (line 5))
  Downloading protobuf-4.25.8-cp37-abi3-macosx_10_9_universal2.whl.metadata (541 bytes)
Collecting torch~=2.2.1 (from -r /Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/requirements/requirements-convert_hf_to_gguf.txt (line 3))
  Downloading https://download.pytorch.org/whl/cpu/torch-2.2.2-cp39-none-macosx_11_0_arm64.whl (59.3 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 59.3/59.3 MB 11.5 MB/s  0:00:05
Requirement already satisfied: filelock in /Users/lines/.local/lib/python3.9/site-packages (from transformers<5.0.0,>=4.46.3->-r /Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/requirements/requirements-convert_legacy_llama.txt (line 3)) (3.12.4)
Collecting huggingface-hub<1.0,>=0.34.0 (from transformers<5.0.0,>=4.46.3->-r /Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/requirements/requirements-convert_legacy_llama.txt (line 3))
  Downloading huggingface_hub-0.35.3-py3-none-any.whl.metadata (14 kB)
Collecting packaging>=20.0 (from transformers<5.0.0,>=4.46.3->-r /Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/requirements/requirements-convert_legacy_llama.txt (line 3))
  Using cached packaging-25.0-py3-none-any.whl.metadata (3.3 kB)
Collecting pyyaml>=5.1 (from transformers<5.0.0,>=4.46.3->-r /Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/requirements/requirements-convert_legacy_llama.txt (line 3))
  Downloading pyyaml-6.0.3-cp39-cp39-macosx_11_0_arm64.whl.metadata (2.4 kB)
Collecting regex!=2019.12.17 (from transformers<5.0.0,>=4.46.3->-r /Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/requirements/requirements-convert_legacy_llama.txt (line 3))
  Downloading regex-2025.9.18-cp39-cp39-macosx_11_0_arm64.whl.metadata (40 kB)
Collecting requests (from transformers<5.0.0,>=4.46.3->-r /Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/requirements/requirements-convert_legacy_llama.txt (line 3))
  Downloading requests-2.32.5-py3-none-any.whl.metadata (4.9 kB)
Collecting tokenizers<=0.23.0,>=0.22.0 (from transformers<5.0.0,>=4.46.3->-r /Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/requirements/requirements-convert_legacy_llama.txt (line 3))
  Downloading tokenizers-0.22.1-cp39-abi3-macosx_11_0_arm64.whl.metadata (6.8 kB)
Collecting safetensors>=0.4.3 (from transformers<5.0.0,>=4.46.3->-r /Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/requirements/requirements-convert_legacy_llama.txt (line 3))
  Downloading safetensors-0.6.2-cp38-abi3-macosx_11_0_arm64.whl.metadata (4.1 kB)
Collecting tqdm>=4.27 (from transformers<5.0.0,>=4.46.3->-r /Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/requirements/requirements-convert_legacy_llama.txt (line 3))
  Downloading tqdm-4.67.1-py3-none-any.whl.metadata (57 kB)
Collecting typing-extensions>=4.8.0 (from torch~=2.2.1->-r /Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/requirements/requirements-convert_hf_to_gguf.txt (line 3))
  Downloading https://download.pytorch.org/whl/typing_extensions-4.15.0-py3-none-any.whl.metadata (3.3 kB)
Collecting sympy (from torch~=2.2.1->-r /Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/requirements/requirements-convert_hf_to_gguf.txt (line 3))
  Downloading https://download.pytorch.org/whl/sympy-1.14.0-py3-none-any.whl.metadata (12 kB)
Collecting networkx (from torch~=2.2.1->-r /Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/requirements/requirements-convert_hf_to_gguf.txt (line 3))
  Downloading https://download.pytorch.org/whl/networkx-3.5-py3-none-any.whl.metadata (6.3 kB)
Collecting jinja2 (from torch~=2.2.1->-r /Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/requirements/requirements-convert_hf_to_gguf.txt (line 3))
  Downloading https://download.pytorch.org/whl/jinja2-3.1.6-py3-none-any.whl.metadata (2.9 kB)
Collecting fsspec (from torch~=2.2.1->-r /Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/requirements/requirements-convert_hf_to_gguf.txt (line 3))
  Downloading https://download.pytorch.org/whl/fsspec-2025.9.0-py3-none-any.whl.metadata (10 kB)
Collecting hf-xet<2.0.0,>=1.1.3 (from huggingface-hub<1.0,>=0.34.0->transformers<5.0.0,>=4.46.3->-r /Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/requirements/requirements-convert_legacy_llama.txt (line 3))
  Downloading hf_xet-1.1.10-cp37-abi3-macosx_11_0_arm64.whl.metadata (4.7 kB)
Collecting MarkupSafe>=2.0 (from jinja2->torch~=2.2.1->-r /Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/requirements/requirements-convert_hf_to_gguf.txt (line 3))
  Downloading markupsafe-3.0.3-cp39-cp39-macosx_11_0_arm64.whl.metadata (2.7 kB)
INFO: pip is looking at multiple versions of networkx to determine which version is compatible with other requirements. This could take a while.
Collecting networkx (from torch~=2.2.1->-r /Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/requirements/requirements-convert_hf_to_gguf.txt (line 3))
  Downloading https://download.pytorch.org/whl/networkx-3.2.1-py3-none-any.whl.metadata (5.2 kB)
Collecting charset_normalizer<4,>=2 (from requests->transformers<5.0.0,>=4.46.3->-r /Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/requirements/requirements-convert_legacy_llama.txt (line 3))
  Downloading charset_normalizer-3.4.4-cp39-cp39-macosx_10_9_universal2.whl.metadata (37 kB)
Collecting idna<4,>=2.5 (from requests->transformers<5.0.0,>=4.46.3->-r /Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/requirements/requirements-convert_legacy_llama.txt (line 3))
  Downloading idna-3.11-py3-none-any.whl.metadata (8.4 kB)
Collecting urllib3<3,>=1.21.1 (from requests->transformers<5.0.0,>=4.46.3->-r /Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/requirements/requirements-convert_legacy_llama.txt (line 3))
  Downloading urllib3-2.5.0-py3-none-any.whl.metadata (6.5 kB)
Collecting certifi>=2017.4.17 (from requests->transformers<5.0.0,>=4.46.3->-r /Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/requirements/requirements-convert_legacy_llama.txt (line 3))
  Downloading certifi-2025.10.5-py3-none-any.whl.metadata (2.5 kB)
Collecting mpmath<1.4,>=1.1.0 (from sympy->torch~=2.2.1->-r /Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/requirements/requirements-convert_hf_to_gguf.txt (line 3))
  Downloading https://download.pytorch.org/whl/mpmath-1.3.0-py3-none-any.whl (536 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 536.2/536.2 kB 8.9 MB/s  0:00:00
Downloading numpy-1.26.4-cp39-cp39-macosx_11_0_arm64.whl (14.0 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 14.0/14.0 MB 9.7 MB/s  0:00:01
Downloading sentencepiece-0.2.1-cp39-cp39-macosx_11_0_arm64.whl (1.3 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 1.3/1.3 MB 10.4 MB/s  0:00:00
Downloading transformers-4.57.1-py3-none-any.whl (12.0 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 12.0/12.0 MB 11.3 MB/s  0:00:01
Downloading protobuf-4.25.8-cp37-abi3-macosx_10_9_universal2.whl (394 kB)
Downloading huggingface_hub-0.35.3-py3-none-any.whl (564 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 564.3/564.3 kB 8.7 MB/s  0:00:00
Downloading hf_xet-1.1.10-cp37-abi3-macosx_11_0_arm64.whl (2.6 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 2.6/2.6 MB 5.8 MB/s  0:00:00
Downloading tokenizers-0.22.1-cp39-abi3-macosx_11_0_arm64.whl (2.9 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 2.9/2.9 MB 11.7 MB/s  0:00:00
Downloading gguf-0.17.1-py3-none-any.whl (96 kB)
Downloading https://download.pytorch.org/whl/fsspec-2025.9.0-py3-none-any.whl (199 kB)
Using cached packaging-25.0-py3-none-any.whl (66 kB)
Downloading pyyaml-6.0.3-cp39-cp39-macosx_11_0_arm64.whl (174 kB)
Downloading regex-2025.9.18-cp39-cp39-macosx_11_0_arm64.whl (286 kB)
Downloading safetensors-0.6.2-cp38-abi3-macosx_11_0_arm64.whl (432 kB)
Downloading tqdm-4.67.1-py3-none-any.whl (78 kB)
Downloading https://download.pytorch.org/whl/typing_extensions-4.15.0-py3-none-any.whl (44 kB)
Downloading https://download.pytorch.org/whl/jinja2-3.1.6-py3-none-any.whl (134 kB)
Downloading markupsafe-3.0.3-cp39-cp39-macosx_11_0_arm64.whl (12 kB)
Downloading https://download.pytorch.org/whl/networkx-3.2.1-py3-none-any.whl (1.6 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 1.6/1.6 MB 10.9 MB/s  0:00:00
Downloading requests-2.32.5-py3-none-any.whl (64 kB)
Downloading charset_normalizer-3.4.4-cp39-cp39-macosx_10_9_universal2.whl (209 kB)
Downloading idna-3.11-py3-none-any.whl (71 kB)
Downloading urllib3-2.5.0-py3-none-any.whl (129 kB)
Downloading certifi-2025.10.5-py3-none-any.whl (163 kB)
Downloading https://download.pytorch.org/whl/sympy-1.14.0-py3-none-any.whl (6.3 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 6.3/6.3 MB 11.5 MB/s  0:00:00
Installing collected packages: mpmath, urllib3, typing-extensions, tqdm, sympy, sentencepiece, safetensors, regex, pyyaml, protobuf, packaging, numpy, networkx, MarkupSafe, idna, hf-xet, fsspec, charset_normalizer, certifi, requests, jinja2, gguf, torch, huggingface-hub, tokenizers, transformers
Successfully installed MarkupSafe-3.0.3 certifi-2025.10.5 charset_normalizer-3.4.4 fsspec-2025.9.0 gguf-0.17.1 hf-xet-1.1.10 huggingface-hub-0.35.3 idna-3.11 jinja2-3.1.6 mpmath-1.3.0 networkx-3.2.1 numpy-1.26.4 packaging-25.0 protobuf-4.25.8 pyyaml-6.0.3 regex-2025.9.18 requests-2.32.5 safetensors-0.6.2 sentencepiece-0.2.1 sympy-1.14.0 tokenizers-0.22.1 torch-2.2.2 tqdm-4.67.1 transformers-4.57.1 typing-extensions-4.15.0 urllib3-2.5.0
(bitnet-cpp)  lines@SEUNGHWAui-MacBookPro  ~/sources/97_ai/BitNet   main
 huggingface-cli download microsoft/BitNet-b1.58-2B-4T-gguf --local-dir models/BitNet-b1.58-2B-4T
python setup_env.py -md models/BitNet-b1.58-2B-4T -q i2_s
Fetching 3 files:   0%|                                                                                                             | 0/3 [00:00<?, ?it/s]Downloading '.gitattributes' to 'models/BitNet-b1.58-2B-4T/.cache/huggingface/download/wPaCkH-WbT7GsmxMKKrNZTV4nSM=.4e3e1a539c8d36087c5f8435e653b7dc694a0da6.incomplete'
.gitattributes: 1.64kB [00:00, 2.87MB/s]
Download complete. Moving file to models/BitNet-b1.58-2B-4T/.gitattributes
Fetching 3 files:  33%|█████████████████████████████████▋                                                                   | 1/3 [00:00<00:00,  2.19it/s]Downloading 'README.md' to 'models/BitNet-b1.58-2B-4T/.cache/huggingface/download/Xn7B-BWUGOee2Y6hCZtEhtFu4BE=.67c672f98a229cfae21e2333f2b961099a0c448c.incomplete'
README.md: 8.83kB [00:00, 21.7MB/s]
Download complete. Moving file to models/BitNet-b1.58-2B-4T/README.md
Downloading 'ggml-model-i2_s.gguf' to 'models/BitNet-b1.58-2B-4T/.cache/huggingface/download/f28pn7v36EcygdlMWvHpzrkkz6A=.4221b252fdd5fd25e15847adfeb5ee88886506ba50b8a34548374492884c2162.incomplete'
ggml-model-i2_s.gguf: 100%|██████████████████████████████████████████████████████████████████████████████████████████| 1.19G/1.19G [01:42<00:00, 11.6MB/s]
Download complete. Moving file to models/BitNet-b1.58-2B-4T/ggml-model-i2_s.gguf██████████████████████████████████████| 1.19G/1.19G [01:42<00:00, 155MB/s]
Fetching 3 files: 100%|█████████████████████████████████████████████████████████████████████████████████████████████████████| 3/3 [01:43<00:00, 34.36s/it]
/Users/lines/sources/97_ai/BitNet/models/BitNet-b1.58-2B-4T
Traceback (most recent call last):
  File "/Users/lines/sources/97_ai/BitNet/setup_env.py", line 244, in <module>
    main()
  File "/Users/lines/sources/97_ai/BitNet/setup_env.py", line 221, in main
    compile()
  File "/Users/lines/sources/97_ai/BitNet/setup_env.py", line 205, in compile
    cmake_exists = subprocess.run(["cmake", "--version"], capture_output=True)
  File "/opt/anaconda3/envs/bitnet-cpp/lib/python3.9/subprocess.py", line 505, in run
    with Popen(*popenargs, **kwargs) as process:
  File "/opt/anaconda3/envs/bitnet-cpp/lib/python3.9/subprocess.py", line 951, in __init__
    self._execute_child(args, executable, preexec_fn, close_fds,
  File "/opt/anaconda3/envs/bitnet-cpp/lib/python3.9/subprocess.py", line 1837, in _execute_child
    raise child_exception_type(errno_num, err_msg, err_filename)
FileNotFoundError: [Errno 2] No such file or directory: 'cmake'
(bitnet-cpp)  ✘ lines@SEUNGHWAui-MacBookPro  ~/sources/97_ai/BitNet   main
 huggingface-cli download microsoft/BitNet-b1.58-2B-4T-gguf --local-dir models/BitNet-b1.58-2B-4T
python setup_env.py -md models/BitNet-b1.58-2B-4T -q i2_s
Fetching 3 files: 100%|███████████████████████████████████████████████████████████████████████████████████████████████████| 3/3 [00:00<00:00, 2259.46it/s]
/Users/lines/sources/97_ai/BitNet/models/BitNet-b1.58-2B-4T
```

- 설치 과정에 cmake가 존재하지 않아 brew install cmake로 설치 진행 

# 아래 단계에서 더이상 진행되지 않음 

```
 sudo python setup_env.py -md models/BitNet-b1.58-2B-4T -q i2_s
Password:
INFO:root:Compiling the code using CMake.
```

# 로그 확인 

- 시간이 많지 않아 우선 여기까지 로그 확인 
- MacOS 내의 환경을 다시 맞춰서 수행 필요

```shell
Building CXX object 3rdparty/llama.cpp/ggml/src/CMakeFiles/ggml.dir/__/__/__/__/src/ggml-bitnet-lut.cpp.o
In file included from /Users/lines/sources/97_ai/BitNet/src/ggml-bitnet-lut.cpp:9:
In file included from /Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/ggml/src/./ggml-quants.h:4:
/Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/ggml/src/./ggml-common.h:154:9: warning: anonymous structs are a GNU extension [-Wgnu-anonymous-struct]
  154 |         struct {
      |         ^
/Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/ggml/src/./ggml-common.h:154:9: warning: anonymous types declared in an anonymous union are an extension [-Wnested-anon-types]
/Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/ggml/src/./ggml-common.h:175:9: warning: anonymous structs are a GNU extension [-Wgnu-anonymous-struct]
  175 |         struct {
      |         ^
/Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/ggml/src/./ggml-common.h:175:9: warning: anonymous types declared in an anonymous union are an extension [-Wnested-anon-types]
/Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/ggml/src/./ggml-common.h:196:9: warning: anonymous structs are a GNU extension [-Wgnu-anonymous-struct]
  196 |         struct {
      |         ^
/Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/ggml/src/./ggml-common.h:196:9: warning: anonymous types declared in an anonymous union are an extension [-Wnested-anon-types]
/Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/ggml/src/./ggml-common.h:261:9: warning: anonymous structs are a GNU extension [-Wgnu-anonymous-struct]
  261 |         struct {
      |         ^
/Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/ggml/src/./ggml-common.h:261:9: warning: anonymous types declared in an anonymous union are an extension [-Wnested-anon-types]
/Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/ggml/src/./ggml-common.h:294:9: warning: anonymous structs are a GNU extension [-Wgnu-anonymous-struct]
  294 |         struct {
      |         ^
/Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/ggml/src/./ggml-common.h:294:9: warning: anonymous types declared in an anonymous union are an extension [-Wnested-anon-types]
/Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/ggml/src/./ggml-common.h:311:9: warning: anonymous structs are a GNU extension [-Wgnu-anonymous-struct]
  311 |         struct {
      |         ^
/Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/ggml/src/./ggml-common.h:311:9: warning: anonymous types declared in an anonymous union are an extension [-Wnested-anon-types]
In file included from /Users/lines/sources/97_ai/BitNet/src/ggml-bitnet-lut.cpp:10:
/Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/ggml/src/../../../../include/bitnet-lut-kernels.h:24:6: warning: no previous prototype for function 'per_tensor_quant' [-Wmissing-prototypes]
   24 | void per_tensor_quant(int k, void* lut_scales_, void* b_) {{
      |      ^
/Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/ggml/src/../../../../include/bitnet-lut-kernels.h:24:1: note: declare 'static' if the function is not intended to be used outside of this translation unit
   24 | void per_tensor_quant(int k, void* lut_scales_, void* b_) {{
      | ^
      | static
/Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/ggml/src/../../../../include/bitnet-lut-kernels.h:53:6: warning: no previous prototype for function 'partial_max_reset' [-Wmissing-prototypes]
   53 | void partial_max_reset(void* lut_scales_) {{
      |      ^
/Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/ggml/src/../../../../include/bitnet-lut-kernels.h:53:1: note: declare 'static' if the function is not intended to be used outside of this translation unit
   53 | void partial_max_reset(void* lut_scales_) {{
      | ^
      | static
/Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/ggml/src/../../../../include/bitnet-lut-kernels.h:287:9: warning: no previous prototype for function 'qgemm_lut_3200_8640' [-Wmissing-prototypes]
  287 | int32_t qgemm_lut_3200_8640(void* A, void* LUT, void* Scales, void* LUT_Scales, void* C) {
      |         ^
/Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/ggml/src/../../../../include/bitnet-lut-kernels.h:287:1: note: declare 'static' if the function is not intended to be used outside of this translation unit
  287 | int32_t qgemm_lut_3200_8640(void* A, void* LUT, void* Scales, void* LUT_Scales, void* C) {
      | ^
      | static
/Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/ggml/src/../../../../include/bitnet-lut-kernels.h:299:2: warning: extra ';' outside of a function is incompatible with C++98 [-Wc++98-compat-extra-semi]
  299 | };
      |  ^
/Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/ggml/src/../../../../include/bitnet-lut-kernels.h:421:9: warning: no previous prototype for function 'qgemm_lut_3200_3200' [-Wmissing-prototypes]
  421 | int32_t qgemm_lut_3200_3200(void* A, void* LUT, void* Scales, void* LUT_Scales, void* C) {
      |         ^
/Users/lines/sources/97_ai/BitNet/3rdparty/llama.cpp/ggml/src/../../../../include/bitnet-lut-kernels.h:421:1: note: declare 'static' if the function is not intended to be used outside of this translation unit
  421 | int32_t qgemm_lut_3200_3200(void* A, void* LUT, void* Scales, void* LUT_Scales, void* C) {
      | ^
```