🧬Fine-tuning Guide
Learn all the basics and best practices of fine-tuning. Beginner-friendly.

1. Understand Fine-tuning
Fine-tuning an LLM customizes its behavior, enhances + injects knowledge, and optimizes performance for domains/specific tasks. For example:

GPT-4 serves as a base model; however, OpenAI fine-tuned it to better comprehend instructions and prompts, leading to the creation of ChatGPT-4 which everyone uses today.

​DeepSeek-R1-Distill-Llama-8B is a fine-tuned version of Llama-3.1-8B. DeepSeek utilized data generated by DeepSeek-R1, to fine-tune Llama-3.1-8B. This process, known as distillation (a subcategory of fine-tuning), injects the data into the Llama model to learn reasoning capabilities.

With Unsloth, you can fine-tune for free on Colab, Kaggle, or locally with just 3GB VRAM by using our notebooks. By fine-tuning a pre-trained model (e.g. Llama-3.1-8B) on a specialized dataset, you can:

Update + Learn New Knowledge: Inject and learn new domain-specific information.

Customize Behavior: Adjust the model’s tone, personality, or response style.

Optimize for Tasks: Improve accuracy and relevance for specific use cases.

Example usecases:

Train LLM to predict if a headline impacts a company positively or negatively.

Use historical customer interactions for more accurate and custom responses.

Fine-tune LLM on legal texts for contract analysis, case law research, and compliance.

You can think of a fine-tuned model as a specialized agent designed to do specific tasks more effectively and efficiently. Fine-tuning can replicate all of RAG's capabilities, but not vice versa.

Fine-tuning misconceptions:
You may have heard that fine-tuning does not make a model learn new knowledge or RAG performs better than fine-tuning. That is false. Read more FAQ + misconceptions here:

🤔
FAQ + Is Fine-tuning Right For Me?
2. Choose the Right Model + Method
If you're a beginner, it is best to start with a small instruct model like Llama 3.1 (8B) and experiment from there. You'll also need to decide between QLoRA and LoRA training:

LoRA: Fine-tunes small, trainable matrices in 16-bit without updating all model weights.  

QLoRA: Combines LoRA with 4-bit quantization to handle very large models with minimal resources. 


You can change the model name to whichever model you like by matching it with model's name on Hugging Face e.g. 'unsloth/llama-3.1-8b-bnb-4bit'.

There are 3 other settings which you can toggle:

max_seq_length = 2048 – Controls context length. While Llama-3 supports 8192, we recommend 2048 for testing. Unsloth enables 4× longer context fine-tuning.

dtype = None – Defaults to None; use torch.float16 or torch.bfloat16 for newer GPUs.

load_in_4bit = True – Enables 4-bit quantization, reducing memory use 4× for fine-tuning on 16GB GPUs. Disabling it on larger GPUs (e.g., H100) slightly improves accuracy (1–2%).

For full-finetuning - set full_finetuning = True  and 8-bit finetuning - set load_in_8bit = True 

We recommend starting with QLoRA, as it is one of the most accessible and effective methods for training models. Our dynamic 4-bit quants, the accuracy loss for QLoRA compared to LoRA is now largely recovered.

You can also do reasoning (GRPO), vision, reward modelling (DPO, ORPO, KTO), continued pretraining, text completion and other training methodologies with Unsloth.

Read our detailed guide on choosing the right model:

❓
What Model Should I Use?
3. Your Dataset
For LLMs, datasets are collections of data that can be used to train our models. In order to be useful for training, text data needs to be in a format that can be tokenized.

You will need to create a dataset usually with 2 columns - question and answer. The quality and amount will largely reflect the end result of your fine-tune so it's imperative to get this part right.

You can synthetically generate data and structure your dataset (into QA pairs) using ChatGPT or local LLMs.

Fine-tuning can learn from an existing repository of documents and continuously expand its knowledge base, but just dumping data alone won’t work as well. For optimal results, curate a well-structured dataset, ideally as question-answer pairs. This enhances learning, understanding, and response accuracy.

But, that's not always the case, e.g. if you are fine-tuning a LLM for code, just dumping all your code data can actually enable your model to yield significant performance improvements, even without structured formatting. So it really depends on your use case.

Read more about creating your dataset:

📈
Datasets Guide
For most of our notebook examples, we utilize the Alpaca dataset however other notebooks like Vision will use different datasets which may need images in the answer ouput as well.

4. Understand Model Parameters
There are millions of hyperparameters combinations and choosing the right numbers are crucial to a good result. You can edit the parameters (numbers) below, but you can ignore it, since we already select quite reasonable numbers.


The goal is to change these numbers to increase accuracy, but also counteract over-fitting. Over-fitting is when you make the language model memorize a dataset, and not be able to answer novel new questions. We want to a final model to answer unseen questions, and not do memorization. Here are the key parameters:

Learning Rate
Defines how much the model’s weights adjust per training step.

Higher Learning Rates: Faster training, reduces overfitting just make sure to not make it too high as it will overfit

