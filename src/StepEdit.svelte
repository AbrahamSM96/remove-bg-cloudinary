<script lang="ts">
  import { originalImage, modifiedImage } from './store'
  import 'two-up-element'

  let processingImage = true
  let tries = 0
  let intervalId: any
  $: {
    if (processingImage) {
      clearInterval(intervalId)
      intervalId = setInterval(() => {
        tries++
        const img = new Image()
        img.src = $modifiedImage
        img.onload = () => {
          processingImage = false
          clearInterval(intervalId)
        }
      }, 500)
    }
  }
</script>

<two-up>
  <img src={$originalImage} alt="Imagen original subida por usuario" />
  {#if processingImage}
    <div class="flex flex-col justify-center items-center">
      <p class="text-center mt-4">Procesando imagen ...</p>
    </div>
  {:else}
    <img src={$modifiedImage} alt="Imagen sin fondo" />
  {/if}
</two-up>

<a
  download
  href={$modifiedImage}
  class="block bg-blue-500 text-xl font-bold w-full text-white px-4 mt-10 py-2 rounded-full hover:bg-blue-400y-2 text-center"
>
  Descargar imagen sin fondo
</a>
