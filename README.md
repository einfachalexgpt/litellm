<h1 align="center"> 
    🚅 LiteLLM 
</h1> 

<p align="center"> 
    <a href="https://render.com/deploy?repo=https://github.com/BerriAI/litellm" target="_blank" rel="nofollow"><img src="https://render.com/images/deploy-to-render-button.svg" alt="Zum Rendern bereitstellen"></a> 
    <a href="https://railway.app/template/HLP0Ub?referralCode=jch2ME"> 
      <img src="https://railway.app/button.svg" alt="Auf der Eisenbahn einsetzen"> 
    </a> 
</p>

<p align="center">Rufen Sie alle LLM-APIs im OpenAI-Format auf [Bedrock, Huggingface, VertexAI, TogetherAI, Azure, OpenAI, Groq usw.]<br></p> 

<h4 align="center"><a href="https://docs.litellm.ai/docs/simple_proxy" target="_blank">LiteLLM Proxy Server (LLM Gateway)</a> | <a href="https://docs.litellm.ai/docs/hosted" target="_blank">Gehosteter Proxy (Vorschau)</a> | <a href="https://docs.litellm.ai/docs/enterprise" target="_blank">Enterprise Tier</a></h4> 

<h4 align="center"> 
    <a href="https://pypi.org/project/litellm/" target="_blank"> 
        <img src="https://img.shields.io/pypi/v/litellm.svg" alt="PyPI-Version"> 
    </a> 
    <a href="https://dl.circleci.com/status-badge/redirect/gh/BerriAI/litellm/tree/main" target="_blank"> 
        <img src="https://dl.circleci.com/status-badge/img/gh/BerriAI/litellm/tree/main.svg?style=svg" alt="CircleCI"> 
    </a> 
    <a href="https://www.ycombinator.com/companies/berriai"> 
        <img src="https://img.shields.io/badge/Y%20Combinator-W23-orange?style=flat-square" alt="Y Combinator W23"> 
    </a> 
    <a href="https://wa.link/huol9n"> 
        <img src="https://img.shields.io/static/v1?label=Chat%20on&message=WhatsApp&color=success&logo=WhatsApp&style=flat-square" alt="WhatsApp"> 
    </a> 
    <a href="https://discord.gg/wuPM9dRgDw"> 
        <img src="https://img.shields.io/static/v1?label=Chat%20on&message=Discord&color=blue&logo=Discord&style=flat-square" alt="Discord"> 
    </a> 
</h4> 

LiteLLM verwaltet:

