<template>
<div class="flex justify-center flex-col items-center p-4">
    <h5 class="font-bold tracking-tight text-gray-900 text-xl mb-6 w-full text-center">Попробуйте выбрать файлы</h5>
    
    <button @click.prevent="$refs.fileSelector.click()" type="button" class="relative block border-2 border-gray-300 border-dashed rounded-lg p-12 text-center hover:border-gray-400 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
        <svg class="mx-auto h-12 w-12 text-gray-400" xmlns="http://www.w3.org/2000/svg" stroke="currentColor" fill="none" viewBox="0 0 48 48" aria-hidden="true">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 14v20c0 4.418 7.163 8 16 8 1.381 0 2.721-.087 4-.252M8 14c0 4.418 7.163 8 16 8s16-3.582 16-8M8 14c0-4.418 7.163-8 16-8s16 3.582 16 8m0 0v14m0-4c0 4.418-7.163 8-16 8S8 28.418 8 24m32 10v6m0 0v6m0-6h6m-6 0h-6" />
        </svg>
        
        <span class="mt-4 block text-sm font-medium text-gray-900"> Попробовать выбрать файл </span>
    </button>

    <small class="mx-12 mt-4 text-center text-gray-500"> Вам нужно будет выбрать файл с вашего телефона. В случае если после нажатия на кнопку ничего не происходит, возможно вы забыли настроить выборщик файлов в своем WebView. </small>

    <input type="file" id="file-selector" ref="fileSelector" style="display: none;">
</div>
</template>


<script>
export default {
  name: 'ChooseFile',
  props: ["onFileChoose"],
  mounted() {
    this.$refs.fileSelector.addEventListener('change', (event) => {
        const fileList = event.target.files;
        
        if (fileList) {
            this.onFileChoose()
        }
    });
  }
}
</script>
