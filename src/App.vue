<template>
  <div class="min-h-screen bg-gradient-to-br from-orange-100 via-red-50 to-yellow-100 relative overflow-hidden" @click="handleGlobalClick">
    <!-- 更丰富的背景装饰元素 -->
    <div class="absolute inset-0 pointer-events-none">
      <!-- 四叶草装饰 -->
      <div v-for="i in 35" :key="`clover-${i}`"
           class="absolute text-red-300 animate-float"
           :style="{
             left: Math.random() * 100 + '%',
             top: Math.random() * 100 + '%',
             animationDelay: Math.random() * 3 + 's',
             fontSize: (Math.random() * 25 + 12) + 'px'
           }">
        🍀
      </div>

      <!-- 星星装饰 -->
      <div v-for="i in 25" :key="`star-${i}`"
           class="absolute text-yellow-300 animate-twinkle"
           :style="{
             left: Math.random() * 100 + '%',
             top: Math.random() * 100 + '%',
             animationDelay: Math.random() * 2 + 's'
           }">
        ✨
      </div>

      <!-- 飘动的炸弹 -->
      <div v-for="i in 15" :key="`bomb-${i}`"
           class="absolute text-orange-400 animate-float"
           :style="{
             left: Math.random() * 100 + '%',
             top: Math.random() * 100 + '%',
             animationDelay: Math.random() * 4 + 's',
             fontSize: (Math.random() * 18 + 10) + 'px'
           }">
        💣
      </div>

      <!-- 花朵装饰 -->
      <div v-for="i in 20" :key="`flower-${i}`"
           class="absolute animate-sway"
           :style="{
             left: Math.random() * 100 + '%',
             top: Math.random() * 100 + '%',
             animationDelay: Math.random() * 3 + 's',
             fontSize: (Math.random() * 20 + 15) + 'px',
             color: ['#ff6b9d', '#feca57', '#ff9ff3', '#54a0ff'][Math.floor(Math.random() * 4)]
           }">
        🌸
      </div>

      <!-- 蝴蝶装饰 -->
      <div v-for="i in 12" :key="`butterfly-${i}`"
           class="absolute animate-fly"
           :style="{
             left: Math.random() * 100 + '%',
             top: Math.random() * 100 + '%',
             animationDelay: Math.random() * 5 + 's',
             fontSize: (Math.random() * 15 + 10) + 'px'
           }">
        🦋
      </div>

      <!-- 气泡装饰 -->
      <div v-for="i in 18" :key="`bubble-${i}`"
           class="absolute animate-bubble"
           :style="{
             left: Math.random() * 100 + '%',
             top: Math.random() * 100 + '%',
             animationDelay: Math.random() * 4 + 's',
             fontSize: (Math.random() * 12 + 8) + 'px'
           }">
        💭
      </div>

      <!-- 点击特效 -->
      <div v-for="effect in clickEffects" :key="effect.id"
           class="absolute text-4xl animate-ping pointer-events-none z-50"
           :style="{ left: effect.x + 'px', top: effect.y + 'px' }">
        {{ effect.emoji }}
      </div>
    </div>

    <!-- 音乐控制和设置面板 -->
    <div class="absolute top-4 right-4 z-10 flex flex-col gap-2">
      <button @click="toggleMusic"
              class="bg-red-500 hover:bg-red-600 text-white p-3 rounded-full shadow-lg transition-all transform hover:scale-110">
        {{ isPlaying ? '🔊' : '🔇' }}
      </button>
      <button @click="toggleSettings"
              class="bg-orange-500 hover:bg-orange-600 text-white p-3 rounded-full shadow-lg transition-all transform hover:scale-110">
        ⚙️
      </button>
    </div>

    <!-- 设置面板 -->
    <div v-show="showSettings"
         class="absolute top-16 right-4 bg-white/90 backdrop-blur-sm rounded-2xl p-4 shadow-xl z-20 border-2 border-red-200">
      <h4 class="text-lg font-bold text-red-600 mb-3">可莉的设置</h4>
      <div class="space-y-2">
        <label class="flex items-center gap-2">
          <input v-model="autoPlay" type="checkbox" class="rounded">
          <span class="text-sm">自动播放音乐</span>
        </label>
        <label class="flex items-center gap-2">
          <input v-model="showEffects" type="checkbox" class="rounded">
          <span class="text-sm">显示特效</span>
        </label>
        <div class="flex items-center gap-2">
          <span class="text-sm">难度:</span>
          <select v-model="difficulty" class="rounded px-2 py-1 text-sm">
            <option value="easy">简单</option>
            <option value="normal">普通</option>
            <option value="hard">困难</option>
          </select>
        </div>
      </div>
    </div>

    <!-- 顶部状态栏 -->
    <div class="absolute top-4 left-4 z-10">
      <div class="bg-white/80 backdrop-blur-sm rounded-2xl px-4 py-2 shadow-lg border-2 border-yellow-200">
        <div class="flex items-center gap-4 text-sm">
          <div class="flex items-center gap-1">
            <span class="text-2xl">👧</span>
            <span class="font-bold text-red-600">可莉</span>
          </div>
          <div class="flex items-center gap-1">
            <span class="text-xl">🏆</span>
            <span class="font-bold text-orange-600">{{ highScore }}</span>
          </div>
          <div class="flex items-center gap-1">
            <span class="text-xl">🎯</span>
            <span class="font-bold text-blue-600">{{ totalCatch }}</span>
          </div>
        </div>
      </div>
    </div>

    <!-- 主内容区域 -->
    <div class="container mx-auto px-4 py-8 pt-20">
      <!-- 标题区域 -->
      <div class="text-center mb-8">
        <h1 class="text-6xl font-bold text-red-600 mb-4 animate-bounce">
          可莉的冒险世界
        </h1>
        <p class="text-2xl text-red-400 font-medium mb-2">
          "火花骑士，出发！"
        </p>
        <div class="flex justify-center gap-4 text-4xl">
          <span class="animate-bounce" style="animation-delay: 0.1s">🎈</span>
          <span class="animate-bounce" style="animation-delay: 0.2s">💣</span>
          <span class="animate-bounce" style="animation-delay: 0.3s">🍀</span>
          <span class="animate-bounce" style="animation-delay: 0.4s">✨</span>
        </div>
      </div>

      <!-- 可莉技能展示卡片 -->
      <div class="grid lg:grid-cols-3 gap-6 mb-8">
        <div class="bg-gradient-to-br from-red-200 to-pink-200 rounded-3xl p-6 shadow-xl border-4 border-red-300 transform hover:scale-105 transition-all">
          <div class="text-center">
            <div class="text-6xl mb-4">💣</div>
            <h3 class="text-2xl font-bold text-red-700 mb-2">蹦蹦炸弹</h3>
            <p class="text-red-600">可莉的招牌技能，让每次冒险都充满惊喜！</p>
          </div>
        </div>

        <div class="bg-gradient-to-br from-orange-200 to-yellow-200 rounded-3xl p-6 shadow-xl border-4 border-orange-300 transform hover:scale-105 transition-all">
          <div class="text-center">
            <div class="text-6xl mb-4">🔥</div>
            <h3 class="text-2xl font-bold text-orange-700 mb-2">爆裂火花</h3>
            <p class="text-orange-600">火元素的力量，点燃每一个快乐的瞬间！</p>
          </div>
        </div>

        <div class="bg-gradient-to-br from-yellow-200 to-amber-200 rounded-3xl p-6 shadow-xl border-4 border-yellow-300 transform hover:scale-105 transition-all">
          <div class="text-center">
            <div class="text-6xl mb-4">☀️</div>
            <h3 class="text-2xl font-bold text-yellow-700 mb-2">闪耀光芒</h3>
            <p class="text-yellow-600">如太阳般温暖，照亮所有人的心！</p>
          </div>
        </div>
      </div>

      <!-- 可莉介绍卡片 -->
      <div class="bg-white/80 backdrop-blur-sm rounded-3xl p-8 mb-8 shadow-xl border-4 border-red-200">
        <div class="flex flex-col lg:flex-row items-center gap-8">
          <!-- 可莉头像区域 -->
          <div class="flex-shrink-0">
            <div class="relative">
              <div class="w-64 h-64 bg-gradient-to-br from-red-400 to-orange-400 rounded-full flex items-center justify-center text-8xl animate-pulse shadow-2xl border-4 border-white">
                <img  src="./kell.jpg"/>
              </div>
              <!-- 环绕装饰 -->
              <div class="absolute -top-4 -left-4 text-3xl animate-spin-slow">💣</div>
              <div class="absolute -top-4 -right-4 text-3xl animate-bounce">🍀</div>
              <div class="absolute -bottom-4 -left-4 text-3xl animate-twinkle">✨</div>
              <div class="absolute -bottom-4 -right-4 text-3xl animate-pulse">🎈</div>
            </div>
          </div>

          <!-- 介绍文字 -->
          <div class="flex-1 text-center lg:text-left">
            <h2 class="text-4xl font-bold text-red-600 mb-4">可莉 - 逃跑的太阳</h2>
            <div class="grid grid-cols-1 md:grid-cols-2 gap-3 text-lg text-gray-700 mb-6">
              <div class="flex items-center gap-2">
                <span class="text-2xl">🎈</span>
                <span>蒙德城最活泼的小骑士</span>
              </div>
              <div class="flex items-center gap-2">
                <span class="text-2xl">💣</span>
                <span>西风骑士团的火花骑士</span>
              </div>
              <div class="flex items-center gap-2">
                <span class="text-2xl">🐟</span>
                <span>最爱炸鱼的小冒险家</span>
              </div>
              <div class="flex items-center gap-2">
                <span class="text-2xl">🍀</span>
                <span>充满好奇心的小天使</span>
              </div>
              <div class="flex items-center gap-2">
                <span class="text-2xl">🔥</span>
                <span>火元素神之眼持有者</span>
              </div>
              <div class="flex items-center gap-2">
                <span class="text-2xl">📚</span>
                <span>阿贝多的得意弟子</span>
              </div>
            </div>
            <div class="flex flex-wrap justify-center lg:justify-start gap-4">
              <button @click="startGame"
                      class="bg-red-500 hover:bg-red-600 text-white px-8 py-3 rounded-full font-bold text-xl shadow-lg transform hover:scale-105 transition-all">
                和可莉一起炸鱼！🐟💥
              </button>
              <button @click="showStats = !showStats"
                      class="bg-blue-500 hover:bg-blue-600 text-white px-6 py-3 rounded-full font-bold text-lg shadow-lg transform hover:scale-105 transition-all">
                查看战绩 📊
              </button>
            </div>
          </div>
        </div>
      </div>

      <!-- 统计面板 -->
      <div v-show="showStats"
           class="bg-gradient-to-r from-blue-200 to-purple-200 rounded-3xl p-6 mb-8 shadow-xl border-4 border-blue-300">
        <h3 class="text-3xl font-bold text-blue-800 mb-6 text-center">可莉的冒险记录</h3>
        <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
          <div class="bg-white/70 rounded-2xl p-4 text-center">
            <div class="text-4xl mb-2">🏆</div>
            <div class="text-2xl font-bold text-blue-700">{{ highScore }}</div>
            <div class="text-blue-600">最高分数</div>
          </div>
          <div class="bg-white/70 rounded-2xl p-4 text-center">
            <div class="text-4xl mb-2">🎯</div>
            <div class="text-2xl font-bold text-green-700">{{ totalCatch }}</div>
            <div class="text-green-600">总捕获数</div>
          </div>
          <div class="bg-white/70 rounded-2xl p-4 text-center">
            <div class="text-4xl mb-2">⏰</div>
            <div class="text-2xl font-bold text-purple-700">{{ gamesPlayed }}</div>
            <div class="text-purple-600">游戏次数</div>
          </div>
        </div>
      </div>

      <!-- 游戏区域 -->
      <div v-show="gameStarted"
           class="bg-gradient-to-b from-blue-300 to-blue-500 rounded-3xl p-8 shadow-xl border-4 border-blue-200 relative overflow-hidden">
        <div class="text-center mb-6">
          <h3 class="text-3xl font-bold text-white mb-2">可莉的炸鱼大作战！</h3>
          <div class="grid grid-cols-1 md:grid-cols-3 gap-4 text-xl text-blue-100">
            <div>得分: <span class="font-bold text-yellow-300">{{ score }}</span></div>
            <div>时间: <span class="font-bold text-yellow-300">{{ timeLeft }}s</span></div>
            <div>连击: <span class="font-bold text-orange-300">{{ combo }}x</span></div>
          </div>
        </div>

        <!-- 游戏池塘 -->
        <div class="relative h-96 bg-blue-400/50 rounded-2xl border-4 border-blue-300"
             @click="explodeBomb">

          <!-- 水波纹效果 -->
          <div class="absolute inset-0 opacity-30">
            <div v-for="i in 8" :key="`wave-${i}`"
                 class="absolute w-32 h-32 border-2 border-blue-300 rounded-full animate-ping"
                 :style="{
                   left: Math.random() * 70 + '%',
                   top: Math.random() * 70 + '%',
                   animationDelay: Math.random() * 3 + 's'
                 }" />
          </div>

          <!-- 鱼群 -->
          <div v-for="fish in fishes" :key="fish.id"
               class="absolute cursor-pointer transition-all duration-300 hover:scale-110 z-10"
               :style="{ left: fish.x + '%', top: fish.y + '%' }"
               @click.stop="catchFish(fish)">
            <div class="text-3xl animate-swim">{{ fish.emoji }}</div>
          </div>

          <!-- 爆炸效果 -->
          <div v-for="explosion in explosions" :key="explosion.id"
               class="absolute text-4xl animate-ping pointer-events-none z-20"
               :style="{ left: explosion.x + '%', top: explosion.y + '%' }">
            💥
          </div>

          <!-- 可莉在游戏中的状态 -->
          <div class="absolute bottom-4 left-4 text-6xl animate-bounce z-10">
            {{ kleeExpression }}
          </div>

          <!-- 连击提示 -->
          <div v-if="combo > 1"
               class="absolute top-4 right-4 bg-orange-500 text-white px-4 py-2 rounded-full font-bold text-xl animate-bounce z-10">
            {{ combo }}连击！
          </div>
        </div>

        <!-- 游戏结束界面 -->
        <div v-if="gameOver"
             class="absolute inset-0 bg-black/50 flex items-center justify-center rounded-3xl z-30">
          <div class="bg-white p-8 rounded-2xl text-center max-w-md">
            <h4 class="text-3xl font-bold text-red-600 mb-4">游戏结束！</h4>
            <div class="space-y-3 mb-6">
              <p class="text-xl text-gray-700">最终得分: <span class="font-bold text-red-600">{{ score }}分</span></p>
              <p class="text-lg text-blue-600">最大连击: <span class="font-bold">{{ maxCombo }}连击</span></p>
              <p class="text-lg text-green-600">捕获数量: <span class="font-bold">{{ fishCaught }}条鱼</span></p>
            </div>
            <p class="text-lg text-red-500 mb-6">{{ getScoreMessage() }}</p>
            <div class="flex gap-4 justify-center">
              <button @click="resetGame"
                      class="bg-red-500 hover:bg-red-600 text-white px-6 py-3 rounded-full font-bold">
                再来一次！
              </button>
              <button @click="shareScore"
                      class="bg-blue-500 hover:bg-blue-600 text-white px-6 py-3 rounded-full font-bold">
                分享成绩
              </button>
            </div>
          </div>
        </div>
      </div>

      <!-- 可莉的朋友们 -->
      <div class="mt-8 bg-gradient-to-r from-green-200 to-teal-200 rounded-3xl p-8 shadow-xl border-4 border-green-300">
        <h3 class="text-3xl font-bold text-green-800 mb-6 text-center">可莉的朋友们</h3>
        <div class="grid grid-cols-2 md:grid-cols-4 gap-6">
          <div class="bg-white/70 rounded-2xl p-4 text-center hover:bg-white/90 transition-all">
            <div class="text-4xl mb-2">🧪</div>
            <div class="font-bold text-green-700">阿贝多</div>
            <div class="text-sm text-green-600">温柔的老师</div>
          </div>
          <div class="bg-white/70 rounded-2xl p-4 text-center hover:bg-white/90 transition-all">
            <div class="text-4xl mb-2">⚔️</div>
            <div class="font-bold text-blue-700">琴</div>
            <div class="text-sm text-blue-600">代理团长</div>
          </div>
          <div class="bg-white/70 rounded-2xl p-4 text-center hover:bg-white/90 transition-all">
            <div class="text-4xl mb-2">🏹</div>
            <div class="font-bold text-purple-700">安柏</div>
            <div class="text-sm text-purple-600">侦察骑士</div>
          </div>
          <div class="bg-white/70 rounded-2xl p-4 text-center hover:bg-white/90 transition-all">
            <div class="text-4xl mb-2">🎵</div>
            <div class="font-bold text-cyan-700">温迪</div>
            <div class="text-sm text-cyan-600">吟游诗人</div>
          </div>
        </div>
      </div>

      <!-- 可莉语录轮播 -->
      <div class="mt-8 text-center">
        <div class="bg-gradient-to-r from-pink-200 to-rose-200 backdrop-blur-sm rounded-2xl p-6 shadow-lg border-4 border-pink-300">
          <div class="flex items-center justify-center mb-4">
            <span class="text-4xl mr-3">💬</span>
            <h4 class="text-2xl font-bold text-pink-800">可莉说</h4>
            <span class="text-4xl ml-3">💬</span>
          </div>
          <p class="text-xl text-red-600 font-medium animate-pulse min-h-[2rem]">
            "{{ currentQuote }}"
          </p>
          <div class="flex justify-center mt-4 gap-2">
            <div v-for="i in 3" :key="i"
                 class="w-2 h-2 rounded-full bg-red-400 animate-bounce"
                 :style="{ animationDelay: i * 0.2 + 's' }" />
          </div>
        </div>
      </div>
    </div>

    <!-- 背景音乐 -->
    <audio ref="bgMusic" loop>
      <!-- 音乐文件需要用户上传或使用可用的音频源 -->
      <source src="data:audio/mpeg;base64," type="audio/mpeg">
    </audio>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'

