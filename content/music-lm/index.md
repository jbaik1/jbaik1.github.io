---
title: Music LM
share: false
date: 2023-10-01

---

Music is often considered as a universal language. That is, every culture has developed their own kind of music, and we eventually learn to enjoy various genres, even if some of them feel foreign at first. 

Why not try modeling music as another type of language? 

This is a personal side project that I did, and see the [Github repo](https://github.com/jbaik1/Music-LM) for more details.
 
The [Maestro](https://magenta.tensorflow.org/datasets/maestro) dataset contains classical western repertoire from the 17th to early 20th century. I used [Miditok](https://github.com/Natooz/MidiTok) to tokenize the music. It's like converting music notes into words (it takes into accound pitch, rhythm, etc). 
Miditok uses [BPE](https://huggingface.co/learn/nlp-course/en/chapter6/5) , as well as the [TSD](https://aclanthology.org/2023.emnlp-main.123/) algorithm.

Compared to regular text, the amount of music is actually pretty limited. In order to have enough training data, augmentation is required. 
In computer vision, examples of augmentations are rotations, flipping, etc. 
Here, the augmentations will be in the form of shifting the pitches up and down, changing the tempo, and so on. Here is the configuration I used:

```
TOKENIZER_PARAMS = {
    "pitch_range": (21, 109),
    "beat_res": {(0, 4): 8, (4, 12): 4},
    "num_velocities": 16,
    "use_chords": True,
    "use_rests": True,
    "use_tempos": True,
    "use_time_signatures": True,
    "use_programs": False,
    "num_tempos": 16,  # number of tempo bins
    "tempo_range": (40, 250),  # (min, max)
}
config = TokenizerConfig(**TOKENIZER_PARAMS)
```

A GPT model then trains on this data.

After training for about 24 hours on 1 Tesla V100 GPU (with Google Cloud console), here are some generated music samples:

{{< audio src="norm_long_0.mp3" >}}
 
{{< audio src="norm_long_1.mp3" >}}
 
{{< audio src="norm_long_2.mp3" >}}
 
{{< audio src="norm_long_4.mp3" >}}
 
{{< audio src="norm_long_4.mp3" >}}

Even with the resource limitations, not too bad!

#### Is there only one language for music?

The Maestro dataset only has music from classical western music. However, even within this genre there are significant differences. Baroque, classical, romantic, and modern classical music don't really sound the same. An extreme example is comparing Mozart's Eine Kleine Nachtmusik with Schoenberg's atonal pieces. To be fair, it's also true that all of these eras do share a common understanding of western harmony (except atonal music).

Now there's the issue of combining multiple genres. Jazz and pop songs aren't so bad, since they still have your typical western chord progressions. But what about non-western music([world music](https://en.wikipedia.org/wiki/World_music))? 
If you include every genre, then it might be like training your language model on English and French at the same time. The model might just output random notes.

Also, how would you deal with different "instrumentations"? You can't really represent rap or drums with music notes.  


#### What would pre-trained models look like?

Given a large amount of data for classic western music, it might be possible to create some sort of a “pre-trained” model. When fine-tuning it for a specific composer, the model may be able to generate music more in the style of that composer. 


#### Music Style Transfer 
Style transfer has been studied in vision and generative AI. An analogous subject within NLP is text style transfer. Given that we are able to tokenize music and train ML models as a result, this may enable us to do some version of “music style transfer”. For example, playing happy birthday in the style of Bach. 
