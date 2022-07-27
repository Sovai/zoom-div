<script setup>
import Hammer from "hammerjs";
import { nextTick, onMounted } from "vue";

function hammerIt(elm) {
  const instance = new Hammer(elm, {});
  instance.get("pinch").set({
    enable: true,
  });
  var posX = 0,
    posY = 0,
    scale = 1,
    last_scale = 1,
    last_posX = 0,
    last_posY = 0,
    max_pos_x = 0,
    max_pos_y = 0,
    transform = "",
    el = elm;

  instance.on("doubletap pan pinch panend pinchend", function (ev) {
    if (ev.type === "doubletap") {
      transform = "translate3d(0, 0, 0) " + "scale3d(2, 2, 1) ";
      scale = 2;
      last_scale = 2;
      try {
        if (
          window
            .getComputedStyle(el, null)
            .getPropertyValue("-webkit-transform")
            .toString() != "matrix(1, 0, 0, 1, 0, 0)"
        ) {
          transform = "translate3d(0, 0, 0) " + "scale3d(1, 1, 1) ";
          scale = 1;
          last_scale = 1;
        }
      } catch (err) {
        console.log("err: ", err);
      }
      el.style.webkitTransform = transform;
      transform = "";
    }

    //pan
    if (scale != 1) {
      posX = last_posX + ev.deltaX;
      posY = last_posY + ev.deltaY;
      max_pos_x = Math.ceil(((scale - 1) * el.clientWidth) / 2);
      max_pos_y = Math.ceil(((scale - 1) * el.clientHeight) / 2);
      if (posX > max_pos_x) {
        posX = max_pos_x;
      }
      if (posX < -max_pos_x) {
        posX = -max_pos_x;
      }
      if (posY > max_pos_y) {
        posY = max_pos_y;
      }
      if (posY < -max_pos_y) {
        posY = -max_pos_y;
      }
    }

    //pinch
    if (ev.type === "pinch") {
      scale = Math.max(0.999, Math.min(last_scale * ev.scale, 4));
    }
    if (ev.type === "pinchend") {
      last_scale = scale;
    }

    //panend
    if (ev.type === "panend") {
      last_posX = posX < max_pos_x ? posX : max_pos_x;
      last_posY = posY < max_pos_y ? posY : max_pos_y;
    }

    if (scale !== 1) {
      transform =
        "translate3d(" +
        posX +
        "px," +
        posY +
        "px, 0) " +
        "scale3d(" +
        scale +
        ", " +
        scale +
        ", 1)";
    }

    if (transform) {
      console.log("transform: ", transform);
      el.style.webkitTransform = transform;
    }
  });
}

function onButtonClick(e) {
  // const el = e.target;
  console.log("e: ", e);
  const el = document.getElementById("elem");
  const transform = `translate3d(${e.x}px, ${e.y}px, 0) scale3d(2, 2, 1)`;
  el.style.transform = transform;
}
onMounted(async () => {
  await nextTick();
  hammerIt(document.getElementById("elem"));
});
</script>

<template>
  <div class="bg-red-500 relative h-20 w-full">
    <div
      class="absolute bg-red-500 border-t-black rounded-t-lg top-0 lef-0 w-full h-20"
    ></div>
  </div>
  <div id="elem" class="w-[300px] h-[300px] bg-green-300 rounded p-5">
    <button
      @click="onButtonClick"
      class="bg-green-600 rounded active:bg-green-800 p-1 text-white"
    >
      Click
    </button>
  </div>
</template>
