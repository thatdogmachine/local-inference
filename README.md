# local-inference

Repo currently documents a mostly-working works-on-my-machine configuration that delivers a "useful" solution to local inference on a M4 Max 128Gb device. Contents may evolve over time.

This may, or not, be a configuration already folded into eg LM-Studio, but at the time of creation (early May 2026) no such off-the-shelf working configuration was found by the author.

## power throttling

```
sudo powermetrics -i 100 -s gpu_power
```

If SW requested state is higher than SW state, there is throttling of some description, likely thermal:

```
GPU SW requested state: (P1 :   0% P2 :   0% P3 :   0% P4 :   0% P5 :   0% P6 :   0% P7 :   0% P8 :   0% P9 :   0% P10 : 100% P11 :   0% P12 :   0% P13 :   0% P14 :   0% P15 :   0%)
GPU SW state: (SW_P1 :   0% SW_P2 :   0% SW_P3 :   0% SW_P4 :   0% SW_P5 :   0% SW_P6 :   0% SW_P7 :   0% SW_P8 :  48% SW_P9 :  52% SW_P10 :   0% SW_P11 :   0% SW_P12 :   0% SW_P13 :   0% SW_P14 :   0% SW_P15 :   0%)
```

## opencode

The goal is to minimise the circumstances in which opencode does compaction, since this is essentially a prefill.

compaction snippet:
- leave some space for the compact summary
- bump max history messages to a value that we should be unlikely to reach

```
{
  "$schema": "https://opencode.ai/config.json",
  "compaction": {
    "auto": true,
    "threshold": 0.98,
    "max_history_messages": 4000,
    "prune": false,
    "reserved": 4096
  },
```

model snippet:
- we leave some headroom between stated max context and actual to avoid failures
- set a max output that should match the llama.cpp setting, as this seems to help contain thinking loops

```
        "Qwen3.6-27B-Q8_4.gguf": {
          "name": "Qwen3.6-27B-Q4_0.gguf",
          "limit": { "context": 260000, "input": 260000, "output": 8192 }
        }
```

