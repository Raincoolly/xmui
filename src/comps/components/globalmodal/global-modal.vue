<template>
  <transition name="fade">
    <div class="xm__dialog--wrap" v-show="isVisible" ref="modal" >
      <div class="xm__mask" @click="maskClose"></div>
      <div class="xm__dialog">
        <div class="xm__dialog--hd">
          <strong class="xm__dialog--title" v-if="title">{{title}}</strong>
        </div>
        <div class="xm__dialog--bd">
          {{content}}
        </div>
        <div class="xm__dialog--ft">
          <div v-if="type&&type=='alert'">
            <xm-button @click="confirm" :style="{'color':color}" long>确定</xm-button>
          </div>
          <div v-else-if="type&&type=='confirm'">
          <xm-button-group class="xm__btn--group" >
            <xm-button @click="close">取消</xm-button>
            <xm-button @click="confirm" :style="{'color':color}">确定</xm-button>
          </xm-button-group>
          </div>
          <div v-else>
            <xm-button @click="confirm" :style="{'color':color}" long>确定</xm-button>
          </div>
        </div>
      </div>
      
    </div>
  </transition>
</template>

<script>
import ModalMixin from './ModalMixin'
export default {
  mixins: [ModalMixin],
  props: {
    type: {
      type: String,
      default: 'default'
    },
    title: {
      type: String,
      default: ''
    },
    color: {
      type: String,
      default: ''
    },
    maskClosable: {
      type: Boolean,
      default: true
    },
    content: String,
    autoClose: {
      type: Boolean,
      default: false
    },
    callBack: {
      type: Function,
      default () {}
    }
  },
  methods: {
    close () {
      this.$emit('close')
      this.isVisible = false
      this.destroyed()
    },
    confirm () {
      this.$emit('confirm')
      this.isVisible = false
      this.callBack()
      this.destroyed()
    },
    maskClose () {
      if (this.maskClosable) {
        this.close()
      }
    },
    destroyed () {
      setTimeout(() => {
        this.$destroy()
      }, 200)
    }
  },
  mounted () {
    setTimeout(() => {
      this.isVisible = true
    }, 100)

    if (this.autoClose) {
      setTimeout(() => {
        this.close()
      }, 3000)
    }
  }
}
</script>