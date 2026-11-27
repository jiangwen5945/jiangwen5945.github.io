<script setup>
import { ref, computed, onMounted, onUnmounted, nextTick } from 'vue'
import { useThrottleFn } from '@vueuse/core'
import CodeCard from '@/components/CodeCard.vue'
import IframeModal from '@/components/IframeModal.vue'

const baseUrl = import.meta.env.BASE_URL

const preloaderHide = ref(false)
const showTerminal = ref(false)

const typewriterText = ref('')

const hoverClass = ref('')

const works = [
  {
    title: 'code-lab',
    desc: '代码实验室',
    img: `${baseUrl}img/work/item-1.webp`,
    url: `${baseUrl}zendo`,
    platforms: ['pc']
  },
  {
    title: 'my-blog',
    desc: '个人博客',
    img: `${baseUrl}img/work/item-2.webp`,
    url: 'https://jiangwen.site/blog',
    platforms: ['pc', 'app']
  },
  {
    title: 'xiamao-mall',
    desc: '电商网站',
    img: `${baseUrl}img/work/item-4.webp`,
    url: 'https://xiamao-mall.jiangwen.site',
    platforms: ['pc']
  },
  {
    title: 'xiamao-admin',
    desc: '后台管理系统',
    img: `${baseUrl}img/work/item-3.webp`,
    url: 'https://xiamao-admin.jiangwen.site',
    platforms: ['pc']
  },
  {
    title: 'live-dashboard',
    desc: '实时数据大屏展示',
    img: `${baseUrl}img/work/item-5.webp`,
    url: 'https://xiamao-dashboard.jiangwen.site',
    platforms: ['pc']
  }
]

const contactInfo = {
  phone: '17759795597',
  email: 'jiangwen5945@gmail.com'
}

const timers = []

const isMobile = ref(window.innerWidth < 768)
const deviceType = computed(() => (isMobile.value ? 'app' : 'pc'))

function isMatched(item) {
  return item.platforms.includes(deviceType.value)
}

const breakpointHandler = () => {
  isMobile.value = window.innerWidth < 768
}

function showToast(msg) {
  const el = document.createElement('div')
  el.textContent = msg
  el.className = 'toast-message'
  Object.assign(el.style, {
    position: 'fixed',
    top: '50%',
    left: '50%',
    transform: 'translate(-50%, -50%)',
    background: 'rgba(0,0,0,0.75)',
    color: '#fff',
    padding: '12px 24px',
    borderRadius: '8px',
    fontFamily: 'var(--font-primary, serif)',
    zIndex: 9999,
    fontSize: '14px',
    lineHeight: '1.4',
    textAlign: 'center',
    pointerEvents: 'none',
    transition: 'opacity 0.3s'
  })
  document.body.appendChild(el)
  setTimeout(() => {
    el.style.opacity = '0'
    setTimeout(() => el.remove(), 300)
  }, 2500)
}

function handleWorkClick(item) {
  if (isMatched(item)) {
    window.open(item.url, '_blank')
    return
  }
  const target = isMobile.value ? '电脑' : '手机'
  showToast(`请在${ target }端打开此项目`)
}

function handleCodeCardClick() {
  if (isMobile.value) {
    window.open('https://jiangwen.site/terminal-portfolio/', '_blank')
  } else {
    showTerminal.value = true
  }
}

const services = [
  { icon: 'Monitor', label: 'Vue 前端开发' },
  { icon: 'Odometer', label: '后台管理系统' },
  { icon: 'ShoppingCart', label: '电商平台方案' },
  { icon: 'Iphone', label: '移动端 H5' },
  { icon: 'Refresh', label: '优化与重构' }
]

let mouseMoveHandler = null
let mouseLeaveHandler = null
let observer = null

function smoothScroll(e, targetId) {
  e.preventDefault()
  const el = document.querySelector(targetId)
  if (el) el.scrollIntoView({ behavior: 'smooth', block: 'start' })
}

