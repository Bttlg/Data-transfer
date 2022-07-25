<template>
  <div class="container">
    <div class="content">
      <h2>Тохиргоо</h2>
      <b-field label="Төхөөрөмжийн нэр">
        <b-input v-model="settings.name" maxlength="30"></b-input>
      </b-field>
      <div class="field card card-content">
        <label class="label">Төхөөрөмжид өнгө өгөх</label>
        <div class="control">
          <span class="device-color" v-for="(color, index) in colors" :key="index" v-bind:class="{ selected: color == settings.color }" v-bind:style="{ 'background-color': color }" @click="settings.color = color"></span>
          <p>Өнгө өгөх эффектийг ашиглахыг тулд та сайтаа бүрэн refresh хийх хэрэгтэй.</p>
        </div>
      </div>
      <div class="field card card-content">
        <label class="label">Автоматаар эхлүүлэх</label>
        <div class="control">
          <b-checkbox v-model="settings.autoStart">Автоматаар файлуудыг хүлээж авах</b-checkbox>
        </div>
        <div class="control">
          <b-checkbox v-model="settings.autoBrowserDownload">Автоматаар файлуудыг браузероор эхлүүлж файл татагч програмаар татах (Зарим браузер ажиллахгүй)</b-checkbox>
        </div>
      </div>
      <div class="field card card-content">
        <label class="label">Автоматаар копидох</label>
        <div class="control">
          <b-checkbox v-model="settings.autoCopy">Ирсэн мессежийг автоматаар копидож авах.</b-checkbox>
        </div>
      </div>
      <div class="field card card-content">
        <label class="label">Үндсэн хуудас</label>
        <div class="control">
          <b-checkbox v-model="settings.defaultTab" true-value="1" false-value="0">Үндсэн хуудсаар <b>Мессеж</b> цонхыг сонгох</b-checkbox>
        </div>
      </div>
      <div class="field card card-content">
        <label class="label">Анимэйшн</label>
        <div class="control">
          <b-checkbox v-model="settings.anim">Сайтад анимейшн, эффект ашиглах.</b-checkbox>
        </div>
      </div>
      <div class="field card-content is-grouped" style="justify-content: center;">
        <div class="control">
          <button class="button is-medium is-success" @click="save">Хадгалах</button>
        </div>
        <div class="control">
          <button class="button is-medium is-light" @click="init">Болих</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Settings',

  data () {
    return {
      colors: [],
      settings: {}
    }
  },

  methods: {
    init () {
      this.settings = Object.assign({}, this.settings, this.$store.state.settings)

      const colors = [this.settings.color]
      let i = 0
      while (i < 15) {
        const color = `hsla(${~~(360 * Math.random())},60%,60%,1)`
        if (colors.indexOf(color) === -1) {
          colors.push(color)
          i++
        }
      }
      this.colors = colors
    },

    save () {
      this.$buefy.toast.open({
        duration: 2000,
        message: 'Settings Saved',
        position: 'is-top',
        type: 'is-primary'
      })
      this.$store.commit('updateSettings', this.settings)
      this.$router.push('/')
    }
  },

  mounted () {
    this.init()
  }
}
</script>

<style lang="sass">
.control .help.counter
  float: right
  margin-left: .5em

.device-color
  display: inline-block
  margin-right: 5px
  height: 30px
  width: 30px

.device-color.selected
  border: 3px solid rgba(0, 0, 0, 0.7)
</style>
