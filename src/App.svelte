<script lang="ts">
  import Dropzone from 'dropzone'
  import { ImageStatus } from '../types.d'
  import StepEdit from './StepEdit.svelte'
  import StepUpload from './StepUpload.svelte'
  import { imageStatus } from './store'
</script>

<div
  class="max-w-xl grid grid-cols-1 m-auto place-content-center w-full h-screen p-2"
>
  <header class="flex justify-center py-10">
    <h1 class="text-3xl font-bold text-blue-400 tracking-tighter">
      remove <span class="text-blue-800">bg</span>
    </h1>
  </header>
  <main>
    {#if $imageStatus === ImageStatus.READY || $imageStatus === ImageStatus.UPLOADING}
      <StepUpload />
    {:else if $imageStatus === ImageStatus.DONE}
      <StepEdit />
    {/if}
  </main>

  <footer class="flex justify-center items-center gap-x-2 font-semibold pt-10">
    Hecho con <a href="https://cloudinary" target="_blank" rel="noreferrer" />
  </footer>
</div>
