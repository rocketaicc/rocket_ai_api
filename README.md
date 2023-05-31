# AI小火箭 - REST API



# API地址

| 环境     | URL                                  |
| -------- | ------------------------------------ |
| 生产环境 | https://openai.api.ai-whistle.com |



# 认证

所有 API 请求都应在 `Authorization` HTTP 标头中包含您的 API 密钥，如下所示：

```http
Authorization: Bearer AIR_KEY
```

`AIR_KEY`从[控制台](https://account.ai-rocket.cc/)获取。



# 第一个请求

您可以将下面的命令粘贴到您的终端中以运行您的第一个 API 请求。确保将 `$AIR_KEY` 替换为您的 API 密钥。

```shell
curl https://openai.api.ai-whistle.com/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $AIR_KEY" \
  -d '{
     "model": "gpt-3.5-turbo",
     "messages": [{"role": "user", "content": "Say this is a test!"}],
     "temperature": 0.7
   }'
```

此请求查询 `gpt-3.5-turbo` 模型以完成以提示“Say this is a test”开头的文本。您应该会收到类似于以下内容的响应：

```json
{
  "id": "chatcmpl-796sNf3vN4DWAXyKpyn9diumOAljv",
  "object": "chat.completion",
  "created": 1682405739,
  "model": "gpt-3.5-turbo-0301",
  "usage": {
    "prompt_tokens": 14,
    "completion_tokens": 5,
    "total_tokens": 19
  },
  "choices": [
    {
      "message": {
        "role": "assistant",
        "content": "This is a test!"
      },
      "finish_reason": "stop",
      "index": 0
    }
  ]
}
```

现在你已经生成了你的第一个聊天完成。我们可以看到 `finish_reason` 是 `stop` ，这意味着 API 返回了模型生成的完整完成。在此示例中， `gpt-3.5-turbo` 用于更多的传统文本完成任务。该模型还针对聊天应用程序进行了优化。



# 支持的API和模型

## 稳定版本

| URL                  | 支持的模型                | 备注               |
| -------------------- | ------------------------- | ------------------ |
| /v1/chat/completions | gpt-3.5-turbo             | ChatGPT            |
|                      | gpt-3.5-turbo-0301        |                    |
|                      | gpt-4                     | 暂不支持，即将支持 |
|                      | gpt-4-0314                | 暂不支持，即将支持 |
| /v1/completions      | text-ada-001              | 内容补全           |
|                      | text-babbage-001          |                    |
|                      | text-curie-001            |                    |
|                      | text-davinci-002          |                    |
|                      | text-davinci-003          |                    |
| /v1/embeddings       | text-embedding-ada-002    | 向量生成           |
|                      | text-similarity-ada-001   |                    |
|                      | text-similarity-curie-001 |                    |
| /v1/edits/v1/edits   | code-davinci-edit-001     | 文本编辑           |
|                      | text-davinci-edit-001     |                    |



## Beta版本

以下的接口和模型：访问可能不稳定。

| URL                | 支持的模型                                                   | 备注     |
| ------------------ | ------------------------------------------------------------ | -------- |
| /v1/embeddings     | text-search-ada-doc-001<br/> text-search-ada-query-001<br/> text-search-babbage-doc-001<br/> text-search-babbage-query-001<br/> text-search-curie-doc-001<br/> text-search-curie-query-001<br/> text-search-davinci-doc-001<br/> text-search-davinci-query-001 | 向量生成 |
|                    | text-similarity-ada-001                                      | 向量生成 |
|                    | text-similarity-curie-001                                    | 向量生成 |
| /v1/edits/v1/edits | code-davinci-edit-001                                        | 文本编辑 |
|                    | text-davinci-edit-001                                        | 文本编辑 |





# **接口详情**




## Completions

> HTTP方法

```bash
POST
```



> Header参数

```bash
Reduce-Stream
```

如果请求体中的`stream`为`true`，并且头部中的`Reduce-Stream`为`true`，返回结果将精简。



> URL

```bash
{{BASE_URL}}/v1/completions
```



> 请求体-参数

| 参数名            | 类型            | 参数说明                                                     |
| ----------------- | --------------- | ------------------------------------------------------------ |
| model             | String          | 模型名称；必选填写                                           |
| prompt            | string or array |                                                              |
| suffix            | String          | 插入文本完成后出现的后缀。  The suffix that  comes after a completion of inserted text. |
| max_tokens        | integer         | 完成时生成的最大tokens数量  您的提示加上 max_tokens 的标记计数不能超过模型的上下文长度。大多数模型的上下文长度为 2048 个标记（最新模型除外，它支持 4096）。 |
| temperature       | number          |                                                              |
| top_p             | number          |                                                              |
| n                 | integer         | 为每个提示生成多少批结果。  本系统会限制数量不超过1。    |
| stream            | boolean         | 是否启用Event  Stream                                        |
| logprobs          | integer         |                                                              |
| echo              | boolean         | 是否返回prompt                                               |
| stop              | string or array |                                                              |
| presence_penalty  | number          |                                                              |
| frequency_penalty | number          |                                                              |
| best_of           | integer         |                                                              |
| logit_bias        | map             |                                                              |
| user              | string          | 代表您的最终用户的唯一标识符                                 |



> 请求体-范例

```json
{
  "model": "text-davinci-003",
  "prompt": "Say this is a test",
  "max_tokens": 7,
  "temperature": 0,
  "top_p": 1,
  "n": 1,
  "stream": false,
  "logprobs": null,
  "stop": "\n"
}
```



> 响应

```json
{
    "id": "cmpl-78sY9YEf4NoN8J16dsZPfJibghPJt",
    "object": "text_completion",
    "created": 1682350669,
    "model": "text-curie-001",
    "choices": [
        {
            "text": "\n\nA rose by any other name would smell just as sweet\n\nBut",
            "index": 0,
            "logprobs": null,
            "finish_reason": "length"
        }
    ],
    "usage": {
        "prompt_tokens": 8,
        "completion_tokens": 16,
        "total_tokens": 24
    }
}
```



如果请求体中的`stream`为`true`，并且头部中的`Reduce-Stream`为`true`，返回结果将精简，例如：

```shell
data: ["\n"]

data: ["\n"]

data: ["清"]

data: [DONE]
```



## Chat

HTTP方法

```bash
POST
```

URL

```bash
{{BASE_URL}}/v1/chat/completions
```



> Header参数

```bash
Reduce-Stream
```

如果请求体中的`stream`为`true`，并且头部中的`Reduce-Stream`为`true`，返回结果将精简。



> 请求体-参数

| **参数名**        | 子参数  | **类型**        | **参数说明**                                                 |
| ----------------- | ------- | --------------- | ------------------------------------------------------------ |
| model             |         | String          | 模型名称；必选填写                                           |
| messages          |         | array           | 到目前为止描述对话的消息列表。                               |
|                   | role    | string          | 此消息的角色。 system 、 user 或 assistant 之一。            |
|                   | content | string          | 消息的内容。                                                 |
|                   | name    | string          | 此消息的角色的姓名。可以包含 a-z、A-Z、0-9 和下划线，最大长度为 64 个字符。 |
| stream            |         | boolean         | 是否启用Event Stream                                         |
| max_tokens        |         | integer         | 聊天完成时生成的最大令牌数；输入标记和生成标记的总长度受模型上下文长度的限制。 |
| user              |         | string          | 代表您的最终用户的唯一标识符                                 |
| temperature       |         | number          |                                                              |
| top_p             |         | number          |                                                              |
| n                 |         | integer         | 为每个提示生成多少批结果。 本系统会限制数量不超过1。     |
| stop              |         | string or array |                                                              |
| presence_penalty  |         | number          |                                                              |
| frequency_penalty |         | number          |                                                              |
| logit_bias        |         | map             |                                                              |



> 请求体-范例

```json
{
  "messages": [
    {
      "role": "user",
      "content": "写一首诗"
    }
  ],
  "stream": false,
  "model": "gpt-3.5-turbo",
  "temperature": 1,
  "presence_penalty": 0
}
```



> 响应

```json
{
    "id": "chatcmpl-793ZpEIz52Qpjki3qpcPDdracq18Z",
    "object": "chat.completion",
    "created": 1682393057,
    "model": "gpt-3.5-turbo-0301",
    "usage": {
        "prompt_tokens": 13,
        "completion_tokens": 218,
        "total_tokens": 231
    },
    "choices": [
        {
            "message": {
                "role": "assistant",
                "content": "花开花谢，流年匆匆\n曾经拥有，如今已成空\n时光荏苒，记忆犹存\n轻轻诉说，心中千言\n\n我看见落日的余晖\n静静地坐在山顶\n感受身心的沉静\n思绪纷飞\n\n岁月如梭，光阴易逝\n回忆往昔，心中泪洒\n未知未来，怀揣着期待\n扬帆远航，愿与幸福同行\n\n天高云淡，心中自在\n人生难得，不放过每一秒\n我只想，做个淡然的风\n让人们感受，生命的珍贵"
            },
            "finish_reason": "stop",
            "index": 0
        }
    ]
}
```



如果请求体中的`stream`为`true`，并且头部中的`Reduce-Stream`为`true`，返回结果将精简，例如：

```shell
data: [{"role":"assistant"}]

data: [{"content":"闪"}]

data: [{"content":"光"}]

data: [{"content":"的"}]

data: [{"content":"夜"}]

data: [{"content":"晚"}]

data: [{"content":"，"}]

data: [{}]

data: [DONE]

```



## Edits

见[官网](https://platform.openai.com/docs/api-reference/edits)



## Embeddings

见[官网](https://platform.openai.com/docs/api-reference/embeddings/create)

