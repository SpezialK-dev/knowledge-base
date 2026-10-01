<the Source code for this tool can be found [here](https://github.com/ai-dynamo/aiperf)

# Good literature to read up on before using AIperf

this is some good literature that I recommend that gives an idea of how to value the benchmarks produced by aiperf

- https://developer.nvidia.com/blog/llm-benchmarking-fundamental-concepts/
- https://developer.nvidia.com/blog/llm-performance-benchmarking-measuring-nvidia-nim-performance-with-genai-perf/
# basic Usage

installation should just be done via python virtual enviroment

```shell
aiperf profile --model "<api name of your model endpoint>" \
--streaming \
--endpoint-type chat \
--tokenizer <full name of the model on huggingface> \
--url <url for endpoint>
--concurrency <int how many requests at the same time>
--request-count <int how many requests to send>
```

`--isl` also specifies how long the random garbage to send should be. 

## Some things 

`--osl` should be able to specifiey how many output tokens are expected, this currently does not work with olllama since nvidia_aiperf uses the `max_completion_tokens` instead of `max_tokens` and currently ollama does not support the first option as seen in this [github issue](https://github.com/ollama/ollama/issues/7125). Hence this paramtere as of writing this does not work. Maybe there is a way to get it to work I am not aware of but it should work with a different compatible openAI endpoint that respects this option. 