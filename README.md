# HF-DGF: Directed Grey-box Fuzzing guided by Data Dependency (Paper Artifact)


This is the artifact of the paper *HF-DGF: Hybrid Feedback Guided Directed Grey-box Fuzzing*.

## __1. Getting started__
### __1.1. System requirements__
To run the experiments in the paper, we used a 64-core (AMD(R) EPYC 9354 CPU @3.80GHz) machine,
with 256 GB of RAM and Ubuntu 20.04. Out of 64 cores, We utilized 54 cores with 4 GB of RAM assigned for each core.

Additionally, we assume that the following environment settings are met.
- Ubuntu 20.04
- Docker
- python 3.8+
- pip3

For the Python dependencies required to run the experiment scripts, run
```
cd runner && pip3 install -r requirements.txt
```

&nbsp;

### __1.2. System configuration__

To run AFL-based fuzzers, you should first fix the core dump name pattern.
```
$ echo core | sudo tee /proc/sys/kernel/core_pattern
```

If your system has `/sys/devices/system/cpu/cpu*/cpufreq` directory, AFL may
also complain about the CPU frequency scaling configuration. Check the current
configuration and remember it if you want to restore it later. Then, set it to
`performance`, as requested by AFL.
```
$ cat /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor
powersave
powersave
powersave
powersave
$ echo performance | sudo tee /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor
```

&nbsp;

### __1.3. Preparing the Docker image__

Our artifact is composed of two parts: the Docker image and the framework to build and utilize it.
The Docker image contains all the necessary tools and dependencies to run the fuzzing experiments.
The framework, that holds this README file, is used to build the Docker image and orchestrate the fuzzing experiments.

**Recommended**
We have built a Docker image that contains everything we need,
contact `wangfengyu@sdu.edu.cn` to get the download link of the pre-built Docker image.

Once you've acquired the docker image, execute the following command to import it into your docker
The image is big (around 25 GB) and it may take a while to download.


## 2. __Reproducing the results in the paper__

&nbsp;

### __2.1. Running the experiment on specific targets__

To run the experiment on specific targets, you can run
```
$ python3 ./scripts/reproduce.py run [target] [timelimit] [iteration] [tool list]
```

For example, you can run the experiment on CVE `2018-11496` in `lrzip-ed51e14` for 60 seconds and 40 iterations
with the tool `AFL`, `AFLGo`, `WindRanger`, `Beacon`, `DAFL` and `HF-DGF` by the following command.
```
$ python3 ./scripts/reproduce.py run lrzip-ed51e14-2018-11496 60 40 "AFL AFLGo WindRanger Beacon DAFL HF-DGF"
```

The result will be parsed and summarized in a CSV file, `lrzip-ed51e14-2018-11496.csv`,
under `output/lrzip-ed51e14-2018-11496-60sec-40iters`.

For the available choices of targets, refer to the `FUZZ_TARGETS` in `scripts/benchmark.py`.
