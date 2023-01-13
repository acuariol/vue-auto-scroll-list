<template>
  <div class="box" ref="box">
    <template v-for="(item,index) of list">
      <slot name="item" v-bind:item="item" v-bind:index="index"></slot>
    </template>
  </div>
</template>

<script>
export default {
  name: 'auto-scroll-list',
  props: {
    list: {
      type: Array,
      default: () => ([]),
    },
    interval: {
      type: Number,
      default: 3000,
    },
  },
  computed: {
    itemHeight() {
      if (this.boxRef) {
        return this.boxRef.scrollHeight / this.list.length;
      }
      return 0;
    },
  },
  watch: {
    list() {
      this.$nextTick(() => {
        this.setup();
      });
    },
  },
  data() {
    return {
      boxRef: null,
      timer: null,
      current: 0,
    };
  },
  beforeDestroy() {
    this.stopAutoScroll();
  },
  mounted() {
    this.boxRef = this.$refs['box'];
    // console.log(this.boxRef.scrollHeight);
    // console.log(this.boxRef.offsetHeight);
    this.setup();
  },
  methods: {
    setup() {
      if (this.boxRef && (this.boxRef.offsetHeight < this.boxRef.scrollHeight)) {
        this.addMouseEvent();
        this.autoScroll();
      }
    },
    addMouseEvent() {
      this.boxRef.onmouseleave = this.onMouseLeave;
      this.boxRef.onmouseenter = this.onMouseEnter;
    },
    stopAutoScroll() {
      clearInterval(this.timer);
      this.timer = null;
    },
    onMouseEnter() {
      this.stopAutoScroll();
    },
    onMouseLeave() {
      this.autoScroll();
    },
    autoScroll() {
      this.timer = setInterval(() => {
        this.current += 1;
        if (this.current >= this.list.length) {
          this.current = 0;
        }
        this.handleScroll();
      }, this.interval);
    },
    handleScroll() {

      // console.log({
      //   index: this.current,
      //   scrollTop: this.boxRef.scrollTop,
      // });

      const { offsetHeight, scrollTop, scrollHeight } = this.boxRef;
      const isBottom = offsetHeight + scrollTop >= scrollHeight;

      let top = this.current * this.itemHeight;

      if (isBottom) {
        this.current = 0;
        this.boxRef.scrollTo({
          top: 0,
          left: 0,
          behavior: 'smooth',
        });
        return;
      }

      this.boxRef.scrollTo({
        top: top,
        left: 0,
        behavior: 'smooth',
      });

    },
  },
};
</script>

<style scoped>
.box {
  height: 100%;
  overflow-y: auto;
}


.box::-webkit-scrollbar {
  width: 5px;
  height: 5px;
  border-radius: 10px;
}

.box::-webkit-scrollbar-thumb {
  border-radius: 10px;
  background-color: #adadaba4;

}

.box::-webkit-scrollbar-track {
  border-radius: 10px;
  background-color: #fff;
}
</style>
