# Llama2_LORA_Finetune_Multilingual_Sentiment_Analysis

In this project, we perform Parameter Efficient Fine-Tuning (PEFT) on Meta's Llama2-7b model using Quantized - Low Rank Adaptation (QLoRA) technique.

[Llama2](https://huggingface.co/meta-llama/Llama-2-7b): Llama 2 is a collection of pretrained and fine-tuned generative text models by Meta, ranging in scale from 7 billion to 70 billion parameters. Here, we specifially use the 7 Billion version.

[QLoRA](https://arxiv.org/abs/2305.14314): QLoRA is a technique to dramatically reduce the trainable parameters and speed-up the finetuning of **quantized** Large Language Models (LLMs). Using QLoRA, we only train 0.6% of the parameters of Llama2-7B.

### Task: Multilingual Sentiment Analysis
#### Dataset: 
- We use a multilingual sentiment dataset from huggingface: [tyqiangz/multilingual-sentiments](https://huggingface.co/datasets/tyqiangz/multilingual-sentiments)
- The dataset contains reviews/tweets from more than 14 different languages and categorizes them into three labels: **positive, neutral and negative**.
- We finetune our LLM only on ~2% of the training set and still see a major improvement in it's generalization performance post finetuning.

#### Results: 
Performance on the test set:
| Model       | Test Accuracy (%)   | Test AUC (%)|
| ------------- |:-------------:|:-----:|
| Llama2-7b (base) | 33.67 | 48.84 |
| Llama2-7b (finetuned with QLoRA) | 62.17 | 78.78 |

#### Takeaways: 
- We get ~30% increase in AUC with just 10 epochs of finetuning on just ~2% of the training data.
- The results can be improved further with more sophisticated PEFT for larger epochs on a larger subset of data
- Nevertheless, the significant boost in the downstream performance with minimal finetuning highlights the power of LLMs coupled with QLoRA which provides an efficient way to finetune LLMs in a limited compute capacity.

#### Performance on Unseen Samples

| Text       | Label   |Base Model   | Finetuned Model|
| ------------- |:-------------:|:-------------:|:-----:|
| Horrible Uber Go trips! #UBER get the things sorted plz. #UberIndia | Negative | Neutral | Negative |
|@user Never trust Comey😡 if it wasn't for him we would have Hillary 😠 F Comey!! Get rid of him | Negative | Neutral | Negative |
| #2016 is the new #1966 first #brexit then #trumpton now a crap band from #Romford have done for the Italian president #Renzi #5* #5star  | Negative | Neutral | Negative |
| #cannabis The Associated PressAssistant cultivator Emily Errico examines cannabis plants grown by Vireo Health of … | Neutral | Positive | Positive |

- In this case, identifying a negative comment is much important than neutral/positive comment as the goal is to make LLM understand hateful content.
- As we can observe, the base model wrongly identifies most negative comments as neutral.
- Whereas the finetuned model understands the negative comments better!

#### We can see some misclassifications from the finetuned model as well. 
#### NOTE that we have just finetuned on a minute subset (~2%) of the training data (due to compute restraints). We can get better performance as we upscale the training corpus and the finetuning hyperparameters




