<template>
  <v-row class="field">
    <v-col>
      <v-text-field
        label="Type your message"
        single-line
        filled
        :disabled="disabled"
        v-model="input"
        @keydown="onKeyDown"
      />
    </v-col>
    <v-col cols="3">
      <v-btn :disabled="disabled" @click="send" x-large color="primary" dark class="field-input">{{ btnText }}</v-btn>
    </v-col>
  </v-row>
</template>

<script>
export default {
  name: 'ChatInput',
  props: {
    value: {
      type: String,
      default: ''
    },
    disabled: Boolean,
    isFirstMessage: Boolean
  },
  data() {
    return {
      input: null
    };
  },
  computed: {
    btnText() {
      return this.isFirstMessage ? `Let's chat` : 'Send Message';
    }
  },
  methods: {
    send() {
      if (this.input) {
        this.$emit('input', this.input);
        this.input = null;
      }
    },
    onKeyDown(e) {
      const ENTER = 13;
      if (e.keyCode === ENTER) {
        this.send();
      }
    }
  }
};
</script>

<style lang="scss" scoped>
.field {
  height: 100%;

  &-input {
    width: 100%;
  }
}
</style>