// 音乐控制
const isPlaying = ref(false)
const bgMusic = ref<HTMLAudioElement>()

// 设置相关
const showSettings = ref(false)
const autoPlay = ref(false)
const showEffects = ref(true)
const difficulty = ref('normal')

// 统计相关
const showStats = ref(false)
const highScore = ref(0)
const totalCatch = ref(0)
const gamesPlayed = ref(0)

// 游戏状态
const gameStarted = ref(false)
const score = ref(0)
const timeLeft = ref(60)
const gameOver = ref(false)
const combo = ref(0)
const maxCombo = ref(0)
const fishCaught = ref(0)
const fishes = ref<Array<{id: number, x: number, y: number, emoji: string}>>([])
const explosions = ref<Array<{id: number, x: number, y: number}>>([])
const clickEffects = ref<Array<{id: number, x: number, y: number, emoji: string}>>([])
const kleeExpression = ref('👧🎀')

// 可莉语录
const quotes = [
  "可莉要去炸鱼了！",
  "火花骑士，出发！",
  "嘿嘿，又炸到鱼了～",
  "可莉是最棒的骑士！",
  "一起去冒险吧！",
  "四叶草会带来好运哦～",
  "蹦蹦炸弹～轰轰～",
  "可莉想要去星落湖炸鱼！",
  "今天也要努力做个好孩子！",
  "可达鸭很可爱呢～",
  "可莉会保护大家的！",
  "炸弹是艺术！是艺术啦！",
  "让蒙德充满快乐吧～",
  "可莉最喜欢大家了！"
]
const currentQuote = ref('')

