
<template>
  <main class="container mt-5" v-if="!is_recording">
    <template v-if="!loading">
      <div class="row justify-content-end mt-3">
        <div class="col-auto me-auto">
          <div class="lh-1">
            <small>Snapbyte > My Recordings</small>
            <h1 class="h6 mb-0 mt-3 lh-1">My Recordings <strong>25</strong></h1>
          </div>
        </div>
        <div class="col-auto">
          <button type="button" class="btn rounded-pill mx-4 btn-light btn-outline-dark">By Date</button>
          <button type="button" class="btn rounded-pill mx-4 btn-outline-dark">Add Filter</button>
          <button type="button" class="btn rounded-pill mx-4 btn-info">New Request</button>
          <button type="button" data-bs-toggle="modal" data-bs-target="#record" class="btn rounded-pill mx-4 btn-danger">Start Recording</button>
        </div>

      </div>

      <div class="row mt-5">
        <table class="table table-borderless">
          <thead>
          <tr>
            <th scope="col" style="width: 10%">Recordings</th>
            <th scope="col" style="width: 40%">Title</th>
            <th scope="col" style="width: 15%"></th>
            <th scope="col">Views</th>
            <th scope="col">Size</th>
            <th scope="col">Last Modified</th>
            <th scope="col"></th>
          </tr>
          </thead>
          <tbody>
          <tr v-for="(i, index) in recordings" :key="index">
            <th scope="row">
              <svg class="bd-placeholder-img flex-shrink-0 me-2 rounded" width="100" height="65" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="Placeholder: 32x32" preserveAspectRatio="xMidYMid slice" focusable="false"><title>Placeholder</title><rect width="100%" height="100%" fill="#007bff"/><text x="50%" y="50%" fill="#007bff" dy=".3em">32x32</text></svg>
            </th>
            <td>
              <p>{{  i.title }}<br/>
                <small>Lorem ipsum dolor sit amet, consectetur adipisicing elit. A alias aperiam cupiditate deserunt dolorum eligendi hic ipsum itaque,</small>
              </p>
            </td>
            <td></td>
            <td>{{  i.views }}</td>
            <td>{{ i.size }}</td>
            <td>{{ i.last_modified }}</td>
            <td>...</td>
          </tr>

          </tbody>
        </table>
      </div>


      <!-- Start Recording Modal -->
      <div class="modal fade" id="record" tabindex="-1" aria-labelledby="record" aria-hidden="true">
        <div class="modal-dialog modal-dialog-centered">
          <div class="modal-content">
            <div class="modal-header">
              <h5 class="modal-title" id="record">New Recording</h5>
              <button type="button" class="btn-close" id="close" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body">
              <div class="row">
                <div class="col-12">
                  <p>Save the recording in</p>
                  <select class="form-select">
                    <option selected>Select a project</option>
                  </select>
                </div>

              </div>
              <div class="row mt-3">
                <div class="col-auto me-auto">
                  <div class="lh-1">
                    <h1 class="h6">Record Screen</h1>
                  </div>
                </div>
                <div class="col-auto">
                  <div class="form-check form-switch">
                    <input v-model="screen" class="form-check-input" type="checkbox" id="screen">
                  </div>
                </div>
              </div>
              <div class="row mt-3">
                <div class="col-auto me-auto">
                  <div class="lh-1">
                    <h1 class="h6">Record Camera</h1>
                  </div>
                </div>
                <div class="col-auto">
                  <div class="form-check form-switch">
                    <input v-model="video" class="form-check-input" type="checkbox" id="video">
                  </div>
                </div>
              </div>
              <div class="row mt-3">
                <div class="col-auto me-auto">
                  <div class="lh-1">
                    <h1 class="h6">Record Mic</h1>
                  </div>
                </div>
                <div class="col-auto">
                  <div class="form-check form-switch">
                    <input v-model="audio" class="form-check-input" type="checkbox" id="audio">
                  </div>
                </div>
              </div>

            </div>
            <div class="modal-footer text-center justify-content-center">
              <button @click="startRecord" type="button" class="btn rounded-pill btn-primary text-center">Start Recording</button>
            </div>
          </div>
        </div>
      </div>

    </template>

  </main>

  <main v-show="is_recording || loading" class="container text-center mt-5">

    <div class="video" v-if="loading"></div>

      <video v-if="video" ref="camera_container" :width="450" :height="337.5" autoplay></video>

    <div class="audio" v-if="!video && !screen && audio">
      <p>Recording Audio ...........</p>
    </div>
    <div class="screen" v-if="!video && screen">
      <p>Recording Screen ...........</p>
    </div>

    <div class="row mt-5">
      <div class="col">
        <button v-if="is_recording" @click="stopRecording" type="button" class="btn rounded-pill btn-danger text-center">Stop Recording</button>
      </div>
    </div>

  </main>

  <div  class="cam_container">
    <video  ref="camera_container" :width="450" :height="337.5" autoplay></video>
  </div>

</template>

<script>
// recordings dummy data
import {ref} from "vue";

const recordings = [
  { title: "Getting right the first time",views : '324', size : '923kb', last_modified : '3 months ago' },
  { title: "Getting right the first time",views : '324', size : '923kb', last_modified : '3 months ago' },
  { title: "Getting right the first time",views : '324', size : '923kb', last_modified : '3 months ago' },
  { title: "Getting right the first time",views : '324', size : '923kb', last_modified : '3 months ago' },
  { title: "Getting right the first time",views : '324', size : '923kb', last_modified : '3 months ago' },
]
export default {

  setup(){

    const audio = ref(false)
    const camera_container = ref(null)
    const video = ref(false)
    // const recorder = ref(null)
    const screen = ref(false)
    const is_recording = ref(false)
    const loading = ref(false)

    async function getMedia(constraints) {
      loading.value = true
      let stream = null;
      try {
        stream = await navigator.mediaDevices.getUserMedia(constraints);
        loading.value = false;
        is_recording.value = true;

        //if camera is selected stream to video ref
        if(video.value){
          camera_container.value.srcObject = stream
        }
        if(!video.value && !screen.value && audio.value){
          //check if only audio is selected then stream to mediaRecorder
        }
      } catch(err) {
        loading.value = false
        console.log(err)
        alert(err);
      }
    }

    function startRecord(){
      //check if at least a recording type is checked
      if(!audio.value && !video.value){
        alert("Select type of recording to proceed ");
        return null
      }
      loading.value = true
      getMedia({ audio: audio.value, video : video.value })
      document.getElementById('close').click();
    }

    function stopRecording(){
      is_recording.value = false
      if(video.value || screen.value){
        stopCameraStreaming()
      }
    }

    function stopCameraStreaming() {
      const streams = camera_container.value.srcObject.getTracks();
      streams.forEach(stream => {
        stream.stop();
      });
    }

    return {
      recordings,camera_container,
      startRecord, screen, video,audio, is_recording,loading,stopRecording
    }
  }
}

</script>

<style>
.video{
  height: 500px;
  width: 80vw;
  background-color: #2c3e50;
  border-radius: 10px;
}

.screen{
  height: 500px;
  width: 80vw;
  border : 1px solid red;
  border-radius: 10px;
}
/*.video_card {*/
/*  min-width: 100%;*/
/*  min-height: 100%;*/
/*  width: 100%;*/
/*}*/
/*.video_wrapper {*/
/*  max-width: 80vw;*/
/*}*/
</style>
