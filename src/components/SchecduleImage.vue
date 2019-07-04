<template>
  <div class="my-2">
    <div class="img-fluid bg-dark px-2 py-1" >
      <div class="overflow-auto">
        <v-stage ref="stage" :config="configKonva">
          <v-layer>
            <v-group>
              <!-- <v-rect :config="{
                y: 8,
                width: width,
                height: titleHeight + schecduleArray.length * 80 + 16
                }"></v-rect> -->
              </v-group>
              <!-- 背景白色 -->
              <v-group>
                <!-- TITLE圖檔 -->
                <v-rect :config="{y: 8, width: width, height: titleHeight, fill: '#f2f4f6' }"></v-rect>
                <v-image :config="{ y: 20, image: image }"/>
                <v-text :config="{
                  x: 60,
                    y: 12 + 28 + 8,
                    fontSize: 70,
                    fontFamily: font,
                    fontStyle: 'bold',
                    fontWeight: 700,
                    fill: '#002d47',
                    text: displayDate,
                    width: 250,
                    height: titleHeight,
                    align: 'center'
                }"></v-text>
              </v-group>
              <v-group v-for="(elem, index) in schecduleArray" :key=" date +'_' + index">
                <!-- 背景白色 -->
                <v-rect :config="{x:0, y:titleHeight + 80 * index + 8, width: width, height:80, fill: index%2 === 0 ? 'white' : '#f2f4f6'}"></v-rect>
                <v-text :config="{
                  x: 0,
                    y: getListTimeY(index),
                    fontSize: 40,
                    fontFamily: font,
                    fontStyle: '700',
                    fill: '#ff7500',
                    text: elem.time,
                    width: 160,
                    height: 80,
                    align: 'center',

                }"></v-text>
                <v-text :config="{
                  x: 180,
                    y: getListMemberY(index),
                    fontSize: 40,
                    fontFamily: textFont,
                    fontStyle: '900',
                    fill: '#050505',
                    text: elem.member.join('、'),
                    width: 1000,
                    height: 80,
                    align: 'left',
                }"></v-text>
              </v-group>


            </v-layer>
            </v-stage>
      </div>

      </div>
      <button class="btn btn-warning" @click="save">下載 {{date}} 圖片</button>

  </div>
</template>
<script>
export default {
  name: "",
  props: ['schecduleArray', 'date'],
  data: () => ({
    // font: "'Noto Sans TC', sans-serif",
    textFont: 'Microsoft JhengHei',
    font: "Langdon",
    // textFont: 'Arial',
    width: 1280,
    height: 400,
    titleHeight: 144,
     configCircle: {
       x: 100,
       y: 100,
       radius: 70,
       fill: "red",
       stroke: "black",
       strokeWidth: 4
     },
     image: null,
  }),
  methods: {
    getListTimeY(index) {
      return 8 + this.titleHeight + 20 + 80 * index
    },
    getListMemberY(index) {
      return 8 + this.titleHeight + 16 + 80 * index
    },
    downloadURI(uri, name) {
        var link = document.createElement('a');
        link.download = name;
        link.href = uri;
        document.body.appendChild(link);
        link.click();
        document.body.removeChild(link);
      },
      save(){
        var dataURL = this.$refs.stage.getStage().toDataURL().replace("data:image/png", 'data:application/octet-stream');
        const name = this.displayDate.replace('/', "_").replace(" ", "") + '.png'
          this.downloadURI(dataURL,  name);
      }
  },
  computed:{
    displayDate(){
      return this.date.replace("/", " / ")
    },
    configKonva(){
      return {
         width: this.width,
         height: this.titleHeight + this.schecduleArray.length * 80 + 16,
         fill: 'white',
       }
    }
  },
  created() {
    const image = new window.Image();
    image.src = require('@/assets/title.png');
    image.onload = () => {
      // set image only when it is loaded
      this.image = image;
    };
  }
}
</script>
<style lang="scss" scoped>
</style>
