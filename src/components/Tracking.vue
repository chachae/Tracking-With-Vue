<template>
  <div>
    <div class="tip custom-block">
      <p class="custom-block-title">VUE.js X Tracking.js 人脸识别案例</p>
      <p>对准镜头，缓慢摇动面部以便系统快速捕捉人脸</p>
    </div>
    <el-divider />
    <div id="video-background" class="bk-radius">
      <video
        v-show="base64Str === ''"
        id="video"
        class="video-camera"
        preload
        autoplay
        loop
        muted
      />
    </div>
    <el-button style="margin-top: 30px; text-align: center" type="primary" @click="resetTracking">重启</el-button>
    <el-divider />
    <canvas v-show="flag === false" id="canvas1" width="600" height="600" />
  </div>
</template>

<script>
require('tracking/build/tracking-min.js')
require('tracking/build/data/face-min.js')
require('tracking/build/data/mouth-min.js')
export default {
  name: 'TrackingDemo',
  data() {
    return {
      trackerTask: '',
      tracker: {},
      mediaStreamTrack: {},
      video: {},
      count: 0,
      flag: true,
      base64Str: ''
    }
  },
  mounted() {
    this.initTracking()
  },
  methods: {
    resetTracking() {
      this.flag = true
      this.base64Str = ''
      this.trackerTask = ''
      this.tracker = {}
      this.mediaStreamTrack = {}
      this.video = {}
      this.count = 0
      this.initTracking()
    },
    initTracking() {
      const _this = this
      const video = this.video = document.getElementById('video')
      _this.tracker = new window.tracking.ObjectTracker('face')
      _this.tracker.setInitialScale(4)
      _this.tracker.setStepSize(2)
      // 转头角度影响识别率
      _this.tracker.setEdgesDensity(0.1)
      _this.openCamera()
      _this.trackerTask = window.tracking.track('#video', _this.tracker)
      _this.tracker.on('track', function(event) {
        if (event.data.length) {
          // 会不停的去检测人脸，所以这里需要做个锁
          if (_this.flag) {
            _this.flag = false
            // 裁剪并绘制
            const canvasUpload = document.getElementById('canvas1')
            const contextUpload = canvasUpload.getContext('2d')
            contextUpload.drawImage(video, 0, 0, 600, 600)
            // 人脸 base64
            _this.base64Str = canvasUpload.toDataURL('image/jpeg')
            console.log(_this.base64Str)
            // 关闭摄像头
            _this.closeCamera()
            // 直接获取 base64 不显示画布可开启
            // _this.flag = true
          }
        }
      })
    },
    openCamera() {
      if (navigator.mediaDevices === undefined) {
        navigator.mediaDevices = {}
      }
      if (navigator.mediaDevices.getUserMedia === undefined) {
        navigator.mediaDevices.getUserMedia = function(constraints) {
          // 首先，如果有getUserMedia的话，就获得它
          const getUserMedia = navigator.getUserMedia || navigator.webkitGetUserMedia || navigator.mozGetUserMedia || navigator.msGetUserMedia
          // 一些浏览器根本没实现它 - 那么就返回一个error到promise的reject来保持一个统一的接口
          if (!getUserMedia) {
            return Promise.reject(new Error('getUserMedia 浏览器不支持摄像头'))
          }
          // 否则，为老的navigator.getUserMedia方法包裹一个Promise
          return new Promise(function(resolve, reject) {
            getUserMedia.call(navigator, constraints, resolve, reject)
          })
        }
      }
      const constraints = {
        video: true,
        audio: false
      }
      const promise = navigator.mediaDevices.getUserMedia(constraints)
      promise.then(stream => {
        this.mediaStreamTrack = stream.getTracks()[0]
        window.stream = stream
        const video = document.getElementById('video')
        // 旧的浏览器可能没有srcObject
        if ('srcObject' in video) {
          video.srcObject = stream
        } else {
          video.src = window.URL.createObjectURL(stream)
        }
        video.onloadedmetadata = function(e) {
          video.play()
        }
      }).catch(err => {
        console.error(err.name + ': ' + err.message)
        console.error('获取权限失败')
      })
    },
    closeCamera() {
      if (typeof window.stream === 'object') {
        if ('srcObject' in this.video) {
          this.video.srcObject = null
        }
        window.stream.getTracks().forEach(track => track.stop())
      }
    }
  }
}
</script>

<style scoped>
  .bk-radius {
    width: 200px;
    height: 200px;
    overflow: hidden;
    background-color: #42b983;
    border-radius: 50%;
    margin: auto
  }

  .video-camera {
    width: 300px;
    height: 300px;
    margin-top: -40px;
    margin-left: -40px;
  }

  .custom-block.tip {
    background-color: #ecf8ff;
    border-color: #50bfff;
    color: #409EFF;
  }

  .custom-block, .custom-block.tip, .custom-block {
    padding: .1rem 1.5rem;
    border-left-width: .5rem;
    border-left-style: solid;
    margin: 1rem 0;
  }
</style>
