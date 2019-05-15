<template>

  <div class="custom-input" :style="cStyle">

    <input
      ref="input"
      type="number"
      :value="cValue"
      @input="emitValue"
      :maxlength="length"
      :focus="focus"
    >

    <ul>
      <li v-for="i in length" :style="cGridStyle">
        <template v-if="type!=='password'">
          {{showInGrid(--i)}}
        </template>
        <template v-else>
          <div class="pwd-dot" v-if="cValue.length >= i"></div>
        </template>
      </li>
    </ul>

  </div>

</template>

<script>
  export default {
    name: "CustomInput",
    props: {
      value: {
        type: String,
        default: ''
      },
      type: {
        type: String,
        default: 'number'
      },
      length: {
        type: Number,
        default: 6
      },
      width: {
        type: [Number, String],
        default: 100
      },
      height: {
        type: [Number, String],
        default: 0
      },
      focus: {
        type: Boolean,
        default: false
      }
    },
    computed: {
      cValue() {
        return this.value ? this.value.substr(0, this.length) : '';
      },
      calcHeight() {
        return this.height ? this.height + 'vw' : +this.width / this.length;
      },
      cStyle() {
        return {
          width: this.width + 'vw',
          height: this.calcHeight + 'vw'
        }
      },
      cGridStyle() {
        return {
          width: this.calcHeight + 'vw',
          lineHeight: this.calcHeight + 'vw',
          fontSize: this.calcHeight / 2 + 'vw'
        }
      },
      showInGrid(i) {
        return function (i) {
          if (this.cValue && this.cValue[i]) {
            return this.cValue[i];
          } else {
            return null;
          }
        }
      }
    },
    watch: {
      focus(newValue, oldValue) {
        if (newValue) {
          this.focusInput();
        } else {
          this.blurInput();
        }
      },
      cValue(newValue, oldValue) {
        if (newValue.length === this.length) {
          this.finishInput();
        }
      }
    },
    mounted() {
      this.focusInput();
    },
    methods: {
      emitValue(e) {
        const value = e.target.value.substr(0, this.length);
        e.target.value = value;
        this.$emit('input', value);
      },
      focusInput() {
        this.$refs.input.focus();
      },
      blurInput() {
        this.$refs.input.blur();
      },
      /**
       * 输入完成的回调
       */
      finishInput() {
        this.$emit('done', this.cValue);
      }
    }
  }
</script>

<style lang="scss" scoped>
  * {
    box-sizing: border-box;
  }

  .custom-input {
    position: relative;
    input {
      position: absolute;
      /*width: 0;*/
      height: 0;
      padding: 0;
      margin: 0;
      border: none;
      outline: none;
      background: transparent;
    }

    ul {
      border: 2px solid rgb(194, 194, 194);
      padding: 0;
      margin: 0;
      display: flex;
      height: 100%;
      width: 100%;
    }
    li {
      list-style: none;
      border-right: 1px solid rgb(239, 239, 239);
      height: 100%;
      display: flex;
      justify-content: center;
      align-items: center;
      &:last-child {
        border-right: none;
      }
    }
    .pwd-dot {
      border-radius: 50%;
      background-color: black;
      width: 28.57%;
      height: 28.57%;
    }
  }

</style>
