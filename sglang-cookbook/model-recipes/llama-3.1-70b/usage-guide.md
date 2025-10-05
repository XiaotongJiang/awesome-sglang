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
        --model meta-llama/Llama-3.1-70B-Instruct \
        --tp 4 --trust-remote-code \
        --mem-fraction-static 0.95 \
        --port 30000 \
        --attention-backend triton
```
{% endstep %}

{% step %}
#### Benchmark

```shell
# BS=1/Input=1024/Ouput=1024
python3 -m sglang.bench_one_batch_server \
        --model meta-llama/Llama-3.1-70B-Instruct \
        --base-url http://localhost:30000 \
        --batch-size 1 \
        --input-len 1024 \
        --output-len 1024


# 1/8192/1024
python3 -m sglang.bench_one_batch_server \
        --model meta-llama/Llama-3.1-70B-Instruct \
        --base-url http://localhost:30000 \
        --batch-size 1 \
        --input-len 8192 \
        --output-len 1024

# 8/1024/1024
python3 -m sglang.bench_one_batch_server \
        --model meta-llama/Llama-3.1-70B-Instruct \
        --base-url http://localhost:30000 \
        --batch-size 8 \
        --input-len 1024 \
        --output-len 1024

# 8/8192/1024
python3 -m sglang.bench_one_batch_server \
        --model meta-llama/Llama-3.1-70B-Instruct \
        --base-url http://localhost:30000 \
        --batch-size 8 \
        --input-len 8192 \
        --output-len 1024
```

<table data-header-hidden><thead><tr><th width="227.57421875"></th><th width="92.63671875"></th><th width="85.66015625"></th><th></th><th></th></tr></thead><tbody><tr><td>BS/Input/Output Length</td><td>TTFT(s)</td><td>ITL(ms)</td><td>Input Throughput</td><td>Output Throughput</td></tr><tr><td>1/1024/1024</td><td>0.23</td><td>63</td><td>4418.12</td><td>15.73</td></tr><tr><td>1/8192/1024</td><td>2.19</td><td>50</td><td>3737.24</td><td>19.75</td></tr><tr><td>8/1024/1024</td><td>0.58</td><td>2</td><td>14052.0</td><td>479.11</td></tr><tr><td>8/8192/1024</td><td>5.22</td><td>3</td><td>12556.62</td><td>355.16</td></tr></tbody></table>
{% endstep %}
{% endstepper %}

