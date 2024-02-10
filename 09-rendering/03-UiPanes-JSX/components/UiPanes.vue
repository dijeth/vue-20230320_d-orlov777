<script lang="jsx">
import UiButton from './UiButton.vue';
import VNode from './VNode.vue';

export default {
  name: 'UiPanes',

  components: {
    UiButton,
  },

  render() {
    const vnodes = this.$slots.default();
    return (
      <div class="panes">
        {this.panes.map((it, i) => (
          <div class="pane">
            <div class="pane__content">
              <VNode vnode={vnodes[it]}></VNode>
            </div>
            <div class="pane__controls">
              <UiButton
                class={i === 0 ? 'pane__disabled-button' : ''}
                variant="secondary"
                block
                onClick={() => this.up(i)}
              >
                Up
              </UiButton>
              <UiButton
                class={i === vnodes.length - 1 ? 'pane__disabled-button' : ''}
                variant="danger"
                block
                onClick={() => this.down(i)}
              >
                Down
              </UiButton>
            </div>
          </div>
        ))}
      </div>
    );
  },

  data() {
    return {
      /**
       * Массив с текущим порядком номеров моделей, например
       * [0, 1, 2]
       * @type {number[]|null}
       */
      panes: Array(this.$slots.default().length)
        .fill()
        .map((_, i) => i),
      // Сейчас здесь массив ровно из трёх элементов, но решение должно быть универсальным для любого количества узлов
    };
  },

  methods: {
    /**
     * Переместить i-ую панель вверх
     *
     * @param {number} i
     */
    up(i) {
      const temp = this.panes[i];
      this.panes[i] = this.panes[i - 1];
      this.panes[i - 1] = temp;
    },

    /**
     * Переместить i-ую панель вниз
     *
     * @param {number} i
     */
    down(i) {
      const temp = this.panes[i];
      this.panes[i] = this.panes[i + 1];
      this.panes[i + 1] = temp;
    },
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
