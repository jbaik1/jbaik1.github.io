---
title: Music LM
share: false
date: 2023-10-01

profiles:
- icon: brands/github
   url: https://github.com/jbaik1
---

Music is often considered as a universal language. That is, every culture has developed their own kind of music, and we eventually learn to enjoy various genres, even if some of them feel foreign at first. 

Why not try modeling music as another type of language? 

This is a personal side project that I did, and see the [Github repo](https://github.com/jbaik1/Music-LM) for more details.
 
The [Maestro](https://magenta.tensorflow.org/datasets/maestro) dataset contains classical western repertoire from the 17th to early 20th century. I used [Miditok](https://github.com/Natooz/MidiTok) to tokenize the music. It's like converting music notes into words (it takes into accound pitch, rhythm, etc). 
Miditok uses [BPE](https://huggingface.co/learn/nlp-course/en/chapter6/5) , as well as the [TSD](https://aclanthology.org/2023.emnlp-main.123/) algorithm.

The music is tokenized

A GPT model then trains on this data

After training for about 24 hours on 1 Tesla V100 GPU, here are some generated music samples:

{{< audio src="norm_long_0.mp3" >}}
 
{{< audio src="norm_long_1.mp3" >}}
 
{{< audio src="norm_long_2.mp3" >}}
 
{{< audio src="norm_long_4.mp3" >}}
 
{{< audio src="norm_long_4.mp3" >}}

Even with the resource limitations, not too bad!


#### Some potential links with pre-trained models

Given a large amount of data for classic western music, it might be possible to create some sort of a “pre-trained” model. When fine-tuning it for a specific composer, the model may be able to generate music more in the style of that composer. 

A hypothesis is that the music will have to be related to eachother to have better results. Classical, Jazz, Rock, etc. share similar western style chord progressions. Non-western music ([world music](https://en.wikipedia.org/wiki/World_music)) may be too dissimilar. Even within classical western music, atonal music from composers like Schoenberg are quite different from music from Mozart.

#### Music Style Transfer 
Style transfer has been studied in vision and generative AI. An analogous subject within NLP is text style transfer. Given that we are able to tokenize music and train ML models as a result, this may enable us to do some version of “music style transfer”. For example, playing happy birthday in the style of Bach. 
