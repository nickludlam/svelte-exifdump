<script lang="ts">
  import exifr from 'exifr';

  let exifTags: Record<string, any>[] = [];
  let imageThumbs: string[] = [];

  // PWA install prompt
  let deferredPrompt: any = null;
  let showInstall = false;

  if (typeof window !== 'undefined') {
    window.addEventListener('beforeinstallprompt', (e) => {
      e.preventDefault();
      deferredPrompt = e;
      showInstall = true;
    });
  }

  async function promptInstall() {
    if (deferredPrompt) {
      deferredPrompt.prompt();
      const { outcome } = await deferredPrompt.userChoice;
      if (outcome === 'accepted') {
        showInstall = false;
      }
      deferredPrompt = null;
    }
  }

  function onFilesChanged(event: Event) {
    const input = event.target as HTMLInputElement;
    const files = input.files ? Array.from(input.files) : [];
    exifTags = [];
    imageThumbs = [];
    files.forEach((file, idx) => {
      // Generate thumbnail
      const reader = new FileReader();
      reader.onload = (e) => {
        imageThumbs = [...imageThumbs.slice(0, idx), e.target?.result as string, ...imageThumbs.slice(idx + 1)];
      };
      reader.readAsDataURL(file);
      // Parse EXIF
      exifr.parse(file).then(data => {
        if (data) {
          exifTags = [...exifTags, data];
        }
        console.log('EXIF data:', data);
      });
    });
  }
</script>
<div class="container mx-auto mt-6">
<h1 class="text-3xl font-bold mb-4 text-center">EXIF Tag Viewer</h1>

  <div class="flex flex-col items-center justify-center gap-4">
    {#if showInstall}
      <button on:click={promptInstall} class="bg-blue-600 hover:bg-blue-700 text-white font-semibold px-4 py-2 rounded shadow">
        Install App
      </button>
    {/if}
    <input
      type="file"
      id="file-upload"
      multiple
      on:change={onFilesChanged}
      class="hidden"
      accept=".jpg,.jpeg,.heic,.heif"
    />
    <label for="file-upload" class="cursor-pointer bg-gray-500 text-white px-4 py-2 rounded">
      Select images
    </label>
  </div>

  {#if exifTags.length > 0}
  <div class="my-12 ">
    {#each exifTags as tags, i}
      <div class="flex flex-col items-center m-2">
        {#if imageThumbs[i]}
          <img src={imageThumbs[i]} alt="thumbnail" class="w-16 h-16 object-cover rounded mb-2" />
        {/if}
      </div>
      <table class="min-w-max w-full table-fixed p-2 sm:p-4 text-xs sm:text-sm md:text-base">
        <thead>
          <tr class="bg-gray-200 text-gray-600 uppercase text-sm leading-normal">
            <th class="py-2 px-2 sm:py-3 sm:px-6 text-left">Tag</th>
            <th class="py-2 px-2 sm:py-3 sm:px-6 text-left">Value</th>
          </tr>
        </thead>
        <tbody class="text-gray-600 text-xs sm:text-sm md:text-base font-light">
          {#each Object.entries(tags) as [key, value]}
            <tr class="break-words border-b border-gray-200 hover:bg-gray-100">
              <td class="py-2 px-2 sm:py-3 sm:px-6 text-left">
                <span class="font-medium">{key}</span>
              </td>
              <td class="py-2 px-2 sm:py-3 sm:px-6 text-left text-wrap">
                {String(value)}
              </td>
            </tr>
          {/each}
        </tbody>
      </table>
    {/each}
  </div>
  {/if}
</div>
