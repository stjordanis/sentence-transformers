# Pretrained Models

We provide various pre-trained models. Using these models is easy:

```python
from sentence_transformers import SentenceTransformer
model = SentenceTransformer('model_name')
```

Alternatively, you can download and unzip them from [here](https://public.ukp.informatik.tu-darmstadt.de/reimers/sentence-transformers/v0.2/).

## Sentence Embedding Models

The following models have been tuned to embed sentences and short paragraphs up to a length of 128 word pieces.

Use **paraphrase-mpnet-base-v2** for the best quality, and **paraphrase-MiniLM-L6-v2** if you want a quick model with high quality.


<iframe src="../_static/html/models_en_sentence_embeddings.html" height="500" style="width:100%; border:none;" title="Iframe Example"></iframe>


## Question-Answer Retrieval - MSMARCO

The following models were trained on [MSMARCO Passage Ranking](https://github.com/microsoft/MSMARCO-Passage-Ranking), a dataset with 500k real queries from Bing search. Given a search query, find the relevant passages. 

Models tuned to be used with **cosine-similarity**:
- **msmarco-distilbert-base-v4**: MRR@10: 33.79 on MS MARCO  dev set

Models tuned to be used with **dot-product**:
- **msmarco-distilbert-base-dot-prod-v3**: MRR@10: 33.04 on MS MARCO dev set
- **msmarco-distilbert-base-tas-b**: MRR@10: 34.43 on MS MARCO dev set
- **msmarco-roberta-base-ance-firstp**: MRR@10: 33.03 on MS MARCO  dev set


Models tuned for cosine-similarity will prefer the retrieval of short documents, while models tuned for dot-product will prefer the retrieval of longer documents. Depending on your task, the models of the one or the other type are preferable.

```python
from sentence_transformers import SentenceTransformer, util
model = SentenceTransformer('msmarco-distilbert-base-v3')

query_embedding = model.encode('How big is London')
passage_embedding = model.encode('London has 9,787,426 inhabitants at the 2011 census')

print("Similarity:", util.pytorch_cos_sim(query_embedding, passage_embedding))
```

You can index the passages as shown [here](../examples/applications/semantic-search/README.md).

[More details](pretrained-models/msmarco-v3.md)

## Question-Answer Retrieval - Natural Questions
The following models were trained on [Google's Natural Questions dataset](https://ai.google.com/research/NaturalQuestions), a dataset with 100k real queries from Google search together with the relevant passages from Wikipedia.

- **nq-distilbert-base-v1**: MRR10: 72.36 on NQ dev set (small)

```python
from sentence_transformers import SentenceTransformer, util
model = SentenceTransformer('nq-distilbert-base-v1')

query_embedding = model.encode('How many people live in London?')

#The passages are encoded as [ [title1, text1], [title2, text2], ...]
passage_embedding = model.encode([['London', 'London has 9,787,426 inhabitants at the 2011 census.']])

print("Similarity:", util.pytorch_cos_sim(query_embedding, passage_embedding))
```

You can index the passages as shown [here](../examples/applications/semantic-search/README.md).

[More details](pretrained-models/nq-v1.md)

**DPR-Models**

In [Dense Passage Retrieval  for Open-Domain Question Answering](https://arxiv.org/abs/2004.04906)  Karpukhin et al. trained models based on [Google's Natural Questions dataset](https://ai.google.com/research/NaturalQuestions):
- **facebook-dpr-ctx_encoder-single-nq-base** 
- **facebook-dpr-question_encoder-single-nq-base**

They also trained models on the combination of Natural Questions, TriviaQA, WebQuestions, and CuratedTREC.
- **facebook-dpr-ctx_encoder-multiset-base** 
- **facebook-dpr-question_encoder-multiset-base**

[More details & usage of the DPR models](pretrained-models/dpr.md)

## Multi-Lingual Models
The following models generate aligned vector spaces, i.e., similar inputs in different languages are mapped close in vector space. You do not need to specify the input language.  Details are in our publication [Making Monolingual Sentence Embeddings Multilingual using Knowledge Distillation](https://arxiv.org/abs/2004.09813):

Currently, there are models for two use-cases: 

**Semantic Similarity**

These models find semantically similar sentences within one language or across languages:

- **distiluse-base-multilingual-cased-v1**: Multilingual knowledge distilled version of [multilingual Universal Sentence Encoder](https://arxiv.org/abs/1907.04307). Supports 15 languages:  Arabic, Chinese, Dutch, English, French, German, Italian, Korean, Polish, Portuguese, Russian, Spanish, Turkish. 
- **distiluse-base-multilingual-cased-v2**: Multilingual knowledge distilled version of [multilingual Universal Sentence Encoder](https://arxiv.org/abs/1907.04307). While v1 model supports 15 languages, this version supports 50+ languages. However, performance on the 15 languages mentioned above are reported to be a bit lower. 
- **paraphrase-xlm-r-multilingual-v1** - Multilingual version of *paraphrase-distilroberta-base-v1*, trained on parallel data for 50+ languages. 
- **stsb-xlm-r-multilingual**: Produces similar embeddings as the *stsb-bert-base* model. Trained on parallel data for 50+ languages.
- **quora-distilbert-multilingual** - Multilingual version of *quora-distilbert-base*.  Fine-tuned with parallel data for 50+ languages. 
- **T-Systems-onsite/cross-en-de-roberta-sentence-transformer** - Multilingual model for English an German. [[More]](https://huggingface.co/T-Systems-onsite/cross-en-de-roberta-sentence-transformer)
- **paraphrase-multilingual-MiniLM-L12-v2** - Multilingual version of *paraphrase-MiniLM-L12-v2*, trained on parallel data for 50+ languages. 
- **paraphrase-multilingual-mpnet-base-v2** - Multilingual version of *paraphrase-mpnet-base-v2*, trained on parallel data for 50+ languages. 

**Bitext Mining** 

Bitext mining describes the process of finding translated sentence pairs in two languages. If this is your use-case, the following model gives the best performance:
- **LaBSE** - [LaBSE](https://arxiv.org/abs/2007.01852) Model. Supports 109 languages. Works well for finding translation pairs in multiple languages. As detailed  [here](https://arxiv.org/abs/2004.09813), LaBSE works less well for assessing the similarity of sentence pairs that are not translations of each other.



---

XLM-R models support the following 100 languages.

 Language | Language|Language |Language | Language
---|---|---|---|---
Afrikaans | Albanian | Amharic | Arabic | Armenian 
Assamese | Azerbaijani | Basque | Belarusian | Bengali 
Bengali Romanize | Bosnian | Breton | Bulgarian | Burmese 
Burmese zawgyi font | Catalan | Chinese (Simplified) | Chinese (Traditional) | Croatian 
Czech | Danish | Dutch | English | Esperanto 
Estonian | Filipino | Finnish | French | Galician
Georgian | German | Greek | Gujarati | Hausa
Hebrew | Hindi | Hindi Romanize | Hungarian | Icelandic
Indonesian | Irish | Italian | Japanese | Javanese
Kannada | Kazakh | Khmer | Korean | Kurdish (Kurmanji)
Kyrgyz | Lao | Latin | Latvian | Lithuanian
Macedonian | Malagasy | Malay | Malayalam | Marathi
Mongolian | Nepali | Norwegian | Oriya | Oromo
Pashto | Persian | Polish | Portuguese | Punjabi
Romanian | Russian | Sanskrit | Scottish Gaelic | Serbian
Sindhi | Sinhala | Slovak | Slovenian | Somali
Spanish | Sundanese | Swahili | Swedish | Tamil
Tamil Romanize | Telugu | Telugu Romanize | Thai | Turkish
Ukrainian | Urdu | Urdu Romanize | Uyghur | Uzbek
Vietnamese | Welsh | Western Frisian | Xhosa | Yiddish

We used the following languages for [Multilingual Knowledge Distillation](https://arxiv.org/abs/2004.09813): ar, bg, ca, cs, da, de, el, es, et, fa, fi, fr, fr-ca, gl, gu, he, hi, hr, hu, hy, id, it, ja, ka, ko, ku, lt, lv, mk, mn, mr, ms, my, nb, nl, pl, pt, pt, pt-br, ro, ru, sk, sl, sq, sr, sv, th, tr, uk, ur, vi, zh-cn, zh-tw. 

Extending a model to new languages is easy by following [the description here](https://www.sbert.net/examples/training/multilingual/README.html).


## Scientific Publications
[SPECTER](https://arxiv.org/abs/2004.07180) is a model trained on scientific citations and can be used to estimate the similarity of two publications. We can use it to find similar papers.

- **allenai-specter** - [Semantic Search Python Example](https://github.com/UKPLab/sentence-transformers/blob/master/examples/applications/semantic-search/semantic_search_publications.py) / [Semantic Search Colab Example](https://colab.research.google.com/drive/12hfBveGHRsxhPIUMmJYrll2lFU4fOX06)


## Average Word Embeddings Models

The following models apply compute the average word embedding for some well-known word embedding methods. Their computation speed is much higher than the transformer based models, but the quality of the embeddings are worse.
- **average_word_embeddings_glove.6B.300d**
- **average_word_embeddings_komninos**
- **average_word_embeddings_levy_dependency**
- **average_word_embeddings_glove.840B.300d**


## Image & Text-Models
The following models can embed images and text into a joint vector space. See [Image Search](../examples/applications/image-search/README.md)  for more details how to use for text2image-search, image2image-search, image clustering, and zero-shot image classification.
- **clip-ViT-B-32** - [OpenAPI CLIP Model](https://github.com/openai/CLIP)
- **clip-ViT-B-32-multilingual-v1** - Multilingual text encoder for the CLIP model using [Multilingual Knowledge Distillation](https://arxiv.org/abs/2004.09813).