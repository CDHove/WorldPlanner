<template>
  <div id="mydiv" ref="mydiv" class="mydiv" :class="{ ismerging: isMerging }">Drag me!</div>
</template>
<script setup lang="ts">
import { ref, onMounted } from 'vue';

const mydiv = ref<HTMLElement | null>(null);
const isMerging = ref(false);

onMounted(() => {
  dragElement(mydiv.value);
});

// Your dragElement function (with null check)
function dragElement(elmnt: HTMLElement | null) {
  if (!elmnt) return;
  let pos1 = 0,
    pos2 = 0,
    pos3 = 0,
    pos4 = 0;
  const header = document.getElementById(elmnt.id + 'header');
  if (header) {
    header.onmousedown = dragMouseDown;
  } else {
    elmnt.onmousedown = dragMouseDown;
  }

  function dragMouseDown(e: MouseEvent) {
    e.preventDefault();
    pos3 = e.clientX;
    pos4 = e.clientY;
    document.onmouseup = closeDragElement;
    document.onmousemove = elementDrag;
  }

  function elementDrag(e: MouseEvent) {
    e.preventDefault();
    pos1 = pos3 - e.clientX;
    pos2 = pos4 - e.clientY;
    pos3 = e.clientX;
    pos4 = e.clientY;
    if (elmnt) {
      const top = elmnt.offsetTop - pos2;
      const left = elmnt.offsetLeft - pos1;
      console.log(top);
      console.log(left);
      console.log(isMerging);
      if (top < 60 && left < 60) {
        isMerging.value = true;
      } else {
        isMerging.value = false;
      }
      elmnt.style.top = top + 'px';
      elmnt.style.left = left + 'px';
    }
  }

  function closeDragElement() {
    document.onmouseup = null;
    document.onmousemove = null;
  }
}
</script>

<style>
.ismerging {
  opacity: 0.5;
  background-color: aqua;
}

.mydiv {
  position: absolute;
  width: 200px;
  height: 100px;
  background-color: rgb(136, 136, 136);
  z-index: 9;
  border: 1px solid #d3d3d3;
  text-align: center;
}

#mydivheader {
  padding: 10px;
  cursor: move;
  z-index: 10;
  background-color: #2196f3;
  color: #fff;
}
</style>
