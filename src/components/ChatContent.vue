<template>
  <section ref="section" class="content">
    <ul>
      <chat-item-component
        v-for="(item, index) of items"
        :key="index"
        :source="item"
        tag="li"
      />
    </ul>
  </section>
</template>

<script>
import ChatItemComponent from './ChatItemComponent';

export default {
  name: 'ChatContent',
  components: {
    ChatItemComponent
  },
  props: {
    items: {
      required: true,
      type: Array
    }
  },
  watch: {
    items() {
      this.scrollBottom();
    },
  },
  methods: {
    scrollBottom() {
      setTimeout(() => {
        this.$refs.section.scrollTop = this.$refs.section.scrollHeight;
      }, 10);
    },
  }
};
</script>

<style lang="scss" scoped>
  .content {
    width: 100%;
    height: 100%;
    overflow: auto;

    ul {
      margin: 0;
      padding: 12px 0;
    }

    li {
      list-style-type: none;
    }
  }
</style>
