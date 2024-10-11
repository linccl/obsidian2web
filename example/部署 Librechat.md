
**Librechat：**  


**部署**
1. 先把项目克隆到本地

`git clone https://github.com/danny-avila/LibreChat.git`

## 2. 编辑配置文件（必须）

克隆到本地后你会看到 [.env.example 27](https://github.com/danny-avila/LibreChat/blob/main/.env.example)的文件，**把它改名成`.env`**，前期你需要编辑的所有项目都在里面了。该配置文件里充斥很多对我们身处国内的我们基本一点用没有的配置，所以需要按需启用。

### 3. 配置其他的AI模型的（可选）

Librechat的其他ai模型的配置在另一个配置文件里`librechat.example.yaml`，把名字改成`librechat.yaml`使用。  
这里先放两个官方文档，有需要的自己去翻，我这里只说最简单直观的
[https://docs.librechat.ai/install/configuration/azure_openai.html 8](https://docs.librechat.ai/install/configuration/azure_openai.html)  
[https://docs.librechat.ai/install/configuration/custom_config.html 3](https://docs.librechat.ai/install/configuration/custom_config.html)

配置模板

模板

```
version: 1.0.5
cache: true
# fileStrategy: "firebase"  # If using Firebase CDN
fileConfig:
  endpoints:
version: 1.0.4
cache: true
endpoints:
  azureOpenAI:
    titleModel: "gpt-3.5-turbo"
    assistants: true
    plugins: true
    groups:
    - group: "assistants"
      apiKey: ""
      instanceName: ""
    # Mark this group as assistants compatible
      assistants: true
    # version must be "2024-02-15-preview" or later
      version: "2024-03-01-preview"
      models:
        gpt-3.5-turbo:
          deploymentName: "gpt-35-turbo-0125"
        gpt-3.5-turbo-16k: 
          deploymentName: "gpt-35-turbo-16k"
        gpt-4-1106-preview:
          deploymentName: "gpt-4-1106-preview"
        gpt-4-vision-preview:
          deploymentName: "gpt-4-vision-preview"
          version: "2024-03-01-preview"
  custom:
    - name: "Mistral"
      apiKey: "${MISTRAL_API_KEY}"
      baseURL: "https://api.mistral.ai/v1"
      models:
        default: ["mistral-tiny", "mistral-small", "mistral-medium", "mistral-large-latest"]
        # Attempt to dynamically fetch available models
        fetch: true  
        userIdQuery: false
      iconURL: "https://example.com/mistral-icon.png"
      titleConvo: true
      titleModel: "mistral-tiny"
      modelDisplayLabel: "Mistral AI"
      # addParams:
      # Mistral API specific value for moderating messages
      #   safe_prompt: true 
      dropParams:
        - "stop"
        - "user"
        - "presence_penalty"
        - "frequency_penalty"
      # headers:
      #    x-custom-header: "${CUSTOM_HEADER_VALUE}"
    - name: "OpenRouter"
      apiKey: "${OPENROUTER_API_KEY}"
      baseURL: "https://openrouter.ai/api/v1"
      models:
        default: ["gpt-3.5-turbo"]
        fetch: false
      titleConvo: true
      titleModel: "gpt-3.5-turbo"
      modelDisplayLabel: "OpenRouter"
      dropParams:
        - "stop"
        - "frequency_penalty"
```

参考模板Azure的部分如果不需要可删除或者注释掉Azure

```
azureOpenAI:
    titleModel: "gpt-3.5-turbo"
    assistants: true
# 是否启用助手api
    plugins: true
# 是否可以使用插件
    groups:
    - group: "assistants"
# 名字可以随便取
      apiKey: ""
# 微软给你的密钥
      instanceName: ""
# 你不是的这个Azure OpenAI的服务名
    # Mark this group as assistants compatible
      assistants: true
    # version must be "2024-02-15-preview" or later
      version: "2024-03-01-preview"
      models:
        gpt-3.5-turbo:
          deploymentName: "gpt-35-turbo-0125"
        gpt-3.5-turbo-16k: 
          deploymentName: "gpt-35-turbo-16k"
        gpt-4-1106-preview:
          deploymentName: "gpt-4-1106-preview"
        gpt-4-vision-preview:
          deploymentName: "gpt-4-vision-preview"
          version: "2024-03-01-preview"
```

因为很多人不止部署一个azure的服务，可以以相同的格式再加一组`- group: "assistants2"`

其他模型

```
custom:
    - name: "Mistral"
      apiKey: "${MISTRAL_API_KEY}"
      baseURL: "https://api.mistral.ai/v1"
      models:
        default: ["mistral-tiny", "mistral-small", "mistral-medium", "mistral-large-latest"]
        # Attempt to dynamically fetch available models
        fetch: true  
        userIdQuery: false
      iconURL: "https://example.com/mistral-icon.png"
      titleConvo: true
      titleModel: "mistral-tiny"
      modelDisplayLabel: "Mistral AI"
      # addParams:
      # Mistral API specific value for moderating messages
      #   safe_prompt: true 
      dropParams:
        - "stop"
        - "user"
        - "presence_penalty"
        - "frequency_penalty"
      # headers:
      #    x-custom-header: "${CUSTOM_HEADER_VALUE}"
    - name: "OpenRouter"
      apiKey: "${OPENROUTER_API_KEY}"
      baseURL: "https://openrouter.ai/api/v1"
      models:
        default: ["gpt-3.5-turbo"]
        fetch: false
      titleConvo: true
      titleModel: "gpt-3.5-turbo"
      modelDisplayLabel: "OpenRouter"
      dropParams:
        - "stop"
        - "frequency_penalty"
```

这里简单的解释一下：  
`- name` 随便填  
`apiKey: "${OPENROUTER_API_KEY}"` apikey，`${OPENROUTER_API_KEY}`这个部分括号里的内容，你可以按照这个格式自定义变量，然后在**.env**里填apikey，当然你也可以直接填在这里  
`baseURL`中转api，v1结尾  
`iconURL:`图标网址  
`models:`可以用的模型，有啥填啥，支持中文，按格式一个一个写就行  
`fetch:`是否从当前站点获取所有可用模型，建议false

```
titleConvo: true
titleModel: "gpt-3.5-turbo"
```

标题生成和使用模型，**不一定非要使用前面`models:`里的模型，只要是你的中转api里能使用的就行**  
`modelDisplayLabel:`随便填，一般和name一样就行

```
dropParams:
        - "stop"
        - "user"
        - "presence_penalty"
        - "frequency_penalty"
```

一般不动保持这样就行

## 配置可以按需叠加

**如果你启动后没有看见你配置的模型，那就说明你的配置`librechat.yaml`有问题一般都是格式上的仔细检查**

## 4. 启动

在启动前你需要编辑一下`docker-compose.yml`文件

1. 如果你配置了`librechat.yaml`文件请在api容器的`volumes:`部分加入一行  
    `- ./librechat.yaml:/app/librechat.yaml`
2. 如果你的服务器不支持avx的话请把mongod的版本改到5.0以下，5.0以上强制要求avx，作者推荐的是`image: mongo:4.4.18`

之后就可以正常启动了  
`docker compose up -d`

没有安装 docker 的需要先[[安装 docker]]

## 5.域名
参考[[宝塔#配置反向代理]]
