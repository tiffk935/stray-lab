<template lang="pug">
  .data-modal(:class="{ show: showSelf }")
    .data-title {{chart}}
    .data-question
      .lost-count(v-if="chart=='家寵遺失數'") 0
    .data-actions
      .action-ok(@click="hide(); $emit('ended')") 我知道了
</template>

<script>
import * as d3 from 'd3'

export default {
  // props: {
  //   situation: {
  //     type: Object,
  //     required: true
  //   }
  // },
  props: {
    chart: String
  },
  data () {
    return {
      showSelf: false,
      lostCount: 41663, // 寵物走失數
    }
  },
  methods: {
    show () {
      this.showSelf = true
    },
    hide () {
      this.showSelf = false
    },

    lostCountAnimation () {
      var end = this.lostCount;
      setTimeout(function(){
        d3.select('.lost-count').transition()
          .tween("text", () => {
            var interpolator = d3.interpolateNumber(0, end)
            return function(t) { d3.select(this).text(Math.round(interpolator(t))) }
          })
          .duration(1000);
      }, 100);
    }
  },
  watch: {
    chart: {
      immediate: true,
      deep: true,
      handler(newValue) {
        if(newValue == '家寵遺失數'){
          this.lostCountAnimation();
        }
      }
    }
  },
}
</script>

<style lang="scss">
.data-modal {
  position: fixed;
  top: 50px;
  left: 100px;
  right: 100px;
  bottom: 50px;
  padding: 30px;
  background-color: rgba(255, 255, 255, 0.8);
  color: white;
  border-radius: 10px;
  transition: all 1s;
  opacity: 0;
  pointer-events: none;
  z-index: 5 !important;
  display: flex;
  flex-direction: column;
  color: black;

  &.show {
    opacity: 1;
    pointer-events: auto;
  }

  .data-title {
    font-size: 24px;
    margin-bottom: 20px;
  }

  .data-question {
    font-size: 18px;
  }

  .data-actions {
    margin-top: auto;
    display: flex;

    .action-ok {
      width: 100%;
      border-radius: 4px;
      padding: 10px;
      text-align: center;
      font-size: 18px;
      margin: 0 20px;
      transition: all .3s;

      &:hover {
        background-color: black;
        border: 1px solid white;
        color: white;
        cursor: pointer;
      }
    }

    .action-ok {
      background-color: transparent;
      border: 1px solid black;
      color: black;
    }
  }
}

.lost-count {
  text-align: center;
  font-size: 100px;
  font-weight: 700;
}
</style>