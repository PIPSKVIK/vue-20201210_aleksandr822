<template>
  <div class="input-group" :class="classIcon">
    <slot name="left-icon" />

    <component
      :is="multiline ? 'textarea' : 'input'"
      class="form-control"
      :class="classComponent"
      :value.prop="value"
      v-bind="$attrs"
      v-on="listeners"
    />

    <slot name="right-icon" />
  </div>
</template>

<script>
export default {
  name: 'AppInput',

  inheritAttrs: false,

  props: {
    small: {
      type: Boolean,
    },
    rounded: {
      type: Boolean,
    },
    multiline: {
      type: Boolean,
    },
    value: {},
  },

  mode: {
    prop: 'value',
    event: 'input',
  },

  data() {
    return {
      iconLeft: false,
      iconRight: false,
    };
  },

  mounted() {
    this.updateIcon();
  },

  updated() {
    this.updateIcon();
  },

  methods: {
    updateIcon() {
      this.iconLeft = !!this.$slots['left-icon'];
      this.iconRight = !!this.$slots['right-icon'];
    },
  },

  computed: {
    listeners() {
      return {
        ...this.$listeners,
        input: ($event) => {
          this.$emit('input', $event.target.value);
        },
        change: ($event) => {
          this.$emit('change', $event.target.value);
        },
      };
    },
    classIcon() {
      return {
        'input-group_icon': this.iconLeft || this.iconRight,
        'input-group_icon-left': this.iconLeft,
        'input-group_icon-right': this.iconRight,
      };
    },
    classComponent() {
      return {
        'form-control_sm': this.small,
        'form-control_rounded': this.rounded,
      };
    },
  },
};
</script>

<style scoped>
.form-control {
  padding: 12px 16px;
  height: 52px;
  border-radius: 8px;
  border: 2px solid var(--blue-light);
  font-family: 'Nunito', sans-serif;
  font-weight: 600;
  font-size: 20px;
  line-height: 28px;
  color: var(--body-color);
  transition: 0.2s all;
  background-color: var(--white);
  outline: none;
  box-shadow: none;
}

.form-control::placeholder {
  font-weight: 400;
  color: var(--blue-2);
}

.form-control:focus {
  border-color: var(--blue);
}

textarea.form-control {
  width: 100%;
  min-height: 211px;
}

.form-control.form-control_rounded {
  border-radius: 26px;
}

.form-control.form-control_sm.form-control_rounded {
  border-radius: 22px;
}

.form-control.form-control_sm {
  padding: 8px 16px;
  height: 44px;
  border-radius: 4px;
}

.input-group {
  position: relative;
}

.input-group .form-control {
  width: 100%;
}

.input-group.input-group_icon .form-control {
  padding-left: 50px;
}

.input-group.input-group_icon .icon {
  position: absolute;
  top: 50%;
  transform: translate(0, -50%);
}

.input-group.input-group_icon.input-group_icon-left .icon:first-child {
  left: 16px;
}

.input-group.input-group_icon.input-group_icon-right .icon:last-child {
  right: 16px;
}
</style>
