protoc object_detection/protos/*.proto --python_out=.
export PYTHONPATH=$PYTHONPATH:`pwd`:`pwd`/slim

# Create train data:
python xml_to_csv.py -i ../../workspace/training_demo/images/train -o ../../workspace/training_demo/annotations/train_labels.csv
# Create test data:
python xml_to_csv.py -i ../../workspace/training_demo/images/test -o ../../workspace/training_demo/annotations/test_labels.csv

# Create train data:
python generate_tfrecord.py  --csv_input=../../workspace/training_demo/annotations/train_labels.csv --img_path=../../workspace/training_demo/images/train  --output_path=../../workspace/training_demo/annotations/train.record

# Create test data:
python generate_tfrecord.py --csv_input=../../workspace/training_demo/annotations/test_labels.csv --img_path=../../workspace/training_demo/images/test --output_path=../../workspace/training_demo/annotations/test.record


pip install "git+https://github.com/philferriere/cocoapi.git#egg=pycocotools&subdirectory=PythonAPI"


python model_main.py --alsologtostderr --model_dir=training/ --pipeline_config_path=pre-trained-model/ssd_mobilenet_v2_coco_2018_03_29/pipeline.config
