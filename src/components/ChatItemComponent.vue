<template>
  <component :is="tag" class="chat-item" :class="isOwner ? 'me' : 'him' ">
    <v-chip :color="isOwner ? 'primary' : ''">
      <vue-typed-js :strings="[source.text]" :showCursor="false">
        <span class="typing"></span>
      </vue-typed-js>
    </v-chip>
  </component>
</template>

<script>
export default {
  name: 'ChatItemComponent',
  props: {
    source: {
      type: Object,
      required: true,
      default: () => ({})
    },
    tag: {
      type: String,
      default: 'div'
    }
  },
  computed: {
    isOwner() {
      return this.source.owner === 'me';
    }
  }
};
</script>

<style lang="scss" scoped>
.chat-item {
  width: 100%;
}

.him {
  .v-chip {
    border-top-left-radius: 0;
  }
}

.him + .me {
  & > * {
    border-bottom-right-radius: 5px;
  }
}

.me{
  text-align: right;

  .v-chip {
    border-bottom-right-radius: 5px;
    transition: border-radius .3s ease;
  }

  & + & {
    .v-chip {
      border-top-right-radius: 5px;
      border-bottom-right-radius: 5px;
    }
  }

  &:last-child, &:last-of-type {
    .v-chip {
      border-bottom-right-radius: 16px;
    }
  }
}
</style>
