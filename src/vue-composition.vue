<template>
  <div class="flex-1">
    <textarea class="w-full h-full border border-gray-400" v-model="value" />
  </div>
  <svg class="flex-1" ref="svgRef" />
</template>

<script>
import { ref, onMounted, onUpdated } from 'vue';
import { Transformer } from 'markmap-lib';
import { Markmap } from 'markmap-view/dist/index.esm';

const transformer = new Transformer();
const initValue = `# markmap

- beautiful
- useful
- easy
- interactive
`;

export default {
  name: 'App',
  setup() {
    const svgRef = ref();
    const value = ref(initValue);
    let mm;

    const update = () => {
      const { root } = transformer.transform(value.value);
      mm.setData(root);
      mm.fit();
    };

    onMounted(() => {
      mm = Markmap.create(svgRef.value);
      update();
    });
    onUpdated(update);
    return {
      svgRef,
      value,
    };
  },
};
</script>