onMounted(() => {
  if (document.readyState === 'complete') {
    preloaderHide.value = true
  } else {
    window.addEventListener('load', () => { preloaderHide.value = true }, { once: true })
  }

  timers.push(setTimeout(() => {
    document.querySelectorAll('#intro .animation-container').forEach((el) => {
      const delay = parseInt(el.getAttribute('data-animation-delay') || '0')
      timers.push(setTimeout(() => {
        el.classList.add('run-animation')
      }, 700 + delay))
    })
  }, 700))

  timers.push(setTimeout(() => {
    const lines = 
      [
        '从前我写代码，现在我描述代码',
        '从古法编程到许愿式编程（Vibe Coding）',
        '一路见证前端的进化.....',
      ]
    let lineIdx = 0
    let charIdx = 0

    const typeNext = () => {
      if (lineIdx >= lines.length) return
      if (charIdx < lines[lineIdx].length) {
        typewriterText.value += lines[lineIdx][charIdx]
        charIdx++
        timers.push(setTimeout(typeNext, 80 + Math.random() * 40))
      } else {
        if (lineIdx < lines.length - 1) {
          typewriterText.value += '<br>'
        }
        lineIdx++
        charIdx = 0
        timers.push(setTimeout(typeNext, 500))
      }
    }
    typeNext()
  }, 1000))

  const isDesktop = window.innerWidth >= 768
  if (isDesktop) {
    mouseMoveHandler = useThrottleFn((e) => {
      hoverClass.value = e.clientX >= window.innerWidth / 2 ? 'hover-right' : ''
    }, 100)
    mouseLeaveHandler = () => {
      hoverClass.value = ''
    }
    document.addEventListener('mousemove', mouseMoveHandler)
    document.documentElement.addEventListener('mouseleave', mouseLeaveHandler)
  }

  observer = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          entry.target.classList.add('revealed')
          observer.unobserve(entry.target)
        }
      })
    },
    { threshold: 0.3 }
  )

  window.addEventListener('resize', breakpointHandler)

  nextTick(() => {
    document.querySelectorAll('.scroll-animated-from-right').forEach((el) => {
      observer.observe(el)
    })
  })
})

onUnmounted(() => {
  timers.forEach(clearTimeout)
  if (mouseMoveHandler) {
    document.removeEventListener('mousemove', mouseMoveHandler)
  }
  if (mouseLeaveHandler) {
    document.documentElement.removeEventListener('mouseleave', mouseLeaveHandler)
  }
  if (observer) {
    observer.disconnect()
  }
  window.removeEventListener('resize', breakpointHandler)
})
</script>

<template>
  <div :class="['home', hoverClass]">
    <div :class="['preloader', { 'hide-preloader': preloaderHide }]">
      <div class="spinner"></div>
    </div>

    <IframeModal :visible="showTerminal" src="https://jiangwen.site/terminal-portfolio/" @close="showTerminal = false" />

    <div class="image-container">
      <div class="background-img"></div>
    </div>

    <div class="content-area">
      <div class="content-area-inner">
        <section id="intro">
          <div class="container-mid">
            <div class="animation-container animation-fade-down cursor-pointer" data-animation-delay="300" @click="handleCodeCardClick">
              <CodeCard/>
            </div>
            <div class="animation-container animation-fade-left" data-animation-delay="300">
              <p class="subline">
                <span v-html="typewriterText"></span
                ><span class="typewriter-cursor">|</span>
              </p>
            </div>

            <div class="animation-container animation-fade-up" data-animation-delay="600">
              <a
                href="#work"
                @click="smoothScroll($event, '#work')"
                class="btn-more"
              >
                个人作品集
                <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M6 9l6 6 6-6"/></svg>
              </a>
            </div>
          </div>
        </section>

        <section id="service">
          <ul class="services-list">
            <li v-for="s in services" :key="s.label" class="scroll-animated-from-right">
              <span class="icon-circle"
                ><el-icon :size="24"><component :is="s.icon" /></el-icon></span
              >{{ s.label }}
            </li>
          </ul>
        </section>

        <section id="work">
          <h3 class="headline scroll-animated-from-right">我的作品</h3>
          <div class="showcase">
            <div v-for="item in works" :key="item.title" class="item scroll-animated-from-right" v-hover3d>
              <div class="work-link" @click="handleWorkClick(item)">
                <div class="info">
                  <div class="container-mid">
                    <h5>{{ item.title }}</h5>
                    <p>{{ item.desc }}</p>
                  </div>
                </div>
                <div
                  class="background-image"
                  :style="{ backgroundImage: `url(${item.img})` }"
                ></div>
              </div>
              <div class="device-badges">
                <span v-for="p in item.platforms" :key="p" class="badge">
                  {{ p === 'pc' ? 'PC' : 'App' }}
                </span>
              </div>
            </div>
          </div>
        </section>

        <section id="contact">
          <h3 class="headline scroll-animated-from-right">联系我</h3>
          <ul class="contact-list">
            <li class="scroll-animated-from-right">
              <el-icon><Iphone /></el-icon>{{ contactInfo.phone }}
            </li>
            <li class="scroll-animated-from-right">
              <el-icon><Message /></el-icon>{{ contactInfo.email }}
            </li>
          </ul>
        </section>
      </div>
    </div>
  </div>
