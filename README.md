# RealTime Video Face Detection

## Demo Link
[![Face detection](http://img.youtube.com/vi/82zVzJDMcNo/0.jpg)](http://www.youtube.com/watch?v=82zVzJDMcNo "RealTime Face Detection")

## Installation

* Install Homebrew
	* <pre>
	  /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
	  </pre>
	* <pre>
	  vim ~/.bash_profile
	  export PATH=/usr/local/bin:$PATH
	  </pre>
	  <pre>
	  source ~/.bash_profile
	  </pre>

* Install python
	* <pre>
	  brew install python python3
	  cd /usr/local/bin
	  ln -s ../Cellar/python/2.7.14/bin/python2 python
	  </pre>

* Install ffmpeg
	* <pre>
	  brew install ffmpeg \
    	--with-tools \
    	--with-fdk-aac \
   		--with-freetype \
	   --with-fontconfig \
	   --with-libass \
	   --with-libvorbis \
	   --with-libvpx \
	   --with-opus \
	   --with-x265
    </pre>
* Install opencv
	* <pre>
	  brew tap homebrew/science
	  brew install opencv3 --with-contrib --with-python3
	  </pre>
* Install boto3, watchdog
  * <pre>
	  pip2 install boto3 watchdog
	</pre>
   or `python -m pip install boto3 watchdog`

## Execution - Recognizing Faces in a Streaming Video

Clone this repo:

`git clone git@github.com:imyoungyang/video-streaming.git`

### Step1: IAM Role & SNS Topic
Create an IAM service role to give Rekognition Video access to your Kinesis video streams and your Kinesis data streams. SNS Topic to recieve the reginition name.

```
python iam-role-helper.py --create
python sns-helper.py --create
```
		
### Step2: Create Collection

```
python collection-helper.py --create
```

### Step3: Add faces to a collection
  
```
python index_faces.py ./young-yang.jpg Young
```

### Step4: Create a Kinesis Video Stream

```
python video-stream-helper.py --create
```
	
### Step5: Create a Kinesis Data Stream

```
python data-stream-helper.py --create
```

### Step6: Create the stream processor

* run command to create a stream processor.

  `python rekognition-process.py --create`
  
* run command to check process status

  `aws rekognition describe-stream-processor --name appStreamProcessor-videoFaceRek`
	
### Step7: Start the stream processor

* run command `python rekognition-process.py --start` to start the process

### Step8: Start video stream

* Open terminal and exeucte the upload to kinesis videos
	* `python watch_for_changes.py`

* Execute face detection in another terminal
	* `python face-detection-multi-files.py`

### Step9: Consume the analysis result

* run command `python get-rekognition-result.py`

## Reference
* [AWS developer guide: recognize faces in a video stream](https://docs.aws.amazon.com/rekognition/latest/dg/recognize-faces-in-a-video-stream.html)

