<template>
<div class="bg-white overflow-hidden shadow sm:rounded-lg">
  <div class="px-4 py-12 sm:p-6 flex justify-center content-center items-center">
    <div class="flex justify-center flex-col items-center p-4">
        <!-- Content goes here -->
        <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8 animate-bounce" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2" v-if="state == 1">
            <path stroke-linecap="round" stroke-linejoin="round" d="M12 18h.01M8 21h8a2 2 0 002-2V5a2 2 0 00-2-2H8a2 2 0 00-2 2v14a2 2 0 002 2z" />
        </svg>

        <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8 animate-bounce" viewBox="0 0 20 20" fill="currentColor" v-if="state == 0">
            <path fill-rule="evenodd" d="M7 2a2 2 0 00-2 2v12a2 2 0 002 2h6a2 2 0 002-2V4a2 2 0 00-2-2H7zm3 14a1 1 0 100-2 1 1 0 000 2z" clip-rule="evenodd" />
        </svg>

        <h5 class="font-bold tracking-tight text-gray-900 text-xl mt-3" v-if="state == 1">Поверните телефон еще раз</h5>
        <h5 class="font-bold tracking-tight text-gray-900 text-xl mt-3" v-if="state == 0">Поверните телефон набок</h5>

        <p class="text-center text-gray-500 lg:mx-auto text-sm mt-2">Теперь попробуйте перевернуть устройство, не забудьте включить автоповорот. А если вдруг тест начнется снова, то вы неправильно настроили манифест своего приложения </p>
    </div>
  </div>
</div>

</template>


<script>
export default {
  name: 'TryRotateScreen',
  data() {
    return {
        currentScreenOrientation: 0,
        state: 0
    }
  },
  methods: {
    getOrientation() {
      return (screen.orientation || {}).type || screen.mozOrientation || screen.msOrientation;
    }
  },
  mounted() {
    this.state = 0
    this.currentScreenOrientation = this.getOrientation() 

    screen.orientation.addEventListener('change', () => {
        if (this.currentScreenOrientation != this.getOrientation()) {
            this.state = 1
        }
    });
  }
}
</script>