</template>

<style scoped lang="scss">
.home {
  --font-primary: 'Arapey', serif;
  --font-heading: 'Abril Fatface', cursive;
  --color-black: #000;
  --color-white: #fff;
  --color-dark: #111;
  --color-overlay: #00000097;
  --fs-lg: min(5.333vw, 24px);
  font-family: var(--font-primary);
  font-weight: 300;
  color: var(--color-black);
  -webkit-font-smoothing: antialiased;

  h1,
  h2,
  h3,
  h4,
  h5,
  h6 {
    font-family: var(--font-heading);
    font-weight: normal;
    letter-spacing: 0.05em;
    color: var(--color-black);
  }

  h3 {
    font-size: min(7.467vw, 33.6px);
  }
  h5 {
    font-size: var(--fs-lg);
  }

  a,
  .work-link {
    cursor: pointer;
    text-decoration: none;
  }
}

/* 预加载 */
.preloader {
  position: fixed;
  z-index: 100000;
  bottom: 0;
  left: 0;
  overflow: hidden;
  width: 100%;
  height: 100%;
  background: var(--color-white);

  &.hide-preloader {
    height: 0;
    transition: 0.7s ease 0.3s;

    .spinner {
      opacity: 0;
      transition: 0.3s ease;
    }
  }

  .spinner {
    position: absolute;
    top: 50%;
    left: 50%;
    display: block;
    width: 50px;
    height: 50px;
    margin-top: -25px;
    margin-left: -25px;
    border-radius: 100%;
    background-color: var(--color-dark);
    animation: spinner-pulse 0.6s infinite ease-in-out;
  }
}

@keyframes spinner-pulse {
  0% {
    transform: scale(0);
  }
  100% {
    transform: scale(1);
    opacity: 0;
  }
}

/* 图片容器（左侧） */
.image-container {
  position: fixed;
  z-index: 10;
  top: 0;
  left: 0;
  width: 50%;
  height: 100%;

  .background-img {
    position: absolute;
    z-index: 1;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: url(../assets/img/background.webp) no-repeat top / cover;
  }
}

/* 内容区域（右侧） */
.content-area {
  position: relative;
  left: 0;
  width: 100%;

  .content-area-inner {
    position: relative;
    left: 50%;
    width: 50%;
    padding: 0 20px;
    box-sizing: border-box;
    overflow-x: hidden;

    section {
      max-width: 530px;
      min-height: 100vh;
      margin: 0 auto 18vh auto;
      display: flex;
      flex-direction: column;
      justify-content: center;

      &:last-child {
        margin-bottom: 10vh;
      }

      h3.headline {
        margin-bottom: 1.2em;
      }
    }
  }
}

/* 介绍 */
#intro {
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-evenly;
  .container-mid {
    position: relative;
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;

    p.subline {
      font-size: min(4vw, 24px);
      line-height: 1.6em;
      margin-top: 2em;
      margin-bottom: 2.2em;
      position: relative;
      padding: 0 1.8em;
      min-height: 7em;

      &::before {
        content: '\201C';
      }

      &::after {
        content: '\201D';
      }
    }

    .btn-more {
      display: inline-flex;
      align-items: center;
      gap: 8px;
      color: var(--color-black);
      font-size: 18px;
      padding: 14px 40px;
      border: 1px solid var(--color-black);
      border-radius: 100px;
      background: transparent;
      transition: all 0.3s ease;

      svg {
        transition: transform 0.3s ease;
      }

      &:hover {
        background: var(--color-black);
        color: var(--color-white);

        svg {
          animation: btn-more-bounce 1s ease infinite;
        }
      }

      &:active {
        transform: scale(0.95);
      }
    }
  }
}

