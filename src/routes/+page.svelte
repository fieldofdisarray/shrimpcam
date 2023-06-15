<script>
  // Imports
  import { Transition } from "@rgossiaux/svelte-headlessui"
  import { Player, Ui, Hls } from "@vime/svelte"
  import Viewport from "svelte-viewport-info"
  import Device from "svelte-device-info"
  import { text } from "svelte/internal"
  import 'vidstack/styles/defaults.css'
  import 'vidstack/styles/community-skin/video.css'

  import { defineCustomElements } from 'vidstack/elements';

  defineCustomElements();


  //  set video player to fullscreen

  const setFullscreen = () => {
    const player = document.querySelector("media-player")
    player.enterFullscreen()
  }

  let textColor = "text-gray-n-300"

  let isLandscape = false

  // Check if device is in landscape mode every 100ms
  setInterval(() => {
    if (Viewport.Orientation === "landscape" && Device.isMobile === true) {
      isLandscape = true
      // wait 500ms before setting fullscreen
      setTimeout(() => {
        setFullscreen()
      }, 500)
    } else {
      isLandscape = false
    }
  }, 100)

  // Shrimpcam player visibility

  // Shrimp notification transition
  let show = true

  // Shrimpcam player visibility
  let playerHiddenStyle = "block"
  let shrimpSleepingStyle = "hidden"

  // HLS config
  const hlsConfig = {
    debug: true,
    enableWorker: true,
    lowLatencyMode: true,
    backBufferLength: 90,
  }

  // Get the current time in PST and update it every second
  let pstTime = new Date().toLocaleTimeString("en-US", {
    timeZone: "America/Los_Angeles",
    hour12: false,
  })
  setInterval(() => {
    pstTime = new Date().toLocaleTimeString("en-US", {
      timeZone: "America/Los_Angeles",
      hour12: false,
    })

    if (pstTime > "21:00:00" || pstTime < "12:00:00") {
    playerHiddenStyle = "hidden"
    shrimpSleepingStyle = "inline"
  } else {
    playerHiddenStyle = "inline"
    shrimpSleepingStyle = "hidden"
  }
  }, 1000)

  if (pstTime > "21:00:00" || pstTime < "12:00:00") {
    playerHiddenStyle = "hidden"
    shrimpSleepingStyle = "inline"
  } else {
    playerHiddenStyle = "inline"
    shrimpSleepingStyle = "hidden"
  }
</script>

{#if isLandscape === true}
  <!-- Shrimpcam -->
  <div class="min-w-full mx-auto">
    <!-- Old vidstack shrimpcam! -->
    <div class={playerHiddenStyle}>
      <media-player autoplay playsinline controls muted aspect-ratio="16/9">
        <media-outlet>
          <source
            src="https://shrimpcam.app/hls/shrimpcam.m3u8"
            type="application/x-mpegurl"
          />
        </media-outlet>
      </media-player>
    </div>

    <!-- New (yet old lol) vime shrimpcam! -->
  </div>{:else}
  <!-- Shrimpcam hero -->
  <div class="pt-8 pb-8 min-w-full">
    <div class="flex items-center max-w-fit mx-auto">
      <h1 class="text-5xl font-semibold tracking-tight inline-block mr-4">
        Shrimpcam!
      </h1>
      <img src="/shrimp.png" alt="shrimp" class="w-16 h-16 inline-block" />
    </div>
  </div>

  <!-- Shrimpcam banner w/ time -->
  <Transition
    {show}
    enter="transition-opacity duration-75"
    enterFrom="opacity-0"
    enterTo="opacity-100"
    leave="transition-opacity duration-150"
    leaveFrom="opacity-100"
    leaveTo="opacity-0"
  >
    <div class="pt-4 pb-8 min-w-full">
      <div class="flex items-center max-w-3xl w-full mx-auto">
        <!-- banner -->
        <div
          class="p-3 rounded-xs outline outline-2 outline-blue-200 bg-blue-400 min-w-full"
        >
          <div class="flex justify-between items-center">
            <!-- left -->
            <div class="mx-auto md:mx-0 px-1">
              <p class="text-white font-bold text-sm text-center md:text-left">
                The shrimps' lights are on from
                <br class="md:hidden" />
                12:00:00 to 21:00:00 PST.
                <br class="md:hidden" />
                The current time is {pstTime} PST.
                <br class={shrimpSleepingStyle} />
                <span class={shrimpSleepingStyle}>
                  Shrimps are sleeping — Good night!
                </span>
              </p>
            </div>
            <!-- right -->
            <div class="px-1">
              <button
                on:click={() => (show = false)}
                class="py-1 px-1.5 rounded-xs outline outline-2 outline-white/40 text-white bg-red-400 hover:bg-red-500 hover:text-white transition-colors duration-300"
              >
                <i class="fa fa-times" aria-hidden="true" />
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </Transition>

  <!-- Shrimpcam -->
  <div class="max-w-[1920px] mx-auto">
    <!-- Old vidstack shrimpcam! -->
    <div class={playerHiddenStyle}>
      <media-player autoplay playsinline controls muted aspect-ratio="16/9">
        <media-outlet>
          <source
            src="https://shrimpcam.app/hls/shrimpcam.m3u8"
            type="application/x-mpegurl"
          />
        </media-outlet>
      </media-player>
    </div>

    <!-- New (yet old lol) vime shrimpcam! -->
  </div>

  <!-- footer bar -->
  <div class="py-6 px-8 lg:px-12">
    <div class="lg:container lg:mx-auto">
      <!-- grid -->
      <div class="grid-cols-1 lg:grid-cols-3 grid">
        <!-- grid item 1 -->
        <div class="hidden lg:block mx-auto" />
        <!-- grid item 2 -->
        <div class="block mx-auto">
          <!-- desktop copyright -->
          <span>
            <p
            class="
         text-xs tracking-tight text-center
         "
          >
            Shrimpcam! <img
              src="/shrimp.png"
              alt="shrimp"
              class="w-3.5 h-3.5 mb-0.5 inline"
            />
            ·

            <a
              class="hover:underline font-medium transition-all duration-300"
              href="https://github.com/fieldofdisarray/shrimpcam"
              >View Source Code</a
            >
            <!-- Add in the future -->
            <!-- ·
          <a
            class="hover:text-white font-medium text-gray-200 transition-all duration-300"
            href="">License</a
          > -->
          </p></span>
        </div>
        <!-- grid item 3 -->
        <div class="hidden lg:block mx-auto" />
      </div>
    </div>
  </div>
{/if}
