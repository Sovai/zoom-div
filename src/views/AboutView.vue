<template>
  <div class="p-20 bg-slate-100">
    <div
      class="h-8 w-3/5 bg-blue-700 rounded-md mb-20 mx-auto text-white grid place-content-center"
    >
      SCREEN
    </div>
    <div
      class="relative w-[500px] h-[500px] overflow-hidden rounded border border-red-600"
    >
      <div ref="layoutRef" class="flex flex-wrap gap-2">
        <div
          v-for="i in 150"
          :key="i"
          @click="onSeatClick($event, i)"
          class="rounded-md w-10 h-10"
          :class="i === selected ? 'bg-red-500' : 'bg-blue-400'"
        ></div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { onBeforeUnmount, onMounted, ref } from "vue";
import panzoom from "panzoom";
const layoutRef = ref(null);
const instance = ref(null);
let selected = ref(null);
let isPanning = ref(false);
function onSeatClick(event, i) {
  if (isPanning.value) return;
  selected.value = i;

  console.log("test: ", getZoomState());
  if (getZoomState() <= 1) {
    instance.value.smoothZoom(event.clientX, event.clientY, 1.5);
  }
}
let centerCenter = { x: 0.5, y: 0.5 };

const zoomOptions = {
  maxZoom: 1.5,
  minZoom: 1,
  // initialX: 0,
  // initialY: 0,
  // transformOrigin: { x: 0.5, y: 0.5 },
  transformOrigin: centerCenter,
  initialZoom: 1,
  // zoomDoubleClickSpeed: 1, // disable double click zoom

  // beforeWheel: () =>
  //   function (e) {
  //     // allow wheel-zoom only if altKey is down. Otherwise - ignore
  //     var shouldIgnore = !e.altKey;
  //     return shouldIgnore;
  //   },
  // beforeMouseDown: () =>
  //   function (e) {
  //     // allow wheel-zoom only if altKey is down. Otherwise - ignore
  //     var shouldIgnore = !e.altKey;
  //     return shouldIgnore;
  //   },
};

function getZoomState() {
  const { scale } = instance.value.getTransform();

  return scale;
}

async function initZoom() {
  instance.value = panzoom(layoutRef.value, zoomOptions);
  instance.value.on("panstart", function (e) {
    console.log("Fired when pan is just started ", e);
    // Note: e === instance.
  });

  instance.value.on("pan", function (e) {
    isPanning.value = true;
    console.log("Fired when the `element` is being panned", e);
  });

  instance.value.on("panend", function (e) {
    setTimeout(() => {
      isPanning.value = false;
    }, 300);

    console.log("Fired when pan ended", e);
  });

  instance.value.on("zoom", function (e) {
    console.log("Fired when `element` is zoomed", e);
  });

  instance.value.on("zoomend", function (e) {
    console.log("Fired when zoom animation ended", e);
  });

  instance.value.on("transform", function (e) {
    // This event will be called along with events above.
    console.log("Fired when any transformation has happened", e);
  });
}
onMounted(async () => {
  initZoom();
});

onBeforeUnmount(() => {
  instance.value.dispose();
});
</script>

<style lang="scss" scoped></style>
