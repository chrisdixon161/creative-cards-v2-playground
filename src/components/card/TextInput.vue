<template>
  <div
    @mouseover="state.showOptions = true"
    @mouseout="state.showOptions = false"
  >
    {{ state.currentSection }}
    <h4>Edit Text:</h4>
    <button @click="move('up')">up</button>
    <button @click="move('down')">down</button>
    <textarea
      rows="4"
      cols="50"
      v-model="state.currentSection.userInput"
    ></textarea>
    <div class="menu" v-show="state.showOptions">
      <label for="selectBox">Font size:</label>
      <!-- the color picker popup is separate so acts as our mouse leaving the menu, so we keep it open -->
      <input
        type="color"
        @mouseleave="keepOpen"
        v-model="state.currentSection.color"
      />
      <input
        type="color"
        @mouseleave="keepOpen"
        v-model="state.currentSection.background"
      />
      <select id="selectBox" v-model="state.currentSection.fontSize">
        <option value="42px">42px</option>
        <option value="48px">48px</option>
        <option value="56px">56px</option>
        <option value="64px">64px</option>
      </select>

      <label>
        <input type="checkbox" v-model="state.currentSection.isBold" />
      </label>
      <label>
        <input type="checkbox" v-model="state.currentSection.isItalic" />
      </label>
      <div>
        <label>
          <input
            type="radio"
            v-model="state.currentSection.justifyContent"
            value="flex-start"
            name="horizontal"
          />
          <img src="@/assets/icons/left.svg" alt="left" />
        </label>
        <label>
          <input
            type="radio"
            v-model="state.currentSection.justifyContent"
            value="center"
            name="horizontal"
          />
          <img src="@/assets/icons/center.svg" alt="center" />
        </label>
        <label>
          <input
            type="radio"
            v-model="state.currentSection.justifyContent"
            value="flex-end"
            name="horizontal"
          />
          <img src="@/assets/icons/right.svg" alt="right" />
        </label>
        <label>
          <input
            type="radio"
            v-model="state.currentSection.alignItems"
            value="flex-start"
            name="vertical"
          />
          <img src="@/assets/icons/top.svg" alt="top" />
        </label>
        <label>
          <input
            type="radio"
            v-model="state.currentSection.alignItems"
            value="center"
            name="vertical"
          />
          <img src="@/assets/icons/middle.svg" alt="middle" />
        </label>
        <label>
          <input
            type="radio"
            v-model="state.currentSection.alignItems"
            value="flex-end"
            name="vertical"
          />
          <img src="@/assets/icons/bottom.svg" alt="bottom" />
        </label>
      </div>
    </div>
  </div>
</template>

<script>
import { reactive, watchEffect } from "vue";
export default {
  props: ["section"],
  setup(props) {
    let state = reactive({
      currentSection: props.section,
      showOptions: false,
    });
    watchEffect(() => {
      // update when new page is selected
      // state.currentSection = props.section || {};
    });

    function keepOpen() {
      state.showOptions = true;
    }

    // refactor to one function
    // use to add to the 4 slots on the card not to reorder
    // emit to parent to place in correct slot
    function move(direction) {
      if (direction === "up" && state.currentSection.position !== 1) {
        state.currentSection.position--;
      } else if (direction === "down" && state.currentSection.position !== 3) {
        state.currentSection.position++;
      }
    }

    return { state, keepOpen, move };
  },
};
</script>

<style scoped>
.menu {
  position: absolute;
  background: turquoise;
  padding: 5px;
}
/* HIDE RADIO */
input[type="radio"] {
  opacity: 0;
  width: 0;
  height: 0;
}
/* IMAGE STYLES */
input[type="radio"] + img {
  max-width: 40px;
  cursor: pointer;
}
/* CHECKED STYLES */
input[type="radio"]:checked + img,
/* For accessibility tabbing- tab selects group, navigate with left/right keys */
input[type="radio"]:focus + img {
  outline: 2px solid rgb(78, 70, 70);
}
</style>