let gameTimer: number | null = null
let fishSpawner: number | null = null
let quoteTimer: number | null = null
let comboTimer: number | null = null

// 音乐控制
const toggleMusic = () => {
  if (!bgMusic.value) return

  if (isPlaying.value) {
    bgMusic.value.pause()
  } else {
    bgMusic.value.play().catch(console.error)
  }
  isPlaying.value = !isPlaying.value
}

// 设置控制
const toggleSettings = () => {
  showSettings.value = !showSettings.value
}

// 开始游戏
const startGame = () => {
  gameStarted.value = true
  score.value = 0
  timeLeft.value = 60
  gameOver.value = false
  combo.value = 0
  maxCombo.value = 0
  fishCaught.value = 0
  fishes.value = []
  explosions.value = []
  kleeExpression.value = '😊🎀'

  gamesPlayed.value++

  // 根据难度设置生成速度
  const spawnInterval = difficulty.value === 'easy' ? 2000 : difficulty.value === 'hard' ? 1000 : 1500

  // 开始计时
  gameTimer = setInterval(() => {
    timeLeft.value--
    if (timeLeft.value <= 0) {
      endGame()
    }
  }, 1000)

  // 生成鱼
  fishSpawner = setInterval(spawnFish, spawnInterval)
}

// 生成鱼
const spawnFish = () => {
  const fishEmojis = ['🐟', '🐠', '🐡', '🦈', '🐙', '🦐', '🐋', '🦑']
  const newFish = {
    id: Date.now() + Math.random(),
    x: Math.random() * 80 + 10,
    y: Math.random() * 70 + 10,
    emoji: fishEmojis[Math.floor(Math.random() * fishEmojis.length)]
  }
  fishes.value.push(newFish)

  // 根据难度设置鱼的存活时间
  const lifetime = difficulty.value === 'easy' ? 6000 : difficulty.value === 'hard' ? 3000 : 5000

  setTimeout(() => {
    const index = fishes.value.findIndex(f => f.id === newFish.id)
    if (index !== -1) {
      fishes.value.splice(index, 1)
      // 鱼逃跑时重置连击
      combo.value = 0
    }
  }, lifetime)
}

