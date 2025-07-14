---
layout: post
title: Harmonic in speech processing
date: 2025-03-12 14:00:00
description: Understand the concept of harmonics in audio
tags:
categories:
thumbnail: assets/img/harmonic_audio/spectrogram.png
---

When I started working in the audio group, I often heard people mention the term **harmonic** in the context of speech processing. Something like *the harmonics become stronger when this solution is applied* or *the lower harmonics are emphasized were commonly used*. At first, I wasn’t sure what they meant.

According to Wikipedia, a harmonic is defined as "*a sinusoidal wave with a frequency that is a positive integer multiple of the fundamental frequency of a periodic signal*".
In other words, harmonics are a collection of frequency components that include the fundamental frequency and its integer multiples. The fundamental frequency is the lowest frequency of a periodic waveform, and the human voice is, essentially, a complex periodic waveform.

As we know from the Fourier series, any periodic signal can be decomposed into a sum of sinusoidal signals. The formula is typically written as:  
$$
\begin{align}
x(t) = \sum_{n=\infty}^{\infty}{c_ne^{j2{\pi}nF_0t}}
\end{align}
$$

This equation shows that a periodic signal contains only frequencies that are integer multiples of the fundamental frequency $F_0$. 

So now we understand why harmonics naturally occur in periodic signals. But this raises another question: Why is the human voice a periodic waveform?  
 
The answer is quite straightforward: the vocal folds (or vocal cords) generate sound by vibrating periodically. This periodic vibration results in a waveform with a clear fundamental frequency and its harmonics. We can actually visualize these harmonics in a spectrogram of human speech, where the horizontal bands represent harmonic components.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/harmonic_audio/spectrogram.png" title="spectrogram" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    A spectrogram (0-5000 Hz) of the sentence "it's all Greek to me" spoken by a female voice from <a href="https://en.m.wikipedia.org/wiki/File:Human_voice_spectrogram.jpg">here</a>
</div>  

In speech processing tasks such as noise reduction or source separation, recovering or preserving harmonic structures is very important. Harmonics hold essential information about the pitch and tone of the voice, and they can affect perceived quality and intelligibility.