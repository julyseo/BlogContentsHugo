# AVFoundation Preview

실시간 촬영/업로드 어플리케이션을 개발하기 위해 찾아본 AVFoundation 의 메소드들이다.

개발된 앱의 기본적인 촬영/업로드 프로세스는 다음과 같다.

1. RAW A/V Capture
2. A/V Encoding (H264, AAC)
3. Muxing/Uploading

1의 RAW A/V Capture 는 AVCaptureSession 을 통해 수행한다.

H264/AAC 인코딩 된 결과물들을 Muxing 하지 않고 ES 단위로 업로드 하는 것이 요구사항이었기 때문에

인코딩 시에는 일반적으로 사용하는 AVAssetWriter 가 아닌 AudioToolbox, VideoToolbox 를 사용했고,

 AVAssetWriter는 디바이스 내 영상 저장을 하기 위해 Muxer 로서 사용했다.

## RAW Audio / Video Capture

카메라, 마이크 로부터 RAW Data 를 캡쳐한다. (Video : yuv format, )

### AVCaptureSession (AVFoundation)

Input, Output 을 지정하여 Input 으로부터 받는 데이터를 Output 에 전달

``` swift
func addInput(_ input: AVCaptureInput)
func addOutput(_ output: AVCaptureOutput)
```

#### Input

Input Device 를 지정한다.

AVCaptureDevice 획득 - AVCaptureDeviceInput init - AVCaptureSession 에 addInput 의 순서로 설정한다.

- ***AVCaptureDevice***

  Camera, Mic 디바이스를 얻어오기 위해 사용하는 클래스

  ```swift
  class func `default`(for mediaType: AVMediaType) -> AVCaptureDevice?
  ```

- ***AVCaptureDeviceInput***

	디바이스 객체를 세션에 등록하기 위한 클래스
	
	```swift
	init(device: AVCaptureDevice) throws
	```

#### Output

Output 의 형태를 지정한다. (file, frame data, ...)

AVCaptureOutput init - delegate 설정 - AVCaptureSession 에 addOutput 의 순서로 설정한다.

##### Video

카메라 스트림을 yuv 형태로 실시간 캡쳐한다.

- ***AVCaptureVideoDataOutput (videoSetting, func : setDelegate)***

  Input 으로부터 넘어오는 Video Raw 데이터를 지정된 Delegate 로 전달 (Delegate.Callback 호출)

  - ***AVCaptureVideoDataOutputSampleBufferDelegate (func : captureOutput)***

    AVCaptureVideoDataOuptut 의 Delegate, captureOuptut 이 callback

##### Audio

Mic Stream to audio raw data (format : pcm)

- ***AVCaptureAudioDataOutput (audioSetting, func : setDelegate)***

  Input 으로부터 넘어오는 Audio Raw 데이터를 지정된 Delegate 로 전달 (Delegate.Callback 호출)

  - ***AVCaptureAudioDataOutputSampleBufferDelegate (func : captureOutput)***

    CaptureAudioDataOuptut 의 Delegate, captureOutput 이 callback

##### Audio, Video 공통

- *captureOutput(_:didOutput:from)*

  캡쳐된 데이터 (프레임, 시간정보, ...)가 CMSampleBuffer 객체로 넘어옴

- *captureOutput(_:didDrop:from)*

  버려진 데이터가 넘어옴

=> AVCaptureConnection 이 넘어오므로 Audio 인지 Video 인지 분별이 가능함

	*AVCaptureConnection : CaptureSesison 에 등록한 Device 정보*

=> Audio, Video 모두 동일한 callback 을 호출하기 때문에 captureOutput 내 에서 분기처리 해야함

### 그 외

#### Preview

- ***AVCaptureVideoPreviewLayer***

## Audio/Video Encoding

From Captured Data (frame)

### Hardware Accelerated Encoding

비디오만 하드웨어 가속 인코딩이 가능 (to h.264)

### Video Encoding

encode yuv to h.264 *(yuv, data from AVCaptureVideoDataOutput)*

#### VTCompressionSession (VideoToolBox)

- VTCompressionSessionCreate

- VTSessionSetProperties

- VTCompressionOutputCallback

- VTCompressionSessionEncodeFrame

### AudioEncoding

encode pcm to aac *(pcm, data from AVCaptureAudioDataOutput)*

#### AudioConverter (AudioToolBox)

- **CMFormatDescription**
  - CMAudioFormatDescriptionCreate

- AudioConverterNewSpecific
- AudioConverterComplexInputDataProc
- AudioConverterFillComplexBuffer

## Muxing

conbine **encoded audio/video** and subtitle components to container format

### for Local Play

combine A/V data to MP4/MOV format (using AVAssetWriter)

### for Network Transmission

combine A/V data to TS format (not supported on AVAssetWriter)



## Protocol

data from **muxed stream** to RTP, HLS, ...