<template lang="pug">
  .container
    textarea.content(v-model="content" v-on:keydown="replaceTab" v-on:input="setData")
    .processed(v-html="processedMarkdown")
</template>

<script>
import Vue from 'vue'
import marked from 'marked'

export default {
  data() {
    return {
      storeLabel: 'MDEditorContent',
      content: '# Start typing here...'
    }
  },
  computed: {
    processedMarkdown: function () {
      return marked(this.content, { sanitize: true })
    }
  },
  mounted() {
    if ( localStorage.getItem(this.storeLabel) ) {
      this.content = this.getData()
    }
  },
  methods: {
    replaceTab: function (e) {
      const tabNode = '\u00a0'

      if (e.which === 9) {
        e.preventDefault()
        document.execCommand('insertHTML', false, tabNode)
      }
    },
    setData: function () {
      localStorage.setItem(this.storeLabel, this.content)
      this.content = this.getData()
    },
    getData: function () {
      return localStorage.getItem(this.storeLabel)
    }
  }
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css?family=Fira+Mono');

html {
  font-size: 62.5%;
}

body {
  --color-1: rgba(255, 190, 11, 1);
  --color-2: rgba(252, 128, 40, 1);
  --color-3: rgba(255, 0, 110, 1);
  --color-4: rgba(167, 62, 159, 1);
  --color-5: rgba(131, 56, 236, 1);
  --color-6: #555;
  
  font-family: 'Fira Mono', monospace;
  font-size: 1.3rem;
  line-height: 1.4;
  color: #eee;
  background-color: #000;
  -webkit-font-smoothing: antialiased;
  text-rendering: optimizeLegibility;
  -moz-osx-font-smoothing: grayscale;
}

@mixin element-label {
  position: relative;
  
  &::before {
    position: absolute;
    top: 50%;
    left: 0;
    display: block;
    font-size: 1.2rem;
    font-weight: 700;
    color: #333;
    letter-spacing: 0;
    text-transform: none;
    transform: translate(-150%,-50%);
    background-color: #111;
    padding: .15rem .25rem;
    border-radius: .25rem;
  }
}

.container {
  display: flex;
  flex-direction: column;
  align-items: stretch;
  justify-content: stretch;
  min-height: 100vh;
}

.content,
.processed {
  flex: 1 0 0;
  padding: 2rem 4rem;
  outline: 0;
  word-break: break-all;
}

.content {
  border: 0;
  border-bottom: 1px solid #333;
  resize: none;
  font: inherit;
  color: inherit;
  background-color: inherit;
}

.processed {
  
  > *::first-child {
    margin-top: 0;
    padding-top: 0;
  }
  
  h1, h2, h3, h4, h5, h6 {
    margin: 1rem 0;
    padding: 0;

    @include element-label;
  }
  
  ul, ol, li {
    margin: 0;
    padding-top: 0;
    padding-bottom: 0;
  }
  
  > ul, > ol {
    color: var(--color-6);
    padding-left: 2rem;
    
    @include element-label;
    
    &::before {
      top: 0;
      transform: translate(-140%,0);
    }
  }
  
  > ul::before { content: "ul"; }
  
  > ol::before { content: "ol"; }
}

h1 {
  color: var(--color-4);
}

h2 {
  color: var(--color-1);
}

h3, h4 {
  color: var(--color-2);
}

h5, h6 {
  color: var(--color-6);
  text-transform: uppercase;
  letter-spacing: .1rem;
}

hr {
  height: 1px;
  border: 0;
  background-color: var(--color-3);
  margin: 3rem 0;
  padding: 0;
}

a {
  color: var(--color-2);
}

@for $i from 1 through 6 {
  h#{$i}::before {
    content: "h#{$i}";
  }
}

@media (min-width: 960px) {
  body {
    font-size: 1.8rem;  
  }
  
  .container {
    flex-direction: row;
  }
  
  .content {
    border-bottom: 0;
    border-right: 1px solid #333;
  }
}
</style>
