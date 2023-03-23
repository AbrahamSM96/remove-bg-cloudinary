<script lang="ts">
  import { Cloudinary } from '@cloudinary/url-gen'
  import { backgroundRemoval } from '@cloudinary/url-gen/actions/effect'
  import { ImageStatus } from '../types.d'
  import { imageStatus, modifiedImage, originalImage } from './store'
  import Dropzone from 'dropzone'
  import 'dropzone/dist/dropzone.css'

  import { onMount } from 'svelte'

  const cloudinary = new Cloudinary({
    cloud: {
      cloudName: 'dchfrwaei'
    },
    url: {
      secure: true
    }
  })

  onMount(() => {
    const dropzone = new Dropzone('#dropzone', {
      uploadMultiple: false,
      acceptedFiles: '.jpg,.png,.webp,.jpeg',
      maxFiles: 1
    })

    dropzone.on('sending', (file, xhr, formData) => {
      imageStatus.set(ImageStatus.UPLOADING)
      // aqui podemos añadir la apikey, configuration
      formData.append('upload_preset', 'qqfiphdl')
      formData.append('timestamp', Date.now() / 1000)
      formData.append('api_key', 371216553271757)
    })

    dropzone.on('success', (file, response) => {
      const { public_id: publicId, secure_url: url } = response

      const imageWithoutBackground = cloudinary
        .image(publicId)
        .effect(backgroundRemoval())

      console.log(imageWithoutBackground.toURL())

      imageStatus.set(ImageStatus.DONE)
      modifiedImage.set(imageWithoutBackground.toURL())
      originalImage.set(url)
      console.log(response)
    })

    dropzone.on('error', (file, response) => {
      console.log('TODO MAL')
      console.log(response)
    })
  })
</script>

<form
  id="dropzone"
  class="shadow-2xl border-dashed border-2 border-gray-300 rounded-lg aspect-video w-full flex items-center justify-center flex-col"
  action="https://api.cloudinary.com/v1_1/dchfrwaei/image/upload"
>
  {#if $imageStatus === ImageStatus.READY}
    <button
      class="font-bold pointer-events-none bg-blue-600 rounded-full text-white text-bold text-xl px-6 py-4 "
    >
      Upload Files
    </button>
    <strong class="text-lg mt-4 text-gray-800">or drop a file</strong>
  {/if}
  {#if $imageStatus === ImageStatus.UPLOADING}
    <strong class="text-lg mt-4 text-gray-800">Uploading files ...</strong>
  {/if}
</form>