/* 入场动画 */
.animation-container {
  transition: 0.5s ease;

  display: flex;
  justify-content: center;
  width: 100%;

  &.cursor-pointer {
    cursor: pointer;
  }

  &.animation-fade-up {
    transform: translateY(10vh);
    opacity: 0;

    &.run-animation {
      transform: translateY(0);
      opacity: 1;
    }
  }

  &.animation-fade-down {
    transform: translateY(-10vh);
    opacity: 0;

    &.run-animation {
      transform: translateY(0);
      opacity: 1;
    }
  }

  &.animation-fade-left {
    transform: translateX(10vh);
    opacity: 0;

    &.run-animation {
      transform: translateX(0);
      opacity: 1;
    }
  }

}

/* 打字机光标 */
.typewriter-cursor {
  display: inline-block;
  margin-left: 2px;
  color: var(--color-black);
  animation: cursor-blink 0.8s step-end infinite;
}

@keyframes cursor-blink {
  0%,
  100% {
    opacity: 1;
  }
  50% {
    opacity: 0;
  }
}

/* 服务 */
#service .services-list {
  margin-bottom: 0;
  padding: 0;
  list-style: none;

  li {
    font-size: var(--fs-lg);
    line-height: 3.4em;
    cursor: pointer;
    transition: transform 0.25s ease;

    &:hover {
      transform: translateX(6px);

      .el-icon {
        animation: service-icon-bounce 0.4s ease;
      }
    }

    .icon-circle {
      display: inline-flex;
      align-items: center;
      justify-content: center;
      width: 3em;
      height: 3em;
      margin-right: 1.5em;
      border-radius: 100px;
      background: var(--color-dark);
      flex-shrink: 0;
      vertical-align: middle;

      .el-icon {
        font-size: var(--fs-lg);
        color: var(--color-white);
      }
    }
  }
}

@keyframes service-icon-bounce {
  0% { transform: scale(1); }
  50% { transform: scale(1.2); }
  100% { transform: scale(1); }
}

/* 作品 */
#work .showcase .item {
  position: relative;
  overflow: hidden;
  width: 100%;
  height: min(53.333vw, 240px);
  margin-bottom: 6vh;
  cursor: pointer;
  border-radius: var(--fs-lg);
  background: var(--color-dark);

  &:last-child {
    margin-bottom: 0;
  }

  .info {
    position: absolute;
    z-index: 100;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    clip-path: inset(0 100% 0 0);
    transition: clip-path 0.25s ease;
    color: var(--color-white);
    background: var(--color-overlay);

    .container-mid {
      position: absolute;
      top: 50%;
      left: 0;
      width: 100%;
      padding: 0 min(16vw, 72px);
      transform: translateY(-50%);
      box-sizing: border-box;

      h5 {
        transform: translateX(4vh);
        opacity: 0;
        color: var(--color-white);
      }

      p {
        position: relative;
        transform: translateX(4vh);
        letter-spacing: 0.025em;
        opacity: 0;
        font-size: min(4vw, 16px);
      }
    }
  }

  &:hover {
    .info {
      clip-path: inset(0 0 0 0);
    }

    .info .container-mid h5 {
      transition: 0.2s ease 0.2s;
      transform: translateX(0);
      opacity: 1;
    }

    .info .container-mid p {
      transition: 0.2s ease 0.3s;
      transform: translateX(0);
      opacity: 1;
    }
  }

  .background-image {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-repeat: no-repeat;
    background-position: center;
    background-size: cover;
  }

  .device-badges {
    position: absolute;
    z-index: 200;
    top: 12px;
    right: 12px;
    display: flex;
    gap: 6px;

    .badge {
      display: inline-block;
      padding: 2px 10px;
      border-radius: 100px;
      font-size: 11px;
      font-family: var(--font-primary);
      font-weight: 700;
      letter-spacing: 0.05em;
      line-height: 1.6;
      border: 1.5px solid rgba(255, 255, 255, 0.8);
      color: rgba(255, 255, 255, 0.9);
      background: rgba(0, 0, 0, 0.45);
      backdrop-filter: blur(4px);
    }

  }
}

