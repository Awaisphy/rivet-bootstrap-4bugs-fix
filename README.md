# rivet-bootstrap-4bugs-fix
`Fixed bootstrap installer for Rivet 4.1.3 - resolves fjcontrib path, ONNX ARM support, venv prompt, and curl quoting bugs`
cat > README.md << 'EOF'
# Rivet Bootstrap Script - Patch for v4.1.3

This repository provides patches for bugs in the official Rivet bootstrap installer.

**I do NOT distribute the full bootstrap script.** Download it from the official source and apply the patch below.

## Official Script
Download from HepCEDAR GitLab:  
https://gitlab.com/hepcedar/rivet/-/blob/release-4-1-x/doc/tutorials/bootstrap-rivet.sh

## Bugs Fixed

1. **fjcontrib directory name** - Line ~187  
   Script looks for `fastjet-1.101` instead of `fjcontrib-1.101`. Install fails immediately.

2. **ONNX Runtime on ARM** - Line ~231  
   Hardcodes `x64` architecture. Breaks on Apple Silicon Macs and ARM Linux servers.

3. **Python venv prompt** - Line ~251  
   Creates venv without `--prompt ""`. Users see `(local)` in their shell after sourcing `rivetenv.sh`. Looks like an error.

4. **curl URL quoting** - Line ~138  
   Unquoted `$1` in `wget_untar()` function. Can break if URL contains spaces.

## How to Apply

```bash
# 1. Download official script
wget https://gitlab.com/hepcedar/rivet/-/raw/release-4-1-x/doc/tutorials/bootstrap-rivet.sh

# 2. Download patch from this repo
wget https://raw.githubusercontent.com/Awaisphy/rivet-bootstrap-fixed/main/fix.patch

# 3. Apply patch
patch < fix.patch

# 4. Run as normal
chmod +x bootstrap-rivet.sh
INSTALL_PREFIX=$HOME/rivet ./bootstrap-rivet.sh
source $HOME/rivet/local/rivetenv.sh
