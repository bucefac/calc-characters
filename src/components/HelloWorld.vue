<template>
  <v-container class="fill-height page">
    <v-responsive
      class="align-centerfill-height mx-auto"
      max-width="900"
    >
    <v-data-table :items="items">
      <template v-slot:item.accuracy="{ item }">
        <v-text-field
          v-model="item.accuracy[0]"
          label="value"
          :counter="10"
          required
        ></v-text-field>
        <v-text-field
          v-model="item.accuracy[1]"
          label="price"
          :counter="10"
          required
        ></v-text-field>
      </template>
      <template v-slot:item.stamina="{ item }">
        <v-text-field
          v-model="item.stamina[0]"
          :counter="10"
          required
        ></v-text-field>
        <v-text-field
          v-model="item.stamina[1]"
          :counter="10"
          required
        ></v-text-field>
      </template>
      <template v-slot:item.strength="{ item }">
        <v-text-field
          v-model="item.strength[0]"
          :counter="10"
          required
        ></v-text-field>
        <v-text-field
          v-model="item.strength[1]"
          :counter="10"
          required
        ></v-text-field>
      </template>
      <template v-slot:item.intellect="{ item }">
        <v-text-field
          v-model="item.intellect[0]"
          :counter="10"
        ></v-text-field>
        <v-text-field
          v-model="item.intellect[1]"
          :counter="10"
        ></v-text-field>
      </template>
      <template v-slot:item.time="{ index }">
        {{ times[index] }} seconds
      </template>
      <template v-slot:item.price="{ index }">
        {{ totalPrices[index] }}
      </template>
      </v-data-table>
      <v-row>

        <v-btn @click="onAddLevel">
          Add level
        </v-btn>
        <v-card
        class="mx-auto my-8"
        elevation="16"
        max-width="344"
        >
        <v-card-item>
          <v-card-title>
            Total time: {{ Math.round(totalTime / 3600 )}} hours
          </v-card-title>
        </v-card-item>
      </v-card>
    </v-row>
    </v-responsive>

  </v-container>
</template>

<script setup lang="ts">
import { computed, ref } from 'vue';

const items = ref([
  {
    level: 1,
    accuracy: [0.5, 0],
    stamina: [100, 0],
    strength: [100, 0],
    intellect: [60, 0],
    price: 0,
    time: 10,
  },
  {
    level: 2,
    accuracy: [0.53, 1700],
    stamina: [120, 2000],
    strength: [120, 3700],
    intellect: [58, 3500],
    price: 0,
    time: 10,
  },
  {
    level: 3,
    accuracy: [0.56, 3000],
    stamina: [140, 3500],
    strength: [140, 6300],
    intellect: [56, 6000],
    price: 0,
    time: 10,
  },
  {
    level: 4,
    accuracy: [0.59, 5700],
    stamina: [160, 6900],
    strength: [160, 12500],
    intellect: [54, 12000],
    price: 0,
    time: 10,
  },
  {
    level: 5,
    accuracy: [0.62, 11500],
    stamina: [180, 13000],
    strength: [180, 25000],
    intellect: [52, 23000],
    price: 0,
    time: 10,
  },
  {
    level: 6,
    accuracy: [0.65, 30000],
    stamina: [200, 36000],
    strength: [200, 65000],
    intellect: [50, 62000],
    price: 0,
    time: 10,
  },
  {
    level: 7,
    accuracy: [0.68, 90000],
    stamina: [220, 110000],
    strength: [220, 190000],
    intellect: [48, 180000],
    price: 0,
    time: 10,
  },
  {
    level: 8,
    accuracy: [0.71, 360000],
    stamina: [240, 430000],
    strength: [240, 780000],
    intellect: [46, 750000],
    price: 0,
    time: 10,
  },
  {
    level: 9,
    accuracy: [0.74, 1440000],
    stamina: [260, 1730000],
    strength: [260, 3130000],
    intellect: [44, 3000000],
    price: 0,
    time: 10,
  },
  {
    level: 10,
    accuracy: [0.77, 5700000],
    stamina: [280, 6900000],
    strength: [120, 3700],
    intellect: [42, 12000000],
    price: 0,
    time: 10,
  },

])

const onAddLevel = () => {
  const lastLevel = items.value[items.value.length - 1]
  items.value.push({
    level: lastLevel.level + 1,
    accuracy: [lastLevel.accuracy[0], lastLevel.accuracy[1]],
    stamina: [lastLevel.stamina[0], lastLevel.stamina[1]],
    strength: [lastLevel.strength[0], lastLevel.strength[1]],
    intellect: [lastLevel.intellect[0], lastLevel.intellect[1]],
    price: 0,
    time: 10,
  })
}

const totalPrices = computed(() => {
  const fields = ['accuracy', 'stamina', 'strength', 'intellect']
  const res = []
  for (const l of items.value) {
    if (l.level === 1) {
      res.push(0)
      continue
    }
    // @ts-ignore
    res.push(fields.reduce((acc, field) => Number(l[field][1]) + acc, 0))
  }
  return res
})

const times = computed(() => {
  const res = []
  for (const i in items.value) {
    if (!Number(i)) {
      res.push(0)
      continue
    }
    const curLevel = items.value[Number(i) - 1]
    const shots = totalPrices.value[Number(i)] / curLevel.strength[0] * 10 / curLevel.accuracy[0]
    res.push(Math.round(shots * curLevel.intellect[0] / curLevel.stamina[0]))
  }
  return res
})

const totalTime = computed(() => {
  return Math.round(times.value.reduce((acc, t) => acc + t))
})
</script>

<style>
.page {
}
</style>
