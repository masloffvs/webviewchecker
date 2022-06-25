<template>
<div class="bg-white overflow-hidden shadow sm:rounded-lg">
  <div class="px-4 py-5 sm:p-6 flex justify-center content-center items-center">
    <div class="flex justify-center flex-col items-center p-4">
        <h5 class="font-bold tracking-tight text-gray-900 text-xl mt-3">Доступ к камере и микрофону</h5>

        <p class="text-center text-gray-500 lg:mx-auto text-sm mt-2">На протяжении нескольких секунд мы запросим у вас доступ к камере. Мы должны убедиться, что камера вашего WebView работает корректно и не дает сбоев. При этом если после 10 секунд ничего не происходит, то вы забыли настроить права к камере в своем WebView.</p>
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
