# Tensorflow 2.1.0

```
# md5sum
10a46e8ee9c0b27437a26d7a4252d452  tensorflow-2.1.0-cp37-cp37m-linux_x86_64_with_SSE3_SSE4.1.whl
```

Tensorflow has been built following [the documentation](https://www.tensorflow.org/install/source) on Debian with the following CPU :

```
Architecture:        x86_64
CPU op-mode(s):      32-bit, 64-bit
Byte Order:          Little Endian
Address sizes:       40 bits physical, 48 bits virtual
CPU(s):              18
On-line CPU(s) list: 0-17
Thread(s) per core:  1
Core(s) per socket:  6
Socket(s):           3
NUMA node(s):        1
Vendor ID:           GenuineIntel
CPU family:          6
Model:               29
Model name:          Intel(R) Xeon(R) CPU           X7460  @ 2.66GHz
Stepping:            1
CPU MHz:             2659.778
BogoMIPS:            5319.55
Hypervisor vendor:   KVM
Virtualization type: full
L1d cache:           32K
L1i cache:           32K
L2 cache:            4096K
L3 cache:            16384K
NUMA node0 CPU(s):   0-17
Flags:               fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx lm constant_tsc arch_perfmon rep_good nopl xtopology cpuid tsc_known_freq pni ssse3 cx16 sse4_1 x2apic tsc_deadline_timer hypervisor lahf_lm cpuid_fault pti tsc_adjust arat arch_capabilities
```

# How to build

```bash
# On Debian
apt install python3-dev python3-pip git screen
pip3 install --upgrade pip
pip3 install -U --user pip six numpy wheel setuptools mock 'future>=0.17.1'
pip3 install -U --user keras_applications --no-deps
pip3 install -U --user keras_preprocessing --no-deps
```

You need to [download Bazelisk](https://github.com/bazelbuild/bazelisk/releases). 
You can add the directory where you downloaded Bazelisk in your PATH temporarily.

```bash
export PATH=/path/to/folder:$PATH
```

Then you need to git clone Tensorflow :
```bash
git clone https://github.com/tensorflow/tensorflow.git
cd tensorflow
# If you want to build a specific version on your computer
# git checkout branchname 
```

Then you need to configure the system build. You can let everything by default EXCEPT the location of Python (I needed to set /usr/bin/python3 on my Debian) :

```bash
./configure
```

Finally you are ready to build. 

Tips : As I built Tensorflow on a server, I run the script thanks to `screen` :

```bash
screen
bazel build --config=opt //tensorflow/tools/pip_package:build_pip_package
```

You can now grab a coffee (or several, because it's very long... More than 10hours on my (old) computer). 

Finally, you can build the package and install it :

```
./bazel-bin/tensorflow/tools/pip_package/build_pip_package /tmp/tensorflow_pkg
pip install /tmp/tensorflow_pkg/tensorflow-*.whl
```