// 抓鱼
const catchFish = (fish: {id: number, x: number, y: number, emoji: string}) => {
  const index = fishes.value.findIndex(f => f.id === fish.id)
  if (index !== -1) {
    // 增加连击
    combo.value++
    maxCombo.value = Math.max(maxCombo.value, combo.value)
    fishCaught.value++
    totalCatch.value++

    // 重置连击计时器
    if (comboTimer) clearTimeout(comboTimer)
    comboTimer = setTimeout(() => {
      combo.value = 0
    }, 3000)

    // 根据鱼类型和连击计算分数
    let baseScore = 10
    if (fish.emoji === '🦈') baseScore = 20
    if (fish.emoji === '🐋') baseScore = 30
    const comboBonus = combo.value > 1 ? combo.value * 2 : 0
    score.value += baseScore + comboBonus

    // 创建爆炸效果
    const explosion = {
      id: Date.now() + Math.random(),
      x: fish.x,
      y: fish.y
    }
    explosions.value.push(explosion)

    // 移除鱼
    fishes.value.splice(index, 1)

    // 更新可莉表情
    kleeExpression.value = combo.value > 5 ? '🤩💥' : '😄💣'
    setTimeout(() => {
      kleeExpression.value = '😊🎀'
    }, 1000)

    // 移除爆炸效果
    setTimeout(() => {
      const expIndex = explosions.value.findIndex(e => e.id === explosion.id)
      if (expIndex !== -1) {
        explosions.value.splice(expIndex, 1)
      }
    }, 1000)
  }
}

