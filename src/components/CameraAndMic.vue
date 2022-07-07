<template>
  <div>
    <div class="bg-white overflow-hidden shadow sm:rounded-lg">
      <div class="px-4 py-12 sm:p-6 flex justify-center content-center items-center">
        <div class="flex justify-center flex-col items-center p-4">
          <div class="flex">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8 animate-bounce mr-2" viewBox="0 0 20 20" fill="currentColor">
              <path fill-rule="evenodd" d="M7 4a3 3 0 016 0v4a3 3 0 11-6 0V4zm4 10.93A7.001 7.001 0 0017 8a1 1 0 10-2 0A5 5 0 015 8a1 1 0 00-2 0 7.001 7.001 0 006 6.93V17H6a1 1 0 100 2h8a1 1 0 100-2h-3v-2.07z" clip-rule="evenodd" />
            </svg>

            <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8 animate-bounce ml-2" viewBox="0 0 20 20" fill="currentColor">
              <path d="M2 6a2 2 0 012-2h6a2 2 0 012 2v8a2 2 0 01-2 2H4a2 2 0 01-2-2V6zM14.553 7.106A1 1 0 0014 8v4a1 1 0 00.553.894l2 1A1 1 0 0018 13V7a1 1 0 00-1.447-.894l-2 1z" />
            </svg>
          </div>

          <h5 class="font-bold tracking-tight text-gray-900 text-xl mt-3">Доступ к камере и микрофону</h5>

          <p class="text-center text-gray-500 lg:mx-auto text-sm mt-2">На протяжении нескольких секунд мы запросим у вас доступ к камере. Мы должны убедиться, что камера вашего WebView работает корректно и не дает сбоев. При этом если после 10 секунд ничего не происходит, то вы забыли настроить права к камере в своем WebView.</p>
        </div>
      </div>
    </div>

    <div class="mt-4">
      <div class="flex items-center content-center">
        <img src="https://stackoverflow.com/favicon.ico" alt="" class="w-4 h-4"/>
        <a href="https://stackoverflow.com/questions/61495505/android-webcam-error-in-webview-camera-permission-not-working" class="text-blue-500 ml-2 text-sm" target="_blank">Android : Webcam Error in WebView </a>
      </div>
    </div>
  </div>
</template>


<script>
export default {
  name: 'CameraAndMic',
  data() {
    return {
    
    }
  },
  methods: {
   
  },
  mounted() {
   
  }
}
</script>
