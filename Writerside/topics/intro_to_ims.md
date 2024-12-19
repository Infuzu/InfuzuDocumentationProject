# Intro to IMS
Infuzu’s Intelligent Model Selection (IMS) system automatically determines which Large Language Model should be used to 
handle your request. Instead of manually picking from available LLMs, you send your request including messages and 
optionally a candidate list of LLM references. IMS uses your input message chain and various heuristics to pick the 
optimal model.

## Key Concepts
- **Messages**: A conversation with a sequence of messages. The first message must be from the `system` or `user` role. All 
other messages can be either `user` or `assistant` roles. Before every `assistant` message, there must be a `user` 
message.
- **LLMs List (Optional)**: You can provide a list of LLMs you want IMS to consider. If omitted, IMS chooses from all 
available LLMs. 
    <tip>You can submit only one llm in the llm list if you already found the best LLM for your use case.</tip>
- **Scoring & Selection**: IMS scores each candidate LLM based on the provided messages and selects the highest scoring 
model automatically.

## Benefits
- **Flexibility**: Easily switch between different LLMs without changing your code. 
- **Cost & Performance Optimization**: Use the model that best fits your use case, potentially reducing costs or 
increasing response quality.
- **Extensibility**: As Infuzu adds more models, your application automatically has access to them without code changes.