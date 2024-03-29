---
layout: post
title: Thoughts on how modern ASR can be improved
tags: deep_learning, ASR, Whisper
---



These days I'm experimenting with Whisper ASR. This model has a remarkable accuracy for many languages, and what's surprising is that the Word error rate (WER) for Spanish is one of the lowest across all the languages model was trained.

Whisper was trained on 680,000 hours of audio, predominantly English, with only 117 hours covering all other 96 languages. I'll be speculative now, though the following makes sense. English and Spanish are relatively close to each other, so training on English should already result in a model which should be easily adaptable to Spanish and perhaps other languages. Another observation is that Spanish phonetically and orthographically is more straightforward than English. In Spanish, what you write corresponds precisely to what you hear.

On MLS (multi-lingual Librispeech), it has already reached 4.2 % WER - this is remarkable; it's on par with native Spanish speakers' capabilities. Imagine the degree of synergy if a human expert were involved only for problematic words, where ASR results are of low confidence.

For my exploration, I picked [El Hilo](https://elhilo.audio/) podcast produced by Radio Ambulante. This fact is important - because "Radio Ambulante" is a proper noun, it's not common to see words Radio and Ambulante next to each other, it will be difficult for the model to recognize it correctly.
<audio controls src="https://sphinx.acast.com/p/acast/s/el-hilo/e/63d34c0fdd7a730010e0f4f3/media.mp3" />


Here is what I've got intially:
```
whisper_stt.transcribe(wav_clip)
{
	'text': ' Bienvenidos a El Hilo, un podcast de Radio Mulante Studio.',
	'avg_logprob': -0.313,
	'compression_ratio': 0.906,
}
```

The result is okay, though **Ambulante** was recognized as **Mulante**. Whisper supports prompting, a kind of conditional information that should navigate the model's outputs. So I tried to use prompts as dictionaries.
```
whisper_stt.transcribe(wav_clip, initial_prompt="Ambulante")
{
	'text': ' Bienvenidos a El Hilo, un podcast de Radio Ambulante Studios.',
	'avg_logprob': -0.263,
	'compression_ratio': 0.897,
}
```


Notice that now **Ambulante** is transcribed correctly. Also, the probability of this transcript increased. Interestingly, adding **El Hilo** to the prompt helped to increase probability even more. So the model pays attention to prompts and also "enjoys" seeing something similar to its hypothesis. It still struggles to transcribe **<span style="color:red">E</span>studios**, perhaps due to the model being trained in a multilingual setting with a huge imbalance towards English data.


Lastly, I decided to mislead the model
```
whisper_stt.transcribe(wav_clip, initial_prompt="Radio Aminilantes")
{
	'text': ' Bienvenidos a El Hilo, un podcast de Radio Aminilantes Studios.',
	'avg_logprob': -0.301,
	'compression_ratio': 0.913,
}
```

Well, now it follows custom spelling and transcribes words paying attention to prompts; now we have **Radio Aminilantes** exactly the way we prompted it to be. 

Overall this is pretty cool, and now I'm wondering how transcriptions could be improved for domain-specific data with a lot of terminologies: medicine, etc. I think this is not a part of the model's design and is a nice side effect, though a powerful one. There is no explicit structure or guarantee that model will pay attention to the prompt. However this can be designed explicitly and incorporated in the decoding process.

Another idea is to use prompting for creating pronunciation dictionaries because today's high-quality TTS models fail to pronounce foreign words that are uncommon in English. Let me know if you'd like to work on this problem; we could join our efforts. Also, if you'll find more interesting behavior modes of whisper - please share your findings!
