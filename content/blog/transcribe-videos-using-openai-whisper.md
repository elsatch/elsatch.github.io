---
title: "Transcribe videos using OpenAI Whisper for free"
description: "meta description"
image: "images/post/transcribe-videos-using-openai-whisper-for-free/transcribe-videos-using-openai-whisper-for-free.jpeg"
date: 2022-11-14T18:00:00+02:00
categories: ["deep learning"]
tags: ["audio transcription"]
type: "regular" # available types: [featured/regular]
draft: false
mermaid: false
---

#### Introduction

OpenAI, the company behind GPT-3 and DALL-E 2 has just released a voice model called Whisper that can transcribe audio fragments to multiple languages and translate them to English. The main difference to the other two models is that Whisper is available with an open source license. So we can download it, customize it and run it as much as we want. Let's try it!

We will setup a local environment to run our Whisper project. In particular, we will be interested to transcribe some of our Youtube videos to text.

#### Setting up the environment for Data Science

During the last weeks, I have been exploring different templates to organize my data science projects. So I have decided to apply one of those, the Cookiecutter for Data Science as basis for this project. Let's configure it from scratch

##### Setting up the virtual environment

CookieCutter is a python program to produce templates based on rules and user prompts. We will use in particular the [Data Science CookieCutter](https://github.com/drivendata/cookiecutter-data-science). The documentation recommends setting up the virtual environments using virtualenvwrapper.

```sh
pip install virtualenvwrapper
export WORKON_HOME=~/Envs
mkdir -p $WORKON_HOME
source /home/elsatch/miniconda3/bin/virtualenvwrapper.sh
```

Please note that the route to the `virtualenvwrapper.sh` file might vary on your system. This route worked for Ubuntu plus miniconda, but on my Mac the route was: `source /Users/username/opt/anaconda3/bin/virtualenvwrapper.sh`.

To create and activate the virtual environment:

```sh
mkvirtualenv whisper_env
```

##### Setting up CookieCutter

To install CookieCutter:

```sh
python3 -m pip install cookiecutter
```

To initialize our project, `yt_whisper` we just run this command, filling the different options:

```sh
cookiecutter https://github.com/drivendata/cookiecutter-data-science
```

##### Setting up Whisper

To install Whisper:

```sh
pip install git+https://github.com/openai/whisper.git
```

##### Testing Whisper

To test Whisper, we have created a couple of scripts inside the data folder. First one, get_videos.py will download the audio from a Youtube video to your `data/external` folder. YouTube offers several formats (or streams) for each video. To list all of them, you can use the tools `youtube-dl` or `yt-dlp`:

```sh
yt-dlp -F https://www.youtube.com/watch\?v\=9hfqdGO49Po

[youtube] 9hfqdGO49Po: Downloading webpage
[youtube] 9hfqdGO49Po: Downloading android player API JSON
[info] Available formats for 9hfqdGO49Po:
ID  EXT   RESOLUTION FPS │   FILESIZE   TBR PROTO │ VCODEC          VBR ACODEC      ABR     ASR MORE INFO
──────────────────────────────────────────────────────────────────────────────────────────────────────────────────
sb2 mhtml 48x27          │                  mhtml │ images                                      storyboard
sb1 mhtml 80x45          │                  mhtml │ images                                      storyboard
sb0 mhtml 160x90         │                  mhtml │ images                                      storyboard
139 m4a                  │   12.25MiB   48k https │ audio only          mp4a.40.5   48k 22050Hz low, m4a_dash
249 webm                 │   12.78MiB   50k https │ audio only          opus        50k 48000Hz low, webm_dash
250 webm                 │   15.88MiB   63k https │ audio only          opus        63k 48000Hz low, webm_dash
140 m4a                  │   32.51MiB  129k https │ audio only          mp4a.40.2  129k 44100Hz medium, m4a_dash
251 webm                 │   28.98MiB  115k https │ audio only          opus       115k 48000Hz medium, webm_dash
17  3gp   176x144      6 │   20.78MiB   82k https │ mp4v.20.3       82k mp4a.40.2    0k 22050Hz 144p
394 mp4   256x144     25 │   21.94MiB   87k https │ av01.0.00M.08   87k video only              144p, mp4_dash
160 mp4   256x144     25 │   11.21MiB   44k https │ avc1.4d400c     44k video only              144p, mp4_dash
278 webm  256x144     25 │   23.34MiB   92k https │ vp9             92k video only              144p, webm_dash
395 mp4   426x240     25 │   34.87MiB  138k https │ av01.0.00M.08  138k video only              240p, mp4_dash
133 mp4   426x240     25 │   22.90MiB   91k https │ avc1.4d4015     91k video only              240p, mp4_dash
242 webm  426x240     25 │   40.59MiB  161k https │ vp9            161k video only              240p, webm_dash
396 mp4   640x360     25 │   61.12MiB  243k https │ av01.0.01M.08  243k video only              360p, mp4_dash
134 mp4   640x360     25 │   40.80MiB  162k https │ avc1.4d401e    162k video only              360p, mp4_dash
18  mp4   640x360     25 │ ~ 74.99MiB  291k https │ avc1.42001E    291k mp4a.40.2    0k 44100Hz 360p
243 webm  640x360     25 │   74.18MiB  295k https │ vp9            295k video only              360p, webm_dash
397 mp4   854x480     25 │  107.40MiB  427k https │ av01.0.04M.08  427k video only              480p, mp4_dash
135 mp4   854x480     25 │   68.83MiB  274k https │ avc1.4d401e    274k video only              480p, mp4_dash
244 webm  854x480     25 │  124.10MiB  494k https │ vp9            494k video only              480p, webm_dash
398 mp4   1280x720    25 │  205.31MiB  817k https │ av01.0.05M.08  817k video only              720p, mp4_dash
136 mp4   1280x720    25 │  109.82MiB  437k https │ avc1.4d401f    437k video only              720p, mp4_dash
22  mp4   1280x720    25 │ ~145.66MiB  566k https │ avc1.64001F    566k mp4a.40.2    0k 44100Hz 720p
247 webm  1280x720    25 │  239.34MiB  953k https │ vp9            953k video only              720p, webm_dash
399 mp4   1920x1080   25 │  395.44MiB 1575k https │ av01.0.08M.08 1575k video only              1080p, mp4_dash
137 mp4   1920x1080   25 │  334.54MiB 1332k https │ avc1.640028   1332k video only              1080p, mp4_dash
248 webm  1920x1080   25 │  432.80MiB 1723k https │ vp9           1723k video only              1080p, webm_dash
```

Out of all the audio formats, the "best quality" in m4a is 140 (129kbps at 44100Hz). You might try also the webm format (251). We will pass this as a parameter in our script get_videos.py:

```python
from pytube import YouTube

# Video Negocios de Impresión 3D bajo demanda con Diego Trapero

yt = YouTube('https://www.youtube.com/watch?v=9hfqdGO49Po')

stream = yt.streams.get_by_itag(140)
stream.download(output_path='../../data/external/', filename='negocios3D.m4a')
```

Now it's time to transcribe it using Whisper. You can do it using a script like this:

```python
import whisper

model = whisper.load_model('medium')
result = model.transcribe('../../data/external/negocios3D.m4a')
print(result['text'])
```

This a sample output using the medium model:

> Hola, bienvenidos a un nuevo episodio de Laura Maker, bienvenidos a una nueva entrevista. Hoy, por petición popular, volvemos a estar con... De una persona. Diego Trapero, efectivamente. Alguien vio la entrevista y le gustó y dijo, tenéis que volver a veros. Y aquí estamos. Solo que veréis que esto ha cambiado un poco para los que nos estéis suscitando por el podcast. Tenéis un vídeo con un tour que acabamos de preparar para que veáis cómo son las oficinas de Bitfab, que es el proyecto que está lanzando Diego y en el que hablamos en la última entrevista. Pero claro, no es lo mismo hablarlo en abstracto que estar aquí. Bueno, cuéntanos dónde estamos y qué es lo que tenemos por aquí. 

As you, Spanish speakers can see, the output is quite precise. It got almost everything right, except for the podcast name.

The larger the model, the better the accuracy but... the longer the times and the biggest GPU memory you need. As per the [OpenAI Whisper Documentation](https://raw.githubusercontent.com/openai/whisper/main/README.md):

|  Size  | Parameters | English-only model | Multilingual model | Required VRAM | Relative speed |
|:------:|:----------:|:------------------:|:------------------:|:-------------:|:--------------:|
|  tiny  |    39 M    |     `tiny.en`      |       `tiny`       |     ~1 GB     |      ~32x      |
|  base  |    74 M    |     `base.en`      |       `base`       |     ~1 GB     |      ~16x      |
| small  |   244 M    |     `small.en`     |      `small`       |     ~2 GB     |      ~6x       |
| medium |   769 M    |    `medium.en`     |      `medium`      |     ~5 GB     |      ~2x       |
| large  |   1550 M   |        N/A         |      `large`       |    ~10 GB     |       1x       |

In my particular case, I could only use up to medium as my RTX2060 has 6 GB of VRAM. I tried running large, but run into an error:

```none
OutOfMemoryError: CUDA out of memory. Tried to allocate 26.00 MiB (GPU 0; 5.79 GiB total capacity; 4.49 GiB already allocated; 11.19 MiB free; 4.70 GiB reserved in total by PyTorch)
```

##### How much would cost to transcribe at scale

I'd like to transcribe my complete channel of Youtube videos. In total, they are around 300 videos, with an average duration of 15 min (aprox). Total running time would be around 75 hours. Let's say there are 100 hours to round up.

To run the transcription, I'd need to get a cloud instance equipped with a GPU that has at least 10 Gb of RAM. Checking some providers, and assuming no overhead to run this task (setting up the environment, scheduling, etc) it would cost:

| Provider   | Instance      | GPU  | GPU VRAM | Cost per hour | Total cost |
|:----------:|:-------------:|:----:|:--------:|:-------------:|:----------:|
| Lambda Labs| 1xNVIDIA A100 | A100 | 40 GB    | 1.10 USD      |  110 USD   |
| Paperspace | P5000         | P5000| 16 GB    | 0.78 USD      |  78 USD    |

Then you'd need to add storage, network transfer, etc. But this is a starting point!

##### Conclusions

Compared to other previous attempts at transcribing, the quality of OpenAI Whisper is astonishing. It creates punctuation and can work in several languages. Next step on the road, learn how to serialize/paralelize the work, so I can process all videos in a controlled manner.

_Photo by Kristina Flour at Unsplash_

