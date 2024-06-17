<template>
  <section @keyup.space="handleClick()" class="flex flex-col items-center bg-slate-200 h-full py-20">
    <Alert v-if="copied" class="absolute top-4 bg-slate-950 text-white rounded-[32px] px-4 py-1 text-sm font-light"></Alert>
    <h1 class="text-2xl font-semibold mb-14">Color palette generator</h1>
    <div class="lg:flex gap-6 lg:mb-0 sm:mb-4">
      <ColorCard @copyClicked="handleCopyClick()"></ColorCard>
      <ColorCard @copyClicked="handleCopyClick()"></ColorCard>
      <ColorCard @copyClicked="handleCopyClick()"></ColorCard>
      <ColorCard @copyClicked="handleCopyClick()"></ColorCard>
      <ColorCard @copyClicked="handleCopyClick()"></ColorCard>
    </div>
    <button @click="handleClick()" class="bg-purple text-white text-lg font-medium lg:px-28 sm:px-16 py-4 rounded mb-4 sm:mx-4">Generate palette</button>
    <p class="mb-10 sm:mx-2 text-sm">Or just press the "Spacebar" to generate new palettes.</p>
    <div class="flex items-center gap-1.5 bg-white rounded-[32px] px-4 py-1 sm:mx-4 text-sm">
      <span>Click to copy individual color</span>
      <i class="fa-solid fa-circle text-[6px]"></i>
      <span id="c">Press "C" to copy palette</span>
    </div>
  </section>
</template>

<script>
export default {
  name: 'IndexPage',
  data() {
    return {
      copied: false,
      cPress: false
    }
  },
  methods: {
    handleClick() {
      var url = "http://colormind.io/api/";
      var data = {
        model: "default"
      }
      var http = new XMLHttpRequest();

      http.onreadystatechange = function () {
        if (http.readyState == 4 && http.status == 200) {
          var palette = JSON.parse(http.responseText).result;
          var hexArr = []
          function toHex(c) {
            var hex = c.toString(16)
            return hex
          }
          function rgbToHex(r, g, b) {
            return (toHex(r) + toHex(g) + toHex(b))
          }
          for (let i = 0; i < 5; i++) {
            var hex = rgbToHex(...palette[i])
            document.getElementsByClassName('hexcode')[i].innerHTML = '#' + hex
            document.getElementsByClassName('colour')[i].style.backgroundColor = '#' + hex
            document.getElementsByClassName('colour')[i].ariaLabel = '#' + hex
            hexArr.push('#' + hex)
          }
          document.getElementById('c').ariaLabel = hexArr
        }
      }
      http.open("POST", url, true);
      http.send(JSON.stringify(data));
    },
    handleCopyClick() {
      this.copied = true
      setTimeout(() => {
        this.copied = false
      }, 3000);
    }
  },
  mounted() {
    this.handleClick()
    document.addEventListener('keyup', event => {
      if (event.code === 'Space') {
        this.handleClick()
      }
    })
    document.addEventListener('keyup', event => {
      if (event.key === 'c') {
        this.cPress = true
        this.handleCopyClick()
        document.getElementById('c').click()
      }
      if (this.cPress == true) {
        new ClipboardJS('#c', {
          text: function (trigger) {
            return trigger.getAttribute('aria-label')
          }
        })
      }
    })
    new ClipboardJS('.colour', {
      text: function (trigger) {
        return trigger.getAttribute('aria-label')
      }
    })
  }
}
</script>
