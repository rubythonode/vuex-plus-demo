<script>
  import { map, addStore } from 'vuex+';
  import counterGroup from '../counter-group/counter-group.vue';

  const { mixin, api } = addStore('item-list-store');

  export default {
    mixins: [mixin],
    computed: {
      ...map.getters({
        items: api.get.items,
      }),
    },
    methods: {
      ...map.actions({
        removeItem: api.act.removeItem,
      }),
    },
    components: {
      counterGroup,
    },
  };
</script>

<template>
  <transition-group name="fade" appear tag="div" class="item-list">
    <div v-for="instance in items" :key="instance.id" class="group">
      <counterGroup :instance="instance.name"
                    @removeCounter="removeItem(instance.id)"></counterGroup>
    </div>
  </transition-group>
</template>

<style scoped>
  .item-list {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-wrap: wrap;
    height: 100%;
  }

  .fade-enter-active, .fade-leave-active {
    transition: transform .75s ease-in-out, opacity 0.75s ease;
  }
  .fade-enter {
    transform: scale(0.9);
    opacity: 0;
  }
  .fade-enter-to {
    transform: scale(1);
    opacity: 1;
  }
  .fade-leave {
    transform: scale(1);
    opacity: 1;
  }
  .fade-leave-to {
    transform: scale(0.4);
    opacity: 0;
  }

  .group {
    position: relative;
    margin: 10px
  }

</style>