- Übersetzen von Eingaben in die Endpunkte "Completion", "Embedding" und "image_generation" des Anbieters 
- [Konsistente Ausgabe](https://docs.litellm.ai/docs/completion/output), Textantworten sind immer unter `['choices'][0]['message']['content']` verfügbar. 
- Wiederholungs-/Fallback-Logik über mehrere Bereitstellungen hinweg (z. B. Azure/OpenAI) – [Router](https://docs.litellm.ai/docs/routing) 
- Festlegen von Budgets und Ratenlimits pro Projekt, API-Schlüssel, Modell [LiteLLM Proxy Server (LLM Gateway)](https://docs.litellm.ai/docs/simple_proxy) 

[**Springen Sie zu den Dokumenten zum LiteLLM-Proxy (LLM-Gateway)**](https://github.com/BerriAI/litellm?tab=readme-ov-file#openai-proxy---docs)<br> 
[**Springen Sie zu Unterstützte LLM-Anbieter**](https://github.com/BerriAI/litellm?tab=readme-ov-file#supported-providers-docs) 

🚨 **Stabile Version:** Verwenden Sie Docker-Images mit dem '-stable'-Tag. Diese wurden 12-stündigen Lasttests unterzogen, bevor sie veröffentlicht wurden.  

Unterstützung für mehr Anbieter. Wenn ein Anbieter oder eine LLM-Plattform fehlt, stellen Sie eine [Featureanforderung](https://github.com/BerriAI/litellm/issues/new?assignees=&labels=enhancement&projects=&template=feature_request.yml&title=%5BFeature%5D%3A+). 

# Nutzung ([**Docs**](https://docs.litellm.ai/docs/)) 

> **WICHTIG**  
> LiteLLM v1.0.0 benötigt jetzt `openai>=1.0.0`. Leitfaden für die Migration [hier](https://docs.litellm.ai/docs/migration)  
> LiteLLM v1.40.14+ benötigt nun `pydantic>=2.0.0`. Es sind keine Änderungen erforderlich. 

[![In Colab öffnen](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/BerriAI/litellm/blob/main/cookbook/liteLLM_Getting_Started.ipynb) 

```shell
pip install litellm 
``` 

```python
from litellm import completion 
import os 

## ENV-Variablen setzen 
os.environ["OPENAI_API_KEY"] = "Ihr-openai-Schlüssel" 
os.environ["COHERE_API_KEY"] = "Ihr-cohere-Schlüssel" 

messages = [{ "content": "Hallo, wie geht es Ihnen?", "role": "user"}] 

# OpenAI-Aufruf 
response = completion(model="gpt-3.5-turbo", messages=messages) 

# Cohere-Aufruf 
response = completion(model="command-nightly", messages=messages) 
print(response) 
``` 

Rufen Sie ein beliebiges Modell, das von einem Anbieter unterstützt wird, mit `model=<provider_name>/<model_name>` auf. Hier kann es anbieterspezifische Details geben, also lesen Sie [Anbieterdokumentation für weitere Informationen](https://docs.litellm.ai/docs/providers) 

## Asynchron ([Dokumente](https://docs.litellm.ai/docs/completion/stream#async-completion)) 

```python
from litellm import acompletion 
import asyncio 

async def test_get_response(): 
    user_message = "Hallo, wie geht es dir?" 
    messages = [{"content": user_message, "role": "user"}] 
    response = await acompletion(model="gpt-3.5-turbo", messages=messages) 
    return response 

response = asyncio.run(test_get_response()) 
print(response) 
``` 

## Streaming ([Dokumente](https://docs.litellm.ai/docs/completion/stream)) 

LiteLLM unterstützt das Zurückstreamen der Modellantwort, übergeben Sie `stream=True`, um einen Streaming-Iterator als Antwort zu erhalten.   
Streaming wird für alle Modelle unterstützt (Bedrock, Huggingface, TogetherAI, Azure, OpenAI, etc.) 

```python
from litellm import completion 

response = completion(model="gpt-3.5-turbo", messages=messages, stream=True) 
for part in response: 
    print(part.choices[0].delta.content or "") 

# Claude 2 
response = completion('claude-2', messages, stream=True) 
for part in response: 
    print(part.choices[0].delta.content or "") 
``` 

## Beobachtbarkeit der Protokollierung ([Dokumente](https://docs.litellm.ai/docs/observability/callbacks)) 

LiteLLM stellt vordefinierte Callbacks zur Verfügung, um Daten an Lunary, Langfuse, DynamoDB, s3 Buckets, Helicone, Promptlayer, Traceloop, Athina, Slack zu senden 

```python
from litellm import completion 
import os

## ENV-Variablen für Logging-Tools setzen 
os.environ["LUNARY_PUBLIC_KEY"] = "Ihr-lunary-public_key" 
os.environ["HELICONE_API_KEY"] = "Ihr-helicone-auth-key" 
os.environ["LANGFUSE_PUBLIC_KEY"] = "" 
os.environ["LANGFUSE_SECRET_KEY"] = "" 
os.environ["ATHINA_API_KEY"] = "Ihr-athina-api-key" 

os.environ["OPENAI_API_KEY"] = "Ihr-openai-Schlüssel"

# Callbacks setzen 
litellm.success_callback = ["lunary", "langfuse", "athina", "helicone"] # log Ein-/Ausgabe an lunary, langfuse, supabase, athina, helicone usw. 

# OpenAI Anruf 
response = completion(model="gpt-3.5-turbo", messages=[{"role": "user", "content": "Hallo 👋 - ich bin openai"}]) 
``` 

# LiteLLM Proxy Server (LLM Gateway) - ([Dokumente](https://docs.litellm.ai/docs/simple_proxy)) 

Verfolgen Sie Ausgaben + Load Balance über mehrere Projekte hinweg 

[Gehosteter Proxy (Vorschau)](https://docs.litellm.ai/docs/hosted) 

Der Proxy bietet Folgendes: 

1. [Hooks für die Authentifizierung](https://docs.litellm.ai/docs/proxy/virtual_keys#custom-auth) 
2. [Hooks für die Protokollierung](https://docs.litellm.ai/docs/proxy/logging#step-1---create-your-custom-litellm-callback-class) 
3. [Kostenverfolgung](https://docs.litellm.ai/docs/proxy/virtual_keys#tracking-spend) 
4. [Ratenbegrenzung](https://docs.litellm.ai/docs/proxy/users#set-rate-limits) 

## 📖 Proxy-Endpunkte - [Swagger Docs](https://litellm-api.up.railway.app/) 

## Schnellstart-Proxy - CLI 

```shell
pip install 'litellm[Proxy]' 
``` 

### Schritt 1: LiteLLM Proxy starten 

```shell
litellm --model huggingface/bigcode/starcoder 

#INFO: Proxy wird auf http://0.0.0.0:4000 ausgeführt 
``` 

### Schritt 2: Stellen Sie eine ChatCompletions-Anfrage an den Proxy 

> **WICHTIG**  
> 💡 [Verwenden Sie den LiteLLM-Proxy mit Langchain (Python, JS), OpenAI SDK (Python, JS), Anthropic SDK, Mistral SDK, LlamaIndex, Instructor, Curl](https://docs.litellm.ai/docs/proxy/user_keys)  

```python
import openai  # OpenAI v1.0.0+

client = openai.OpenAI(api_key="anything", base_url="http://0.0.0.0:4000")  # Proxy auf base_url setzen

# Anfrage an das Model-Set auf litellm proxy gesendet, 'litellm --model'
response = client.chat.completions.create(
    model="gpt-3.5-turbo",
    messages=[
        {
            "role": "user",
            "content": "Das ist eine Testaufforderung, schreibe ein kurzes Gedicht"
        }
    ]
)

print(response)
``` 

## Verwaltung von Proxy-Schlüsseln ([Dokumente](https://docs.litellm.ai/docs/proxy/virtual_keys)) 

Verbinden Sie den Proxy mit einer Postgres-DB, um Proxy-Schlüssel zu erstellen 

```shell
# Holen Sie sich den Code 
git clone https://github.com/BerriAI/litellm 

# Zum Ordner gehen 
cd litellm 

# Fügen Sie den Master Key hinzu - Sie können dies nach der Einrichtung ändern 
echo 'LITELLM_MASTER_KEY="SK-1234"' > .env 

# Fügen Sie den LiteLLM Salt-Schlüssel hinzu - Sie können dies nach dem Hinzufügen eines Modells nicht mehr ändern 
# Er wird verwendet, um Ihre LLM-API-Schlüsseldaten zu verschlüsseln / zu entschlüsseln 
# Wir empfehlen den [Passwort-Generator](https://1password.com/password-generator/), um einen zufälligen Hash für den LiteLLM Salt-Schlüssel zu erhalten 
echo 'LITELLM_SALT_KEY="SK-1234"' >> .env 

source .env 

# Starten 
docker-compose up 
``` 

Benutzeroberfläche auf `/ui` auf Ihrem Proxyserver 

![ui_3](https://github.com/BerriAI/litellm/assets/29436595/47c97d5e-b9be-4839-b28c-43d7f4f10033) 

Legen Sie Budgets und Ratenlimits für mehrere Projekte fest  
`POST /key/generate` 

### Anfrage 

```shell
curl 'http://0.0.0.0:4000/key/generate' \
--header 'Authorization: Bearer sk-1234' \
--header 'Content-Type: application/json' \
--data-raw '{"models": ["gpt-3.5-turbo", "gpt-4", "claude-2"], "duration": "20m", "metadata": {"user": "ishaan@berri.ai", "team": "core-infra"}}'
``` 

### Erwartete Antwort 

```shell
{ 
    "key": "sk-kdExB7Sfa", # Bearer Token 
    "expires": "2023-11-19T01:38:25.838000+00:00" # datetime-Objekt 
} 
``` 

## Unterstützte Anbieter ([Docs](https://docs.litellm.ai/docs/providers)) 

| Anbieter | [Completion](https://docs.litellm.ai/docs/#basic-usage) | [Streaming](https://docs.litellm.ai/docs/completion/stream#streaming-responses) | [Asynchrone Completion](https://docs.litellm.ai/docs/completion/stream#async-completion) | [Asynchrones Streaming](https://docs.litellm.ai/docs/completion/stream#async-streaming) | [Asynchrone Embeddings](https://docs.litellm.ai/docs/embedding/supported_embedding) | [Asynchrone Bilderzeugung](https://docs.litellm.ai/docs/image_generation) | 
|-------------------------------------------------------------------------------------|---------------------------------------------------------|---------------------------------------------------------------------------------|-------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------|-------------------------------------------------------------------------------|-------------------------------------------------------------------------| 
| [openai](https://docs.litellm.ai/docs/providers/openai) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | 
| [Azure](https://docs.litellm.ai/docs/providers/azure) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | 
| [aws - Sagemaker](https://docs.litellm.ai/docs/providers/aws_sagemaker) | ✅ | ✅ | ✅ | ✅ | ✅ |  | 
| [aws - Bedrock](https://docs.litellm.ai/docs/providers/bedrock) | ✅ | ✅ | ✅ | ✅ | ✅ |  | 
| [google - vertex_ai](https://docs.litellm.ai/docs/providers/vertex) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | 
| [google - Palm](https://docs.litellm.ai/docs/providers/palm) | ✅ | ✅ | ✅ | ✅ |  |  | 
| [google AI Studio - Gemini](https://docs.litellm.ai/docs/providers/gemini) | ✅ | ✅ | ✅ | ✅ |  |  | 
| [Mistral AI API](https://docs.litellm.ai/docs/providers/mistral) | ✅ | ✅ | ✅ | ✅ | ✅ |  | 
| [Cloudflare AI Workers](https://docs.litellm.ai/docs/providers/cloudflare_workers) | ✅ | ✅ | ✅ | ✅ |  |  | 
| [cohere](https://docs.litellm.ai/docs/providers/cohere) | ✅ | ✅ | ✅ | ✅ | ✅ |  | 
| [anthropic](https://docs.litellm.ai/docs/providers/anthropic) | ✅ | ✅ | ✅ | ✅ |  |  | 
| [empower](https://docs.litellm.ai/docs/providers/empower) | ✅ | ✅ | ✅ | ✅ |  |  | 
| [huggingface](https://docs.litellm.ai/docs/providers/huggingface) | ✅ | ✅ | ✅ | ✅ | ✅ |  | 
| [replicate](https://docs.litellm.ai/docs/providers/replicate) | ✅ | ✅ | ✅ | ✅ |  |  | 
| [together_ai](https://docs.litellm.ai/docs/providers/togetherai) | ✅ | ✅ | ✅ | ✅ |  |  | 
| [OpenRouter](https://docs.litellm.ai/docs/providers/openrouter) | ✅ | ✅ | ✅ | ✅ |  |  | 
| [ai21](https://docs.litellm.ai/docs/providers/ai21) | ✅ | ✅ | ✅ | ✅ |  |  | 
| [baseten](https://docs.litellm.ai/docs/providers/baseten) | ✅ | ✅ | ✅ | ✅ |  |  | 
| [vllm](https://docs.litellm.ai/docs/providers/vllm) | ✅ | ✅ | ✅ | ✅ |  |  | 
| [nlp_cloud](https://docs.litellm.ai/docs/providers/nlp_cloud) | ✅ | ✅ | ✅ | ✅ |  |  | 
| [Aleph Alpha](https://docs.litellm.ai/docs/providers/aleph_alpha) | ✅ | ✅ | ✅ | ✅ |  |  | 
| [Petals](https://docs.litellm.ai/docs/providers/petals) | ✅ | ✅ | ✅ | ✅ |  |  | 
| [Ollama](https://docs.litellm.ai/docs/providers/ollama) | ✅ | ✅ | ✅ | ✅ | ✅ |  | 
| [deepinfra](https://docs.litellm.ai/docs/providers/deepinfra) | ✅ | ✅ | ✅ | ✅ |  |  | 
| [Perplexity AI](https://docs.litellm.ai/docs/providers/perplexity) | ✅ | ✅ | ✅ | ✅ |  |  | 
| [Groq AI](https://docs.litellm.ai/docs/providers/groq) | ✅ | ✅ | ✅ | ✅ |  |  | 
| [Deepseek](https://docs.litellm.ai/docs/providers/deepseek) | ✅ | ✅ | ✅ | ✅ |  |  | 
| [anyscale](https://docs.litellm.ai/docs/providers/anyscale) | ✅ | ✅ | ✅ | ✅ |  |  | 
| [IBM - watsonx.ai](https://docs.litellm.ai/docs/providers/watsonx) | ✅ | ✅ | ✅ | ✅ | ✅ |  | 
| [Voyage AI](https://docs.litellm.ai/docs/providers/voyage) |  |  |  |  | ✅ |  | 
| [xinference [Xorbits Inference]](https://docs.litellm.ai/docs/providers/xinference) |  |  |  |  | ✅ |  | 
| [Friendliai](https://docs.litellm.ai/docs/providers/friendliai) | ✅ | ✅ | ✅ | ✅ |  |  | 

[**Lesen Sie die Dokumente**](https://docs.litellm.ai/docs/) 

## Beitragen 

So tragen Sie bei: Klonen Sie das Repository lokal → Nehmen Sie eine Änderung vor → Übermitteln Sie eine PR mit der Änderung. 

So ändern Sie das Repository lokal: 

**Schritt 1:** Klonen des Repositorys 

```shell
git clone https://github.com/BerriAI/litellm.git 
``` 

**Schritt 2:** Navigieren Sie in das Projekt, und installieren Sie Abhängigkeiten: 

```shell
cd litellm 
poetry install -E extra_proxy -E proxy 
``` 

**Schritt 3:** Testen Sie Ihre Änderung: 

```shell
cd litellm/tests  # pwd: docs/litellm/litellm/tests 
poetry run flake8 
poetry run pytest . 
``` 

**Schritt 4:** Reichen Sie eine PR mit Ihren Änderungen ein! 🚀 

- Pushen Sie Ihren Fork in Ihr GitHub-Repository 
- Von dort aus eine PR einreichen 

# Unternehmen 

Für Unternehmen, die mehr Sicherheit, Benutzerverwaltung und professionellen Support benötigen 

[Sprechen Sie mit Gründern](https://calendly.com/d/4mp-gd3-k5k/litellm-1-1-onboarding-chat) 

Dies umfasst:  
- ✅ **Funktionen unter der [LiteLLM Commercial License](https://docs.litellm.ai/docs/proxy/enterprise)** 
- ✅ **Priorisierung von Funktionen** 
- ✅ **Benutzerdefinierte Integrationen** 
- ✅ **Professioneller Support – Dedizierter Discord + Slack** 
- ✅ **Benutzerdefinierte SLAs** 
- ✅ **Sicherer Zugriff mit Single Sign-On** 

# Unterstützung / Gespräch mit Gründern 

- [Demo planen 👋](https://calendly.com/d/4mp-gd3-k5k/berriai-1-1-onboarding-litellm-hosted-version) 
- [Community Discord 💭 ](https://discord.gg/wuPM9dRgDw) 
- Unsere Nummern 📞 +1 (770) 8783-106 / ‭+1 (412) 618-6238‬ 
- Unsere E-Mails ✉️ ishaan@berri.ai / krrish@berri.ai 

# Warum haben wir das entwickelt? 

- **Bedürfnis nach Einfachheit**: Unser Code wurde extrem kompliziert, wenn es darum ging, Aufrufe zwischen Azure, OpenAI und Cohere zu verwalten und zu übersetzen. 

# Mitwirkende 

<!-- ALL-CONTRIBUTORS-LIST:START - Diesen Abschnitt nicht entfernen oder ändern --> 
<!-- prettier-ignore-start --> 
<!-- markdownlint-disable --> 

<!-- markdownlint-restore --> 
<!-- prettier-ignore-end --> 

<!-- ALL-CONTRIBUTORS-LIST:END --> 

<a href="https://github.com/BerriAI/litellm/graphs/contributors"> 
  <img src="https://contrib.rocks/image?repo=BerriAI/litellm" /> 
</a> 
