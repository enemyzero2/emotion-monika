<template>
    <div 
      class="min-h-screen p-6 overflow-hidden relative transition-all duration-1000"
      :style="{ background: `linear-gradient(135deg, ${currentEmotionTheme.gradientStart}40, ${currentEmotionTheme.gradientEnd}30)` }"
    >
      <!-- Subtle Background Pattern -->
      <div class="absolute inset-0 opacity-5 pointer-events-none z-0">
        <svg width="100%" height="100%">
          <pattern id="subtle-pattern" width="40" height="40" patternUnits="userSpaceOnUse">
            <path 
              d="M0 20 L40 20 M20 0 L20 40" 
              :stroke="currentEmotionTheme.primary" 
              stroke-width="0.5" 
              fill="none"
              class="transition-all duration-1000"
            />
          </pattern>
          <rect width="100%" height="100%" fill="url(#subtle-pattern)" />
        </svg>
      </div>
      
      <!-- Background Heart Rate Monitor -->
      <div class="absolute inset-0 opacity-15 pointer-events-none z-0 overflow-hidden">
        <svg class="w-full h-full" viewBox="0 0 1000 600">
          <path 
            :d="backgroundHeartbeatPath" 
            fill="none" 
            :stroke="currentEmotionTheme.primary" 
            stroke-width="6"
            class="transition-all duration-1000"
          />
        </svg>
      </div>
      
      <!-- Floating Hearts Background -->
      <div class="absolute inset-0 pointer-events-none z-0 overflow-hidden">
        <div 
          v-for="(heart, index) in floatingHearts" 
          :key="index"
          class="absolute text-2xl animate-float"
          :style="{
            left: `${heart.x}%`,
            bottom: `-5%`,
            animationDuration: `${heart.duration}s`,
            animationDelay: `${heart.delay}s`,
            opacity: heart.opacity,
            transform: `scale(${heart.scale})`,
            color: heart.color
          }"
        >
          {{ heart.emoji }}
        </div>
      </div>
  
      <div class="max-w-6xl mx-auto relative z-10">
        <header class="text-center mb-8">
          <h1 
            class="text-4xl font-bold bg-clip-text text-transparent transition-all duration-1000"
            :style="{ backgroundImage: `linear-gradient(to right, ${currentEmotionTheme.textGradientStart}, ${currentEmotionTheme.textGradientEnd})` }"
          >
            情感共鸣系统
          </h1>
          <p :style="{ color: currentEmotionTheme.text }" class="mt-2 transition-colors duration-1000">
            可视化你的心灵景观
          </p>
        </header>
  
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
          <!-- Emotional State Machine -->
          <div 
            class="backdrop-blur-md rounded-xl p-4 border transition-all duration-1000"
            :style="{ 
              backgroundColor: `${currentEmotionTheme.cardBg}`,
              borderColor: currentEmotionTheme.borderColor,
              boxShadow: `0 4px 20px ${currentEmotionTheme.primary}20`
            }"
          >
            <h2 
              class="text-xl font-semibold mb-4 flex items-center transition-colors duration-1000"
              :style="{ color: currentEmotionTheme.primary }"
            >
              <Heart class="w-5 h-5 mr-2" />
              情感状态
            </h2>
            <div class="relative h-48 overflow-hidden rounded-lg">
              <div class="absolute inset-0 flex items-center justify-center">
                <div class="text-6xl font-bold text-center">{{ currentEmotion.emoji }}</div>
              </div>
              <div class="absolute bottom-0 left-0 right-0 bg-gradient-to-t from-black/70 to-transparent p-3">
                <div class="text-center">
                  <span class="text-lg font-medium">{{ currentEmotion.name }}</span>
                  <div class="w-full bg-black/20 rounded-full h-2 mt-2">
                    <div 
                      class="h-2 rounded-full transition-all duration-500"
                      :style="{ 
                        width: `${currentEmotion.intensity}%`,
                        backgroundColor: currentEmotionTheme.primary 
                      }"
                    ></div>
                  </div>
                </div>
              </div>
            </div>
            <div class="mt-4 flex justify-between">
              <button 
                v-for="emotion in emotions" 
                :key="emotion.id"
                @click="triggerEmotion(emotion.id)"
                class="p-2 rounded-full transition-all duration-300"
                :class="currentEmotionId === emotion.id ? 'ring-2 scale-110' : 'hover:bg-white/10'"
                :style="currentEmotionId === emotion.id ? { ringColor: currentEmotionTheme.primary } : {}"
                :title="emotion.name"
              >
                {{ emotion.emoji }}
              </button>
            </div>
          </div>
  
          <!-- Heart Rate Monitor -->
          <div 
            class="backdrop-blur-md rounded-xl p-4 border transition-all duration-1000"
            :style="{ 
              backgroundColor: `${currentEmotionTheme.cardBg}`,
              borderColor: currentEmotionTheme.borderColor,
              boxShadow: `0 4px 20px ${currentEmotionTheme.primary}20`
            }"
          >
            <h2 
              class="text-xl font-semibold mb-4 flex items-center transition-colors duration-1000"
              :style="{ color: currentEmotionTheme.primary }"
            >
              <Activity class="w-5 h-5 mr-2" />
              心跳节奏
            </h2>
            <div class="relative h-48 flex items-center justify-center">
              <svg class="w-full h-32" viewBox="0 0 400 100">
                <path 
                  :d="heartbeatPath" 
                  fill="none" 
                  :stroke="currentEmotionTheme.primary" 
                  stroke-width="2.5"
                  class="transition-all duration-1000"
                />
                <!-- 添加网格线，模拟医疗心电图 -->
                <g class="ecg-grid" stroke="#9ca3af20">
                  <!-- 水平网格线 -->
                  <line v-for="i in 10" :key="`h-${i}`" x1="0" :y1="i * 10" x2="400" :y2="i * 10" stroke-width="0.5" />
                  <!-- 垂直网格线 -->
                  <line v-for="i in 40" :key="`v-${i}`" :x1="i * 10" y1="0" :x2="i * 10" y2="100" stroke-width="0.5" />
                  <!-- 加粗的主网格线 -->
                  <line v-for="i in 2" :key="`h-bold-${i}`" x1="0" :y1="i * 50" x2="400" :y2="i * 50" stroke-width="1" />
                  <line v-for="i in 8" :key="`v-bold-${i}`" :x1="i * 50" y1="0" :x2="i * 50" y2="100" stroke-width="1" />
                </g>
              </svg>
              <div 
                class="absolute bottom-4 right-4 px-3 py-1 rounded-full flex items-center transition-colors duration-1000"
                :style="{ 
                  backgroundColor: currentEmotionTheme.accentBg,
                  color: currentEmotionTheme.primary 
                }"
              >
                <Heart class="w-4 h-4 mr-1 animate-pulse" />
                {{ heartRate }} BPM
              </div>
            </div>
            <div class="mt-2">
              <label 
                class="block text-sm mb-1 transition-colors duration-1000"
                :style="{ color: currentEmotionTheme.text }"
              >
                情感敏感度
              </label>
              <input 
                type="range" 
                min="1" 
                max="10" 
                v-model="sensitivity" 
                class="w-full"
                :style="{ accentColor: currentEmotionTheme.primary }"
              />
            </div>
          </div>
  
          <!-- Emotional Composition -->
          <div 
            class="backdrop-blur-md rounded-xl p-4 border transition-all duration-1000"
            :style="{ 
              backgroundColor: `${currentEmotionTheme.cardBg}`,
              borderColor: currentEmotionTheme.borderColor,
              boxShadow: `0 4px 20px ${currentEmotionTheme.primary}20`
            }"
          >
            <h2 
              class="text-xl font-semibold mb-4 flex items-center transition-colors duration-1000"
              :style="{ color: currentEmotionTheme.primary }"
            >
              <PieChart class="w-5 h-5 mr-2" />
              情感构成
            </h2>
            <div class="relative h-48 flex items-center justify-center">
              <div class="w-32 h-32 rounded-full relative">
                <div 
                  v-for="(segment, index) in emotionalComposition" 
                  :key="index"
                  class="absolute top-0 left-0 w-full h-full transition-all duration-1000"
                  :style="{
                    background: segment.color,
                    clipPath: `conic-polygon(50% 50%, 50% 0%, ${getConicPolygonPath(segment.startAngle, segment.endAngle)})`
                  }"
                ></div>
                <div class="absolute inset-0 flex items-center justify-center">
                  <div 
                    class="w-16 h-16 rounded-full backdrop-blur-sm flex items-center justify-center transition-colors duration-1000"
                    :style="{ backgroundColor: currentEmotionTheme.accentBg }"
                  >
                    <div class="text-xs text-center">
                      <div>平衡度</div>
                      <div class="font-bold text-lg">{{ emotionalBalance }}%</div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <div class="mt-2 grid grid-cols-2 gap-2 text-sm">
              <div v-for="(segment, index) in emotionalComposition" :key="index" class="flex items-center">
                <div class="w-3 h-3 rounded-full mr-2 transition-all duration-1000" :style="{ background: segment.color }"></div>
                <span>{{ segment.name }}: {{ segment.value }}%</span>
              </div>
            </div>
          </div>
  
          <!-- Emotional Memory & Journal -->
          <div 
            class="backdrop-blur-md rounded-xl p-4 border md:col-span-2 lg:col-span-2 transition-all duration-1000"
            :style="{ 
              backgroundColor: `${currentEmotionTheme.cardBg}`,
              borderColor: currentEmotionTheme.borderColor,
              boxShadow: `0 4px 20px ${currentEmotionTheme.primary}20`
            }"
          >
            <div class="flex justify-between items-center mb-4">
              <h2 
                class="text-xl font-semibold flex items-center transition-colors duration-1000"
                :style="{ color: currentEmotionTheme.primary }"
              >
                <BookOpen class="w-5 h-5 mr-2" />
                情感记忆
              </h2>
              <div class="flex space-x-2">
                <button 
                  @click="activeTab = 'memories'"
                  class="px-3 py-1 text-sm rounded-full transition-all duration-300"
                  :style="activeTab === 'memories' ? 
                    { backgroundColor: currentEmotionTheme.primary, color: currentEmotionTheme.buttonText } : 
                    { backgroundColor: currentEmotionTheme.buttonBg, color: currentEmotionTheme.text }"
                >
                  记忆
                </button>
                <button 
                  @click="activeTab = 'journal'"
                  class="px-3 py-1 text-sm rounded-full transition-all duration-300"
                  :style="activeTab === 'journal' ? 
                    { backgroundColor: currentEmotionTheme.primary, color: currentEmotionTheme.buttonText } : 
                    { backgroundColor: currentEmotionTheme.buttonBg, color: currentEmotionTheme.text }"
                >
                  日记
                </button>
                <button 
                  @click="activeTab = 'trends'"
                  class="px-3 py-1 text-sm rounded-full transition-all duration-300"
                  :style="activeTab === 'trends' ? 
                    { backgroundColor: currentEmotionTheme.primary, color: currentEmotionTheme.buttonText } : 
                    { backgroundColor: currentEmotionTheme.buttonBg, color: currentEmotionTheme.text }"
                >
                  趋势
                </button>
              </div>
            </div>
            
            <!-- Memories Tab -->
            <div v-if="activeTab === 'memories'" class="h-48 overflow-y-auto pr-2 custom-scrollbar">
              <div v-for="(memory, index) in emotionalMemories" :key="index" class="mb-3 flex">
                <div class="mr-3 flex-shrink-0">
                  <div class="w-10 h-10 rounded-full flex items-center justify-center" :class="memory.bgColor">
                    {{ memory.emoji }}
                  </div>
                </div>
                <div>
                  <div class="flex items-center">
                    <span class="font-medium">{{ memory.emotion }}</span>
                    <span 
                      class="text-xs ml-2 transition-colors duration-1000"
                      :style="{ color: currentEmotionTheme.secondaryText }"
                    >
                      {{ memory.time }}
                    </span>
                  </div>
                  <p 
                    class="text-sm transition-colors duration-1000"
                    :style="{ color: currentEmotionTheme.secondaryText }"
                  >
                    {{ memory.description }}
                  </p>
                </div>
              </div>
            </div>
            
            <!-- Journal Tab -->
            <div v-if="activeTab === 'journal'" class="h-48">
              <textarea 
                v-model="journalEntry" 
                class="w-full h-full border rounded-lg p-3 resize-none transition-colors duration-1000"
                :style="{ 
                  backgroundColor: currentEmotionTheme.inputBg, 
                  color: currentEmotionTheme.text,
                  borderColor: currentEmotionTheme.borderColor
                }"
                placeholder="Write your thoughts and feelings here..."
              ></textarea>
            </div>
            
            <!-- Trends Tab -->
            <div v-if="activeTab === 'trends'" class="h-48 flex items-center justify-center">
              <div class="w-full h-full relative">
                <svg class="w-full h-full" viewBox="0 0 400 150">
                  <!-- Axis -->
                  <line x1="40" y1="130" x2="380" y2="130" stroke="#9ca3af" stroke-width="1" />
                  <line x1="40" y1="20" x2="40" y2="130" stroke="#9ca3af" stroke-width="1" />
                  
                  <!-- Emotion trend lines -->
                  <path 
                    :d="getTrendPath('love')" 
                    fill="none" 
                    stroke="#ec4899" 
                    stroke-width="2"
                    class="transition-all duration-1000"
                  />
                  <path 
                    :d="getTrendPath('joy')" 
                    fill="none" 
                    stroke="#facc15" 
                    stroke-width="2"
                    class="transition-all duration-1000"
                  />
                  <path 
                    :d="getTrendPath('calm')" 
                    fill="none" 
                    stroke="#60a5fa" 
                    stroke-width="2"
                    class="transition-all duration-1000"
                  />
                  
                  <!-- Labels -->
                  <text x="20" y="20" :fill="currentEmotionTheme.secondaryText" font-size="10">High</text>
                  <text x="20" y="130" :fill="currentEmotionTheme.secondaryText" font-size="10">Low</text>
                  <text x="40" y="145" :fill="currentEmotionTheme.secondaryText" font-size="10">7 days ago</text>
                  <text x="360" y="145" :fill="currentEmotionTheme.secondaryText" font-size="10">Today</text>
                  
                  <!-- Legend -->
                  <circle cx="300" cy="20" r="4" fill="#ec4899" />
                  <text x="310" y="23" fill="#ec4899" font-size="10">Love</text>
                  <circle cx="300" cy="35" r="4" fill="#facc15" />
                  <text x="310" y="38" fill="#facc15" font-size="10">Joy</text>
                  <circle cx="300" cy="50" r="4" fill="#60a5fa" />
                  <text x="310" y="53" fill="#60a5fa" font-size="10">Calm</text>
                </svg>
              </div>
            </div>
            
            <div class="mt-4">
              <div class="flex">
                <input 
                  v-if="activeTab === 'memories'"
                  v-model="newMemory" 
                  type="text" 
                  placeholder="Record a new emotional memory..." 
                  class="flex-grow border rounded-l-lg px-3 py-2 transition-colors duration-1000"
                  :style="{ 
                    backgroundColor: currentEmotionTheme.inputBg, 
                    color: currentEmotionTheme.text,
                    borderColor: currentEmotionTheme.borderColor
                  }"
                />
                <button 
                  v-if="activeTab === 'memories'"
                  @click="addMemory" 
                  class="px-4 rounded-r-lg flex items-center justify-center transition-colors duration-1000"
                  :style="{ 
                    backgroundColor: currentEmotionTheme.primary,
                    color: currentEmotionTheme.buttonText
                  }"
                >
                  <Send class="w-5 h-5" />
                </button>
                <button 
                  v-if="activeTab === 'journal'"
                  @click="saveJournalEntry" 
                  class="w-full px-4 py-2 rounded-lg flex items-center justify-center transition-colors duration-1000"
                  :style="{ 
                    backgroundColor: currentEmotionTheme.primary,
                    color: currentEmotionTheme.buttonText
                  }"
                >
                  <Save class="w-5 h-5 mr-2" />
                  Save Entry
                </button>
              </div>
            </div>
          </div>
  
          <!-- Emotional Triggers & Customization -->
          <div 
            class="backdrop-blur-md rounded-xl p-4 border transition-all duration-1000"
            :style="{ 
              backgroundColor: `${currentEmotionTheme.cardBg}`,
              borderColor: currentEmotionTheme.borderColor,
              boxShadow: `0 4px 20px ${currentEmotionTheme.primary}20`
            }"
          >
            <div class="flex justify-between items-center mb-4">
              <h2 
                class="text-xl font-semibold flex items-center transition-colors duration-1000"
                :style="{ color: currentEmotionTheme.primary }"
              >
                <Zap class="w-5 h-5 mr-2" />
                情感触发
              </h2>
              <button 
                @click="showCustomizeTriggers = !showCustomizeTriggers"
                class="text-xs px-2 py-1 rounded-full transition-colors duration-1000"
                :style="{ 
                  backgroundColor: currentEmotionTheme.buttonBg,
                  color: currentEmotionTheme.text
                }"
              >
                {{ showCustomizeTriggers ? 'Done' : 'Customize' }}
              </button>
            </div>
            
            <div v-if="!showCustomizeTriggers" class="h-48 overflow-y-auto pr-2 custom-scrollbar">
              <div v-for="(trigger, index) in emotionalTriggers" :key="index" class="mb-2">
                <div 
                  class="p-3 rounded-lg cursor-pointer transition-all hover:translate-x-1"
                  :style="trigger.active ? 
                    { 
                      background: `linear-gradient(to right, ${currentEmotionTheme.primaryTransparent}, ${currentEmotionTheme.secondaryTransparent})`,
                      borderLeft: `4px solid ${currentEmotionTheme.primary}`
                    } : 
                    { backgroundColor: currentEmotionTheme.buttonBg }"
                  @click="toggleTrigger(index)"
                >
                  <div class="flex justify-between items-center">
                    <div class="flex items-center">
                      <span class="mr-2">{{ trigger.emoji }}</span>
                      <span>{{ trigger.name }}</span>
                    </div>
                    <div 
                      class="w-10 h-5 rounded-full relative transition-colors duration-300"
                      :style="{ backgroundColor: trigger.active ? currentEmotionTheme.primary : '#374151' }"
                    >
                      <div 
                        class="absolute w-4 h-4 rounded-full bg-white top-0.5 transition-all"
                        :style="{ left: trigger.active ? '1.5rem' : '0.25rem' }"
                      ></div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            
            <div v-else class="h-48 overflow-y-auto pr-2 custom-scrollbar">
              <div 
                v-for="(trigger, index) in emotionalTriggers" 
                :key="index" 
                class="mb-2 p-3 rounded-lg transition-colors duration-1000"
                :style="{ backgroundColor: currentEmotionTheme.buttonBg }"
              >
                <div class="flex items-center mb-2">
                  <input 
                    v-model="trigger.emoji" 
                    class="w-10 border-b text-center focus:outline-none transition-colors duration-1000"
                    :style="{ 
                      backgroundColor: 'transparent',
                      borderColor: currentEmotionTheme.borderColor,
                      color: currentEmotionTheme.text
                    }"
                  />
                  <input 
                    v-model="trigger.name" 
                    class="ml-2 flex-grow border-b focus:outline-none transition-colors duration-1000"
                    :style="{ 
                      backgroundColor: 'transparent',
                      borderColor: currentEmotionTheme.borderColor,
                      color: currentEmotionTheme.text
                    }"
                  />
                </div>
                <div class="flex items-center">
                  <span 
                    class="text-xs mr-2 transition-colors duration-1000"
                    :style="{ color: currentEmotionTheme.secondaryText }"
                  >
                    Linked emotion:
                  </span>
                  <select 
                    v-model="trigger.emotion" 
                    class="border rounded px-2 py-1 text-sm transition-colors duration-1000"
                    :style="{ 
                      backgroundColor: currentEmotionTheme.inputBg,
                      borderColor: currentEmotionTheme.borderColor,
                      color: currentEmotionTheme.text
                    }"
                  >
                    <option v-for="emotion in emotions" :key="emotion.id" :value="emotion.id">
                      {{ emotion.name }}
                    </option>
                  </select>
                </div>
              </div>
              <button 
                @click="addNewTrigger" 
                class="w-full mt-2 py-2 rounded-lg transition-colors duration-1000 flex items-center justify-center"
                :style="{ 
                  backgroundColor: currentEmotionTheme.buttonBg,
                  color: currentEmotionTheme.text
                }"
              >
                <PlusCircle class="w-4 h-4 mr-2" />
                Add New Trigger
              </button>
            </div>
          </div>
        </div>
  
        <!-- Emotional Expression & Support -->
        <div class="mt-6 grid grid-cols-1 md:grid-cols-3 gap-6">
          <div 
            class="backdrop-blur-md rounded-xl p-4 border md:col-span-2 transition-all duration-1000"
            :style="{ 
              backgroundColor: `${currentEmotionTheme.cardBg}`,
              borderColor: currentEmotionTheme.borderColor,
              boxShadow: `0 4px 20px ${currentEmotionTheme.primary}20`
            }"
          >
            <h2 
              class="text-xl font-semibold mb-4 flex items-center transition-colors duration-1000"
              :style="{ color: currentEmotionTheme.primary }"
            >
              <MessageSquare class="w-5 h-5 mr-2" />
              情感表达
            </h2>
            <div 
              class="h-24 rounded-lg p-3 overflow-hidden relative transition-colors duration-1000"
              :style="{ backgroundColor: currentEmotionTheme.inputBg }"
            >
              <div class="animate-typing-effect">
                <p class="text-lg">{{ currentExpression }}</p>
              </div>
              <div 
                class="absolute bottom-0 left-0 right-0 h-8 bg-gradient-to-t transition-colors duration-1000"
                :style="{ backgroundImage: `linear-gradient(to top, ${currentEmotionTheme.cardBg}, transparent)` }"
              ></div>
            </div>
            <div class="mt-4 flex flex-wrap gap-2">
              <button 
                v-for="(expression, index) in expressionTypes" 
                :key="index"
                @click="generateExpression(expression.type)"
                class="px-3 py-1 rounded-full text-sm transition-all duration-300"
                :style="expressionType === expression.type ? 
                  { backgroundColor: currentEmotionTheme.primary, color: currentEmotionTheme.buttonText } : 
                  { backgroundColor: currentEmotionTheme.buttonBg, color: currentEmotionTheme.text }"
              >
                {{ expression.name }}
              </button>
              <button 
                @click="playMusic"
                class="px-3 py-1 rounded-full text-sm flex items-center transition-colors duration-1000"
                :style="{ 
                  backgroundColor: currentEmotionTheme.buttonBg,
                  color: currentEmotionTheme.text
                }"
              >
                <Music class="w-4 h-4 mr-1" />
                {{ isPlaying ? 'Stop Music' : 'Play Music' }}
              </button>
            </div>
          </div>
          
          <div 
            class="backdrop-blur-md rounded-xl p-4 border transition-all duration-1000"
            :style="{ 
              backgroundColor: `${currentEmotionTheme.cardBg}`,
              borderColor: currentEmotionTheme.borderColor,
              boxShadow: `0 4px 20px ${currentEmotionTheme.primary}20`
            }"
          >
            <h2 
              class="text-xl font-semibold mb-4 flex items-center transition-colors duration-1000"
              :style="{ color: currentEmotionTheme.primary }"
            >
              <HeartHandshake class="w-5 h-5 mr-2" />
              情感支持
            </h2>
            <div class="h-48 overflow-y-auto pr-2 custom-scrollbar">
              <div 
                class="p-3 rounded-lg mb-3 transition-colors duration-1000"
                :style="{ backgroundColor: currentEmotionTheme.buttonBg }"
              >
                <h3 
                  class="font-medium mb-1 transition-colors duration-1000"
                  :style="{ color: currentEmotionTheme.primary }"
                >
                  {{ supportSuggestion.title }}
                </h3>
                <p 
                  class="text-sm transition-colors duration-1000"
                  :style="{ color: currentEmotionTheme.secondaryText }"
                >
                  {{ supportSuggestion.description }}
                </p>
              </div>
              <button 
                @click="generateSupportSuggestion" 
                class="w-full py-2 rounded-lg transition-colors duration-1000 flex items-center justify-center"
                :style="{ 
                  backgroundColor: currentEmotionTheme.buttonBg,
                  color: currentEmotionTheme.text
                }"
              >
                <RefreshCw class="w-4 h-4 mr-2" />
                新建议
              </button>
              
              <div 
                class="mt-4 p-3 rounded-lg border transition-colors duration-1000"
                :style="{ 
                  backgroundColor: `${currentEmotionTheme.primary}10`,
                  borderColor: `${currentEmotionTheme.primary}30`
                }"
              >
                <h3 
                  class="font-medium mb-1 flex items-center transition-colors duration-1000"
                  :style="{ color: currentEmotionTheme.primary }"
                >
                  <Shield class="w-4 h-4 mr-1" />
                  隐私保护
                </h3>
                <p 
                  class="text-xs transition-colors duration-1000"
                  :style="{ color: currentEmotionTheme.secondaryText }"
                >
                  你的情感数据存储在本地且永不共享。你的感受只属于你自己。
                </p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, computed, onMounted, watch } from 'vue';
  import { Heart, Activity, PieChart, BookOpen, Zap, MessageSquare, Send, Save, PlusCircle, Music, HeartHandshake, RefreshCw, Shield } from 'lucide-vue-next';
  
  // Emotional State Machine with color themes
  const emotions = [
    { 
      id: 'joy', 
      name: '喜悦', 
      emoji: '😊', 
      color: 'bg-yellow-400', 
      intensity: 70,
      theme: {
        gradientStart: '#fbbf24',
        gradientEnd: '#f59e0b',
        primary: '#f59e0b',
        primaryShadow: 'rgba(245,158,11,0.7)',
        primaryTransparent: 'rgba(245,158,11,0.3)',
        secondary: '#fcd34d',
        secondaryTransparent: 'rgba(252,211,77,0.3)',
        text: '#ffffff',
        secondaryText: '#fef3c7',
        cardBg: 'rgba(255,255,255,0.1)',
        buttonBg: 'rgba(255,255,255,0.1)',
        inputBg: 'rgba(0,0,0,0.2)',
        accentBg: 'rgba(0,0,0,0.4)',
        borderColor: 'rgba(245,158,11,0.3)',
        buttonText: '#ffffff',
        textGradientStart: '#fbbf24',
        textGradientEnd: '#fef3c7'
      }
    },
    { 
      id: 'love', 
      name: '爱', 
      emoji: '❤️', 
      color: 'bg-pink-500', 
      intensity: 85,
      theme: {
        gradientStart: '#ec4899',
        gradientEnd: '#be185d',
        primary: '#ec4899',
        primaryShadow: 'rgba(236,72,153,0.7)',
        primaryTransparent: 'rgba(236,72,153,0.3)',
        secondary: '#f472b6',
        secondaryTransparent: 'rgba(244,114,182,0.3)',
        text: '#ffffff',
        secondaryText: '#fbcfe8',
        cardBg: 'rgba(255,255,255,0.1)',
        buttonBg: 'rgba(255,255,255,0.1)',
        inputBg: 'rgba(0,0,0,0.2)',
        accentBg: 'rgba(0,0,0,0.4)',
        borderColor: 'rgba(236,72,153,0.3)',
        buttonText: '#ffffff',
        textGradientStart: '#ec4899',
        textGradientEnd: '#fbcfe8'
      }
    },
    { 
      id: 'calm', 
      name: '平静', 
      emoji: '😌', 
      color: 'bg-blue-400', 
      intensity: 60,
      theme: {
        gradientStart: '#60a5fa',
        gradientEnd: '#3b82f6',
        primary: '#60a5fa',
        primaryShadow: 'rgba(96,165,250,0.7)',
        primaryTransparent: 'rgba(96,165,250,0.3)',
        secondary: '#93c5fd',
        secondaryTransparent: 'rgba(147,197,253,0.3)',
        text: '#ffffff',
        secondaryText: '#dbeafe',
        cardBg: 'rgba(255,255,255,0.1)',
        buttonBg: 'rgba(255,255,255,0.1)',
        inputBg: 'rgba(0,0,0,0.2)',
        accentBg: 'rgba(0,0,0,0.4)',
        borderColor: 'rgba(96,165,250,0.3)',
        buttonText: '#ffffff',
        textGradientStart: '#60a5fa',
        textGradientEnd: '#dbeafe'
      }
    },
    { 
      id: 'excitement', 
      name: '兴奋', 
      emoji: '🤩', 
      color: 'bg-purple-500', 
      intensity: 75,
      theme: {
        gradientStart: '#a855f7',
        gradientEnd: '#7e22ce',
        primary: '#a855f7',
        primaryShadow: 'rgba(168,85,247,0.7)',
        primaryTransparent: 'rgba(168,85,247,0.3)',
        secondary: '#c084fc',
        secondaryTransparent: 'rgba(192,132,252,0.3)',
        text: '#ffffff',
        secondaryText: '#f3e8ff',
        cardBg: 'rgba(255,255,255,0.1)',
        buttonBg: 'rgba(255,255,255,0.1)',
        inputBg: 'rgba(0,0,0,0.2)',
        accentBg: 'rgba(0,0,0,0.4)',
        borderColor: 'rgba(168,85,247,0.3)',
        buttonText: '#ffffff',
        textGradientStart: '#a855f7',
        textGradientEnd: '#f3e8ff'
      }
    },
    { 
      id: 'melancholy', 
      name: '忧郁', 
      emoji: '🥺', 
      color: 'bg-indigo-400', 
      intensity: 45,
      theme: {
        gradientStart: '#4f46e5',
        gradientEnd: '#312e81',
        primary: '#818cf8',
        primaryShadow: 'rgba(129,140,248,0.7)',
        primaryTransparent: 'rgba(129,140,248,0.3)',
        secondary: '#a5b4fc',
        secondaryTransparent: 'rgba(165,180,252,0.3)',
        text: '#e0e7ff',
        secondaryText: '#c7d2fe',
        cardBg: 'rgba(255,255,255,0.1)',
        buttonBg: 'rgba(255,255,255,0.1)',
        inputBg: 'rgba(0,0,0,0.2)',
        accentBg: 'rgba(0,0,0,0.4)',
        borderColor: 'rgba(129,140,248,0.3)',
        buttonText: '#ffffff',
        textGradientStart: '#818cf8',
        textGradientEnd: '#e0e7ff'
      }
    },
    { 
      id: 'nostalgia', 
      name: '怀旧', 
      emoji: '🌙', 
      color: 'bg-teal-400', 
      intensity: 55,
      theme: {
        gradientStart: '#14b8a6',
        gradientEnd: '#0f766e',
        primary: '#2dd4bf',
        primaryShadow: 'rgba(45,212,191,0.7)',
        primaryTransparent: 'rgba(45,212,191,0.3)',
        secondary: '#5eead4',
        secondaryTransparent: 'rgba(94,234,212,0.3)',
        text: '#ffffff',
        secondaryText: '#ccfbf1',
        cardBg: 'rgba(255,255,255,0.1)',
        buttonBg: 'rgba(255,255,255,0.1)',
        inputBg: 'rgba(0,0,0,0.2)',
        accentBg: 'rgba(0,0,0,0.4)',
        borderColor: 'rgba(45,212,191,0.3)',
        buttonText: '#ffffff',
        textGradientStart: '#2dd4bf',
        textGradientEnd: '#ccfbf1'
      }
    }
  ];
  
  const currentEmotionId = ref('love');
  const currentEmotion = computed(() => {
    return emotions.find(e => e.id === currentEmotionId.value);
  });
  
  const currentEmotionTheme = computed(() => {
    return currentEmotion.value.theme;
  });
  
  // Heart Rate Monitor
  const heartRate = ref(72);
  const heartbeatPoints = ref([]);
  const backgroundHeartbeatPoints = ref([]);
  const sensitivity = ref(5);
  const maxPoints = 100;
  const maxBackgroundPoints = 300;
  const lastBeatTime = ref(0);
  const beatInterval = ref(0);
  
  const heartbeatPath = computed(() => {
    if (heartbeatPoints.value.length < 2) return '';
    
    let path = `M 0,50`;
    heartbeatPoints.value.forEach((point, index) => {
      const x = (index / (heartbeatPoints.value.length - 1)) * 400;
      path += ` L ${x},${point}`;
    });
    
    return path;
  });
  
  const backgroundHeartbeatPath = computed(() => {
    if (backgroundHeartbeatPoints.value.length < 2) return '';
    
    let path = `M 0,300`;
    backgroundHeartbeatPoints.value.forEach((point, index) => {
      const x = (index / (backgroundHeartbeatPoints.value.length - 1)) * 1000;
      path += ` L ${x},${point}`;
    });
    
    return path;
  });
  
  // Floating Hearts
  const floatingHearts = ref([]);
  const heartEmojis = ['❤️', '💖', '💕', '💓', '💗', '💘', '💝', '💞'];
  
  const generateFloatingHearts = () => {
    floatingHearts.value = [];
    for (let i = 0; i < 20; i++) {
      floatingHearts.value.push({
        emoji: heartEmojis[Math.floor(Math.random() * heartEmojis.length)],
        x: Math.random() * 100,
        duration: 15 + Math.random() * 20,
        delay: Math.random() * 15,
        opacity: 0.1 + Math.random() * 0.3,
        scale: 0.5 + Math.random() * 2,
        color: currentEmotionTheme.value.primary
      });
    }
  };
  
  // Emotional Composition
  const emotionalComposition = ref([
    { name: '爱', value: 40, color: '#ec4899', startAngle: 0, endAngle: 0 },
    { name: '喜悦', value: 25, color: '#facc15', startAngle: 0, endAngle: 0 },
    { name: '平静', value: 15, color: '#60a5fa', startAngle: 0, endAngle: 0 },
    { name: '怀旧', value: 20, color: '#2dd4bf', startAngle: 0, endAngle: 0 }
  ]);
  
  // Calculate angles for pie chart
  const updateCompositionAngles = () => {
    let currentAngle = 0;
    emotionalComposition.value.forEach(segment => {
      segment.startAngle = currentAngle;
      currentAngle += (segment.value / 100) * 360;
      segment.endAngle = currentAngle;
    });
  };
  
  // Helper function to generate conic polygon path
  const getConicPolygonPath = (startAngle, endAngle) => {
    const startRad = (startAngle - 90) * (Math.PI / 180);
    const endRad = (endAngle - 90) * (Math.PI / 180);
    
    const startX = 50 + 50 * Math.cos(startRad);
    const startY = 50 + 50 * Math.sin(startRad);
    const endX = 50 + 50 * Math.cos(endRad);
    const endY = 50 + 50 * Math.sin(endRad);
    
    return `${startX}% ${startY}%, ${endX}% ${endY}%`;
  };
  
  const emotionalBalance = computed(() => {
    // Calculate balance based on distribution of emotions
    const values = emotionalComposition.value.map(e => e.value);
    const max = Math.max(...values);
    const min = Math.min(...values);
    const range = max - min;
    
    // Higher balance when emotions are more evenly distributed
    return Math.round(100 - (range / 2));
  });
  
  // Emotional Memory & Journal
  const activeTab = ref('memories');
  const emotionalMemories = ref([
    { 
      emotion: '爱', 
      emoji: '❤️', 
      time: '2小时前', 
      description: '收到那条意外的消息时，感受到一波温暖的爱意。',
      bgColor: 'bg-pink-500/20'
    },
    { 
      emotion: '怀旧', 
      emoji: '🌙', 
      time: '昨天', 
      description: '发现一张旧照片，唤起了美好的回忆。',
      bgColor: 'bg-teal-500/20'
    },
    { 
      emotion: '喜悦', 
      emoji: '😊', 
      time: '3天前', 
      description: '那一刻，一切都感觉恰到好处。',
      bgColor: 'bg-yellow-500/20'
    }
  ]);
  
  const newMemory = ref('');
  const journalEntry = ref('');
  
  const addMemory = () => {
    if (!newMemory.value.trim()) return;
    
    emotionalMemories.value.unshift({
      emotion: currentEmotion.value.name,
      emoji: currentEmotion.value.emoji,
      time: 'Just now',
      description: newMemory.value,
      bgColor: `bg-${currentEmotion.value.color.split('-')[1]}-500/20`
    });
    
    newMemory.value = '';
    
    // Update emotional composition based on new memory
    const emotionIndex = emotionalComposition.value.findIndex(e => e.name === currentEmotion.value.name);
    if (emotionIndex >= 0) {
      // Increase this emotion's value and normalize all values to sum to 100
      emotionalComposition.value[emotionIndex].value += 5;
      normalizeComposition();
      updateCompositionAngles();
    }
  };
  
  const saveJournalEntry = () => {
    if (!journalEntry.value.trim()) return;
    
    // Add to memories
    emotionalMemories.value.unshift({
      emotion: currentEmotion.value.name,
      emoji: currentEmotion.value.emoji,
      time: 'Just now',
      description: journalEntry.value.substring(0, 100) + (journalEntry.value.length > 100 ? '...' : ''),
      bgColor: `bg-${currentEmotion.value.color.split('-')[1]}-500/20`
    });
    
    // Clear journal
    journalEntry.value = '';
    
    // Show confirmation
    alert('Journal entry saved successfully!');
  };
  
  const normalizeComposition = () => {
    const total = emotionalComposition.value.reduce((sum, item) => sum + item.value, 0);
    emotionalComposition.value.forEach(item => {
      item.value = Math.round((item.value / total) * 100);
    });
  };
  
  // Trend Analysis
  const emotionTrends = ref({
    love: [60, 65, 75, 70, 80, 85, 85],
    joy: [40, 45, 50, 60, 55, 50, 60],
    calm: [70, 65, 55, 50, 60, 65, 70]
  });
  
  const getTrendPath = (emotion) => {
    const data = emotionTrends.value[emotion];
    if (!data) return '';
    
    const points = data.map((value, index) => {
      const x = 40 + (index * (340 / (data.length - 1)));
      const y = 130 - (value / 100 * 110);
      return `${x},${y}`;
    });
    
    return `M ${points.join(' L ')}`;
  };
  
  // Emotional Triggers
  const emotionalTriggers = ref([
    { name: '浪漫音乐', emoji: '🎵', active: true, emotion: 'love' },
    { name: '自然风景', emoji: '🌿', active: false, emotion: 'calm' },
    { name: '共同回忆', emoji: '📷', active: true, emotion: 'nostalgia' },
    { name: '意外消息', emoji: '💌', active: false, emotion: 'excitement' },
    { name: '星空之夜', emoji: '✨', active: true, emotion: 'melancholy' }
  ]);
  
  const showCustomizeTriggers = ref(false);
  
  const toggleTrigger = (index) => {
    emotionalTriggers.value[index].active = !emotionalTriggers.value[index].active;
    
    // If activated, trigger the associated emotion
    if (emotionalTriggers.value[index].active) {
      triggerEmotion(emotionalTriggers.value[index].emotion);
    }
  };
  
  const addNewTrigger = () => {
    emotionalTriggers.value.push({
      name: '新触发',
      emoji: '🔔',
      active: false,
      emotion: 'joy'
    });
  };
  
  // Emotional Expression
  const expressionTypes = [
    { type: 'poetic', name: '诗意' },
    { type: 'direct', name: '直接' },
    { type: 'metaphorical', name: '隐喻' },
    { type: 'musical', name: '音乐' }
  ];
  
  const expressionType = ref('poetic');
  const currentExpression = ref('');
  const isPlaying = ref(false);
  
  const expressions = {
    love: {
      poetic: [
        "如繁星点缀夜空，我对你的爱愈发璀璨。",
        "在我心灵的园地里，你的爱永恒绽放。",
        "你是我灵魂交响乐中最美的旋律。"
      ],
      direct: [
        "我深深地、完全地爱着你。",
        "我的心永远属于你。",
        "你对我来说意味着一切。"
      ],
      metaphorical: [
        "你是我在暴风雨中稳固的锚。",
        "我对你的爱是无尽的海洋，深邃无垠。",
        "你是我穿越黑暗的指路明灯。"
      ],
      musical: [
        "♪ 我心跳的每一拍都在呼唤你的名字 ♪",
        "♪ 在生命的节奏中，你是我最爱的旋律 ♪",
        "♪ 如一首永不结束的情歌，我的心为你而唱 ♪"
      ]
    },
    joy: {
      poetic: [
        "阳光穿透云层，为我的世界染上金色。",
        "喜悦如蝴蝶在夏日田野翩翩起舞。",
        "欢笑如清泉般涌出。"
      ],
      direct: [
        "我现在感到无比快乐！",
        "这一刻充满纯粹的喜悦。",
        "幸福从我的心中溢出。"
      ],
      metaphorical: [
        "我的精神如鸟儿从笼中释放般翱翔。",
        "喜悦在我心中如香槟气泡般闪耀。",
        "幸福如温暖的毯子包裹着我。"
      ],
      musical: [
        "♪ 带着微笑在人生中起舞 ♪",
        "♪ 我的心跳奏响纯粹的欢乐 ♪",
        "♪ 喜悦是今天在我灵魂中演奏的旋律 ♪"
      ]
    },
    // Add expressions for other emotions...
  };
  
  const generateExpression = (type) => {
    expressionType.value = type;
    
    // Get expressions for current emotion or default to love
    const emotionExpressions = expressions[currentEmotionId.value] || expressions.love;
    const typeExpressions = emotionExpressions[type] || emotionExpressions.poetic;
    
    // Select random expression
    const randomIndex = Math.floor(Math.random() * typeExpressions.length);
    currentExpression.value = typeExpressions[randomIndex];
  };
  
  const playMusic = () => {
    isPlaying.value = !isPlaying.value;
    // In a real app, this would play music based on the current emotion
  };
  
  // Emotional Support
  const supportSuggestions = {
    love: [
      { title: "表达你的感受", description: "给在乎的人写一封信或消息，表达你对他们的感激之情。" },
      { title: "善举", description: "为所爱的人做些小事，以加强你们之间的联系。" }
    ],
    joy: [
      { title: "珍惜当下", description: "花时间充分体验和欣赏你此刻的积极情绪。" },
      { title: "分享喜悦", description: "喜悦在分享时会倍增 - 联系可能需要鼓励的人。" }
    ],
    calm: [
      { title: "正念呼吸", description: "花五分钟进行深呼吸，保持内心的平静。" },
      { title: "亲近自然", description: "花时间在大自然中，强化你平和的心境。" }
    ],
    melancholy: [
      { title: "接纳情绪", description: "允许自己感受而不加评判。忧郁可以是反思的时机。" },
      { title: "创造性表达", description: "将你的感受转化为艺术、音乐或写作，以建设性的方式处理它们。" }
    ]
  };
  
  const supportSuggestion = ref({ title: "", description: "" });
  
  const generateSupportSuggestion = () => {
    const emotionSuggestions = supportSuggestions[currentEmotionId.value] || supportSuggestions.love;
    supportSuggestion.value = emotionSuggestions[Math.floor(Math.random() * emotionSuggestions.length)];
  };
  
  // Function to trigger an emotion
  const triggerEmotion = (emotionId) => {
    currentEmotionId.value = emotionId;
    
    // Update heart rate based on emotion
    const emotion = emotions.find(e => e.id === emotionId);
    if (emotion) {
      if (emotionId === 'excitement') {
        heartRate.value = 90 + Math.floor(Math.random() * 15);
      } else if (emotionId === 'calm') {
        heartRate.value = 60 + Math.floor(Math.random() * 10);
      } else if (emotionId === 'love') {
        heartRate.value = 75 + Math.floor(Math.random() * 10);
      } else {
        heartRate.value = 70 + Math.floor(Math.random() * 10);
      }
      
      // Generate new expression based on emotion
      generateExpression(expressionType.value);
      
      // Generate new support suggestion
      generateSupportSuggestion();
      
      // Update floating hearts with new emotion colors
      generateFloatingHearts();
    }
  };
  
  // 更新心跳函数，使其更加真实
  const updateHeartbeat = () => {
    const now = Date.now();
    // 根据心率计算心跳间隔（毫秒）
    const expectedInterval = 60000 / heartRate.value;
    
    // 检查是否应该生成心跳
    if (now - lastBeatTime.value >= beatInterval.value) {
      // 生成一个完整的心电图波形（P-QRS-T波）
      const baselineY = 50; // 基线位置
      const pWave = baselineY - 5 - (Math.random() * 3); // P波
      const qWave = baselineY + 5 + (Math.random() * 3); // Q波
      const rWave = baselineY - 30 - (Math.random() * 10 * sensitivity.value / 5); // R波（高峰）
      const sWave = baselineY + 10 + (Math.random() * 5); // S波
      const tWave = baselineY - 8 - (Math.random() * 4); // T波
      
      // 根据当前情绪调整波形
      const emotionFactor = currentEmotionId.value === 'excitement' ? 1.5 : 
                            currentEmotionId.value === 'calm' ? 0.7 : 1;
      
      // 添加平稳基线
      for (let i = 0; i < 5; i++) {
        heartbeatPoints.value.push(baselineY + (Math.random() - 0.5) * 2);
      }
      
      // 添加P波（轻微上升）
      heartbeatPoints.value.push(pWave);
      
      // 添加QRS复合波
      heartbeatPoints.value.push(qWave); // Q波（下降）
      heartbeatPoints.value.push(rWave * emotionFactor); // R波（急剧上升）
      heartbeatPoints.value.push(sWave); // S波（急剧下降）
      
      // 添加T波（缓慢上升和下降）
      heartbeatPoints.value.push(tWave);
      
      // 回到基线
      for (let i = 0; i < 3; i++) {
        heartbeatPoints.value.push(baselineY + (Math.random() - 0.5) * 2);
      }
      
      // 更新背景心跳
      const bgBaselineY = 300;
      const bgScale = 3; // 背景心跳的放大倍数
      
      // 添加背景心跳波形，与主心跳同步但放大
      backgroundHeartbeatPoints.value.push(bgBaselineY + (baselineY - pWave) * bgScale);
      backgroundHeartbeatPoints.value.push(bgBaselineY - (qWave - baselineY) * bgScale);
      backgroundHeartbeatPoints.value.push(bgBaselineY + (baselineY - rWave) * bgScale * emotionFactor);
      backgroundHeartbeatPoints.value.push(bgBaselineY - (sWave - baselineY) * bgScale);
      backgroundHeartbeatPoints.value.push(bgBaselineY + (baselineY - tWave) * bgScale);
      backgroundHeartbeatPoints.value.push(bgBaselineY);
      
      // 记录本次心跳时间
      lastBeatTime.value = now;
      
      // 根据心率和一些随机性设置下一次心跳的间隔
      // 添加一些变异性使心跳看起来更自然
      const variability = (Math.random() - 0.5) * 200; // ±100ms的变异性
      beatInterval.value = expectedInterval + variability;
    } else {
      // 在心跳之间添加基线
      heartbeatPoints.value.push(50 + (Math.random() - 0.5) * 1.5);
      backgroundHeartbeatPoints.value.push(300 + (Math.random() - 0.5) * 4);
    }
    
    // 限制点数
    if (heartbeatPoints.value.length > maxPoints) {
      heartbeatPoints.value.shift();
    }
    
    if (backgroundHeartbeatPoints.value.length > maxBackgroundPoints) {
      backgroundHeartbeatPoints.value.shift();
    }
  };
  
  // Initialize
  onMounted(() => {
    // 初始化心跳基线
    for (let i = 0; i < maxPoints; i++) {
      heartbeatPoints.value.push(50 + (Math.random() - 0.5) * 1.5);
    }
    
    for (let i = 0; i < maxBackgroundPoints; i++) {
      backgroundHeartbeatPoints.value.push(300 + (Math.random() - 0.5) * 4);
    }
    
    // 设置初始心跳间隔
    beatInterval.value = 60000 / heartRate.value;
    
    // 启动心跳动画，更快的更新频率
    setInterval(updateHeartbeat, 50);
    
    // 初始化饼图角度
    updateCompositionAngles();
    
    // 生成初始表达
    generateExpression('poetic');
    
    // 生成初始支持建议
    generateSupportSuggestion();
    
    // 生成浮动心形
    generateFloatingHearts();
  });
  
  // Watch for sensitivity changes
  watch(sensitivity, (newVal) => {
    // Adjust heart rate variability based on sensitivity
    const baseRate = currentEmotionId.value === 'excitement' ? 90 : 
                    currentEmotionId.value === 'calm' ? 60 : 75;
    
    heartRate.value = baseRate + Math.floor((newVal / 10) * Math.random() * 20);
  });
  
  // Watch for emotion changes to update colors
  watch(currentEmotionId, () => {
    // Update floating hearts with new emotion colors
    generateFloatingHearts();
  });
  </script>
  
  <style>
  .custom-scrollbar::-webkit-scrollbar {
    width: 6px;
  }
  
  .custom-scrollbar::-webkit-scrollbar-track {
    background: rgba(255, 255, 255, 0.1);
    border-radius: 10px;
  }
  
  .custom-scrollbar::-webkit-scrollbar-thumb {
    background: rgba(236, 72, 153, 0.5);
    border-radius: 10px;
  }
  
  .animate-typing-effect {
    overflow: hidden;
    border-right: 2px solid rgba(236, 72, 153, 0.75);
    white-space: nowrap;
    margin: 0 auto;
    animation: typing 3.5s steps(40, end), blink-caret 0.75s step-end infinite;
  }
  
  @keyframes typing {
    from { width: 0 }
    to { width: 100% }
  }
  
  @keyframes blink-caret {
    from, to { border-color: transparent }
    50% { border-color: rgba(236, 72, 153, 0.75) }
  }
  
  @keyframes float {
    0% {
      transform: translateY(0) rotate(0deg);
      opacity: 0;
    }
    10% {
      opacity: 1;
    }
    90% {
      opacity: 1;
    }
    100% {
      transform: translateY(-100vh) rotate(720deg);
      opacity: 0;
    }
  }
  
  .animate-float {
    animation-name: float;
    animation-timing-function: ease-in-out;
    animation-iteration-count: infinite;
  }
  </style>