<script setup lang="ts">
import type { Ref } from 'vue';
import { storeToRefs } from 'pinia';
import type { Track } from '@/config/types';
import { useBandStore } from '@/stores/band.store';
import { fromSecondsToMinutes } from '@/config/utilities';
import { useMusicPlayer } from '@/stores/music-player.store';

const props = defineProps({
  albumId: {
    type: String,
    required: true,
  },
});

const musicPlayer = useMusicPlayer();

const { currentTrack, isPlaying } = storeToRefs(musicPlayer);

const { fetchTracks } = useBandStore();

const state = reactive({
  tracks: {} as Track[],
});

onMounted(() => {
  fetchTracks(props.albumId).then((res) => {
    state.tracks = res as Track[];
    state.tracks.sort((a, b) => a.number < b.number ? -1 : 1);
  });
});

const onPlay = (track: Track) => {
  if (currentTrack?.value?.id !== track.id) {
    musicPlayer.stop();
    musicPlayer.init(track);
  }

  musicPlayer.play();
};

const showByIndex: Ref<null | number> = ref(null);
</script>

<template>
  <ul>
    <li
      v-for="track in state.tracks"
      :key="track.id"
      class="flex justify-between px-4 py-2 my-2 text-black rounded-full cursor-default bg-slate-200"
      @mouseout="showByIndex = null"
      @mouseover="showByIndex = track.number"
    >
      <div class="flex items-center w-full">
        <div class="flex items-center mr-2 text-sm leading-7 text-center cursor-pointer w-7 text-slate-600">
          <button
            v-if="currentTrack?.id === track.id || showByIndex === track.number"
            class="w-full h-full"
          >
            <IconPause
              v-if="isPlaying && currentTrack?.id === track.id"
              class="w-7 h-7"
              @click="musicPlayer.pause()"
            />
            <IconPlay
              v-else
              class="w-7 h-7"
              @click="onPlay(track)"
              @mouseover="showByIndex = track.number"
            />
          </button>

          <span
            v-else
            class="w-full text-lg text-center"
          >
            {{ track.number }}.
          </span>
        </div>
        <div class="text-xs font-semibold truncate sm:text-lg">
          {{ track.name }}
        </div>
        <div class="ml-auto mr-0">
          {{ fromSecondsToMinutes(track.duration) }}
        </div>
      </div>
      <div />
    </li>
  </ul>
</template>
