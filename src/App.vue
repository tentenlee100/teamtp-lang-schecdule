<template>
  <div >
    <b-navbar type="dark" variant="info">
      <b-navbar-brand href="#">Team TP 浪行程表-轉換圖片</b-navbar-brand>
    </b-navbar>
    <div class="container-fluid">
      <div class="mt-2 row">
        <div class="col-3">
          <div>
            <div class="d-flex justify-content-between align-items-end">
              <span class="col" for="exampleFormControlTextarea1">請輸入Facebook行程表格式做轉換</span>
              <button @click="createdImage" class="flex-shrink-1 btn btn-primary">轉換</button>
            </div>
            <div class="form-group mt-2">
              <textarea v-model="text" class="form-control" id="exampleFormControlTextarea1" rows="20"></textarea>
            </div>
          </div>
        </div>
        <div class="col-8">
          <div v-if="datas.length > 0" class="">
            <h4>請在圖片直接按右鍵另存圖檔 or 點擊下載圖片按紐</h4>
                      <SchecduleImage v-for="(elem, index ) in datas" :key="index + 'image'" :schecduleArray="elem.members" :date="elem.date"></SchecduleImage>
          </div>
          <div v-else class="">
            <h4>請於左邊框框貼上facebook預計貼上的時程表</h4>
          </div>

        </div>
      </div>
    </div>
  </div>
</template>

<script>
import SchecduleImage from '@/components/SchecduleImage'
import moment from 'moment'

export default {
  name: 'app',
  data: () => ({
    text: `📍7月1日（一）
✨林于馨 10:00
✨李采潔 18:00
✨王逸嘉 19:30
✨小山美玲 20:00
✨蔡伊柔 20:00
✨邱品涵 21:00
✨高云珏 21:00
✨潘姿怡 21:00
✨劉潔明 21:00
📍7月2日（二）
✨林易沄 12:00
✨潘姿怡 12:00
✨劉曉晴 12:00
✨李孟純 13:00
✨鄭妤葳 14:00
✨張羽翎 14:30
✨李佳俐 17:30
✨鄭佳郁 19:00
✨柏靈 20:00
✨林倢 21:00
✨高云珏 21:00
✨曾詩羽 21:30
✨阿部瑪利亞 22:00`,
    datas: []
  }),
  methods: {
    createdImage() {
      this.datas = []
      const schedule  = this.text.replace(new RegExp("✨", 'g'), "").replace(new RegExp("#", 'g'), "").split("\n").map(obj => {
        if (obj.length === 0){
          return null
        }
        const array = obj.split(" ").filter(obj => obj.length > 0)
        if (array.length === 1 && array[0].length > 0){
          console.warn(array[0], "not find member");
          if (array[0].indexOf("📍") > -1){
            return {[array[0]]: null}
          }
          return null
        }else if (array.length !== 2){

          return null
        }else if (array.length === 2 && array[1].length == 0){
          if (array[0].indexOf("📍") > -1){
            return {[array[0]]: null}
          }
          return null
        }
        let time = array[1]
        if (time.indexOf("早上") > -1){
          time = time.replace("早上", "")
          if (time.length < 5){
            time = "0" + time
          }
        }else if (time.indexOf("中午") > -1){
          time = time.replace("中午", "")
        }else if (time.indexOf("下午") > -1){
          time = time.replace("下午", "")
          timeArray = time.split(":")
          time = (parseInt(timeArray[0]) + 12 ) + ":" + timeArray[1]

        }else if (time.indexOf("晚上") > -1){
          time = time.replace("晚上", "")
          timeArray = time.split(":")
          time = (parseInt(timeArray[0]) + 12 ) + ":" + timeArray[1]
        }
        time = time.split('(')[0]
        time = time.split('（')[0]

        return {[time]: array[0] }
      }).filter(obj => obj !== null)

      console.log(schedule);
      let allDate = []
      let nowKey = ''
      let addObj = {}
      schedule.forEach((obj, index) => {
        if (Object.keys(obj).length == 0){
          return
        }
        const firstKey = Object.keys(obj)[0]
        if (firstKey.indexOf("📍") > -1){
          let date =  firstKey.replace("📍", "")
          nowKey = moment(date, "MM月DD日").format('M/D')
          // nowKey = '2019' + date
          if (Object.keys(addObj).length > 0){
            allDate.push(addObj)
          }
          addObj = {date: nowKey, members: []}
          // allDate[nowKey] = {}
          return
        }else{
          const index = addObj.members.findIndex(obj => obj.time === firstKey )
          if (index > -1){
            addObj.members[index].member.push(obj[firstKey])
          }else{
            addObj.members.push({time: firstKey, member: [obj[firstKey]] })
          }

        }
      })
      allDate.push(addObj)
      this.datas = allDate
      console.log(allDate);

    }
  },
  components: {
    SchecduleImage
  },
}
</script>

<style>
/* @import url('https://fonts.googleapis.com/css?family=Noto+Sans+TC:400,700&display=swap&subset=chinese-traditional'); */
/* @import url('https://db.onlinewebfonts.com/c/f4b819bb8e3dcf620c21806e8036adbf?family=Langdon'); */
/* .my-font{
font-family: 'Noto Sans TC', sans-serif;
font-weight: 700;
font-size: 80px;
} */
</style>
