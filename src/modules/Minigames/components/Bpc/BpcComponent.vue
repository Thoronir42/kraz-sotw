<template>
  <div :class="['mg-bpc']">

    <form class="bpc-container" @submit.prevent="checkValues">
      <div class="input-pair" v-for="input in inputs" :key="input.name">
        <label :for="input.name">{{ input.caption }}</label>
        <input type="number" inputmode="numeric" v-model.number="inputsModel[input.name]"
               :id="input.name"/>
      </div>

      <div class="input-pair input-pair-full input-pair-center">
        <label>&nbsp;</label>
        <button type="submit" class="btn btn-vivid">Check</button>
      </div>
    </form>

  </div>
</template>

<script lang="ts">
import {computed, defineComponent, PropType, ref} from "vue";
import {hashCode} from "@/utils/stringUtils";

type InputSpec = {
  name: string,
  caption: string,
};

type BpcMinigameData = { inputs: InputSpec[], check: string }

export default defineComponent({
  props: {
    minigameData: {type: Object as PropType<BpcMinigameData>, required: true},
  },
  setup(props, {emit}) {
    const inputs = computed(() => props.minigameData.inputs);
    const inputsModel = ref(Object.fromEntries(inputs.value.map((input) => [input.name, 0])));

    function checkValues() {
      let valueSerialized = Object.values(inputsModel.value).map((v) => 's4lty' + v + 'p3pp3ry').join('-');
      let hash = hashCode(valueSerialized).substr(0, 6);

      if (hash === props.minigameData.check) {
        emit('minigameSignal', {
          type: 'success',
        });
      } else {
        emit('minigameSignal', {
          type: 'error',
        })
      }
    }

    return {
      inputs,
      inputsModel,
      checkValues,
    };
  },
});

</script>

<style lang="scss">
.mg-bpc {
  .bpc-container {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    gap: 0.25em;

    .input-pair {
      flex: 0 1 120px;
      //min-width: 120px;
      display: flex;
      flex-direction: column;
      align-items: flex-start;

      &.input-pair-full {
        flex: 1 0 100%;
      }

      &.input-pair-center {
        align-items: center;
      }

      input {
        align-self: stretch;
      }
    }
  }
}
</style>

