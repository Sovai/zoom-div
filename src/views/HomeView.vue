<!-- eslint-disable no-unused-vars -->
<script setup>
import PinchScrollZoom from "@coddicat/vue3-pinch-scroll-zoom";

import { nextTick, onMounted, ref } from "vue";

onMounted(async () => {
  await nextTick();
});

function scalingHandler(scale) {
  console.log(scale);
}

function onWrapperClick(event) {
  console.log(event.x);
  // zoomer.value.setData({
  //   translateX: -event.x,
  //   translateY: -event.y,
  // });
}

const zoomer = ref(null);
const within = ref(true);
const minScale = ref(1);
const maxScale = ref(2);
const scale = ref(1);
const originX = ref(0);
const originY = ref(0);
const translateX = ref(0);
const translateY = ref(0);
const eventName = ref("N/A");
const eventData = ref({});

let selected = ref(null);
function onSeatClick(event, i) {
  selected.value = i;
}
const onEvent = (name, e) => {
  console.log({ name, e });
  eventName.value = name;
  eventData.value = e;
  scale.value = e.scale;
  originX.value = e.originX;
  originY.value = e.originY;
  translateX.value = e.translateX;
  translateY.value = e.translateY;
};
const reset = () => {
  if (zoomer.value !== null) {
    zoomer.value.setData({
      scale: 1,
      originX: 0,
      originY: 0,
      translateX: 0,
      translateY: 0,
    });
  }
};
</script>

<template inheritAttrs="false">
  scale: {{ scale.toFixed(2) }} <br />
  origin: ({{ originX.toFixed(2) }}, {{ originY.toFixed(2) }}) <br />
  translate: ({{ translateX.toFixed(2) }}, {{ translateY.toFixed(2) }}) <br />

  <div class="p-20 bg-slate-100">
    <div
      class="h-8 w-3/5 bg-blue-700 rounded-md mb-20 mx-auto text-white grid place-content-center"
    >
      SCREEN
    </div>
    <PinchScrollZoom
      ref="zoomer"
      v-bind="$attrs"
      :width="500"
      :height="500"
      :scale="scale"
      :translate-x="translateX"
      :translate-y="translateY"
      :origin-x="originX"
      :origin-y="originY"
      :within="within"
      :min-scale="minScale"
      :max-scale="maxScale"
      @scaling="(e) => onEvent('scaling', e)"
      @startDrag="(e) => onEvent('startDrag', e)"
      @stopDrag="(e) => onEvent('stopDrag', e)"
      @dragging="(e) => onEvent('dragging', e)"
      :enableScaling="true"
      :enableStartDrag="true"
      :enableStopDrag="true"
      :enableDragging="true"
      style="border: 1px solid black"
      :content-width="500"
      :content-height="500"
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
    </PinchScrollZoom>
  </div>

  <button @click="reset">Reset</button>
  <label>
    <input type="checkbox" v-model="within" :value="true" />
    within
  </label>
  <br />
  <label>
    min scale:
    <input type="number" v-model.number="minScale" style="width: 40px" />
  </label>
  <label>
    max scale:
    <input type="number" v-model.number="maxScale" style="width: 40px" />
  </label>
  <p>
    {{ eventName }}:<br />
    {{ eventData }}
  </p>
</template>
