- https://git-lfs.com/

```shell
 brew install git-lfs
==> Auto-updating Homebrew...
Adjust how often this is run with `$HOMEBREW_AUTO_UPDATE_SECS` or disable with
`$HOMEBREW_NO_AUTO_UPDATE=1`. Hide these hints with `$HOMEBREW_NO_ENV_HINTS=1` (see `man brew`).
==> Auto-updated Homebrew!
Updated 4 taps (derailed/k9s, bufbuild/buf, homebrew/core and homebrew/cask).
==> New Formulae
fennel-ls: Language Server for Fennel
taze: Modern cli tool that keeps your deps fresh
zsv: Tabular data swiss-army knife CLI
==> New Casks
chatgpt-atlas: OpenAI's official browser with ChatGPT built in
dockflow: Manage Dock presets and switch between them instantly
launchie: Launchpad replacement
llamabarn: Menu bar app for running local LLMs
pxplay: Third-party Remote Play client for PlayStation consoles

You have 73 outdated formulae and 4 outdated casks installed.

==> Fetching downloads for: git-lfs
==> Downloading https://ghcr.io/v2/homebrew/core/git-lfs/manifests/3.7.1
######################################################################################################################################################################################################################################################### 100.0%
==> Fetching git-lfs
==> Downloading https://ghcr.io/v2/homebrew/core/git-lfs/blobs/sha256:edf7cb5683caadeca2318d455130e4e67ddae8647594760aad039f77c7712df1
######################################################################################################################################################################################################################################################### 100.0%
==> Pouring git-lfs--3.7.1.arm64_tahoe.bottle.tar.gz
==> Caveats
Update your git config to finish installation:

  # Update global git config
  $ git lfs install

  # Update system git config
  $ git lfs install --system
==> Summary
🍺  /opt/homebrew/Cellar/git-lfs/3.7.1: 82 files, 13.4MB
==> Running `brew cleanup git-lfs`...
Disable this behaviour by setting `HOMEBREW_NO_INSTALL_CLEANUP=1`.
Hide these hints with `HOMEBREW_NO_ENV_HINTS=1` (see `man brew`).
==> Caveats
zsh completions have been installed to:
  /opt/homebrew/share/zsh/site-functions
(lines-gpu-finetuning-model-serving-py3.12) (base)  lines@SEUNGHWAui-MacBookPro  ~/sources/00_company/lines-gpu-finetuning-model-serving   master
 git lfs install
Updated Git hooks.
Git LFS initialized.
(lines-gpu-finetuning-model-serving-py3.12) (base)  lines@SEUNGHWAui-MacBookPro  ~/sources/00_company/lines-gpu-finetuning-model-serving   master
 git lfs track *.pdf   
zsh: no matches found: *.pdf
(lines-gpu-finetuning-model-serving-py3.12) (base)  ✘ lines@SEUNGHWAui-MacBookPro  ~/sources/00_company/lines-gpu-finetuning-model-serving   master
 cd docs  
(lines-gpu-finetuning-model-serving-py3.12) (base)  lines@SEUNGHWAui-MacBookPro  ~/sources/00_company/lines-gpu-finetuning-model-serving/docs   master
 ls
references          논문 작성법.md      목표.md             진행과정.md
(lines-gpu-finetuning-model-serving-py3.12) (base)  lines@SEUNGHWAui-MacBookPro  ~/sources/00_company/lines-gpu-finetuning-model-serving/docs   master
 cd references 
(lines-gpu-finetuning-model-serving-py3.12) (base)  lines@SEUNGHWAui-MacBookPro  ~/sources/00_company/lines-gpu-finetuning-model-serving/docs/references   master
 ls
01. (NIPA)AI컴퓨팅자원활용_사용자 데이터 이관 추가 교육_MLOps.pdf
01. [NIPA] AI컴퓨팅 자원 활용 기반 강화(GPU 임차 지원)_사용자교육_MLOps.pdf
02. [NIPA] AI컴퓨팅 자원 활용 기반 강화(GPU 임차 지원)_사용자교육_MLOps_VesslAI.pdf
03. [NIPA] AI컴퓨팅 자원 활용 기반 강화(GPU 임차 지원)_사용자교육_MLOps_녹화자료.mp4
1. 완성_제출용 (서식1-1) 2025년도(추경)「AI컴퓨팅 자원 활용 기반 강화(GPU 임차 지원)」_2025121512000.pdf
(lines-gpu-finetuning-model-serving-py3.12) (base)  lines@SEUNGHWAui-MacBookPro  ~/sources/00_company/lines-gpu-finetuning-model-serving/docs/references   master
 git lfs track *.pdf
Tracking "01. (NIPA)AI컴퓨팅자원활용_사용자 데이터 이관 추가 교육_MLOps.pdf"
Tracking "01. [NIPA] AI컴퓨팅 자원 활용 기반 강화(GPU 임차 지원)_사용자교육_MLOps.pdf"
Tracking "02. [NIPA] AI컴퓨팅 자원 활용 기반 강화(GPU 임차 지원)_사용자교육_MLOps_VesslAI.pdf"
Tracking "1. 완성_제출용 (서식1-1) 2025년도(추경)「AI컴퓨팅 자원 활용 기반 강화(GPU 임차 지원)」_2025121512000.pdf"
(lines-gpu-finetuning-model-serving-py3.12) (base)  lines@SEUNGHWAui-MacBookPro  ~/sources/00_company/lines-gpu-finetuning-model-serving/docs/references   master ±
 git add .gitattributes
(lines-gpu-finetuning-model-serving-py3.12) (base)  lines@SEUNGHWAui-MacBookPro  ~/sources/00_company/lines-gpu-finetuning-model-serving/docs/references   master ±✚
 git add .
(lines-gpu-finetuning-model-serving-py3.12) (base)  lines@SEUNGHWAui-MacBookPro  ~/sources/00_company/lines-gpu-finetuning-model-serving/docs/references   master ✚
 git commit -m "added pdf file for references"
[master bcdda8e] added pdf file for references
 3 files changed, 4 insertions(+)
 create mode 100644 docs/references/.gitattributes
(lines-gpu-finetuning-model-serving-py3.12) (base)  lines@SEUNGHWAui-MacBookPro  ~/sources/00_company/lines-gpu-finetuning-model-serving/docs/references   master
 git push
git: 'credential-manager' is not a git command. See 'git --help'.
git: 'credential-manager' is not a git command. See 'git --help'.
Uploading LFS objects: 100% (2/2), 1.6 MB | 560 KB/s, done.                                                                                                                                                                                                                                                                                                                                                                                
Enumerating objects: 31, done.
Counting objects: 100% (31/31), done.
Delta compression using up to 10 threads
Compressing objects: 100% (23/23), done.
Writing objects:  56% (13/23), 75.48 MiB | 6.95 MiB/s   
```