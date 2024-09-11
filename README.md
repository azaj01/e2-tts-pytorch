
<img src="./e2-tts.png" width="400px"></img>

## E2 TTS - Pytorch

Implementation of E2-TTS, <a href="https://arxiv.org/abs/2406.18009v1">Embarrassingly Easy Fully Non-Autoregressive Zero-Shot TTS</a>, in Pytorch

The repository differs from the paper in that it uses a <a href="https://arxiv.org/abs/2107.10342">multistream transformer</a> for text and audio, with conditioning done every transformer block in the E2 manner.

It also includes an improvisation that was proven out by Manmay, where the text is simply interpolated to the length of the audio for conditioning. You can try this by setting `interpolated_text = True` on `E2TTS`

## Appreciation

- <a href="https://github.com/manmay-nakhashi">Manmay</a> for contributing <a href="https://github.com/lucidrains/e2-tts-pytorch/pull/1">working end-to-end training code</a>!

- <a href="https://github.com/lucasnewman">Lucas Newman</a> for the code contributions, helpful feedback, and for sharing the first set of positive experiments!

- <a href="https://github.com/JingRH">Jing</a> for sharing the second positive result with a multilingual (English + Chinese) dataset!

- <a href="https://github.com/Coice">Coice</a> and <a href="https://github.com/manmay-nakhashi">Manmay</a> for reporting the third and fourth successful runs. Farewell alignment engineering

## Install

```bash
$ pip install e2-tts-pytorch
```

## Usage

```python
import torch

from e2_tts_pytorch import (
    E2TTS,
    DurationPredictor
)

duration_predictor = DurationPredictor(
    transformer = dict(
        dim = 512,
        depth = 8,
    )
)

mel = torch.randn(2, 1024, 100)
text = ['Hello', 'Goodbye']

loss = duration_predictor(mel, text = text)
loss.backward()

e2tts = E2TTS(
    duration_predictor = duration_predictor,
    transformer = dict(
        dim = 512,
        depth = 8        
    ),
)

out = e2tts(mel, text = text)
out.loss.backward()

sampled = e2tts.sample(mel[:, :5], text = text)

```

## Citations

```bibtex
@inproceedings{Eskimez2024E2TE,
    title   = {E2 TTS: Embarrassingly Easy Fully Non-Autoregressive Zero-Shot TTS},
    author  = {Sefik Emre Eskimez and Xiaofei Wang and Manthan Thakker and Canrun Li and Chung-Hsien Tsai and Zhen Xiao and Hemin Yang and Zirun Zhu and Min Tang and Xu Tan and Yanqing Liu and Sheng Zhao and Naoyuki Kanda},
    year    = {2024},
    url     = {https://api.semanticscholar.org/CorpusID:270738197}
}
```

```bibtex
@inproceedings{Darcet2023VisionTN,
    title   = {Vision Transformers Need Registers},
    author  = {Timoth'ee Darcet and Maxime Oquab and Julien Mairal and Piotr Bojanowski},
    year    = {2023},
    url     = {https://api.semanticscholar.org/CorpusID:263134283}
}
```

```bibtex
@article{Bao2022AllAW,
    title   = {All are Worth Words: A ViT Backbone for Diffusion Models},
    author  = {Fan Bao and Shen Nie and Kaiwen Xue and Yue Cao and Chongxuan Li and Hang Su and Jun Zhu},
    journal = {2023 IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR)},
    year    = {2022},
    pages   = {22669-22679},
    url     = {https://api.semanticscholar.org/CorpusID:253581703}
}
```

```bibtex
@article{Burtsev2021MultiStreamT,
    title     = {Multi-Stream Transformers},
    author    = {Mikhail S. Burtsev and Anna Rumshisky},
    journal   = {ArXiv},
    year      = {2021},
    volume    = {abs/2107.10342},
    url       = {https://api.semanticscholar.org/CorpusID:236171087}
}
```
