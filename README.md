# Motorola Edge 60 Fusion (nice) kernel manifest
- Release tag: MMI-W1VC36H.14-20-1
- Android 16

## 1. Create folder & sync repo
<pre>cd ~/
mkdir -p nice && cd nice
repo init -u https://github.com/Motorola-Edge-60-Fusion-Development/kernel-manifest-6.1.git -m default.xml
repo sync -j$(nproc)</pre>

## 2. Create symlinks
<pre>ln -s kernel_device_modules-6.1 kernel_device_modules-mainline
ln -s kernel_device_modules-6.1 kernel_device_modules</pre>

## 3. Copy nice config
<pre>mkdir -p kernel_device_modules-6.1/kernel/configs/ext_config
cp -r kernel_device_modules-6.1/arch/arm64/configs/ext_config/moto-mgk_64_k61-nice.config \
   kernel_device_modules-6.1/kernel/configs/ext_config/moto-mgk_64_k61-nice.config</pre>

## 4. Build kernel
<pre>bazel build //kernel-6.1:kernel --//:kernel_version=6.1 --//:internal_config=true</pre>

## 5. Build device modules
<pre>export DEFCONFIG_OVERLAYS="ext_config/moto-mgk_64_k61-nice.config"
bazel build //kernel_device_modules-6.1:mgk_64_k61.user</pre>

## 6. Clean sources
<pre>bazel clean --expunge </pre>
