<template>
  <div 
    v-if="!isMobileDevice"
    class="custom-cursor" 
    :class="{
      'cursor-hover': isHovering,
      'cursor-click': isClicking,
      'cursor-hidden': isHidden
    }"
    :style="cursorStyle"
  >
    <div class="cursor-inner"></div>
    <div class="cursor-outer"></div>
  </div>
</template>

<script>
export default {
  name: 'CustomCursor',
  data() {
    return {
      x: 0,
      y: 0,
      lastX: 0,
      lastY: 0,
      isHovering: false,
      isClicking: false,
      isHidden: false,
      isMobileDevice: false,
      rafId: null
    }
  },
  computed: {
    cursorStyle() {
      return {
        transform: `translate(${this.x}px, ${this.y}px)`
      }
    }
  },
  mounted() {
    this.checkMobileDevice()
    
    if (!this.isMobileDevice) {
      this.initCursor()
    }
  },
  beforeUnmount() {
    if (!this.isMobileDevice) {
      this.cleanupCursor()
    }
  },
  methods: {
    checkMobileDevice() {
      this.isMobileDevice = 
        'ontouchstart' in window ||
        navigator.maxTouchPoints > 0 ||
        navigator.msMaxTouchPoints > 0 ||
        window.innerWidth <= 768
    },

    initCursor() {
      document.addEventListener('mousemove', this.onMouseMove)
      document.addEventListener('mouseenter', this.onMouseEnter)
      document.addEventListener('mouseleave', this.onMouseLeave)
      document.addEventListener('mousedown', this.onMouseDown)
      document.addEventListener('mouseup', this.onMouseUp)
      
      this.setupHoverListeners()
      this.startAnimation()
    },

    cleanupCursor() {
      document.removeEventListener('mousemove', this.onMouseMove)
      document.removeEventListener('mouseenter', this.onMouseEnter)
      document.removeEventListener('mouseleave', this.onMouseLeave)
      document.removeEventListener('mousedown', this.onMouseDown)
      document.removeEventListener('mouseup', this.onMouseUp)
      
      if (this.rafId) {
        cancelAnimationFrame(this.rafId)
      }
    },

    onMouseMove(e) {
      this.lastX = e.clientX
      this.lastY = e.clientY
    },

    onMouseEnter() {
      this.isHidden = false
    },

    onMouseLeave() {
      this.isHidden = true
    },

    onMouseDown() {
      this.isClicking = true
    },

    onMouseUp() {
      this.isClicking = false
    },

    startAnimation() {
      const animate = () => {
        // Плавное следование с интерполяцией
        this.x += (this.lastX - this.x) * 0.15
        this.y += (this.lastY - this.y) * 0.15
        
        this.rafId = requestAnimationFrame(animate)
      }
      
      animate()
    },

    setupHoverListeners() {
      // Ждем пока DOM полностью загрузится
      this.$nextTick(() => {
        const hoverElements = document.querySelectorAll([
          'a',
          'button',
          '[data-cursor-hover]',
          '.cursor-hover',
          'input',
          'textarea',
          'select'
        ].join(', '))
        
        hoverElements.forEach(el => {
          el.addEventListener('mouseenter', this.onElementHover)
          el.addEventListener('mouseleave', this.onElementLeave)
        })
      })
    },

    onElementHover() {
      this.isHovering = true
    },

    onElementLeave() {
      this.isHovering = false
      this.isClicking = false
    }
  }
}
</script>

<style scoped>
.custom-cursor {
  position: fixed;
  top: 0;
  left: 0;
  pointer-events: none;
  z-index: 9999;
  mix-blend-mode: difference;
  transition: opacity 0.3s ease;
}

.custom-cursor.cursor-hidden {
  opacity: 0;
}

.cursor-inner,
.cursor-outer {
  position: absolute;
  border-radius: 50%;
  transform: translate(-50%, -50%);
  transition: all 0.2s cubic-bezier(0.165, 0.84, 0.44, 1);
}

.cursor-inner {
  width: 6px;
  height: 6px;
  background: #ffffff;
  transition: 
    width 0.3s ease,
    height 0.3s ease,
    background 0.3s ease;
}

.cursor-outer {
  width: 40px;
  height: 40px;
  border: 1px solid rgba(255, 255, 255, 0.5);
  transition: 
    width 0.4s cubic-bezier(0.165, 0.84, 0.44, 1),
    height 0.4s cubic-bezier(0.165, 0.84, 0.44, 1),
    border 0.3s ease;
}

/* Hover состояния */
.custom-cursor.cursor-hover .cursor-inner {
  width: 4px;
  height: 4px;
  background: #ff6b6b;
}

.custom-cursor.cursor-hover .cursor-outer {
  width: 50px;
  height: 50px;
  border: 2px solid #ff6b6b;
  border-radius: 15%;
}

/* Click состояние */
.custom-cursor.cursor-click .cursor-inner {
  width: 8px;
  height: 8px;
  background: #4ecdc4;
}

.custom-cursor.cursor-click .cursor-outer {
  width: 35px;
  height: 35px;
  border: 2px solid #4ecdc4;
  border-radius: 25%;
}

/* Плавная анимация движения */
.custom-cursor {
  transition: transform 0.1s ease-out;
}

/* Адаптивность для разных размеров экранов */
@media (max-width: 1024px) {
  .cursor-inner {
    width: 5px;
    height: 5px;
  }
  
  .cursor-outer {
    width: 35px;
    height: 35px;
  }
  
  .custom-cursor.cursor-hover .cursor-outer {
    width: 45px;
    height: 45px;
  }
}
</style>
