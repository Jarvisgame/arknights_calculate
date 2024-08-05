<template>
  <div class="w-full h-screen justify-center bg-gray-100">
    <div class="w-full flex flex-col gap-6 items-center">
      <!-- 标题欢迎页面 -->
      <div class="text-2xl font-bold">欢迎来到宝石铭刻计算器！</div>
      <!-- 提供的材料 -->
      <div class="flex flex-col gap-2 border p-3 rounded-lg bg-white shadow-sm">
        <div class="text-lg font-bold">提供的原材料</div>
        <div v-for="yina in yinas">
          <div class="flex gap-3">
            <img :src="'/gem/' + yina + '.png'">
            <el-input-number v-model="materials[yina]" :min="0" :max="1000000"/>
          </div>
        </div>
        <div class="flex gap-3">
          <el-button @click="init">重置</el-button>
          <el-button @click="calculate">开始计算</el-button>
        </div>
      </div>
      <!-- 结果页面 -->
      <div class="flex flex-col gap-2 border p-3 rounded-lg bg-white shadow-sm">
        <div class="text-lg font-bold">预计得分</div>
        <div class="text-lg font-bold">{{ score }}</div>
        <div class="text-lg font-bold">{{ processes }}</div>
        <div class="text-lg font-bold">{{ materialsResult }}</div>
        <div></div>
      </div>
    </div>

  </div>
</template>

<script lang="ts" setup>

const yinas = ['nesre_iner_1', 'pet_iner_1', 'gabe_iner_1', 'shay_iner_1']

const materials = ref<Record<string, number>>({
  // 火焰伊纳
  nesre_iner_1: 0,
  nesre_iner_2: 0,
  nesre_iner_3: 0,
  nesre_iner_4: 0,
  nesre_iner_5: 0,
  // 天空伊纳
  pet_iner_1: 0,
  pet_iner_2: 0,
  pet_iner_3: 0,
  pet_iner_4: 0,
  // 草叶伊纳
  gabe_iner_1: 0,
  gabe_iner_2: 0,
  gabe_iner_3: 0,
  gabe_iner_4: 0,
  // 沙伊纳
  shay_iner_1: 0,
})

const materialsCalculate = ref<Record<string, number>>({
  // 火焰伊纳
  nesre_iner_1: 0,
  nesre_iner_2: 0,
  nesre_iner_3: 0,
  nesre_iner_4: 0,
  nesre_iner_5: 0,
  // 天空伊纳
  pet_iner_1: 0,
  pet_iner_2: 0,
  pet_iner_3: 0,
  pet_iner_4: 0,
  // 草叶伊纳
  gabe_iner_1: 0,
  gabe_iner_2: 0,
  gabe_iner_3: 0,
  gabe_iner_4: 0,
  // 沙伊纳
  shay_iner_1: 0,
})

const materialsResult = ref<Record<string, number>>({
  // 火焰伊纳
  nesre_iner_1: 0,
  nesre_iner_2: 0,
  nesre_iner_3: 0,
  nesre_iner_4: 0,
  nesre_iner_5: 0,
  // 天空伊纳
  pet_iner_1: 0,
  pet_iner_2: 0,
  pet_iner_3: 0,
  pet_iner_4: 0,
  // 草叶伊纳
  gabe_iner_1: 0,
  gabe_iner_2: 0,
  gabe_iner_3: 0,
  gabe_iner_4: 0,
  // 沙伊纳
  shay_iner_1: 0,
})

// 计算的最终得分
const score = ref(0)
const scoreMax = ref(0)
// 高级交糅3成立标志
const jr3_success = ref(false)

const process: Record<string,Function> = reactive({
  // 高级淬雕
  cd1: (m: Record<string, number>) => {
    m.nesre_iner_2 += m.nesre_iner_1 * 2
    m.nesre_iner_1 = 0
    return m
  },
  cd2: (m: Record<string, number>) => {
    m.nesre_iner_3 += m.nesre_iner_2 * 2
    m.nesre_iner_2 = 0
    return m
  },
  cd3: (m: Record<string, number>) => {
    m.nesre_iner_4 += Math.round(m.nesre_iner_3 * 2.4)
    m.nesre_iner_3 = 0
    return m
  },
  cd4: (m: Record<string, number>) => {
    m.nesre_iner_5 += m.nesre_iner_4
    m.nesre_iner_4 = 0
    return m
  },
  // 高级滤纯
  lc1: (m: Record<string, number>) => {
    m.gabe_iner_2 += m.gabe_iner_1
    m.gabe_iner_1 = 0
    return m
  },
  lc2: (m: Record<string, number>) => {
    m.gabe_iner_3 += Math.round(m.gabe_iner_2 *0.8)
    m.shay_iner_1 += Math.round(m.gabe_iner_2 *0.2)
    m.gabe_iner_2 = 0
    return m
  },
  lc3: (m: Record<string, number>) => {
    m.gabe_iner_4 += Math.ceil(m.gabe_iner_3 *0.7)
    m.shay_iner_1 += Math.floor(m.gabe_iner_3 *0.3)
    m.gabe_iner_3 = 0
    return m
  },
  // 高级交糅
  jr1: (m: Record<string, number>) => {
    const tempmin = Math.min(m.shay_iner_1,m.pet_iner_1)
    m.pet_iner_2 += tempmin
    m.shay_iner_1 -= tempmin
    m.pet_iner_1 -= tempmin
    return m
  },
  jr2: (m: Record<string, number>) => {
    const tempmin = Math.min(m.gabe_iner_2,m.pet_iner_2)
    m.pet_iner_3 += tempmin
    m.gabe_iner_2 -= tempmin
    m.pet_iner_2 -= tempmin
    return m
  },
  jr3: (m: Record<string, number>) => {
    if(m.nesre_iner_3 === m.pet_iner_3) {
      jr3_success.value = true
    }
    const tempmin = Math.min(m.nesre_iner_3,m.pet_iner_3)
    m.pet_iner_4 += tempmin
    m.nesre_iner_3 -= tempmin
    m.pet_iner_3 -= tempmin
    return m
  },
  // 高级落晶
  lj1: (m: Record<string, number>) => {
    m.shay_iner_1 = m.shay_iner_1 * 5
    return m
  },
  lj2: (m: Record<string, number>) => {
    m.shay_iner_1 = m.shay_iner_1 * 8
    return m
  },
  lj3: (m: Record<string, number>) => {
    m.shay_iner_1 = m.shay_iner_1 * 9
    return m
  }
})