// 爆炸攻击
const explodeBomb = (event: MouseEvent) => {
  const rect = (event.currentTarget as HTMLElement).getBoundingClientRect()
  const x = ((event.clientX - rect.left) / rect.width) * 100
  const y = ((event.clientY - rect.top) / rect.height) * 100

  // 创建大爆炸效果
  const explosion = {
    id: Date.now() + Math.random(),
    x,
    y
  }
  explosions.value.push(explosion)

  // 炸掉附近的鱼
  let bombCatch = 0
  fishes.value = fishes.value.filter(fish => {
    const distance = Math.sqrt(Math.pow(fish.x - x, 2) + Math.pow(fish.y - y, 2))
    if (distance < 25) {
      bombCatch++
      totalCatch.value++
      score.value += 15
      return false
    }
    return true
  })

  if (bombCatch > 0) {
    fishCaught.value += bombCatch
    combo.value += bombCatch
    maxCombo.value = Math.max(maxCombo.value, combo.value)
  }

  // 更新可莉表情
  kleeExpression.value = bombCatch > 2 ? '🤩💥' : bombCatch > 0 ? '😆💣' : '😅💭'
  setTimeout(() => {
    kleeExpression.value = '😊🎀'
  }, 1500)

  // 移除爆炸效果
  setTimeout(() => {
    const expIndex = explosions.value.findIndex(e => e.id === explosion.id)
    if (expIndex !== -1) {
      explosions.value.splice(expIndex, 1)
    }
  }, 1500)
}

