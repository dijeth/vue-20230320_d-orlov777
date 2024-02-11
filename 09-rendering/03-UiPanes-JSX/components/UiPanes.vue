<script lang="tsx">
import { ref } from 'vue';
import UiButton from './UiButton.vue';
import VNode from './VNode.vue';

export default {
  name: 'UiPanes',

  components: {
    UiButton,
  },

  setup(_, { slots }) {
    const vnodes = (slots.default && slots.default()) || [];

    const panes = ref(
      Array(vnodes.length)
        .fill(0)
        .map((_, i) => i),
    );

    /**
     * Переместить i-ую панель вверх
     *
     * @param {number} i
     */
    const up = (i: number) => {
      const temp = panes.value[i];
      panes.value[i] = panes.value[i - 1];
      panes.value[i - 1] = temp;
    };

    /**
     * Переместить i-ую панель вниз
     *
     * @param {number} i
     */
    const down = (i: number) => {
      const temp = panes.value[i];
      panes.value[i] = panes.value[i + 1];
      panes.value[i + 1] = temp;
    };

    return () => (
      <div class="panes">
        {panes.value.map((it, i) => (
          <div class="pane">
            <div class="pane__content">
              <VNode vnode={vnodes[it]}></VNode>
            </div>
            <div class="pane__controls">
              <UiButton class={i === 0 ? 'pane__disabled-button' : ''} variant="secondary" block onClick={() => up(i)}>
                Up
              </UiButton>
              <UiButton
                class={i === vnodes.length - 1 ? 'pane__disabled-button' : ''}
                variant="danger"
                block
                onClick={() => down(i)}
              >
                Down
              </UiButton>
            </div>
          </div>
        ))}
      </div>
    );
  },
};
</script>

<style scoped>
.panes {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.pane {
  display: flex;
  flex-direction: row;
  border: 3px solid var(--blue-light);
}

.pane__content {
  border-right: 3px solid var(--blue-light);
  padding: 8px;
  flex: 1 0;
}

.pane__controls {
  display: flex;
  flex-direction: column;
  gap: 8px;
  padding: 8px;
  width: min-content;
}

.pane__disabled-button {
  visibility: hidden;
}
</style>
