<script setup>
import { ref ,onMounted} from 'vue';
import AutoScrollList from './components/auto-scroll-list.vue';

const d = Array.from({ length: 10 }).map((_, index) => ({
  a1: '测试' + (index + 1),
  a2: '芯片',
  a3: '图片',
  a4: '开放',
}));


const list = ref([]);

onMounted(()=>{
  setTimeout(()=>{
    list.value = d
  },2000)
})

</script>

<template>
  <div class="t">
    <auto-scroll-list :list="list" :interval="3000">
      <template v-slot:item="{item,index}">
        <div class="item">
          <p>{{ index + 1 }}</p>
          <p class="flex1">{{ item['a1'] }}</p>
          <p class="flex1">{{ item['a2'] }}</p>
          <p class="flex1">{{ item['a3'] }}</p>
          <p class="flex1">{{ item['a4'] }}</p>
        </div>
      </template>
    </auto-scroll-list>
  </div>
</template>

<style scoped>
p {
  margin: 0;
}

.t {
  height: 300px;
}

.flex1 {
  display: flex;
  flex: 1;
  align-items: center;
  justify-content: center;
}

.item {
  background: #050A28;
height: 30px; color: #fff;
  padding: 10px 0;
  display: flex;
  align-items: center;
}

.item:nth-child(odd) {

  background: #11265d;
}
</style>