// 结束游戏
const endGame = () => {
  gameOver.value = true
  kleeExpression.value = score.value > highScore.value ? '🥳🎉' : score.value > 100 ? '😊🌟' : '😅💫'

  // 更新最高分
  if (score.value > highScore.value) {
    highScore.value = score.value
    localStorage.setItem('kleeHighScore', highScore.value.toString())
  }

  // 保存统计数据
  localStorage.setItem('kleeTotalCatch', totalCatch.value.toString())
  localStorage.setItem('kleeGamesPlayed', gamesPlayed.value.toString())

  if (gameTimer) {
    clearInterval(gameTimer)
    gameTimer = null
  }
  if (fishSpawner) {
    clearInterval(fishSpawner)
    fishSpawner = null
  }
  if (comboTimer) {
    clearTimeout(comboTimer)
    comboTimer = null
  }
}

// 重置游戏
const resetGame = () => {
  gameStarted.value = false
  gameOver.value = false
  kleeExpression.value = '👧🎀'
  combo.value = 0
}

// 分享成绩
const shareScore = () => {
  const shareText = `我在可莉的炸鱼大作战中得了${score.value}分！最大连击${maxCombo.value}次！一起来挑战吧！`
  if (navigator.share) {
    navigator.share({
      title: '可莉的炸鱼大作战',
      text: shareText,
      url: window.location.href
    }).catch(console.error)
  } else if (navigator.clipboard) {
    navigator.clipboard.writeText(shareText).then(() => {
      alert('成绩已复制到剪贴板！')
    }).catch(console.error)
  }
}

