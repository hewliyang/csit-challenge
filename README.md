# csit-challenge

### VSCode + Jupyter Setup

```
poetry config virtualenvs.in-project true
poetry install
```

### `whisper.cpp` Setup for Task 3

We shall use `whisper.cpp` for better CPU inference performance & so that we don't need to waste hard disk space installing `torch`.

```bash
# clone whisper.cpp
git clone https://github.com/ggerganov/whisper.cpp.git

# download ggml binary (contains tokenizer, model weights and everything else you need)
# warning: large file!
bash ./whisper.cpp/models/download-ggml-model.sh large-v2

# compile (you need to have gcc (c++) installed) the `main` binary
make

# inference on 
./main -f ../data/Task_3/out_16kHz.wav -m models/ggml-large-v2.bin 
```

**Task 3**

**Whisper** requires the input `.wav` files to be single channel with a 16kHz sampling rate. Check [this](https://github.com/openai/whisper/blob/248b6cb124225dd263bb9bd32d060b6517e067f8/whisper/audio.py#L13).
If you are using the `openai-whisper` Python library, the `.transcribe` method already takes care of this conversion for you.

For `whisper.cpp`, first export the cleaned & normalized audio clip as is.

```python
import soundfile as sf
sf.write("../data/Task_3/out.wav", data=new_sig, samplerate=sr)
```

Then you can use `ffmpeg` to downsample the clip.

```bash
ffmpeg -i out.wav -acodec pcm_s16le -ar 16000 -ac 1 out_16kHz.wav
```