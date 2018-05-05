<template lang="html">
    <div>
      <div class="section">
        <label>Length&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</label>
        <input class="char-length" type="number" v-model="charLength">
      </div>
      <div class="section">
        <label>Special&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</label>
        <input class="char-special" type="input" v-model="charSpecial">
      </div>
      <div class="section">
        <label>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</label>
        <input class="char-special" type="button" @click="generate" value="Generate">
      </div>
      <div class="section">
        <label>Result&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</label>
        <input class="char-special" type="text" v-model="password" id="myPassword">
        <input class="char-special" type="button" @click="copy" value="copy">
      </div>
    </div>
</template>
<script>
  import Vue from 'vue'
  import Vuex from 'vuex'
  Vue.use(Vuex)

  const store = new Vuex.Store({
    state: {
      counter: 0
    },
    getters: {
      counter: state => state.counter * 2
    },
    mutations: {
      increment: state => state.counter++,
      decrement: state => state.counter--
    }
  })
  export default {
    data: function () {
      return {
        charLength: 8,
        charSpecial: '~!@#$%^&*',
        password: ''
      }
    },
    computed: {
      counter () {
        return this.$store.getters.counter
      }
    },
    created () { },
    mounted () { },
    methods: {
      tab () {
        chrome.tabs.create({ url: 'pages/app.html' })
      },
      increment: function () {
        this.$store.commit('increment')
      },
      decrement: function () {
        this.$store.commit('decrement')
      },
      generate: function () {
        if (this.charLength > 1) {
          var charList = this.charSpecial.split('')
          var tempPassword = []
          for (var i = 0; i < this.charLength; i++) {
            tempPassword.push(charList[this.getRandomIndex(charList.length)])
          }
        }
        this.password = tempPassword.join('')
      },
      copy: function () {
        var copyText = document.getElementById('myPassword')
        copyText.select()
        document.execCommand('Copy')
      },
      getRandomIndex: function (size) {
        return Math.floor(Math.random() * size)
      }
    },
    store: store
  }
</script>
<style lang="scss">
  div {
    color: blue
  }
  html, body {
    width: 410px;
    height: 200px;
  }

  .container {
    position: absolute;
    top: 50%;
    left: 50%;
    box-shadow: 0 0 25px rgba(0,0,0,0.08);
    transform: translateX(-50%) translateY(-50%);
    width: 300px;
    height: 200px;
    padding-bottom: 20px;
  }

  .counter {
    text-align: center;
    font-size: 40px;
    margin-bottom: 15px;
    color: #4B1248;
  }

  button {
  /*   margin-right: 5px; */
    padding: 10px 15px;
    font-size: 20px;
    background-image:linear-gradient(120deg, #84fab0 0%, #8fd3f4 100%);
    border: 1px solid #8fd3f4;
    outline: none;
  }

  button:hover {
    opacity: .8;
    cursor: pointer;
  }
  .char-length {
    width: 150px;
    height: 20px;
    font-size: 18px;
  }
  .actions-inner {
    display: flex;
    justify-content: center
  }
  .section {
    padding-top: 20px;
    padding-left: 50px;
  }
  label {
    font-size: 18px;
    color: black;
  }
</style>