/* 滚动显现 */
.scroll-animated-from-right {
  transform: translateX(20vh);
  opacity: 0;
  transition:
    transform 0.6s ease,
    opacity 0.6s ease;

  &.revealed {
    transform: translateX(0);
    opacity: 1;
  }
}

/* 联系 */
#contact .contact-list {
  margin-top: 3em;
  margin-bottom: 3.4em;
  padding: 0;
  list-style: none;

  li {
    font-size: var(--fs-lg);
    line-height: 2.4em;

    .el-icon {
      line-height: 1.2em;
      width: 1.2em;
      height: 1.2em;
      margin-right: 1em;
      text-align: center;
      border-radius: 100px;
    }
  }
}

/* 悬停分割效果 */
.image-container {
  transition: width 0.4s ease;
}

.content-area .content-area-inner {
  transition:
    left 0.4s ease,
    width 0.4s ease;
}

.hover-right {
  .image-container {
    width: 35%;
  }

  .content-area .content-area-inner {
    left: 35%;
    width: 65%;
  }
}

/* 移动端样式 */
@media (max-width: 767px) {
  .home {
    color: var(--color-white);

    h3 {
      color: var(--color-white);
    }

    &::before {
      content: '';
      position: fixed;
      z-index: -2;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: url(../assets/img/background.webp) no-repeat center / cover;
    }

    &::after {
      content: '';
      position: fixed;
      z-index: -1;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(0, 0, 0, 0.6);
    }

  }

  .image-container {
    display: none;
  }

  .content-area .content-area-inner {
    left: 0;
    width: 100%;
    padding: 0 20px;

    section {
      max-width: 530px;
      margin: 0 auto;
      flex-direction: column;
      justify-content: center;
      background: rgba(0, 0, 0, 0.45);
      backdrop-filter: blur(4px);
      border-radius: var(--fs-lg);
      margin-bottom: 4vh;
      min-height: auto;
      padding: min(10.667vw, 48px) var(--fs-lg);
      border: 1px solid #2d2d2d;
      margin-top: 4vh;
      min-height: 30vh;
    }

    section h3.headline {
      margin-bottom: 1em;
      margin-top: 0;
    }
  }

  #intro .container-mid {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: space-between;
    min-height: 60vh;

    p.subline {
      line-height: 1.6em;
      margin-top: 1.5em;
      margin-bottom: 1.8em;
    }

    .btn-more {
      padding: 12px 32px;
      border-color: var(--color-white);
      color: var(--color-white);
      font-size: 4vw;

      &:hover {
        background: var(--color-white);
        color: var(--color-dark);
      }

      &:active {
        transform: scale(0.95);
      }
    }
  }

  .typewriter-cursor {
    color: var(--color-white);
  }

  #service .services-list li {
    line-height: 3em;
    display: flex;
    align-items: center;
    cursor: pointer;
    transition: transform 0.25s ease;

    &:hover {
      transform: translateX(4px);
    }

    .icon-circle {
      width: 2.8em;
      height: 2.8em;
      margin-right: 1em;

      .el-icon {
        font-size: 1.3em;
      }
    }
  }

  #work .showcase .item {
    margin-bottom: 3vh;

    .device-badges {
      top: 8px;
      right: 8px;

      .badge {
        font-size: 10px;
        padding: 1px 8px;
      }
    }

    .info {
      bottom: 0;
      left: 0;
      width: 100%;
      clip-path: none;
      background: linear-gradient(to top, rgba(0, 0, 0, 0.85) 0%, transparent 60%);
      padding: var(--fs-lg) var(--fs-lg) min(4vw, 18px);
      box-sizing: border-box;

      .container-mid {
        position: relative;
        top: 0;
        padding: 0;
        transform: none;

        h5 {
          transform: none;
          opacity: 1;
        }

        p {
          padding-left: 0;
          transform: none;
          opacity: 1;

          &::before {
            display: none;
          }
        }
      }
    }
  }

  #contact .contact-list {
    margin-top: 2em;
    margin-bottom: 2em;

    li {
      line-height: 2.2em;
      display: flex;
      align-items: center;

      .el-icon {
        width: 1.2em;
        margin-right: 0.8em;
      }
    }
  }
}

@keyframes btn-more-bounce {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(4px); }
}


</style>
