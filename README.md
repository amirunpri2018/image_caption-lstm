# Image Captioning
The goal of image captioning is to convert a given input image into a natural language description. The encoder-decoder framework is widely used for this task. The image encoder is a convolutional neural network (CNN). In this project we used [resnet-152](https://arxiv.org/abs/1512.03385) model pretrained on the [ILSVRC-2012-CLS](http://www.image-net.org/challenges/LSVRC/2012/) image classification dataset. The decoder is a long short-term memory (LSTM) network. 

![alt text](png/model.png)

#### Training phase
For the encoder part, the pretrained CNN extracts the feature vector from a given input image. The feature vector is linearly transformed to have the same dimension as the input dimension of the LSTM network. For the decoder part, source and target texts are predefined. For example, if the image description is **"The man ready to swing at the pitch"**, the source sequence is a list containing **['\<start\>', 'Giraffes', 'standing', 'next', 'to', 'each', 'other']** and the target sequence is a list containing **['The', 'man', 'ready', 'to', 'swing', 'at','the','pitch' '\<end\>']**. Using these source and target sequences and the feature vector, the LSTM decoder is trained as a language model conditioned on the feature vector.

#### Test phase
In the test phase, the encoder part is almost same as the training phase. The only difference is that batchnorm layer uses moving average and variance instead of mini-batch statistics. This can be easily implemented using [encoder.eval()](https://github.com/yunjey/pytorch-tutorial/blob/master/tutorials/03-advanced/image_captioning/sample.py#L41). For the decoder part, there is a significant difference between the training phase and the test phase. In the test phase, the LSTM decoder can't see the image description. To deal with this problem, the LSTM decoder feeds back the previosly generated word to the next input. This can be implemented using a [for-loop](https://github.com/yunjey/pytorch-tutorial/blob/master/tutorials/03-advanced/image_captioning/model.py#L57-L68).



## Usage 

#### 0. PreRequisites
Latest Version of PyTorch Download from [Here](https://github.com/pytorch/pytorch)
CUDA 8.0 or Higher
CUDnn 7.0 or Higher

#### 1. Clone the repositories
```bash
$ git clone https://github.com/pdollar/coco.git
$ cd coco/PythonAPI/
$ make
$ python setup.py build
$ python setup.py install
$ cd ../../
$ git clone https://github.com/OLeoghain/image_caption-lstm
$ cd image_caption-lstm
```

#### 2. Download the dataset

```bash
$ pip install -r requirements.txt
$ chmod +x download.sh
$ ./download.sh
```

#### 3. Preprocessing

```bash
$ python build_vocab.py   
$ python resize.py
```

#### 4. Train the model

```bash
$ python train.py    
```

#### 5. Test the model 

```bash
$ python sample.py --image='png/example.png'
```

#### 6. Optional

Pretrained Encoder Decoder Models and Vocab [Download](https://drive.google.com/drive/folders/1QW8RsfB11z7Qc7yNYhjBi4eFI5E13cG0?usp=sharing).
You should extract pretrained_model.zip to `./models/` and vocab.pkl to `./data/` using `unzip` command.
<br>

