# Usage Guide

### <mark style="background-color:green;">erving with 1 x 8 x B200</mark>

{% stepper %}
{% step %}
#### Install SGLang

Following [the instruction](../../installation/nvidia-h-series-a-series-and-rtx-gpus.md)
{% endstep %}

{% step %}
#### Serve the model

```sh
python -m sglang.launch_server \
    --model deepseek-ai/DeepSeek-V3.2-Exp \
    --tp 8 --dp 8 --enable-dp-attention \
    --port 30002
```
{% endstep %}

{% step %}
#### Benchmark

```sh
# BS=1/Input=1024/Ouput=1024
python3 -m sglang.bench_one_batch_server \
        --model deepseek-ai/DeepSeek-V3.2-Exp \
        --base-url http://localhost:30002 \
        --batch-size 1 \
        --input-len 1024 \
        --output-len 1024


# 1/8192/1024
python3 -m sglang.bench_one_batch_server \
        --model deepseek-ai/DeepSeek-V3.2-Exp \
        --base-url http://localhost:30002 \
        --batch-size 1 \
        --input-len 8192 \
        --output-len 1024

# 8/1024/1024
python3 -m sglang.bench_one_batch_server \
        --model deepseek-ai/DeepSeek-V3.2-Exp \
        --base-url http://localhost:30002 \
        --batch-size 8 \
        --input-len 1024 \
        --output-len 1024

# 8/8192/1024
python3 -m sglang.bench_one_batch_server \
        --model deepseek-ai/DeepSeek-V3.2-Exp \
        --base-url http://localhost:30002 \
        --batch-size 8 \
        --input-len 8192 \
        --output-len 1024
```



| BS/Input/Output Length | TTFT(s) | ITL(ms) | Input Throughput | Output Throughput |
| ---------------------- | ------- | ------- | ---------------- | ----------------- |
| 1/1024/1024            | 0.33    | 22      | 3076.26          | 44.611            |
| 1/8192/1024            | 1.50    | 23      | 5458.96          | 42.86             |
| 8/1024/1024            | 1.50    | 3       | 5473.35          | 311.95            |
| 8/8192/1024            | 11.82   | 3       | 5544.95          | 304.96            |
{% endstep %}
{% endstepper %}
