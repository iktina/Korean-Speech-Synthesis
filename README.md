# Korean Speech Synthesis


## Welcome

The service receives a text file and uses it as input to the composition of pre-trained neural models.

## What’s the point?

The service makes text to speech transfer using machine learning techniques.
The service outputs a binary audio data containing text recorded in the specified text file voiced by the synthesized voice.

## Model details:

The service receives a raw Korean text sentence and uses it as input to the advanced FastPitch-based model, followed by the WaveGlow vocoder, both of which were consecutively trained on the open KSS dataset and outputs the speech audio samples as a binary file. Both models run on the same P100 GPU to ensure the fastest data flow between models and duplicated to provide more service bandwidth. The scaling factor is 2xServices per 1xP100 GPU. The input sequence is not limited. 

## How does it work?

The user must provide the following inputs in order to start the service and get a response:

Inputs:

 -   `endpoint`: nss.naint.tech.
 -   `method`: text2speech_ko.
 -   `input_path`: Path to '\*.txt' file containing JSON representation of input argument 'text' and its value - text for speech synthesis.
 -   `output_path`: Path to '\*.wav' output audio file.

Example of input file content:

```
{"text": "잠시 후 그녀는 멀리서 발이 덜컹거리는 소리가 들리고 무엇이 오는지 알아보기 위해 서둘러 눈을 감았다."}
```

You can call the service from SingularityNET CLI (`snet`).

Assuming that you have an open channel (`id: 0`) to this service:

```
$ snet client call --save-field data samples/sample.wav 0 0.1 nss.naint.tech text2speech_ko samples/sample.txt

Read call params from the file: samples/sample.txt
```

## What to expect from this service?

Input text:
"잠시 후 그녀는 멀리서 발이 덜컹거리는 소리가 들리고 무엇이 오는지 알아보기 위해 서둘러 눈을 감았다."

Response:
samples/sample.wav
