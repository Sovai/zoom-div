<template>
  <div class="bg-slate-300 relative overflow-hidden" id="wrapper">
    <div class="flex flex-wrap gap-2">
      <div
        v-for="i in 150"
        :key="i"
        @click="clicked($event, i)"
        class="rounded-md w-10 h-10"
        :class="i === selected ? 'bg-red-500' : 'bg-blue-400'"
      ></div>
    </div>
  </div>
</template>

<!-- eslint-disable no-unused-vars -->
<script setup>
import { nextTick, onMounted, ref } from "vue";
import { gsap, Power3 } from "gsap";
import Draggable from "gsap/Draggable";
gsap.registerPlugin(Draggable);

let selected = ref(null);
let scale = ref(1);

onMounted(async () => {
  await nextTick();
  Draggable.create("#wrapper", {
    onDragStart: function () {
      gsap.killTweensOf("#wrapper");
    },
    bounds: document.getElementById("container"),
  });
});

function clicked(e, i) {
  selected.value = selected.value === i ? null : i;
  // scale.value = scale.value === 1 ? 1.5 : 1;
  scale.value = 1.5;
  let rect = e.target.getBoundingClientRect();
  let centerX = window.innerWidth / 2;
  let centerY = window.innerHeight / 2;
  let x = centerX - (rect.x + rect.width / 2);
  let y = centerY - (rect.y + rect.height / 2);

  gsap.to("#wrapper", {
    x: "+=" + x,
    y: "+=" + y,
    duration: 1,
    ease: Power3.easeInOut,
  });

  gsap.to("#wrapper", {
    scale: scale.value,
    duration: 1,
    ease: Power3.easeInOut,
  });
}

// function onSeatClick(e, i) {
//   selected.value = selected.value === i ? null : i;

//   const rect = e.target.getBoundingClientRect();

//   console.log("rect: ", rect, e);

//   const x = -rect.x;
//   const y = -rect.y;
//   scale.value = scale.value === 1 ? 1.5 : 1;

//   console.log("x-y: ", x, y);
//   var tw = gsap
//     .to("#wrapper", {
//       x,
//       y,
//       scale: scale.value,
//       paused: true,
//       ease: Power3.easeInOut,
//       duration: 1,
//     })
//     .reverse();

//   tw.reversed() ? tw.play() : tw.reverse();
// }
</script>

<style lang="scss" scoped></style>
