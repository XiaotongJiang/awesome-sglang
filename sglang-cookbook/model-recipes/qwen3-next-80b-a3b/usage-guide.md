# Usage Guide

### <mark style="background-color:green;">Serving with 1 x 4 x H200</mark>

{% stepper %}
{% step %}
#### Install SGLang

Following [the instruction](../../installation/nvidia-h-series-a-series-and-rtx-gpus.md)
{% endstep %}

{% step %}
#### Serve the model

```sh
python3 -m sglang.launch_server \
        --model Qwen/Qwen3-Next-80B-A3B-Instruct \
        --tp 4 --trust-remote-code \
        --mem-fraction-static 0.95 \
        --port 30000 \
        --attention-backend triton
```
{% endstep %}

{% step %}
#### Benchmark

```sh
# BS=1/Input=1024/Ouput=1024
python3 -m sglang.bench_one_batch_server \
        --model Qwen/Qwen3-Next-80B-A3B-Instruct \
        --base-url http://localhost:30000 \
        --batch-size 1 \
        --input-len 1024 \
        --output-len 1024


# 1/8192/1024
python3 -m sglang.bench_one_batch_server \
        --model Qwen/Qwen3-Next-80B-A3B-Instruct \
        --base-url http://localhost:30000 \
        --batch-size 1 \
        --input-len 8192 \
        --output-len 1024

# 8/1024/1024
python3 -m sglang.bench_one_batch_server \
        --model Qwen/Qwen3-Next-80B-A3B-Instruct \
        --base-url http://localhost:30000 \
        --batch-size 8 \
        --input-len 1024 \
        --output-len 1024

# 8/8192/1024
python3 -m sglang.bench_one_batch_server \
        --model Qwen/Qwen3-Next-80B-A3B-Instruct \
        --base-url http://localhost:30000 \
        --batch-size 8 \
        --input-len 8192 \
        --output-len 1024
```

<table data-header-hidden><thead><tr><th width="217.1875"></th><th width="91.25390625"></th><th width="87.07421875"></th><th></th><th></th></tr></thead><tbody><tr><td>BS/Input/Output Length</td><td>TTFT(s)</td><td>ITL(ms)</td><td>Input Throughput</td><td>Output Throughput</td></tr><tr><td>1/1024/1024</td><td>0.40</td><td>12</td><td>2547.99</td><td>81.41</td></tr><tr><td>1/8192/1024</td><td>1.27</td><td>15</td><td>6459.45</td><td>67.89</td></tr><tr><td>8/1024/1024</td><td>0.95</td><td>3</td><td>8665.16</td><td>289.15</td></tr><tr><td>8/8192/1024</td><td>3.17</td><td>2</td><td>20642.36</td><td>474.05</td></tr></tbody></table>

\

{% endstep %}
{% endstepper %}