// 获取分数评价
const getScoreMessage = () => {
  if (score.value >= 300) return "可莉：哇！你是真正的炸鱼大师！"
  if (score.value >= 200) return "可莉：太棒了！火花骑士称号给你！"
  if (score.value >= 150) return "可莉：太厉害了！可莉都惊呆了～"
  if (score.value >= 100) return "可莉：不错不错，继续加油！"
  if (score.value >= 50) return "可莉：还可以哦，多练习练习～"
  return "可莉：没关系，下次一定行的！"
}

// 随机切换语录
const changeQuote = () => {
  currentQuote.value = quotes[Math.floor(Math.random() * quotes.length)]
}

// 全局点击特效
const handleGlobalClick = (event: MouseEvent) => {
  if (!showEffects.value) return

  const effectEmojis = ['💥', '✨', '🌟', '💫', '🎉', '🎆', '💖', '🍀']
  const emoji = effectEmojis[Math.floor(Math.random() * effectEmojis.length)]

  const newEffect = {
    id: Date.now() + Math.random(),
    x: event.clientX - 20,
    y: event.clientY - 20,
    emoji
  }

  clickEffects.value.push(newEffect)

  // 1.5秒后移除特效
  setTimeout(() => {
    const index = clickEffects.value.findIndex(e => e.id === newEffect.id)
    if (index !== -1) {
      clickEffects.value.splice(index, 1)
    }
  }, 1500)
}