// 生成所有可能的组合
function generateCombinations(arr: string[], comboLength: number): string[][] {
  if (comboLength === 0) return [[]]
  if (arr.length < comboLength) return []

  const combos: string[][] = []
  for (let i = 0; i <= arr.length - comboLength; i++) {
    const smallerCombos = generateCombinations(arr.slice(i + 1), comboLength - 1)
    for (let j = 0; j < smallerCombos.length; j++) {
      combos.push([arr[i], ...smallerCombos[j]])
    }
  }
  return combos
}

// 生成所有可能的排列
function generatePermutations(arr: string[]): string[][] {
  if (arr.length === 0) return [[]];
  const perms: string[][] = [];
  for (let i = 0; i < arr.length; i++) {
    const remainingElements = [...arr.slice(0, i), ...arr.slice(i + 1)];
    const subPerms = generatePermutations(remainingElements);
    for (let j = 0; j < subPerms.length; j++) {
      perms.push([arr[i], ...subPerms[j]]);
    }
  }
  return perms;
}

// 获取所有功能函数的名字
const processNames = Object.keys(process).filter(name => !['cd1', 'cd2', 'cd3'].includes(name))

// 所有可能的6个功能的组合
const allCombinations = generateCombinations(processNames, 6)

// 对每种组合生成所有可能的排列
const allPermutations = allCombinations.flatMap(combo => generatePermutations(combo));

const processes = ref<string[]>([])

// 初始化和重置
function init() {
  // 重置原材料数量
  Object.keys(materials.value).forEach((key) => {
    materials.value[key] = 0
    materialsCalculate.value[key] = 0
    materialsResult.value[key] = 0
  })
  // 重置工艺流程
  processes.value = []
  // 得分归零
  scoreMax.value = 0
  score.value = 0
  jr3_success.value = false
}

function calculate() {
  // 测试
  allPermutations.forEach((combo) => {
    jr3_success.value = false
    materialsCalculate.value = { ...materials.value}
    // 优先执行高级淬雕1、2、3
    materialsCalculate.value = process.cd1(materialsCalculate.value)
    materialsCalculate.value = process.cd2(materialsCalculate.value)
    materialsCalculate.value = process.cd3(materialsCalculate.value)
    combo.forEach((funcName) => {
      materialsCalculate.value = process[funcName](materialsCalculate.value)
    })
    if(JSON.stringify(combo) === JSON.stringify([ "lc1", "lc2", "lc3", "lj1", "lj2", "lj3" ])) {
      const result = materialsCalculate.value
      console.log(result)
      console.log(calculate_score(materialsCalculate.value, combo))
      console.log(combo)
    }
    // 最大得分
    if(calculate_score(materialsCalculate.value, combo) > scoreMax.value) {
      scoreMax.value = calculate_score(materialsCalculate.value, combo)
      processes.value = combo
      materialsResult.value = materialsCalculate.value
    }
  })
  score.value = scoreMax.value
}

function calculate_score(m: Record<string, number>, combo: string[]) {
  let tempscore = m.nesre_iner_1 * 1 + 
    m.nesre_iner_2 * 2 + 
    m.nesre_iner_3 * 10 + 
    m.nesre_iner_4 * 35 + 
    m.nesre_iner_5 * 85 + 
    m.pet_iner_1 * 1 + 
    m.pet_iner_2 * 3 +
    m.pet_iner_3 * 22 + 
    m.pet_iner_4 * 105 + 
    m.gabe_iner_1 * 1 +
    m.gabe_iner_2 * 5 +
    m.gabe_iner_3 * 50 +
    m.gabe_iner_4 * 500 +
    m.shay_iner_1 * 1
  // 高级交糅1
  if(combo.includes('jr1')) {
    tempscore += (m.pet_iner_1 + m.pet_iner_2 + m.pet_iner_3 +m.pet_iner_4) * 5
  }
  // 高级交糅2
  if(combo.includes('jr2')) {
    tempscore += (m.pet_iner_1 + m.pet_iner_2 + m.pet_iner_3 +m.pet_iner_4) * 15
  }
  // 高级交糅3
  if(combo.includes('jr2') && jr3_success.value) {
    tempscore += m.pet_iner_4 * 100
  }
  // 高级落晶3
  if(combo.includes('lj3')) {
    tempscore += m.shay_iner_1
  }
  return tempscore
}
</script>