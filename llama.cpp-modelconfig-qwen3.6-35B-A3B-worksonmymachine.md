```
./bin/llama-server \
  -m ~/.lmstudio/models/lmstudio-community/Qwen3.6-35B-A3B-GGUF/Qwen3.6-35B-A3B-Q8_0.gguf \
  --port 1236 \
  --host 127.0.0.1 \
  --slot-save-path ./slots \
  -fa on \
  -np 1 \
  -b 2048 \
  -ub 2048 \
  -ngl 99 \
  -c 262144 \
  --n-predict 8192 \
  --mlock \
  --cache-ram 32768 \
  --prio 3 \
  --ctx-checkpoints 256 \
  --jinja \
  --chat-template-file ./chat-template_froggeric_qwen-001.jinja \
  --chat-template-kwargs '{"preserve_thinking":true}' \
  --reasoning on \
  --reasoning-format deepseek \
  --reasoning-budget 4096 \
  --reasoning-budget-message "\n\n\n\n" \
  --spec-type ngram-mod \
  --spec-ngram-mod-n-match 24 \
  --spec-draft-n-max 64 \
  --temp 1.0 \
  --top-p 1 \
  --top-k 0 \
  --min-p 0 \
  --presence-penalty 0.0 \
  --repeat-penalty 1.0
```

Chat template `chat-template_froggeric_qwen-001.jinja` is an unversioned copy from [https://huggingface.co/froggeric/Qwen-Fixed-Chat-Templates/tree/main](https://huggingface.co/froggeric/Qwen-Fixed-Chat-Templates/tree/main) somewhere in May 2026, probably earlier than upstream v17