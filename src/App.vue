<template>
  <div id="app">
    <Drawer @close="toggle" align="left" :closeable="true">
      <div v-if="open">
          <h2>Nota</h2>
          <textarea v-model="note" v-html="save_session" placeholder="Note here..." name="name" rows="8" cols="80"></textarea>
      </div>
    </Drawer>
  </div>
</template>

<script>
import Drawer from "vue-simple-drawer"
export default {
  name: "app",
  beforeMount() {
     if ( localStorage.getItem( "note" ) ){
       var json = localStorage.getItem( "note" )
       var obj = JSON.parse(json).filter(a => a.url == document.location.href)
       if (obj) if (obj[0]) this.note = obj[0].content
     } else {
       localStorage.setItem( "note", "[]" )
     }

  },
  mounted() {
    window.addEventListener('keydown', (e) => {
      if (e.key == 'Escape') {
        this.toggle();
      }
    });
  },
  data() {
    return {
      // open: true
      open: false,
      note: '',
    }
  },
  components: {
    Drawer
  },
  methods: {
    toggle() {
      this.open = !this.open
    }
  },
  computed: {
    save_session() {
      var json = localStorage.getItem( "note" )
      var obj_ = JSON.parse(json)
      var obj = JSON.parse(json).filter(a => a.url == document.location.href)
      if (obj) if (!obj[0]) obj_ = obj_.concat(
        {
          url: document.location.href,
          content: this.note
        }
      )
      if (obj) if (obj[0]) obj_.filter(a => a.url == document.location.href)[0].content = this.note
      console.log(JSON.stringify(obj_));
      localStorage.setItem( "note", JSON.stringify(obj_) )
    },
  }
}
</script>