Lower Learning Rates: More stable training, may require more epochs.

Typical Range: 1e-4 (0.0001) to 5e-5 (0.00005).

Epochs
Number of times the model sees the full training dataset.

Recommended: 1-3 epochs (anything more than 3 is generally not optimal unless you want your model to have much less hallucinations but also less creativity and variety in answers)

More Epochs: Better learning, higher risk of overfitting.

Fewer Epochs: May undertrain the model.

For a complete guide on how hyperparameters affect training, see:

🧠
LoRA Hyperparameters Guide
Avoiding Overfitting & Underfitting
Overfitting (Too Specialized)
The model memorizes training data, failing to generalize to unseen inputs. Solution:

If your training duration is short, lower the learning rate. For longer training runs, increase the learning rate. Because of this, it might be best to test both and see which is better.

Increase batch size.

Lower the number of training epochs.

Combine your dataset with a generic dataset e.g. ShareGPT

Increase dropout rate to introduce regularization.

Underfitting (Too Generic)
Though not as common, underfitting is where a low rank model fails to generalize due to a lack of learnable params and so your model may fail to learn from training data. Solution:

If your training duration is short, increase the learning rate. For longer training runs, reduce the learning rate. 

Train for more epochs.

Increasing rank and alpha. Alpha should at least equal to the rank number, and rank should be bigger for smaller models/more complex datasets; it usually is between 4 and 64.

Use a more domain-relevant dataset.

Fine-tuning has no single "best" approach, only best practices. Experimentation is key to finding what works for your needs. Our notebooks auto-set optimal parameters based on evidence from research papers and past experiments.

5. Installing + Requirements
We would recommend beginners to utilise our pre-made notebooks first as it's the easiest way to get started with guided steps. However, if installing locally is a must, you can install and use Unsloth - just make sure you have all the right requirements necessary. Also depending on the model and quantization you're using, you'll need enough VRAM and resources. See all the details here:

🛠️
Unsloth Requirements
Next, you'll need to install Unsloth. Unsloth currently only supports Windows and Linux devices. Once you install Unsloth, you can copy and paste our notebooks and use them in your own local environment. We have many installation methods:

📥
Installing + Updating
6. Training + Evaluation
Once you have everything set, it's time to train! If something's not working, remember you can always change hyperparameters, your dataset etc. 

You will see a log of some numbers whilst training! This is the training loss, and your job is to set parameters to make this go to as close to 0.5 as possible! If your finetune is not reaching 1, 0.8 or 0.5, you might have to adjust some numbers. If your loss goes to 0, that's probably not a good sign as well!


The training loss will appear as numbers
We generally recommend keeping the default settings unless you need longer training or larger batch sizes.

per_device_train_batch_size = 2 – Increase for better GPU utilization but beware of slower training due to padding. Instead, increase gradient_accumulation_steps for smoother training.

gradient_accumulation_steps = 4 – Simulates a larger batch size without increasing memory usage.

max_steps = 60 – Speeds up training. For full runs, replace with num_train_epochs = 1 (1–3 epochs recommended to avoid overfitting).

learning_rate = 2e-4 – Lower for slower but more precise fine-tuning. Try values like 1e-4, 5e-5, or 2e-5.

Evaluation
In order to evaluate, you could do manually evaluation by just chatting with the model and see if it's to your liking.  You can also enable evaluation for Unsloth, but keep in mind it can be time-consuming depending on the dataset size. To speed up evaluation you can: reduce the evaluation dataset size or set evaluation_steps = 100.

For testing, you can also  take 20% of your training data and use that for testing. If you already used all of the training data, then you have to manually evaluate it. You can also use automatic eval tools like EleutherAI’s lm-evaluation-harness. Keep in mind that automated tools may not perfectly align with your evaluation criteria.

7. Running + Saving the model

Now let's run the model after we completed the training process! You can edit the yellow underlined part! In fact, because we created a multi turn chatbot, we can now also call the model as if it saw some conversations in the past like below:


Reminder Unsloth itself provides 2x faster inference natively as well, so always do not forget to call FastLanguageModel.for_inference(model). If you want the model to output longer responses, set max_new_tokens = 128 to some larger number like 256 or 1024. Notice you will have to wait longer for the result as well!

Saving the model
For saving and using your model in desired inference engines like Ollama, vLLM, Open WebUI, we can have more information here:

🖥️
Running & Saving Models
We can now save the finetuned model as a small 100MB file called a LoRA adapter like below. You can instead push to the Hugging Face hub as well if you want to upload your model! Remember to get a Hugging Face token via: https://huggingface.co/settings/tokens and add your token!


After saving the model, we can again use Unsloth to run the model itself! Use FastLanguageModel again to call it for inference!


8. We're done!
You've successfully finetuned a language model and exported it to your desired inference engine with Unsloth!

To learn more about finetuning tips and tricks, head over to our blogs which provide tremendous and educational value: https://unsloth.ai/blog/

If you need any help on finetuning, you can also join our Discord server here. Thanks for reading and hopefully this was helpful!