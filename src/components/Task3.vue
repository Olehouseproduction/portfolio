<script setup>
import { reactive } from "vue";
import BtnTask from "./task-components/Subtask3BtnTask.vue";
import Header from "./ui-components/Header.vue";

const change = reactive({ context: false, numberOfButton: -1 });

const tasks = reactive([
  {
    name: "Последний становится красным",
    handler() {
      tasks[8].isRed = true;
    },
  },
  {
    name: "Первый и четвертый становятся зелеными",
    handler() {
      tasks[0].isGreen = true;
      tasks[3].isGreen = true;
    },
  },
  {
    name: "Отменяет действие первого, и третий становится желтым",
    handler() {
      tasks[2].isYellow = true;
      tasks[8].isRed = false;
    },
  },
  {
    name: "Отменяет действие второго, и первый пульсирует",
    handler() {
      tasks[0].isPulsing = true;
      tasks[0].isGreen = false;
      tasks[3].isGreen = false;
    },
  },
  {
    name: "Меняет цвет задника",
    handler() {
      change.context = true;
    },
  },
  {
    name: "Отменяет все и красит всех в розовый",
    handler() {
      refresh();
      tasks.forEach((item) => {
        item.isPink = true;
      });
    },
  },

  {
    name: "Добавляет пульсацию рандомному",
    handler() {
      handleRandomChange(tasks, "isPulsing");
    },
  },
  {
    name: "Делает рандомного синим",
    handler() {
      handleRandomChange(tasks, "isBlue");
    },
  },
  {
    name: "Делает невидимым рандомного",
    handler() {
      handleRandomChange(tasks, "isInvisible");
    },
  },
]);

function getRandomIndex(array) {
  return Math.floor(Math.random() * array.length);
}

function handleRandomChange(buttons, colorToSet) {
  const notColoredBtn = buttons.filter((btn) => !btn[colorToSet]);
  if (notColoredBtn.length > 0) {
    const randIndex = getRandomIndex(notColoredBtn);
    notColoredBtn[randIndex][colorToSet] = true;
  }
}

function refresh() {
  tasks.forEach((item) => {
    item.isBlue = false;
    item.isRed = false;
    item.isPulsing = false;
    item.isGreen = false;
    item.isPink = false;
    item.isYellow = false;
    item.isInvisible = false;
    item.isActive = false;
  });
  change.context = false;
  change.numberOfButton = -1;
}
</script>

<template>
  <div class="main" :class="{ fond: change.context }">
    <Header name="Смена классов" />
    <div class="reset-wrap">
      <BtnTask class="reset" name="Сброс" @click="refresh" />
    </div>
    <div class="workspace--task3">
      <BtnTask
        :name="task.name"
        v-for="(task, i) in tasks"
        :key="'task' + i"
        :is-blue="task.isBlue"
        :is-red="task.isRed"
        :is-pulse="task.isPulsing"
        :is-pink="task.isPink"
        :is-yellow="task.isYellow"
        :is-green="task.isGreen"
        :is-invisible="task.isInvisible"
        @click="
          () => {
            task.handler();
            change.numberOfButton = i;
          }
        " />
      <br />
      <div id="picture" :class="'button' + change.numberOfButton"></div>
      <!-- {{ change.numberOfButton }}
      {{ "button" + change.numberOfButton }} -->
    </div>
  </div>
</template>

<style lang="scss" scoped>
@import "../styles/mixin.scss";

.reset-wrap {
  display: flex;
  .reset {
    background-color: #ffffff;
    height: 5em;
    margin-left: auto;
    margin-right: 2vw;
    padding: 0 50px;
    width: auto;
  }
}
.workspace--task3 {
  @include size(70%, 50%);
  @include flex(space-around, center);
  margin: 4% auto;
  gap: 1.7rem;
}

#picture {
  background-image: url(../assets/image/bird2.png);
  background-position: center;
  background-repeat: no-repeat;
  position: fixed;
  width: 246px;
  height: 246px;
  margin: 0;
  padding: 0;
  bottom: 5%;
  right: 5%;
  box-shadow: 0 1px 0 rgba(255, 255, 255, 0.2),
    inset 0 4px 5px rgba(0, 0, 0, 0.6), inset 0 1px 0 rgba(0, 0, 0, 0.6);
  object-fit: contain;

  &.button0 {
    background-image: url(../assets/image/small/red.jpg);
  }
  &.button1 {
    background-image: url(../assets/image/small/green.jpg);
  }
  &.button2 {
    background-image: url(../assets/image/bird2.png);
  }
  &.button3 {
    background-image: url(../assets/image/small/green1.jpg);
  }
  &.button4 {
    background-image: url(../assets/image/small/back.jpg);
  }
  &.button5 {
    background-image: url(../assets/image/small/pink.jpg);
  }
  &.button6 {
    background-image: url(../assets/image/small/pulse.jpg);
  }
  &.button7 {
    background-image: url(../assets/image/small/blue.jpg);
  }
  &.button8 {
    background-image: url(../assets/image/small/invis.jpg);
  }
}

.fond {
  background-image: url(../assets/image/sand2.jpg);
  // background-color: purple;
  background-size: cover;
}
</style>

<!-- ../assets/image/small/green1.jpg -->
<!-- ../assets/image/small/ex.jpg -->
<!-- ../assets/image/small/beauti.jpg -->
