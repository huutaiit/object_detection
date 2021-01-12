# object_detection

<p>Để toàn bộ nội dung đoạn code trên vào file partion_dataset.py và chạy dòng lệnh dưới đây, dữ liệu của bạn sẽ tự động lưu vào thư mục TrainValDataset và được chia thành hai thư mục train, test.</p>

<b>1. Chia dữ liệu thành hai tập train và validate</b>

<code>
python partion_dataset.py -i ./Dataset/ -x -r 0.1
</code>
<p>
<b style="margin-left:20">
Trong đó
<ul>
<li>-i : đường dẫn tới thư mục chứa dữ liệu bao gồm cả file xml và image</li>
<li>--x: định dạng file label là xml</li>
<li>-r: tỉ lệ hai tập train và test. Ở đây mình để là 0.1</li>
</ul>

</b>
</p>

<p><b>2. Chuyển dữ liệu về dạng TFRecord</b><p>

create train record </br>
<code>
python create_tf_record.py -x ./TrainValDataset/train/ -l ./label_map.pbxt -o ./TrainValDataset/train.record
</code>
</p>

<p>
 create test record:  </br>
<code>
python create_tf_record.py -x ./TrainValDataset/test/ -l ./label_map.pbtxt -o ./TrainValDataset/test.record
</code>
</p>
<b>3. Cài đặt package python</b>
<p>
<code>cd models/research </code>
 <p>Compile protos. </p>
<code>protoc object_detection/protos/*.proto --python_out=.   </code>

 Install TensorFlow Object Detection API. 

<p><code>cp object_detection/packages/tf2/setup.py .</code></p>
<p><code>python -m pip install --use-feature=2020-resolver . </code></p>
<p><code>!python setup.py build </code></p>
<p><code>!python setup.py install </code></p>
</p>


<p><b>4. training model</b></p>
<code>
%cd ./object_detection/  </br>
!python model_main_tf2.py --model_dir=link_to_output_trained_model --pipeline_config_path=link_to_pipeline.config </b>
</code>
<p>
<b>5. Lưu mô hình đã huấn luyện</b>
<p>
	<code> python exporter_main_v2.py --input_type image_tensor --pipeline_config_path link_to_pipeline.config --trained_checkpoint_dir link_to_used_checkpoint --output_directory link_to_output_saved_model</code>
</p>
<b>6. export tflite</b>
<p />
<code>python3 export_tflite_graph_tf2.py  --pipeline_config_path  ../../../pipeline.config --trained_checkpoint_dir ../../../out    --output_directory ../../../lite</code>
<p />
<code>
	python3 convert_tflite.py
</code>
</p>