// 初始化数据
const loadSavedData = () => {
  const savedHighScore = localStorage.getItem('kleeHighScore')
  const savedTotalCatch = localStorage.getItem('kleeTotalCatch')
  const savedGamesPlayed = localStorage.getItem('kleeGamesPlayed')

  if (savedHighScore) highScore.value = parseInt(savedHighScore)
  if (savedTotalCatch) totalCatch.value = parseInt(savedTotalCatch)
  if (savedGamesPlayed) gamesPlayed.value = parseInt(savedGamesPlayed)
}

onMounted(() => {
  // 加载保存的数据
  loadSavedData()

  // 初始化语录
  changeQuote()

  // 定期切换语录
  quoteTimer = setInterval(changeQuote, 4000)

  // 设置音乐
  if (bgMusic.value) {
    bgMusic.value.volume = 0.3
  }
})

onUnmounted(() => {
  if (gameTimer) clearInterval(gameTimer)
  if (fishSpawner) clearInterval(fishSpawner)
  if (quoteTimer) clearInterval(quoteTimer)
  if (comboTimer) clearTimeout(comboTimer)
})
</script>

<style scoped>
@keyframes float {
  0%, 100% { transform: translateY(0px) rotate(0deg); }
  50% { transform: translateY(-20px) rotate(180deg); }
}

@keyframes twinkle {
  0%, 100% { opacity: 0.3; transform: scale(1); }
  50% { opacity: 1; transform: scale(1.2); }
}

@keyframes sway {
  0%, 100% { transform: rotate(-5deg); }
  50% { transform: rotate(5deg); }
}

@keyframes fly {
  0%, 100% { transform: translateX(0px) translateY(0px); }
  25% { transform: translateX(-10px) translateY(-15px); }
  50% { transform: translateX(10px) translateY(-10px); }
  75% { transform: translateX(-5px) translateY(-20px); }
}

@keyframes bubble {
  0% { transform: translateY(0px) scale(1); opacity: 0.7; }
  50% { transform: translateY(-30px) scale(1.1); opacity: 1; }
  100% { transform: translateY(-60px) scale(0.8); opacity: 0; }
}

@keyframes swim {
  0%, 100% { transform: translateX(0px) rotate(0deg); }
  50% { transform: translateX(10px) rotate(5deg); }
}

@keyframes spin-slow {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}

.animate-float {
  animation: float 4s ease-in-out infinite;
}

.animate-twinkle {
  animation: twinkle 2s ease-in-out infinite;
}

.animate-sway {
  animation: sway 3s ease-in-out infinite;
}

.animate-fly {
  animation: fly 6s ease-in-out infinite;
}

.animate-bubble {
  animation: bubble 5s ease-in-out infinite;
}

.animate-swim {
  animation: swim 2s ease-in-out infinite;
}

.animate-spin-slow {
  animation: spin-slow 8s linear infinite;
}
</style>
