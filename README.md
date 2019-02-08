# Object Detection Training
![](https://bourdakos1.github.io/tfjs-object-detection-training/assets/main.png)

### Walkthrough for training locally:
- [bourdakos1.github.io/tfjs-object-detection-training](https://bourdakos1.github.io/tfjs-object-detection-training/)
### Walkthrough for training on IBM Cloud:
- [bourdakos1.github.io/tfjs-object-detection-training/wml](https://bourdakos1.github.io/tfjs-object-detection-training/wml/)

## Quick & Dirty commands
It's recommended to go through one of the above walkthroughs, but if you already have and just need to remember one of the commands, here they are:

### Project setup
```
git clone https://github.com/bourdakos1/tfjs-object-detection-training.git &&
cd tfjs-object-detection-training
```

Download the classification or object detection api:
Classification |
------------ |
`svn export -r 308 https://github.com/tensorflow/hub/trunk/examples/image_retraining classification` |

Object Detection |
------------ |
```
svn export -r 8436 https://github.com/tensorflow/models/trunk/research/object_detection &&
svn export -r 8436 https://github.com/tensorflow/models/trunk/research/slim
``` |

### Classification local
```
git clone https://github.com/bourdakos1/tfjs-object-detection-training.git &&
cd tfjs-object-detection-training
```

```
svn export -r 308 https://github.com/tensorflow/hub/trunk/examples/image_retraining classification
```

```
python -m bucket.login
```

```
pip install -r requirements_classification.txt
```

```
python -m bucket.download
```

```
python -m classification.retrain \
  --image_dir=.tmp/data \
  --saved_model_dir=exported_graph/saved_model \
  --tfhub_module=https://tfhub.dev/google/imagenet/mobilenet_v1_100_224/feature_vector/1 \
  --how_many_training_steps=500 \
  --output_labels=.tmp/output_labels.txt
```

```
python -m scripts.convert --tfjs --tflite --coreml
```

### Object detection local
```
git clone https://github.com/bourdakos1/tfjs-object-detection-training.git &&
cd tfjs-object-detection-training
```

```
svn export -r 8436 https://github.com/tensorflow/models/trunk/research/object_detection &&
svn export -r 8436 https://github.com/tensorflow/models/trunk/research/slim
```

```
python -m bucket.login
```

```
pip install -r requirements_object_detection.txt
```

```
protoc object_detection/protos/*.proto --python_out=.
```

```
python -m bucket.download
```

```
export PYTHONPATH=$PYTHONPATH:`pwd`/slim
python -m object_detection.model_main \
  --pipeline_config_path=.tmp/pipeline.config \
  --model_dir=.tmp/checkpoint
  --num_train_steps=500 &&
python -m scripts.quick_export_graph
```

```
python -m scripts.convert --tfjs --tflite --coreml
```

### Classification on IBM Cloud
```
git clone https://github.com/bourdakos1/tfjs-object-detection-training.git &&
cd tfjs-object-detection-training
```

```
svn export -r 308 https://github.com/tensorflow/hub/trunk/examples/image_retraining classification
```

```
python setup_classification.py sdist
```

```
python -m wml.login
```

```
pip install -r requirements_wml.txt
```

```
python -m scripts.start_training_run
```

```
python -m scripts.convert --tfjs --tflite --coreml
```

### Object detection on IBM Cloud
```
git clone https://github.com/bourdakos1/tfjs-object-detection-training.git &&
cd tfjs-object-detection-training
```

```
svn export -r 8436 https://github.com/tensorflow/models/trunk/research/object_detection &&
svn export -r 8436 https://github.com/tensorflow/models/trunk/research/slim
```

```
python setup_object_detection.py sdist
```

```
python -m wml.login
```

```
pip install -r requirements_wml.txt
```

```
python -m scripts.start_training_run
```

```
python -m scripts.convert --tfjs --tflite --coreml
```